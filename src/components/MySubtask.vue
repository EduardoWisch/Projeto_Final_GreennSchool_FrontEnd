<script>
import axios from 'axios';

    export default {
        data() {
            return {
                isSubtaskHover: false,
                editTitleSubtask: false,
                subtaskData: {
                    title: '',
                    description: '',
                }
            }
        },
        props: {
            subtask: {
                type: Object,
                required: true
                }
            },
        methods: {
            refresh(){
                this.$emit('refreshTask')
            },
            subtaskHover() {
                this.isSubtaskHover = true
            },
            subtaskNotHover() {
                this.isSubtaskHover = false
            },
            deleteSubtask(id){
                axios.delete(`subtask/${id}`)
                .then (() => {
                    console.log('Subtarefa deletada')
                    this.refresh()
                })
                .catch(error => {
                    console.error('Erro ao deletar a subtarefa:', error);
                });
            },
            updateStatusSubtask(subtask) {

                const newStatus = subtask.status === 'completed' ? 'pending' : 'completed';


                if (newStatus !== 'completed' && newStatus !== 'pending') {
                    console.error('Novo status inválido:', newStatus);
                    return;
                }

                subtask.status = newStatus;

                axios.put(`subtask/${subtask.id}`, { status: newStatus })
                .then(() => {
                    console.log('Status alterado com sucesso')
                })
                .catch(error => {
                    console.error('Erro ao atualizar a tarefa:', error);
                    subtask.status = subtask.status === 'completed' ? 'pending' : 'completed';
                });
            },
            checkTitle(){
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
            editSubtaskData(){
                this.checkTitle()
                this.checkDescription()
                const subtaskFields = {
                    title: this.subtaskData.title !== '' ? this.subtaskData.title : undefined,
                    description: this.subtaskData.description !== '' ? this.subtaskData.description : undefined,
                };
                axios.put(`subtask/${this.subtask.id}`, subtaskFields)
                .then(response => {
                    console.log('Tarefa editada com sucesso:', response.data);
                    this.refresh()
                    this.$emit('closeEditionModal');
                    this.editTitleSubtask = false
                    this.clearSubtaskTask();
                })
                .catch(error => {
                    console.error('Erro ao editar a tarefa:', error);
                });
            },
            clearSubtaskTask() {
                this.subtaskData.title = '';
                this.subtaskData.description = '';
            },
        }       
    }
</script>

<template>
    <div class="mySubtasks" @mouseover="subtaskHover()" @mouseout="subtaskNotHover()">
            <div></div>
            <i class="bi bi-circle" @click="updateStatusSubtask(subtask)" v-if="subtask.status == 'pending'"></i>
            <i class="bi bi-check-circle-fill" @click="updateStatusSubtask(subtask)" v-else></i>
            
            <div class="mySubtask-titles">
                <div v-if="editTitleSubtask === true">
                    <form form-editTitle @submit.prevent="editSubtaskData">
                        <input type="text" id="title" name="title" placeholder="Atualize sua subtask" v-model="subtaskData.title">
                        <input type="text" id="description" name="description" :placeholder="subtask.description" v-model="subtaskData.description">
                        <div id="editCancel" @click="editTitleSubtask = false, isSubtaskHover = false, clearSubtaskTask()">Cancelar</div>
                        <button id="editTitle"   @click="isSubtaskHover = false">Alterar</button>
                    </form>
                </div>
                <div v-else>
                    <p class="titleSubtask">{{ subtask.title }}</p>
                    <p class="descriptionSubtask">{{ subtask.description }}</p>
                </div>
                <div class="mySubtask-container__icons" v-show="isSubtaskHover">
                    <i class="bi bi-pencil" @click="editTitleSubtask = true"></i>
                    <i class="bi bi-trash" @click="deleteSubtask(subtask.id)"></i>
                </div>
            </div>
    </div>
</template>

<style scoped>

.titleSubtask {
  overflow: hidden; 
  text-overflow: ellipsis; 
}

.descriptionSubtask {
    overflow: hidden; 
    text-overflow: ellipsis; 
    font-size: 14px;
}

.bi-circle, .bi-check-circle-fill, .bi-pencil, .bi-trash  {
    cursor: pointer;
} 

.mySubtask-titles{
  width: 84%;
  display: flex;
  justify-content: space-between;
}

.mySubtask i {
  cursor: pointer;
}

.mySubtask-title p{
  white-space: nowrap; 
  overflow: hidden; 
  text-overflow: ellipsis; 
}

.mySubtask-container__icons {
  display: flex;
  gap: 40%;
}

.form-editTitle{
  display: flex;
  width: 100%;
}

#title{
    width: 200px;
    border: none;
    outline: none;
    width: 100%;
    font-size: 20px;
}

#description{
    width: 200px;
    border: none;
    outline: none;
    width: 100%;
    font-size: 16px;
}



#editCancel{
    padding: 1% 4%;
    border: none;
    font-weight: 600;
    font-size: 14px;
    background-color: transparent;
    color: red;
    cursor: pointer;
}

#editTitle{
    padding: 1% 4%;
    border: none;
    font-weight: 600;
    font-size: 14px;
    background-color: transparent;
    color: green;
    cursor: pointer;
}
</style>