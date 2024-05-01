<script>
    export default {
        methods: {
            open() {
                this.$emit('openModal')
            }
        },
    }
</script>

<template>
    <header class="container__navbar">
        <div class="container__rockImg">
            <img class="rockImg" src="./icons/rock.png">
        </div>
        <div class="container__icons" >
            <div class="container__createTask" @click="open()">
                <i class="bi bi-plus-lg"></i>
                <p class="bi">Criar tarefa</p>
            </div>
            <i class="bi bi-question-circle"></i>
            <i class="bi bi-bell"></i>
            <img class="iconImg" src="./icons/icon.png">
        </div>
    </header>
</template>

<style scoped>
    .container__navbar{
        position: fixed;
        display: flex;
        justify-content: space-between;
        padding: 1% 5%;
        background-color: black;
        width: 100vw;
        height: 10vh;
        top: 0;
    }
    .bi {
        font-size: 25px;
        color: white;
    }
    .container__rockImg{
        display: flex;
        margin: auto 0;

    }

    .container__icons{
        display: flex;
        margin: auto 0;
        align-items: center;
        justify-content: flex-end;
        gap: 30%;
    }

    .container__createTask{
        display: flex;
        gap: 10px;
        cursor: pointer;
    }
    .container__createTask p{
        font-size: 16px;
        white-space: nowrap; /* Impede que o texto quebre para uma nova linha */
    }

    .rockImg, .iconImg {
        width: 30px;
        height: 30px;
    }

</style>