<template>
    <div id="app">
        <h1>Play against minimax AI with Alpha-Beta pruning</h1>
        <MinimaxAlphaBetaPruningBoard/>

        <h1>Play against minimax AI</h1>
        <MinimaxBoard/>

        <h1>Play against position eval AI</h1>
        <PositionEvaluationBoard/>

        <h1>Play against random AI</h1>
        <RandomAIBoard/>
    </div>
</template>

<script>
        import RandomAIBoard from "./components/RandomAIBoard";
        import PositionEvaluationBoard from "./components/PositionEvaluationBoard";
        import MinimaxBoard from "./components/MinimaxBoard";
        import MinimaxAlphaBetaPruningBoard from "./components/MinimaxAlphaBetaPruningBoard";

        export default {
        name: 'App',
        components: {
            MinimaxAlphaBetaPruningBoard,
            MinimaxBoard,
            PositionEvaluationBoard,
            RandomAIBoard
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
