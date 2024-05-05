<script>
  import Navbar from '@/components/Navbar.vue'
  import VerticalNavbar from '@/components/VerticalNavbar.vue'
  import TasksAndSubtasks from '@/components/TasksAndSubtasks.vue';
  import CreateModal from '@/components/CreateModal.vue';
  import EditionModal from '@/components/EditionModal.vue';
  import TaskModal from '@/components/TaskModal.vue';
  import axios from 'axios';
  

  export default {
    components: {
      Navbar,
      VerticalNavbar,
      TasksAndSubtasks,
      CreateModal,
      EditionModal,
      TaskModal
    },
    data() {
      return {
        tasks: [],
        selectedEditTask: null,
        showCreateModal: false,
        showEditionModal: false,
        showTaskModal: false,
        contentPage: 'isEntrance'
      }
    },
    methods: {
      createModalIsClosed() {
        console.log('O modal está fechado')
        this.showCreateModal = false
      },
      createModalIsOpen() {
        console.log('O modal está aberto')
        this.showCreateModal = true
      },
      editionModalIsClosed() {
        console.log('O modal está fechado')
        this.showEditionModal = false
      },
      editionModalIsOpen(task) {
        this.selectedEditTask = task
        console.log('O modal está aberto')
        this.showEditionModal = true
      },
      taskModalIsClosed() {
        console.log('O modal está fechado')
        this.showTaskModal = false
      },
      taskModalIsOpen(task) {
        this.selectedTask = task
        console.log('O modal está aberto')
        console.log('Recebido evento para abrir o modal da tarefa:', task);
        this.showTaskModal = true
      },
      entrance(){
        this.contentPage = 'isEntrance' 
        console.log(this.contentPage)
      },
      today(){
        this.contentPage = 'isToday' 
        console.log(this.contentPage)
      },
      expired(){
        this.contentPage = 'isExpired' 
        console.log(this.contentPage)
      },
    }
  }
</script>

<template>
    <Navbar @openCreateModal="createModalIsOpen"/>
    <VerticalNavbar @clickEntrance="entrance()" @clickToday="today()" @clickExpired="expired()"/>

    <div class="container__tasks">
      <h1 v-if="contentPage === 'isEntrance'">Entrada</h1>
      <h1 v-if="contentPage === 'isToday'">Tarefas de hoje</h1>
      <h1 v-if="contentPage === 'isExpired'">Vencidas</h1>
      <TasksAndSubtasks :contentPage="contentPage" @openCreateModal="createModalIsOpen" @openEditionModal="editionModalIsOpen" @openTaskModal="taskModalIsOpen"/>
    </div>

    <CreateModal v-model:showCreateModal="showCreateModal" @closeCreateModal="createModalIsClosed" @refreshTask="refresh()"/>
    <EditionModal v-model:showEditionModal="showEditionModal" :task="selectedEditTask" @closeEditionModal="editionModalIsClosed"/>
    <TaskModal v-model:showTaskModal="showTaskModal" :task="selectedTask" @closeTaskModal="taskModalIsClosed" />

</template>

<style scoped>

.container__tasks{
  margin: 10vh 15% 0 30vw;
  color: black;
  margin-bottom: 5%;
}

.container__tasks h1{
  font-size: 24px;
  font-weight: 800;
  color: black;
  margin-top: 15%;
  margin-bottom: 5%;
}



</style>