<template>
  <BContainer>
    <Room :room = "room" />
    <StartModal v-if="!started" @startGame="handleStartGame"/>
    <Enemies v-if="started"/>
    <Actions v-if="started" :actions="room.actions" />
    <Character v-if="started" :character = "character"/>
    <Log v-if="started"/>
  </BContainer>
</template>

<script setup>
import { BContainer } from 'bootstrap-vue-next';
import Room from './components/Room.vue';
import Enemies from './components/Enemies.vue';
import Actions from './components/Actions.vue';
import Character from './components/Character.vue';
import StartModal from './components/StartModal.vue';
import Log from './components/Log.vue';

import { ref, onMounted } from 'vue';

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

const rooms = [
  {
    title: 'Welcome to Top Tower',
    text: 'this is a text based game.',
    actions: []
  },
  {
    title: 'Top of the Tower',
    text: 'You awake at the top of a tower with no memory of how you got here. You have some janky leather armor on and a copper short sword in your hand. You see a door.',
    actions: [
      {
        text: 'Open the door',
        roomIndex: 2
      }
    ]
  },
];

const room = ref({});

const handleStartGame = (name) => {
  character.value.name = name;
  started.value = true;
  room.value = rooms[1];
}

onMounted(() => {
  room.value = rooms[0];
})



</script>

<style>
  
</style>