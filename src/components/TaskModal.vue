<script>
    
export default {
    props: {
        showTaskModal: {
            type: Boolean,
            required: true
        },
    },
    methods: {
        close() {
            this.$emit('closeTaskModal')
        },
    }
}   
</script>

<template>
     <div class="container__taskModal" v-if="showTaskModal">
        <div class="taskModal">
            <div class="container__top">
                <div class="date">
                    <i class="bi bi-calendar4"></i>
                    <p>No prazo</p>
                </div>
                <div class="container__icons">
                    <i class="bi bi-three-dots"></i>
                    <i class="bi bi-x-lg" @click="close()"></i>
                </div>
            </div>
            <div class="container__content">
                <div  class="container__tasks">
                    <i class="bi bi-circle"></i>
                    <div class="content">
                        <h1>Ir ao mercado</h1>
                        <p>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Aliquid sint iste dolorem cupiditate autem cumque harum officia illum labore unde ea quam magnam corporis neque veritatis sed, deleniti quas sapiente?</p>
                        <h2>Subtarefas</h2>
                        <div class="container__subtask">
                            <div class="subtask">
                                <i class="bi bi-circle"></i>
                                <p>Comprar pão</p>
                            </div>
                            <div class="subtask">
                                <i class="bi bi-circle"></i>
                                <p>Comprar produtos de limpeza</p>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="container__information">
                    <div class="create">
                        <p>Criado em</p>
                        <div>
                            <i class="bi bi-calendar4"></i>
                            <p>10/20/2020 as 17:50</p>
                        </div>
                    </div>
                    <div class="expired">
                        <p>Data de vencimento</p>
                        <div>
                            <i class="bi bi-calendar4"></i>
                            <p>11/20/2020</p>
                        </div>
                    </div>
                    <div class="updated">
                        <p>Modificado em</p>
                        <div>
                            <i class="bi bi-calendar4"></i>
                            <p>11/20/2020 as 17:50</p>
                        </div>
                    </div>
                    <div class="idTask">
                        <p>ID da tarefa</p>
                        <p style="color: black; font-weight: 600;">8</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped> 
.container__taskModal{
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    background-color: rgba(0, 0, 0, 0.2);
    /* backdrop-filter: blur(1px); */
    display: flex;
    align-items: top;
    justify-content: center;
}

.taskModal{
    display: flex;
    flex-direction: column;
    margin-top: 1%;
    width: 60%;
    height: 90vh; 
    position: relative;
    background-color: white;
    color: #81858E;
    border: 1px solid #E5E5E5;
}

.container__top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2% 3%;
    border-bottom: 1px solid #D9D9D9;
}

.date {
    display: flex;
    width: 50%;
    gap: 5%
}

.container__icons i{
    margin-left: 30px;
    cursor: pointer;
}

.container__content {
    display: grid;
    height: 100%;
    grid-template-columns: 65% 35%;
}

.container__tasks{
    padding: 5%;
    display: grid;
    grid-template-columns: 5% 90%;
    gap: 2%;
}

.content {
    font-size: 14px;
}

.content h1 {
    color: black;
    margin-bottom: 2%;
    font-weight: 600;
}

.content p {
    color: #81858E;
    margin-bottom: 3%;
}

.content h2 {
    color: black;
    margin-bottom: 2%;
    font-weight: 600;
}

.container__subtask {
    border-top: 1px solid #E5E5E5;
}

.subtask {
    margin-top: 2%;
    display: flex;
    gap: 5%;
    color: #000;
    font-size: 18px;
}

.container__information{
    display: block;
    background-color: #F7F7F7;
    height: 100%;
    padding: 10%;
}

.create, .expired, .updated, .idTask {
    margin-bottom: 10%;
    font-size: 16px;
}

.create div, .expired div, .updated div{
    display: flex;
    gap: 5%;
}

.create div p, .expired div p, .updated div p {
    color: black;
    font-weight: 600;
}
</style>