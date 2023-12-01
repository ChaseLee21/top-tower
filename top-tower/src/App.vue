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
    inventory: [
        {
          name: 'Torch'
          // TODO: once I know what i want the torch to do add some functionality here 
        },
        {
          name: 'Burnt Boots'
          // TODO: once I know what i want the Burnt Boots to do add some functionality here
        }
      ],
    removeEquipment: 'boots', // remove boots from equipment
    actions: [
      {
        text: 'Go to the door',
        roomIndex: 4,
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
        text: 'Go Through the Door',
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
  },
  {
    title: 'Slime',
    text: `You go left and see a... well a blue looking slime... thing. As it mindlessly jumps around it makes a gurgling noise. You think to yourself "I should probably kill this thing".`,
    enemies: [
      {
        name: 'Slime',
        health: 10,
        attack: 1,
        loot: [
          {
            name: 'Gel',
          }
        ]
      }
    ],
    actions: [
      {
        text: 'Go Back and Go Right',
        roomIndex: 7
      }
    ]
  },
  {
    title: 'Barracks',
    text: 'You go right and enter some sort of barracks. You see a chest in the corner of the room and a helmet on a table.',
    actions: [
      {
        text: 'Open the chest',
        roomIndex: 8
      },
      {
        text: 'Put on the helmet',
        roomIndex: 9
      }
    ]
  },
  {
    title: 'Barracks',
    text: 'You open the chest and find an iron dagger awaiting an adventurer like yourself. As you sheath the dagger you hear a noise coming from around the doorway at the far end of the room.', 
    inventory: {
      name: 'Iron Dagger',
      attack: 2
    },
    actions: [
      {
        text: 'Investigate the noise',
        roomIndex: 10
      }
    ]
  },
  {
    title: 'Barracks',
    text: 'You put on the helmet and feel a little safer. You hear a noise coming from around the doorway at the far end of the room.',
    inventory: {
      name: 'Iron Helmet',
      defense: 2
    },
    actions: [
      {
        text: 'Investigate the noise',
        roomIndex: 10
      }
    ]
  },
  {
    title: 'Barracks',
    text: 'Being the brave adventurer you are you draw your weapon and get the jump on the enemy. Turning the doorway you see a giagantic wolf. It looks at you. Not afraid of your janky leather armor it prepares to fight.',
    enemies: [
      {
        name: 'Wolf',
        health: 20,
        attack: 2,
        loot: [
          {
            name: 'Wolf Pelt',
          }
        ]
      }
    ],
    actions: [
      {
        text: 'Continue',
        roomIndex: 11
      }
    ]
  },
  {
    title: 'Barracks',
    text: 'After defeating the wolf you see a staircase leading down. Since you are an adventurer and do not have a fear of stairs you proceed down the stairs.',
    actions: [
      {
        text: 'Go Down the Stairs',
        roomIndex: 12
      }
    ]
  },
  {
    title: 'Blacksmith',
    text: 'This new area seems to be a blacksmith. Not that you know what a blacksmith looks like but there are anvils and hammers and stuff. You hear the clunking of metal boots hitting the floor coming from behind the forge.',
    actions: [
      {
        text: 'Wait',
        roomIndex: 13
      },
      {
        text: 'Charge in like a real adventurer',
        roomIndex: 14
      }
    ]
  },
  {
    title: 'Blacksmith',
    text: 'You wait for only a few seconds before a large dwarf comes out from behind the forge. He looks at you and shouts something in a language you have never heard before but for some reason you understand. "You beardless fool! Die!"',
    enemies: [
      {
        name: 'Dwarf',
        health: 30,
        attack: 3,
        loot: [
          {
            name: 'Dwarven Armor',
          }
        ]
      }
    ],
    actions: [
      {
        text: 'Continue',
        roomIndex: 15
      }
    ]
  },
  {
    title: 'Blacksmith',
    text: 'You charge in like a real adventurer and see a large dwarf behind the forge. He looks at you and shouts something in a language you have never heard before but for some reason you understand. "Your hammers bigger than your anvil! Die you beardless fool!"',
    enemies: [
      {
        name: 'Dwarf',
        health: 30,
        attack: 3,
        loot: [
          {
            name: 'Dwarven Armor',
          }
        ]
      }
    ],
    actions: [
      {
        text: 'Continue',
        roomIndex: 16
      }
    ]
  },
  {
    title: 'Blacksmith',
    text: 'After defeating the dwarf and admiring the craftmanship of his armor you have taken, you see a staircase going down. You must be getting closer to the bottom of the tower... right?',
  },
  {
    title: 'Blacksmith',
    text: 'You have deafted the dwarf and taken his armor as you yours. You feel... healthier than you did a moment ago. Being a brave adventurer seems to have paid off. "I must continue" You think to yourself as you notice a staircase leading down. You must be getting closer to the bottom of the tower... right?',
    modify: {
      stat: 'health',
      amount: 10
    },
  },

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
  // required item check
  if (action.requiredInventory !== undefined) {
    let hasItem = false;
    character.value.inventory.forEach((item, i) => {
      if (item.name === action.requiredInventory) {
        character.value.inventory.splice(character.value.inventory.indexOf(item), 1);
        hasItem = true;
      }
    });
    if (!hasItem) {
      return;
    }
  }

  // set current room
  room.value = rooms[action.roomIndex];

  // set enemies
  if (room.value.enemies !== undefined) {
    enemies.value = room.value.enemies;
  }

  // add items in room to inventory
  if (room.value.inventory !== undefined) {
    if (Array.isArray(room.value.inventory)) {
      for (let i = 0; i < room.value.inventory.length; i++) {
        character.value.inventory.push(room.value.inventory[i]);
      }
    } else {
      character.value.inventory.push(room.value.inventory);
    }
  }
  
  // removes equipment slot from character object
  if (room.value.removeEquipment !== undefined) {
    character.value.equipment[room.value.removeEquipment] = {name:'none'};
    calculateAttack();
  }
  
  // modify a property of the character object
  // should only really be used for health.
  if (room.value.modify !== undefined) {
    let stat = room.value.modify.stat;
    let amount = room.value.modify.amount;
    if (stat && amount) {
      character.value[stat] += amount;
    }
  }
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