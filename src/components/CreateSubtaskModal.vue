<script>
import axios from 'axios';

export default {
  data() {
    return {
      subtaskData: {
        id_task: '',
        title: '',
        description: '',
        due_date: ''
        // Adicione outros campos de subtarefa conforme necessário
      }
    };
  },
  props: {
    showCreateSubtaskModal: {
      type: Boolean,
      required: true
    },
    id_task: {
      type: Number, // Supondo que taskId seja o ID da tarefa à qual a subtarefa está associada
      required: true
    },
    taskDueDate: {
      type: String, // Suponha que a data de vencimento seja uma string formatada
      required: true
    }
  },
  methods: {
    close() {
        this.$emit('closeCreateSubtaskModal')
    },
    checkTitle(){
      if (this.subtaskData.title.length === 0){
        alert('Por favor, insira um título para a subtarefa')
        return
      }
      if(this.subtaskData.title.length > 30){
        alert('O título deve ter no máximo 50 caracteres')
        return
      }
    },
    checkDescription(){
      if(this.subtaskData.description.length > 50){
        alert('Sua descrição é muito grande para uma subtarefa')
        return
      }
    },
    createSubtask() {
      this.checkTitle()
      this.checkDescription()
      // Antes de criar a subtarefa, atribua o ID da tarefa ao objeto subtaskData
      this.subtaskData.id_task = this.id_task;
      // Defina a data de vencimento da subtarefa como a data de vencimento da tarefa
      this.subtaskData.due_date = this.taskDueDate;

      // Enviar solicitação para criar uma nova subtarefa
      axios.post(`subtask`, this.subtaskData)
        .then(response => {
          console.log('Subtarefa criada com sucesso:', response.data);
          // Emitir um evento para notificar o componente pai sobre a criação da subtarefa
          this.$emit('subtaskCreated', response.data);
          // Fechar o modal de criação de subtarefa
          this.$emit('close');
        })
        .catch(error => {
          console.error('Erro ao criar subtarefa:', error);
          // Trate os erros adequadamente (exibir mensagem de erro, etc.)
        });
    },
    mounted() {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api/'
    },
  }
}
</script>

<template>
    <div class="container__createSubtaskModal" v-if="showCreateSubtaskModal">
      <div class="createSubtaskModal">
        <form @submit.prevent="createSubtask">
            <div class="container__inputs">
                <input type="text" id="title" name="title" placeholder="Nome da Subtarefa" v-model="subtaskData.title">
                <input type="text" id="description" name="description" placeholder="Descrição da Subtarefa" v-model="subtaskData.description">
            </div>
            <div class="container__butons">
                <div id="createCancel" @click="close()">Cancelar</div>
                <button id="createCreate">Criar Subtarefa</button>
            </div>
        </form>
      </div>
    </div>
  </template>

<style scoped>
.container__createSubtaskModal{
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

.createSubtaskModal{
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