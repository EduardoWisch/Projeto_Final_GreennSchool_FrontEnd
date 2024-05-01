<script>
    import CreateModal from '@/components/CreateModal.vue'

    export default {
        data() {
            return {
                showModal: false
            }
        },
        methods: {
            modalIsClosed() {
                console.log('O modal está fechado')
                this.showModal = false
            }
        },
        components: {
            CreateModal
        },
    }
</script>

<template>
    <button @click="showModal = true">Abrir modal</button>
    <CreateModal v-model:showModal="showModal" @closeModal="modalIsClosed" />
</template>

<style scoped>

</style>