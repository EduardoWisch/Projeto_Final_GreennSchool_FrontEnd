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
        getTasks(){
          const tasks = ref()
          axios.get('task')
          .then(response => this.tasks = response.data.data)
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
            <input type="checkbox">
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
            <i class="bi bi-trash"></i>
        </div>
    </div>
    
    <div class="mySubtasks">
      
      <div v-for="subtask in task.subtasks" :key="subtask.id" class="mySubtask">
          <div></div>
            <input type="checkbox">
            <p>{{ subtask.title }}</p>

      </div>
      
    </div>

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

.container__checkbox input {
  border-radius: 100%;
  transform: scale(1.5);
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

.mySubtask input {
  transform: scale(1.5);
}

.mySubtask p{
  white-space: nowrap; /* Impede que o texto quebre para uma nova linha */
  overflow: hidden; /* Oculta qualquer texto que não caiba */
  text-overflow: ellipsis; /* Adiciona reticências (...) quando o texto é muito longo */
}


</style>