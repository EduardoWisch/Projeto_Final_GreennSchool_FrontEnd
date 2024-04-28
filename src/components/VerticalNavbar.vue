<script>

</script>

<template>
    <div class="container__navbar">
        <div class="navbar-itens">
            <i class="bi bi-inbox"></i>
            <h3>Entrada</h3>
        </div>
        <div class="navbar-itens">
            <i class="bi bi-calendar4"></i>
            <h3>Tarefas de hoje</h3>
        </div>
        <div class="navbar-itens">
            <i class="bi bi-exclamation-triangle"></i>
            <h3>Vencidos</h3>
        </div>
    </div>
</template>

<style scoped>
.container__navbar {
  position: fixed;
  top: 10%;
  left: 0;
  width: 20%; 
  height: 100%; 
  box-sizing: border-box;
  overflow-y: auto; 
  padding: 4% 3%;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  gap: 5%;
  background-color: #FAFAFA;
  color: #000; 
}
.navbar-itens{
    display: flex;
    gap: 15%;
}

.bi{
    font-size: 25px;
}
.navbar-itens h3{
    font-size: 18px;
    font-weight: 600;
    white-space: nowrap; /* Impede que o texto quebre para uma nova linha */
}
</style>