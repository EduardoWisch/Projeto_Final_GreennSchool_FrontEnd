<script>
import axios from 'axios';

    export default {
        data() {
            return {
                isSubtaskHover: false
            }
        },
        props: {
            subtask: {
                type: Object,
                required: true
                }
            },
        methods: {
            subtaskHover() {
                this.isSubtaskHover = true
            },
            subtaskNotHover() {
                this.isSubtaskHover = false
            },
            deleteSubtask(id){
                axios.delete(`subtask/${id}`)
                .then (() => {
                    console.log('Subtarefa deletada')
                })
                .catch(error => {
                    console.error('Erro ao deletar a subtarefa:', error);
                });
            },
            updateSubtask(subtask) {
// Invertendo o status atual
                const newStatus = subtask.status === 'completed' ? 'pending' : 'completed';

// Verificando se o novo status é válido
                if (newStatus !== 'completed' && newStatus !== 'pending') {
                    console.error('Novo status inválido:', newStatus);
                    return;
                }
// Atualização otimista
                subtask.status = newStatus;
// Enviando a solicitação de atualização para o servidor
                axios.put(`subtask/${subtask.id}`, { status: newStatus })
                .then(() => {
                    console.log('Status alterado com sucesso')
// Atualização bem-sucedida, nada precisa ser feito aqui
                })
                .catch(error => {
// Se houver um erro, reverta a atualização
                    console.error('Erro ao atualizar a tarefa:', error);
                    subtask.status = subtask.status === 'completed' ? 'pending' : 'completed';
                });
            },
        }       
    }
</script>

<template>
    <div class="mySubtasks" @mouseover="subtaskHover()" @mouseout="subtaskNotHover()">
            <div></div>
            <i class="bi bi-circle" @click="updateSubtask(subtask)" v-if="subtask.status == 'pending'"></i>
            <i class="bi bi-check-circle-fill" @click="updateSubtask(subtask)" v-else></i>
            
            <div class="mySubtask-titles">
                <p>{{ subtask.title }}</p>
                <div class="mySubtask-container__icons" v-show="isSubtaskHover">
                    <i class="bi bi-pencil"></i>
                    <i class="bi bi-trash" @click="deleteSubtask(subtask.id)"></i>
                </div>
            </div>
    </div>
</template>

<style scoped>

.mySubtask-titles{
  width: 84%;
  display: flex;
  justify-content: space-between;
}

.mySubtask i {
  cursor: pointer;
}

.mySubtask-title p{
  white-space: nowrap; /* Impede que o texto quebre para uma nova linha */
  overflow: hidden; /* Oculta qualquer texto que não caiba */
  text-overflow: ellipsis; /* Adiciona reticências (...) quando o texto é muito longo */
}

.mySubtask-container__icons {
  display: flex;
  gap: 40%;
}
</style>