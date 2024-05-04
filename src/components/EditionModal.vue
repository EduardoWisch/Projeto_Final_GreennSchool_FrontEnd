<script>
import axios from 'axios';

    export default {
        data() {
            return {
                editedTask: {
                title: '',
                description: '',
                due_date: ''
                }
            }
        },
        props: {
            showEditionModal: {
                type: Boolean,
                required: true
            },
            task: {
                type: Object, // Defina o tipo da propriedade como um objeto
                required: true
            },
        },
        methods: {
            closeEdition() {
                    this.$emit('closeEditionModal')
                    console.log('até aqui está certo')
                },
            editTask(){
                console.log(this.task.title)
            }
        },
        mounted() {
            axios.defaults.baseURL = 'http://127.0.0.1:8000/api/'
        }
    }
</script>

<template>
    <div class="container__editionModal" v-if="showEditionModal">
        <div class="editionModal">
            <form @submit.prevent="editTask">
                <div class="container__inputs">
                    <input type="text" id="title" name="title" :placeholder="task.title">
                    <input type="text" id="description" name="description" :placeholder="task.description" >
                    <input type="date" id="due_date" name="due_date">
                </div>
                <div class="container__butons">
                    <div id="editionCancel" @click="closeEdition()">Cancelar</div>
                    <button id="editionEdition">Editar tarefa</button>
                </div>
            </form>
        </div>
    </div>
</template>

<style scoped>
    .container__editionModal{
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    background-color: rgba(0, 0, 0, 0.2);
    /* backdrop-filter: blur(1px); */
    display: flex;
    align-items: center;
    justify-content: center;
}

.editionModal{
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

#editionCancel{
    background-color: #F7F7F7;
    color: black;
    cursor: pointer;
}

#editionEdition{
    background-color: black;
    color: white;
    cursor: pointer;
}
</style>