<template>
    <section class="score">
        Player <span>{{ winCount }}</span> || <span>{{ loseCount }}</span> Computer
    </section>
</template>



<script>

    export default {
        props: ['winCount', 'loseCount']
    }

</script>



<style scoped>

section.score {
    border-bottom: 1px solid grey;
    padding: 24px;
    font-size: 18px;
    
}

span {
    padding: 10px;
    font-weight: bold;
    border: 1px solid rgb(106, 143, 208);
}

</style>
