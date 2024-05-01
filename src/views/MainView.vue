<script>
  import Navbar from '@/components/Navbar.vue'
  import VerticalNavbar from '@/components/VerticalNavbar.vue'
  import MyTasks from '@/components/MyTasks.vue';
  import axios from 'axios';
  

  export default {
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
      <!-- <div v-for="task in tasks" :key="task.id">
        <p>{{ task.title }}</p>
        <div v-for="subtask in task.subtasks" :key="subtask.id">
          <p>{{ subtask.title }}</p>
        </div>
      </div> -->
      <MyTasks />
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
  margin-top: 15%;
  margin-bottom: 5%;
}



</style>