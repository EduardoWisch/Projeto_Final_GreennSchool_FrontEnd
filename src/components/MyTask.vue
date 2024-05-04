<script>
import axios from 'axios';
import {format} from 'date-fns'
import MySubtask from '@/components/MySubtask.vue'

    export default {
        data() {
            return {
                isTaskHover: false
            }
        },
        props: {
            task: {
            type: Object,
            required: true
            }
        },
        methods: {
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
                })
                .catch(error => {
                    console.error('Erro ao deletar a tarefa:', error);
                });
            },
            updateTask(task) {
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
        },
        components: {
            MySubtask
        }
    }
</script>

<template>

<div class="myTasks" @mouseover="taskHover()" @mouseout="taskNotHover()"> 
    <div class="container__checkbox">
        <i class="bi bi-circle" @click="updateTask(task)" v-if="task.status == 'pending'"></i>
        <i class="bi bi-check-circle-fill" @click="updateTask(task)" v-else></i>
    </div>
    <div class="myTask">
        <h4>{{ task.title }}</h4>
        <p>{{task.description}}</p>
        <div class="taskDate">
            <i class="bi bi-calendar4"></i>
            <p>{{formatDate(task.due_date)}}</p>
        </div>
    </div>
    <div class="container__icons" v-show="isTaskHover">
      <i class="bi bi-pencil"></i>
      <i class="bi bi-calendar4"></i>
      <i class="bi bi-trash" @click="deleteTask(task.id)"></i>
    </div>
</div>
<div class="container__subtask">
    <MySubtask v-for="subtask in task.subtasks" :key="subtask.id" :subtask="subtask" class="mySubtask"/>
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
}

.myTask p {
  font-size: 16px;
  margin-bottom: 3%;
}

.taskDate{
  display: flex;
  gap: 3%;
}

.taskDate p{
  font-size: 18px;
  margin-bottom: 0px;
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
</style>