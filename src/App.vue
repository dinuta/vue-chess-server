<template>
    <div id="app">
<!--        <h1>Play against random AI</h1>-->
<!--        <RandomAIBoard/>-->

        <h1>Position Evaluation AI</h1>
        <PositionEvaluationBoard/>
    </div>
</template>

<script>
        // import RandomAIBoard from "./components/RandomAIBoard";
        import PositionEvaluationBoard from "./components/PositionEvaluationBoard";

        export default {
        name: 'App',
        components: {
            PositionEvaluationBoard,
            // RandomAIBoard
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
