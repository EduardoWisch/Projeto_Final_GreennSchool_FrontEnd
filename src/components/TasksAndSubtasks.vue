<script>
  import axios from 'axios';
  import MyTask from '@/components/MyTask.vue'
  import {ref} from 'vue';

  export default {
    data() {
        return {
          tasks: []
        }
      },
      methods: {
        open() {
          this.$emit('openCreateModal')
        },
        openEditionModal(task) {
          this.$emit('openEditionModal', task)
        },
        getTasks() {
          axios.get('task')
          .then(response => {
            this.tasks = response.data.data;
// Após obter as tarefas, obtenha as subtarefas
            this.tasks.forEach(task => {
              this.getSubtasks(task.id);
            });
          })
          .catch(error => {
            console.error('Erro ao obter as tarefas:', error);
          });
        },
      },
      mounted() {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api/'
  
        this.getTasks()
      },
      components: {
        MyTask,
      }
  }
</script>

<template>
    <MyTask v-for="task in tasks" :key="task.id" :task="task" @openEditionModal="openEditionModal(task)"/>
  <div class="container__createTask" @click="open()">
    <i class="bi bi-plus-lg"></i>
    <p>Criar tarefa</p>
  </div>
</template>

<style scoped>

.mySubtask {
  display: flex;
  gap: 5%;
  margin: 1% 0 1% 2.5%;
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