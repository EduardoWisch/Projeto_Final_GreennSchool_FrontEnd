<script>
  import axios from 'axios';
  import {ref} from 'vue';

  export default {
    data() {
        return {
          tasks: []
          
        }
      },
      methods: {
        open() {
                this.$emit('openModal')
        },
        getTasks() {
      axios.get('task')
        .then(response => {
          this.tasks = response.data.data;
        })
        .catch(error => {
          console.error('Erro ao obter as tarefas:', error);
        });
    },
        deleteTask(id){
          axios.delete(`task/${id}`)
          .catch(error => {
            console.error('Erro ao deletar a tarefa:', error);
          });
        },
        deleteSubtask(id){
          axios.delete(`subtask/${id}`)
          .catch(error => {
            console.error('Erro ao deletar a subtarefa:', error);
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
            // Atualização bem-sucedida, nada precisa ser feito aqui
          })
          .catch(error => {
            // Se houver um erro, reverta a atualização
            console.error('Erro ao atualizar a tarefa:', error);
            task.status = task.status === 'completed' ? 'pending' : 'completed';
          });
},
        updateSubtask(subtask) {
  // Invertendo o status atual
        const newStatus = subtask.status === 'completed' ? 'pending' : 'completed';

  // Verificando se o novo status é válido
        if (newStatus !== 'completed' && newStatus !== 'pending') {
          console.error('Novo status inválido:', newStatus);
          return;
        }

  // Atualização otimista
        subtask.status = newStatus;

        // Enviando a solicitação de atualização para o servidor
        axios.put(`subtask/${subtask.id}`, { status: newStatus })
          .then(() => {
            // Atualização bem-sucedida, nada precisa ser feito aqui
          })
          .catch(error => {
            // Se houver um erro, reverta a atualização
            console.error('Erro ao atualizar a tarefa:', error);
            subtask.status = subtask.status === 'completed' ? 'pending' : 'completed';
          });
}
      },
      mounted() {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api/'
  
        this.getTasks()
      },
  }
</script>

<template>
  <div v-for="task in tasks" :key="task.id">
    <div class="myTasks">
  
        <div class="container__checkbox">
            <i class="bi bi-circle" @click="updateTask(task)" v-if="task.status == 'pending'"></i>
            <i class="bi bi-check-circle-fill" @click="updateTask(task)" v-else></i>
        </div>
        
        <div class="myTask">
            <h4>{{ task.title }}</h4>
            <p>{{task.description}}</p>
            <div class="taskDate">
                <i class="bi bi-calendar4"></i>
                <p>{{task.due_date}}</p>
            </div>
        </div>
  
        <div class="container__icons">
            <i class="bi bi-pencil"></i>
            <i class="bi bi-calendar4"></i>
            <i class="bi bi-trash" @click="deleteTask(task.id)"></i>
        </div>
    </div>
    
    <div class="mySubtasks">
      
      <div v-for="subtask in task.subtasks" :key="subtask.id" class="mySubtask">
          <div></div>
          <i class="bi bi-circle" @click="updateSubtask(subtask)" v-if="subtask.status == 'pending'"></i>
          <i class="bi bi-check-circle-fill" @click="updateSubtask(subtask)" v-else></i>
            
          <div class="mySubtask-titles">
            <p>{{ subtask.title }}</p>
            <div class="mySubtask-container__icons">
              <i class="bi bi-pencil"></i>
              <i class="bi bi-trash" @click="deleteSubtask(subtask.id)"></i>
            </div>
          </div>
      </div>
    </div>
  </div>

  <div class="container__createTask" @click="open()">
    <i class="bi bi-plus-lg"></i>
    <p>Criar tarefa</p>
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

.mySubtasks {
  border: 1px solid #E5E5E5;
  /* display: grid;
  grid-template-columns: 5% 85%; */
}

.mySubtask {
  display: flex;
  gap: 5%;
  margin: 1% 0 1% 2.5%;
}

.mySubtask-titles{
  width: 80%;
  display: flex;
  justify-content: space-between;
}

.mySubtask-title i {
  cursor: pointer;
}

.mySubtask-title p{
  white-space: nowrap; /* Impede que o texto quebre para uma nova linha */
  overflow: hidden; /* Oculta qualquer texto que não caiba */
  text-overflow: ellipsis; /* Adiciona reticências (...) quando o texto é muito longo */
}

.mySubtask-container__icons {
  display: flex;
  gap: 50%;
}

.mySubtask-container__icons i {
  cursor: pointer;;
}



.container__createTask{
  display: flex;
  gap: 2.5%;
  border: 1px solid #E5E5E5;
  padding: 2% 0% 2% 1.5%;
  font-size: 18px;
  cursor: pointer;
}


</style>