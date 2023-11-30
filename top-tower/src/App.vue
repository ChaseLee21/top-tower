<template>
  <BContainer>
    <Room />
    <StartModal v-if="!started" @startGame="handleStartGame"/>
    <Enemies v-if="started"/>
    <Character v-if="started" :character = "character"/>
    <Log v-if="started"/>
  </BContainer>
</template>

<script setup>
import { BContainer } from 'bootstrap-vue-next';
import Room from './components/Room.vue';
import Enemies from './components/Enemies.vue';
import Character from './components/Character.vue';
import StartModal from './components/StartModal.vue';
import Log from './components/Log.vue';

import { ref } from 'vue';

const started = ref(false);

const character = ref({
  name: 'Character',
  health: 100,
  attack: 0,
  inventory: [
    {
      name: 'Copper Short Sword',
      type: 'weapon',
      attack: 1
    },
  ],
  equipment: {
    weapon: {
      name: 'Copper Short Sword',
      attack: 1
    },
    helm: {
      name: 'none',
      defense: 0
    },
    armor: {
      name: 'Janky Leather Armor',
      defense: 1
    },
    boots: {
      name: 'Torn Leather Boots',
      defense: 1
    }
  }
})

const handleStartGame = (name) => {
  character.value.name = name;
  started.value = true;
}
</script>

<style>
  
</style>