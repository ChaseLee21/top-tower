<template>
    <BRow>
        <BButton @click="modal.show = !modal.show" class="w-auto mx-auto my-2"> Start Game </BButton>
    </BRow>
    <BModal v-model="modal.show" v-on:ok="startGame" class="text-dark" title="Please enter character name"> 
        <BFormInput placeholder="Enter character name" v-model="modal.name" />
    </BModal>
</template>
  
<script setup lang="ts">
    import { BRow, BButton, BModal, BFormInput } from 'bootstrap-vue-next'
    import { ref } from 'vue'

    const emit = defineEmits(['startGame']);

    const modal = ref({
        show: false,
        name: ''
    })

    const startGame = () => {
        if (modal.value.name.length > 0) {
            modal.value.show = false;
            let name = modal.value.name;
            emit('startGame', name);
        }
    }

    

    
</script>