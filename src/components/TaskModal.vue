<script>
import axios from 'axios';
import MySubtask from '@/components/MySubtask.vue';
import CreateSubtaskModal from '@/components/CreateSubtaskModal.vue';
import TaskMenu from '@/components/TaskMenu.vue';
import {format} from 'date-fns'
export default {
    emits: ['closeTaskModal'],
    data(){
        return {
            showCreateSubtaskModal: false,
            showTaskMenu: false
        }
    },
    props: {
        showTaskModal: {
            type: Boolean,
            required: true
        },
        task: {
            type: Object,
            required: true
        }
    },
    computed: {
        formattedCreatedAt(){
            const formattedDate = format(new Date(this.task.created_at), 'dd/MM/yyyy');
            const hour = format(new Date(this.task.created_at), 'HH:mm');
            return `${formattedDate} às ${hour}`;
        },
        formattedDueDate(){
            return format(new Date(this.task.due_date), 'dd/MM/yyyy')
        },
        formattedUpdatedAt(){
            const formattedDate = format(new Date(this.task.updated_at), 'dd/MM/yyyy');
            const hour = format(new Date(this.task.updated_at), 'HH:mm');
            return `${formattedDate} às ${hour}`;
        },
        deadlineStatus() {

            const deadlineDate = new Date(this.task.due_date);
            deadlineDate.setHours(0, 0, 0, 0); 
            
 
            const currentDate = new Date();
            currentDate.setHours(0, 0, 0, 0); 
            
            if (currentDate > deadlineDate) {
                return 'overdue'; 
            } else {
                return 'withinDeadline'; 
            }
        }
    },
    methods: {
        close() {
            this.$emit('closeTaskModal')
        },
        updateStatusTask(task) {

              const newStatus = task.status === 'completed' ? 'pending' : 'completed';

              if (newStatus !== 'completed' && newStatus !== 'pending') {
                console.error('Novo status inválido:', newStatus);
                return;
              }

              task.status = newStatus;

              axios.put(`task/${task.id}`, { status: newStatus })
              .then(() => {
                console.log('Status alterado com sucesso');

                this.updateStatusSubtasks(task.subtasks, newStatus);
              })
              .catch(error => {

                console.error('Erro ao atualizar a tarefa:', error);
                task.status = task.status === 'completed' ? 'pending' : 'completed';
              });
            },
            updateStatusSubtasks(subtasks, newStatus) {

            subtasks.forEach(subtask => {
            subtask.status = newStatus;
   
            axios.put(`subtask/${subtask.id}`, { status: newStatus })
            .then(() => {
              console.log(`Status da subtarefa ${subtask.id} atualizado para ${newStatus}`);
            })
            .catch(error => {
              console.error(`Erro ao atualizar o status da subtarefa ${subtask.id}:`, error);

              subtask.status = newStatus === 'completed' ? 'pending' : 'completed';
            });
          });
        },
        openCreateSubtaskModal() {
            this.showCreateSubtaskModal = true;
        },
        closeCreateSubtaskModal() {
            this.showCreateSubtaskModal = false;
        },
        openTaskMenu() {
            this.showTaskMenu = true;
        },
        closeTaskMenu() {
            this.showTaskMenu = false;
        }
    },
    mounted() {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api/'
    },
    components: {
        MySubtask,
        CreateSubtaskModal,
        TaskMenu,
    }
}   
</script>

<template>
     <div class="container__taskModal" v-if="showTaskModal">
        <div class="taskModal">
            <div class="container__top">
                <div class="date dateWithinDeadline" v-if="deadlineStatus === 'withinDeadline'">
                    <i class="bi bi-calendar4"></i>
                    <p>No prazo</p>
                </div>
                <div class="date dateOverdue" v-else>
                    <i class="bi bi-calendar4"></i>
                    <p>Atrasada</p>
                </div>
                <div class="container__icons">
                    <i class="bi bi-three-dots" @click="openTaskMenu()"></i>
                    <i class="bi bi-x-lg" @click="close()"></i>
                </div>
            </div>
            <div class="container__content">
                <div  class="container__tasks">
                    <i class="bi bi-circle" @click="updateStatusTask(task)" v-if="task.status == 'pending'"></i>
                    <i class="bi bi-check-circle-fill" @click="updateStatusTask(task)" v-else></i>
                    <div class="content">
                        <h1>{{ task.title }}</h1>
                        <p>{{ task.description }}</p>
                        <h2>Subtarefas</h2>
                        <div class="container__subtask">
                            <div class="container__subtask"> 
                                <MySubtask v-for="subtask in task.subtasks" :key="subtask.id" :subtask="subtask" class="subtask"/>
                            </div>
                        </div>
        
                        <div class="createSubtarefa" @click="openCreateSubtaskModal()">
                            <i class="bi bi-plus-lg"></i>
                            <p @click="openCreateSubtaskModal">Criar Subtarefa</p>
                        </div>

                    </div>
                </div>
                <div class="container__information">
                    <div class="create">
                        <p>Criado em</p>
                        <div>
                            <i class="bi bi-calendar4"></i>
                            <p>{{ formattedCreatedAt }}</p>
                        </div>
                    </div>
                    <div class="expired">
                        <p>Data de vencimento</p>
                        <div :class="{ 'within-deadline': deadlineStatus === 'withinDeadline', 'overdue': deadlineStatus === 'overdue' }">
                            <i class="bi bi-calendar4"></i>
                            <p>{{formattedDueDate}}</p>
                        </div>
                    </div>
                    <div class="updated">
                        <p>Modificado em</p>
                        <div>
                            <i class="bi bi-calendar4"></i>
                            <p>{{ formattedUpdatedAt }}</p>
                        </div>
                    </div>
                    <div class="idTask">
                        <p>ID da tarefa</p>
                        <p style="color: black; font-weight: 600;">{{ task.id }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <CreateSubtaskModal v-if="showCreateSubtaskModal" :showCreateSubtaskModal="showCreateSubtaskModal" @closeCreateSubtaskModal="closeCreateSubtaskModal" :id_task="task.id" :taskDueDate="task.due_date" />
    <TaskMenu v-if="showTaskMenu" :showTaskMenu="showTaskMenu" @closeTaskMenu="closeTaskMenu" :taskId="task.id"/>
</template>

<style scoped> 

.bi-check-circle-fill, .bi-circle {
    color: black;
    cursor: pointer;
}

.container__taskModal{
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    background-color: rgba(0, 0, 0, 0.2);
    /* backdrop-filter: blur(1px); */
    display: flex;
    align-items: top;
    justify-content: center;
}

.taskModal{
    display: flex;
    flex-direction: column;
    margin-top: 1%;
    width: 60%;
    height: 90vh; 
    position: relative;
    background-color: white;
    color: #81858E;
    border: 1px solid #E5E5E5;
}

.container__top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2% 3%;
    border-bottom: 1px solid #D9D9D9;
}

.date {
    display: flex;
    justify-content: center;
    width: 15%;
    gap: 5%
}

.container__icons i{
    margin-left: 30px;
    cursor: pointer;
}

.container__content {
    display: grid;
    height: 100%;
    grid-template-columns: 65% 35%;
}

.container__tasks{
    padding: 5%;
    display: grid;
    grid-template-columns: 5% 90%;
    gap: 2%;
}

.content {
    font-size: 14px;
}

.content h1 {
    color: black;
    margin-bottom: 2%;
    font-weight: 600;
}

.content p {
    color: #81858E;
    margin-bottom: 3%;
}

.content h2 {
    color: black;
    margin-bottom: 2%;
    font-weight: 600;
}

.container__subtask {
    border-top: 1px solid #E5E5E5;
}

.subtask {
    margin-top: 3%;
    display: flex;
    margin-left: -5%;
    gap: 5%;
    color: #000;
    font-size: 18px;
}

.container__information{
    display: block;
    background-color: #F7F7F7;
    height: 100%;
    padding: 10%;
}

.create, .expired, .updated, .idTask {
    margin-bottom: 10%;
    font-size: 16px;
}

.create div, .expired div, .updated div{
    display: flex;
    gap: 5%;
}

.create div p {
    color: black;
    font-weight: 600;
}

.expired div p {
    font-weight: 600;
}

.updated div p {
    color: black;
    font-weight: 600;
}

.within-deadline {
    color: #009488;
}

.overdue {
    color: #D31408;
}

.dateWithinDeadline{
    background-color: rgba(0, 148, 136, 0.1);
    color: #009488;
}

.dateOverdue{
    background: rgba(211, 20, 8, 0.1);
    color: #D31408;
}

.createSubtarefa{
  display: flex;
  margin-top: 3%;
  color: black;
  gap: 2.5%;
  padding: 0% 0% 0% 0;
  font-size: 18px;
  cursor: pointer;
}
</style>