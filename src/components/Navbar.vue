<script>

</script>

<template>
    <header class="container__navbar">
        <div class="container__rockImg">
            <img class="rockImg" src="./icons/rock.png">
        </div>
        <div class="container__icons">
            <i class="bi bi-plus-lg"></i>
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

    .rockImg, .iconImg {
        width: 30px;
        height: 30px;
    }

</style>