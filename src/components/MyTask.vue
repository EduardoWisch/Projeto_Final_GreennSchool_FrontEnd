<script>
import axios from 'axios';
import {format} from 'date-fns'
import MySubtask from '@/components/MySubtask.vue'

    export default {
        emits: ['openTaskModal', 'openEditionModal', 'refreshTask'],
        data() {
            return {
                isTaskHover: false,
                editDate: false,
                taskDataDate: {
                due_date: ''
                },
            }
        },
        props: {
            task: {
            type: Object,
            required: true
            },
        },
        computed: {
          deadlineStatus() {
            const deadlineDate = new Date(this.task.due_date);
            const currentDate = new Date();
            const today = new Date(currentDate.getFullYear(), currentDate.getMonth(), currentDate.getDate()); 
            
            if (today > deadlineDate) {
                return 'overdue'; 
            } else if (today.getTime() === deadlineDate.getTime()) {
                return 'today'; 
            } else {
                return 'withinDeadline'; 
            }
          }
        },
        methods: {
            openEdition(){
              this.$emit('openEditionModal', this.task)
            },
            openModalTask(){
              this.$emit('openTaskModal', this.task)
            },
            refresh(){
              this.$emit('refreshTask')
            },
            taskHover() {
                this.isTaskHover = true
            },
            taskNotHover() {
                this.isTaskHover = false
            },
            formatDate(date) {
                return format(new Date(date), 'dd/MM/yyyy');
            },
            deleteTask(id) {
              axios.get(`task/${id}`)
              .then(response => {
                const task = response.data;
      
                if (task.subtasks.length > 0) {
                    alert('Não é possível excluir esta tarefa porque existem subtasks associadas a ela.');
                } else {
                  axios.delete(`task/${id}`)
                .then(() => {
                console.log('Tarefa excluída com sucesso');
                this.refresh();
                })
                .catch(error => {
                  console.error('Erro ao deletar a tarefa:', error);
                  alert('Erro ao excluir a tarefa. Por favor, tente novamente mais tarde.');
                });
                }
              })
              .catch(error => {
                console.error('Erro ao obter a tarefa:', error);
                alert('Erro ao obter a tarefa. Por favor, tente novamente mais tarde.');
              });
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
          editTaskDate(){
            axios.put(`task/${this.task.id}`, this.taskDataDate)
            .then(response => {
              console.log('Data editada com sucesso:', response.data);

              this.editDate = false
              this.refresh()
            })
            .catch(error => {
              console.error('Erro ao editar a tarefa:', error);
              });
            }
          },
        components: {
            MySubtask
        }
    }
</script>

<template>

<div class="myTasks" @mouseover="taskHover()" @mouseout="taskNotHover()"> 
    <div class="container__checkbox">
        <i class="bi bi-circle" @click="updateStatusTask(task)" v-if="task.status == 'pending'"></i>
        <i class="bi bi-check-circle-fill" @click="updateStatusTask(task)" v-else></i>
    </div>
    <div class="myTask">
        <h4 @click="openModalTask()">{{ task.title }}</h4>
        <p>{{task.description}}</p>
        <div class="taskDate" v-if="editDate === true">
          <form class="form-editDate" @submit.prevent="editTaskDate">
            <input type="date" id="due_date" name="due_date" placeholder="Selecione uma data" v-model="taskDataDate.due_date">
            <div id="editCancel" @click="editDate = false">Cancelar</div>
            <button id="editDate">Alterar</button>
          </form>
        </div>
        <div class="container__taskDate" v-else>
          <div v-if="deadlineStatus === 'withinDeadline'" class="dateWithinDeadline taskDate">
            <i class="bi bi-calendar4" ></i>
            <p>{{formatDate(task.due_date)}}</p>
          </div>
          <div v-else-if="deadlineStatus === 'today'" class="dateToday taskDate">
            <i class="bi bi-calendar4"></i>
            <p>hoje</p>
          </div>
          <div v-else class="dateOverdue taskDate">
            <i class="bi bi-calendar4 "></i>
            <p>{{formatDate(task.due_date)}}</p>
          </div>
        </div>
    </div>
    <div class="container__icons" v-show="isTaskHover">
      <i class="bi bi-pencil" @click="openEdition()"></i>
      <i class="bi bi-calendar4" @click="editDate = true"></i>
      <i class="bi bi-trash" @click="deleteTask(task.id)"></i>
    </div>
</div>
<div class="container__subtask">
    <MySubtask v-for="subtask in task.subtasks" :key="subtask.id" :subtask="subtask" class="mySubtask" @refreshTask="refresh()"/>
</div>
</template>

<style scoped>
.myTasks {
  display: grid;
  grid-template-columns: 5% 80% 15%;
  border: 1px solid #E5E5E5;
  padding: 1em;
  margin-top: 5%;

}

.container__checkbox i {
  cursor: pointer;
}

.myTask{
  white-space: nowrap; 
  overflow: hidden; 
  text-overflow: ellipsis; 
}

.myTask h4 {
  font-size: 20px;
  margin-bottom: 3%;
  cursor: pointer;
}

.myTask p {
  font-size: 16px;
  margin-bottom: 3%;
}

.container__taskDate{
  display: flex;
}

.taskDate{
  display: flex;
  gap: 3%;
}

.taskDate i{
  font-size: 18px;
  margin-left: 10px;
  margin-bottom: 0px;
}
.taskDate p{
  font-size: 18px;
  margin-left: 10px;
  margin-right: 10px;
  margin-bottom: 0px;
}

.form-editDate{
  display: flex;
  gap: 5%;
  width: 100%;
}

#due_date{
  width: 200px;
  font-size: 20px;
}

#editCancel{
    padding: 1% 4%;
    border: none;
    font-weight: 600;
    font-size: 18px;
    background-color: transparent;
    color: red;
    cursor: pointer;
}

#editDate{
    padding: 1% 4%;
    border: none;
    font-weight: 600;
    font-size: 18px;
    background-color: transparent;
    color: green;
    cursor: pointer;
}

.container__icons{
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 20%;
}

.container__icons i {
  cursor: pointer;
}

.container__subtask {
    border: 1px solid #E5E5E5;
}

.mySubtask {
  display: flex;
  padding: 1% 2.5%;
  gap: 5%;
}

.dateWithinDeadline{
  background-color: rgba(0, 148, 136, 0.1);
  color: #009488;
}

.dateToday{
  background-color: rgba(0, 148, 136, 0.1);
  color: #009488
}

.dateOverdue{
  background: rgba(211, 20, 8, 0.1);
  color: #D31408;
}

</style>