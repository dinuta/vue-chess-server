<script>
    import {chessboard} from 'vue-chessboard'

    export default {
        name: 'MinimaxAlphaBetaPruningBoard',
        extends: chessboard,
        data() {
            return {
                positionCount: 0,
                depth: 4
            }
        },
        methods: {
            userPlay() {
                return (orig, dest) => {
                    if (this.isPromotion(orig, dest)) {
                        this.promoteTo = this.onPromotion()
                    }
                    this.game.move({from: orig, to: dest, promotion: this.promoteTo}) // promote to queen for simplicity
                    this.board.set({
                        fen: this.game.fen()
                    })
                    this.calculatePromotions()
                    this.aiNextMove()
                };
            },
            minimaxRoot(depth, game, isMaximisingPlayer) {

                var newGameMoves = game.moves();
                var bestMove = -9999;
                var bestMoveFound;

                for (var i = 0; i < newGameMoves.length; i++) {
                    var newGameMove = newGameMoves[i]
                    game.move(newGameMove);
                    this.board.set({
                        fen: game.fen(),
                        turnColor: this.toColor(),
                        movable: {
                            color: this.toColor(),
                            dests: this.possibleMoves(),
                            events: {after: this.userPlay()},
                        }
                    });
                    var value = this.minimax(depth - 1, game, -10000, 10000, !isMaximisingPlayer);
                    game.undo();
                    if (value >= bestMove) {
                        bestMove = value;
                        bestMoveFound = newGameMove;
                    }
                }
                return bestMoveFound;
            },
            minimax(depth, game, alpha, beta, isMaximisingPlayer) {
                this.positionCount++;
                if (depth === 0) {
                    return -this.evaluateBoard();
                }

                var newGameMoves = game.moves();
                var bestMove;
                if (isMaximisingPlayer) {
                    bestMove = -9999;
                    for (var i = 0; i < newGameMoves.length; i++) {
                        game.move(newGameMoves[i]);
                        bestMove = Math.max(bestMove, this.minimax(depth - 1, game, alpha, beta, !isMaximisingPlayer));
                        game.undo();
                        alpha = Math.max(alpha, bestMove);
                        if (beta <= alpha) {
                            return bestMove;
                        }
                    }
                    return bestMove;
                } else {
                    bestMove = 9999;
                    for (var j = 0; j < newGameMoves.length; j++) {
                        game.move(newGameMoves[j]);
                        bestMove = Math.min(bestMove, this.minimax(depth - 1, game, alpha, beta, !isMaximisingPlayer));
                        game.undo();
                        beta = Math.min(beta, bestMove);
                        if (beta <= alpha) {
                            return bestMove;
                        }
                    }
                    return bestMove;
                }
            },
            evaluateBoard() {
                var totalEvaluation = 0;
                var lines = ["1", "2", "3", "4", "5", "6", "7", "8"];
                var columns = ["a", "b", "c", "d", "e", "f", "g", "h"];

                for (var i = 0; i < lines.length; i++) {
                    for (var j = 0; j < columns.length; j++) {
                        totalEvaluation = totalEvaluation +
                            this.getPieceValue(this.board.state.pieces[columns[j] + lines[i]]);
                    }
                }
                return totalEvaluation;
            },
            getPieceValue(piece) {
                if (piece === undefined) {
                    return 0;
                }
                var getAbsoluteValue = function (piece) {
                    if (piece.role === 'pawn') {
                        return 1;
                    } else if (piece.role === 'rook') {
                        return 5;
                    } else if (piece.role === 'knight') {
                        return 3;
                    } else if (piece.role === 'bishop') {
                        return 3;
                    } else if (piece.role === 'queen') {
                        return 9;
                    } else if (piece.role === 'king') {
                        return 90;
                    }
                    throw "Unknown piece type: " + piece.type;
                };

                var absoluteValue = getAbsoluteValue(piece);
                return piece.color === 'white' ? absoluteValue : -absoluteValue;
            },
            aiNextMove() {
                var bestMove = this.minimaxRoot(this.depth, this.game, true);
                this.game.move(bestMove);

                this.board.set({
                    fen: this.game.fen(),
                    turnColor: this.toColor(),
                    movable: {
                        color: this.toColor(),
                        dests: this.possibleMoves(),
                        events: {after: this.userPlay()},
                    }
                });
            },
        }
        ,
        mounted() {
            this.board.set({
                movable: {events: {after: this.userPlay()}},
            })
        }
    }
</script>
