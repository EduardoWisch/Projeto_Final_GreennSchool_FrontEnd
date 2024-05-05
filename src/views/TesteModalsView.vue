<script>
    import CreateModal from '@/components/CreateModal.vue'
    import EditionModal from '@/components/EditionModal.vue'
    import TaskModal from '@/components/TaskModal.vue'

    export default {
        data() {
            return {
                showCreateModal: false,
                showEditionModal: false,
                showTaskModal: false
            }
        },
        methods: {
            createModalIsClosed() {
                console.log('O modal está fechado')
                this.showCreateModal = false
            },
            editionModalIsClosed() {
                console.log('O modal está fechado')
                this.showEditionModal = false
            },
            taskModalIsClosed() {
                console.log('O modal está fechado')
                this.showTaskModal = false
            }
        },
        components: {
            CreateModal,
            EditionModal,
            TaskModal
        },
    }
</script>

<template>
    <button @click="showCreateModal = true">Abrir modal de criação</button>
    <CreateModal v-model:showCreateModal="showCreateModal" @closeCreateModal="createModalIsClosed" />
    <button @click="showEditionModal= true">Abrir modal de edição</button>
    <EditionModal v-model:showEditionModal="showEditionModal" @closeEditionModal="editionModalIsClosed"/>
    <button @click="showTaskModal= true">Abrir modal de tarefa</button>
    <TaskModal v-model:showTaskModal="showTaskModal" @closeTaskModal="taskModalIsClosed"/>
</template>

<style scoped>

</style>