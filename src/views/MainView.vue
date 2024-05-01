<script>
  import Navbar from '@/components/Navbar.vue'
  import VerticalNavbar from '@/components/VerticalNavbar.vue'
  import MyTasks from '@/components/MyTasks.vue';
  import CreateModal from '@/components/CreateModal.vue';
  import axios from 'axios';
  

  export default {
    components: {
      Navbar,
      VerticalNavbar,
      MyTasks,
      CreateModal
    },
    data() {
      return {
        showModal: false
      }
    },
    methods: {
      modalIsClosed() {
                console.log('O modal está fechado')
                this.showModal = false
            },
      modalIsOpen() {
        console.log('O modal está aberto')
        this.showModal = true
      }
      
    }
  }
</script>

<template>
    <Navbar @openModal="modalIsOpen"/>
    <VerticalNavbar />

    <div class="container__tasks">
      <h1>Entrada</h1>
      <MyTasks @openModal="modalIsOpen"/>
    </div>

    <CreateModal v-model:showModal="showModal" @closeModal="modalIsClosed"/>
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