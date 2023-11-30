<template>
    <BRow>
        <BButton @click="modal = !modal" class="w-auto mx-auto my-2"> Start Game </BButton>
    </BRow>
    <BModal v-model="modal" class="text-dark" title="Please enter character name"> 
        <BFormInput placeholder="Enter character name" />
    </BModal>
</template>
  
<script setup lang="ts">
    import { ref } from 'vue'
    const modal = ref(false)
    
</script>