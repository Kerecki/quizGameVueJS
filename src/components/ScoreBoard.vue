<template>
    <section class="score">
        Player <span>{{this.playerScore}}</span> X <span>{{this.computerScore}}</span> Computer
    </section>
</template>

<script>
    export default{
        props : ['playerScore', 'computerScore']
    }
</script>

<style lang="scss">
    section.score {
        border-bottom: 1px solid #000000;
        padding: 24px;
        font-size: 18px;

        span {
            padding: 8px;
            font-weight: bold;
            border: 1px solid #000;
        }
    }

</style>