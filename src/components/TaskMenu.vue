<script>
export default {
    props: {
        showTaskMenu: {
            type: Boolean,
            required: true
        },
    },
    methods: {
        close() {
        this.$emit('closeTaskMenu')
    },
    }
}
</script>

<template>
    <div class="container__taskMenu" v-if="showTaskMenu">
        <div class="taskMenu">
            <div class="container__content">
                <div>
                    <i class="bi bi-link-45deg"></i>
                    <p>Copiar link da tarefa</p>
                </div>
                <div>
                    <i class="bi bi-copy"></i>
                    <p>Duplicar tarefa</p>
                </div>
                <div>
                    <i class="bi bi-printer"></i>
                    <p>Imprimir tarefa</p>
                </div>
                <div class="delete" >
                    <i class="bi bi-trash"></i>
                    <p>Excluir tarefa</p>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>

i {
    font-size: 20px;
    color: black;
}

p{
    color: black;
}
    .container__taskMenu{
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    background-color: transparent;
    /* backdrop-filter: blur(1px); */
    display: flex;
}

.taskMenu{
    display: grid;
    gap: 15px;
    width: 20vw;
    height: 40vh; 
    position: relative;
    margin-left: 65%;
    margin-top: 3.5%;
    background-color: white;
    color: #81858E;
    border: 1px solid #E5E5E5;
}

.container__content {
    padding: 5%;
}

.container__content div {
    display: flex;
    margin-bottom: 10%;
    cursor: pointer;
    gap: 5%;
}

.delete p, .delete i {
    color: red;
}
</style>