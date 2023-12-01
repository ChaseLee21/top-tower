<template>
  <BContainer>
    <Room :room = "room" />
    <StartModal v-if="!started" @startGame="handleStartGame"/>
    <Enemies v-if="started" :enemies="enemies" :character = "character"/>
    <Actions v-if="started && enemies.length == 0" :actions="room.actions" @action="handleAction"/>
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

//state of game
const started = ref(false);

//character object
const character = ref({
  name: 'Character',
  health: 100,
  attack: 0,
  inventory: [],
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

//rooms array
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
  {
    title: 'Dim Hallway',
    text: 'The door closes behind you. Inside is a hallway dimly lit by torches. You see a door at the end of the hallway.',
    actions: [
      {
        text: 'Grab Torch',
        roomIndex: 3
      },
      {
        text: 'Go to the door',
        roomIndex: 4
      }
    ]
  },
  {
    title: 'Dim Hallway',
    text: "You attempt to grab a torch off the wall. It's stuck. You pull harder and as it comes off the wall as you fall to the ground. Your boots get burnt by the torch, but atleast you have a torch now.",
    actions: [
      {
        text: 'Go to the door',
        roomIndex: 4,
        // add torch to inventory
        // remove boots from equipment
        // add burnt boots to inventory
      }
    ]
  },
  {
    title: 'Dim Hallway',
    text: 'You walk to the door at the end of the hallway. You open the door and 2 skeevers come running out. You thought skeevers were just a thing in medieval video games with dragons but not everything makes sense sometimes.',
    enemies: [
      {
        name: 'Skeevers',
        health: 10,
        attack: 1,
        loot: [
          {
            name: 'Skeevers Tail',
          }
        ]
      },
      {
        name: 'Skeevers',
        health: 10,
        attack: 1,
        loot: [
          {
            name: 'Skeevers Tail',
          }
        ]
      }
    ],
    actions: [
      {
        text: 'Test',
        roomIndex: 5
      },
    ]
  },
  {
    title: 'Stone Stairs',
    text: 'With the skeevers dead you go through the door and proceed down some stairs made of stone. At the bottom of the stairs a passage leads to the left and right. You hear a noise coming from the left.',
    actions: [
      {
        text: 'Go Left',
        roomIndex: 6
      },
      {
        text: 'Go Right',
        roomIndex: 7
      }
    ]
  }
];

//enemies array
const enemies = ref([]);

//current room object
const room = ref({});

//start game function
const handleStartGame = (name) => {
  character.value.name = name;
  started.value = true;
  room.value = rooms[1];
}

//action function - changes room, addes enemies, modifies character / inventory, based on the action clicked
const handleAction = (action) => {
  console.log(action);
  // required item check
  if (action.requiredInventory !== undefined) {
    if (!character.value.inventory.includes(action.requiredInventory)) {
      return;
    } 
  }
  // set current room
  room.value = rooms[action.roomIndex];
  // set enemies
  if (room.value.enemies !== undefined) {
    enemies.value = room.value.enemies;
  }

  // if (action.inventory !== undefined) {
  //   // add inventory to inventory array
  // }
  // if (action.removeInventory !== undefined) {
  //   // remove inventory from inventory array
  // }
  // if (action.modifyCharacter !== undefined) {
  //   // modify character
  // }
  // if (action.removeEquipment !== undefined) {
  //   // remove equipment from equipment object
  // }
}

//calculate character attack
const calculateAttack = () => {
  let attack = 0;
  if (character.value.equipment.weapon.attack !== undefined) {
    attack += character.value.equipment.weapon.attack;
  }
  if (character.value.equipment.helm.attack !== undefined) {
    attack += character.value.equipment.helm.attack;
  }
  if (character.value.equipment.armor.attack !== undefined) {
    attack += character.value.equipment.armor.attack;
  }
  if (character.value.equipment.boots.attack !== undefined) {
    attack += character.value.equipment.boots.attack;
  }
  character.value.attack = attack;
}

onMounted(() => {
  room.value = rooms[0];
  calculateAttack();
})



</script>

<style>
  
</style>