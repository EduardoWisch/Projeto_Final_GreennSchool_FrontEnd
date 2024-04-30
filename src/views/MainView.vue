<script>
  import Navbar from '@/components/Navbar.vue'
  import VerticalNavbar from '@/components/VerticalNavbar.vue'
  import MyTasks from '@/components/MyTasks.vue';
  import axios from 'axios';
  

  export default {
    data() {
      return {
        tasks: []
        
      }
    },
    methods: {
      getTasks(){
        axios.get('task')
        .then(response => this.tasks = response.data.data)
      }
    },
    mounted() {
      axios.defaults.baseURL = 'http://127.0.0.1:8000/api/'

      this.getTasks()
    },
    components: {
      Navbar,
      VerticalNavbar,
      MyTasks
    }
  }
</script>

<template>
    <Navbar />
    <VerticalNavbar />
    <div class="container__tasks">
      <h1>Entrada</h1>
      <!-- <MyTasks /> -->
      <div v-for="task in tasks" :key="task.id">
        <p>{{ task.title }}</p>
        <div v-for="subtask in task.subtasks" :key="subtask.id">
          <p>{{ subtask.title }}</p>
        </div>
      </div>
    </div>
</template>

<style scoped>

.container__tasks{
  margin: 10vh 15% 0 30vw;
  color: black;
}

.container__tasks h1{
  font-size: 24px;
  font-weight: 800;
  color: black;
  margin-bottom: 5%;
}



</style>