<template>

    <!-- Placar -->
    <div class="scoreboard">
        <h1>Placar: Usuário {{ this.userScore }} x {{ this.computerScore }} Computador</h1>
    </div>

</template>

<script>
export default {

    props: ['userScore', 'computerScore'],

}
</script>

<style lang="scss">
.scoreboard {
    background: linear-gradient(to right, #4a90e2, #50e3c2);
    color: white;
    padding: 15px 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    margin-bottom: 20px;
    font-size: 24px;
    font-weight: bold;
}

.scoreboard h1 {
    margin: 0;
}
</style>