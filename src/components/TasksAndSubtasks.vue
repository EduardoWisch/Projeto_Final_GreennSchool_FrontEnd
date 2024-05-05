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
      props: {
        contentPage: {
          type: String,
          default: 'isEntrance'
        }
      },
      computed: {
        tasksDueToday() {
        const today = new Date().toISOString().split('T')[0]; 
        return this.tasks.filter(task => {
          const taskDate = new Date(task.due_date).toISOString().split('T')[0]; 
          return taskDate === today;
        });
      },
      tasksExpired() {
        const today = new Date().toISOString().split('T')[0]; 
        return this.tasks.filter(task => {
          const taskDate = task.due_date.split('T')[0]; 
          return taskDate < today;
        });
      }
    },
      methods: {
        openCreate() {
          this.$emit('openCreateModal')
        },
        openEditionModal(task) {
          this.$emit('openEditionModal', task)
        },
        openTaskModal(task) {
          this.$emit('openTaskModal', task)
          console.log('abrindo modal para tarefa', task)
        },
        getTasks() {
          axios.get('task')
          .then(response => {
            this.tasks = response.data.data;

            this.tasks.forEach(task => {
              this.getSubtasks(task.id);
            });
          })
          .catch(error => {
            console.error('Erro ao obter as tarefas:', error);
          });
        },
        fetchTasks() {
          axios.get('task')
          .then(response => {
            this.tasks = response.data.data;

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
        
        setInterval(() => {
          this.fetchTasks(); 
          }, 30000);
      },
      components: {
        MyTask,
      }
  }
</script>

<template>
    <MyTask v-if="contentPage === 'isEntrance'" v-for="task in tasks" :key="task.id" :task="task" @openEditionModal="openEditionModal(task)" @openTaskModal="openTaskModal(task)" @refreshTask="getTasks()"/>
    <MyTask v-if="contentPage === 'isToday'" v-for="task in tasksDueToday" :key="task.id" :task="task" @openEditionModal="openEditionModal(task)" @openTaskModal="openTaskModal(task)" @refreshTask="getTasks()"/>
    <MyTask v-if="contentPage === 'isExpired'" v-for="task in tasksExpired" :key="task.id" :task="task" @openEditionModal="openEditionModal(task)" @openTaskModal="openTaskModal(task)" @refreshTask="getTasks()"/>
  <div class="container__createTask" @click="openCreate()">
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