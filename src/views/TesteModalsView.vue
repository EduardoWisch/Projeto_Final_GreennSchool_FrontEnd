<script>
    import CreateModal from '@/components/CreateModal.vue'
    import EditionModal from '@/components/EditionModal.vue'

    export default {
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
            editionModalIsClosed() {
                console.log('O modal está fechado')
                this.showEditionModal = false
            }
        },
        components: {
            CreateModal,
            EditionModal
        },
    }
</script>

<template>
    <button @click="showCreateModal = true">Abrir modal de criação</button>
    <CreateModal v-model:showCreateModal="showCreateModal" @closeCreateModal="createModalIsClosed" />
    <button @click="showEditionModal= true">Abrir modal de edição</button>
    <EditionModal v-model:showEditionModal="showEditionModal" @closeEditionModal="editionModalIsClosed"/>
</template>

<style scoped>

</style>