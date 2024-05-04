<script>
  import Navbar from '@/components/Navbar.vue'
  import VerticalNavbar from '@/components/VerticalNavbar.vue'
  import TasksAndSubtasks from '@/components/TasksAndSubtasks.vue';
  import CreateModal from '@/components/CreateModal.vue';
  import EditionModal from '@/components/EditionModal.vue';
  import axios from 'axios';
  

  export default {
    components: {
      Navbar,
      VerticalNavbar,
      TasksAndSubtasks,
      CreateModal,
      EditionModal,
    },
    data() {
      return {
        showCreateModal: false,
        showEditionModal: false
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
      editionModalIsOpen() {
        console.log('O modal está aberto')
        this.showEditionModal = true
      },
      
    }
  }
</script>

<template>
    <Navbar @openCreateModal="createModalIsOpen"/>
    <VerticalNavbar />

    <div class="container__tasks">
      <h1>Entrada</h1>
      <TasksAndSubtasks @openCreateModal="createModalIsOpen" @openEditionModal="editionModalIsOpen"/>
    </div>

    <CreateModal v-model:showCreateModal="showCreateModal" @closeCreateModal="createModalIsClosed"/>
    <EditionModal v-model:showEditionModal="showEditionModal" @closeEditionModal="editionModalIsClosed"/>

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