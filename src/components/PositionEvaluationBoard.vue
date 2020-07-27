<script>
    import {chessboard} from 'vue-chessboard'

    export default {
        name: 'PositionEvaluationBoard',
        extends: chessboard,
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
            calculateBestMove(newGameMoves) {
                var bestMove = undefined;
                //use any negative large number
                var bestValue = -9999;

                for (var i = 0; i < newGameMoves.length; i++) {
                    var newGameMove = newGameMoves[i];
                    this.game.move(newGameMove);
                    this.board.set({
                        fen: this.game.fen(),
                        turnColor: this.toColor(),
                        movable: {
                            color: this.toColor(),
                            dests: this.possibleMoves(),
                            events: {after: this.userPlay()},
                        }
                    });
                    //take the negative as AI plays as black
                    var boardValue = -this.evaluateBoard();
                    this.game.undo();
                    if (boardValue > bestValue) {
                        bestValue = boardValue;
                        bestMove = newGameMove
                    }
                }
                return bestMove;
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
                let moves = this.game.moves({verbose: true})
                this.game.move(this.calculateBestMove(moves))

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
        },
        mounted() {
            this.board.set({
                movable: {events: {after: this.userPlay()}},
            })
        }
    }
</script>
