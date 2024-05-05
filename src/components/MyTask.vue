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
            const today = new Date(currentDate.getFullYear(), currentDate.getMonth(), currentDate.getDate()); // Apenas data, sem horário
            
            if (today > deadlineDate) {
                return 'overdue'; // Vencido
            } else if (today.getTime() === deadlineDate.getTime()) {
                return 'today'; // Hoje
            } else {
                return 'withinDeadline'; // Dentro do prazo
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
            // editDate(){
            //   this.editingDate = true
            // },
            taskHover() {
                this.isTaskHover = true
            },
            taskNotHover() {
                this.isTaskHover = false
            },
            formatDate(date) {
                return format(new Date(date), 'dd/MM/yyyy');
            },
            deleteTask(id){
                axios.delete(`task/${id}`)
                .then(() => {
                    console.log('Tarefa excluída com sucesso');
                    this.refresh()
                })
                .catch(error => {
                    console.error('Erro ao deletar a tarefa:', error);
                });
            },
            updateStatusTask(task) {
// Invertendo o status atual
                const newStatus = task.status === 'completed' ? 'pending' : 'completed';
// Verificando se o novo status é válido
                if (newStatus !== 'completed' && newStatus !== 'pending') {
                    console.error('Novo status inválido:', newStatus);
                    return;
                }
// Atualização otimista
                task.status = newStatus;
// Enviando a solicitação de atualização para o servidor
                axios.put(`task/${task.id}`, { status: newStatus })
                .then(() => {
                    console.log('Status alterado')
// Atualização bem-sucedida, nada precisa ser feito aqui
                })
                .catch(error => {
// Se houver um erro, reverta a atualização
                    console.error('Erro ao atualizar a tarefa:', error);
                    task.status = task.status === 'completed' ? 'pending' : 'completed';
                });
            },
            editTaskDate(){
              axios.put(`task/${this.task.id}`, this.taskDataDate)
              .then(response => {
                    console.log('Data editada com sucesso:', response.data);
                    // Emitir evento para fechar o modal de edição
                    this.editDate = false
                    // Limpar os campos do formulário de edição
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
            <i class="bi bi-calendar4" @click="openModalTask()"></i>
            <p>{{formatDate(task.due_date)}}</p>
          </div>
          <div v-else-if="deadlineStatus === 'today'" class="dateToday taskDate">
            <i class="bi bi-calendar4" @click="openModalTask()"></i>
            <p>hoje</p>
          </div>
          <div v-else class="dateOverdue taskDate">
            <i class="bi bi-calendar4 " @click="openModalTask()"></i>
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
  white-space: nowrap; /* Impede que o texto quebre para uma nova linha */
  overflow: hidden; /* Oculta qualquer texto que não caiba */
  text-overflow: ellipsis; /* Adiciona reticências (...) quando o texto é muito longo */
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