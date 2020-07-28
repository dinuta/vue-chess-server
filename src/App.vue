<template>
    <div id="app">
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

        export default {
        name: 'App',
        components: {
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
