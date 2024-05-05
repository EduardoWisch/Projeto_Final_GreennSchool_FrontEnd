<script>
import axios from 'axios';
export default {
    props: {
        showTaskMenu: {
            type: Boolean,
            required: true
        },
        taskId: {
            type: Number,
            required: true
        }
    },
    methods: {
        close() {
        this.$emit('closeTaskMenu')
    },
    deleteTask(taskId){
        axios.delete(`task/all/${taskId}`)
            .then(() => {
                console.log('Tarefa excluída com sucesso');
            })
            .catch(error => {
                console.error('Erro ao deletar a tarefa:', error);
            });
        },
    },
    mounted() {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api/'
    },
}
</script>

<template>
    <div class="container__taskMenu" v-if="showTaskMenu">
        <div class="taskMenu" @mouseleave="close()">
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
                <div class="delete" @click="deleteTask(taskId), close()">
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
    padding: 10%;
}

.container__content div {
    display: flex;
    margin-bottom: 15%;
    cursor: pointer;
    gap: 5%;
}

.delete p, .delete i {
    color: red;
}
</style>