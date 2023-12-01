<template>
    <BRow>
        <h2 class="text-center">Enemies</h2>
    </BRow>
    <BCardGroup class="d-flex justify-content-center">
        <BRow>
            <BCol v-for="enemy in enemies">
                <BCard class="text-center mx-1 mb-3 w-auto" @click="attackEnemy(enemy)">
                    <BCardHeader>
                        <BCardTitle>
                            <h3> {{ enemy.name }}</h3>
                        </BCardTitle>
                        <BCardBody v-if="checkEnemyAlive(enemy)">
                            <BListGroup>
                                <BListGroupItem>
                                    Health: {{ enemy.health }}
                                </BListGroupItem>
                                <BListGroupItem>
                                    Attack: {{ enemy.attack }}
                                </BListGroupItem>
                            </BListGroup>
                        </BCardBody>
                        <BCardBody v-else>
                            <BListGroup>
                                <BListGroupItem v-for="loot in enemy.loot">
                                    <BButton @click="lootEnemy(loot, enemy)">{{ loot.name }}</BButton>
                                </BListGroupItem>
                            </BListGroup>
                        </BCardBody>
                    </BCardHeader>
                </BCard>
            </BCol>
        </BRow>
    </BCardGroup>
</template>

<script setup>
import { BRow, BCard } from 'bootstrap-vue-next';

const props = defineProps({
    enemies: {
        type: Array,
        required: true
    },
    character: {
        type: Object,
        required: true
    }
})

// handles attacking an enemy
const attackEnemy = (enemy) => {
    if (!checkEnemyAlive(enemy)) return;
    let character = props.character;
    let attack = character.attack;
    enemy.health -= attack;
    // may not need the else if statement after adding the loot functionality
    if (!checkEnemyAlive(enemy) && enemy.loot == undefined) {
        removeEnemy(enemy);
    }
}

// checks enemy health 
const checkEnemyAlive = (enemy) => {
    return (enemy.health <= 0) ? false : true;
}

// loot enemy
const lootEnemy = (loot, enemy) => {
    let character = props.character;
    character.inventory.push(loot);
    removeEnemy(enemy);
}

// removes enemy from enemies array
const removeEnemy = (enemy) => {
    let index = props.enemies.indexOf(enemy);
    props.enemies.splice(index, 1);
}

</script>