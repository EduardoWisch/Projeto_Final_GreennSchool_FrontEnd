<script>
    import axios from 'axios';
    
    export default {
        data() {
            return {
                taskData: {title: '', description: '', due_date: ''}
            }
        },
        // watch: {
        //     showModal(newValue) {

        //     }
        // },
        props: {
            showModal: {
                type: Boolean,
                required: true
            }
        },
        methods: {
            close() {
                this.$emit('closeModal')
            },

            createTask(){
                axios.post('task', this.taskData)
                .then((response) => console.log (response))
            }
        },
        mounted() {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api/'
  
        this.createTask()
      },
    }
</script>

<template>
    <div class="container__createModal" v-if="showModal">
        <div class="createModal">
            <form @submit.prevent="createTask">
                <div class="container__inputs">
                    <input type="text" id="title" name="title" placeholder="Nome da Tarefa" v-model="taskData.title">
                    <input type="text" id="description" name="description" placeholder="Descrição" v-model="taskData.description">
                    <input type="date" id="due_date" name="due_date" placeholder="Selecione uma data" v-model="taskData.due_date">
                </div>
                <div class="container__butons">
                    <div id="createCancel" @click="close()">Cancelar</div>
                    <button id="createCreate">Criar tarefa</button>
                </div>
            </form>
        </div>
    </div>
</template>

<style scoped>
.container__createModal{
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    background-color: transparent;
    display: flex;
    align-items: center;
    justify-content: center;
}

.createModal{
    display: grid;
    gap: 15px;
    width: 50%;
    height: 30vh; 
    height: fit-content;
    position: relative;
    background-color: white;
    color: #81858E;
    border: 1px solid #E5E5E5;
}

.container__inputs{
    padding: 2% 2% 0 2%;
}

.container__inputs input{
    display: block;
    border: none;
    outline: none;
    width: 100%;
    margin-bottom: 2%;
    font-size: 20px;
}

#dateTask {
    padding: 1% 2%;
    width: fit-content;
    border: 1px solid #E5E5E5;
    color: #81858E;
}

.container__butons{
    display: flex;
    padding: 2% 2%;
    justify-content: flex-end;
    gap: 5%;
    border-top: 2px solid #E5E5E5;
}

.container__butons button, .container__butons div {
    padding: 1.5% 4%;
    border: none;
    font-weight: 600;
    font-size: 18px;
}

#createCancel{
    background-color: #F7F7F7;
    color: black;
    cursor: pointer;
}

#createCreate{
    background-color: black;
    color: white;
    cursor: pointer;
}

</style>