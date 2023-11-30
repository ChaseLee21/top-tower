<template>
  <BContainer>
    <Room />
    <StartModal />
    <Enemies />
    <Character />
    <Log />
  </BContainer>
</template>

<script setup>
import { BContainer } from 'bootstrap-vue-next';
import Room from './components/Room.vue';
import Enemies from './components/Enemies.vue';
import Character from './components/Character.vue';
import StartModal from './components/StartModal.vue';
import Log from './components/Log.vue';



</script>

<style>
  
</style>