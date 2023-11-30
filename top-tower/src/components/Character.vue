<template>
    <BCardGroup class="text-center flex justify-content-center">
        <BRow>
            <BCol>
                <BCard>
                    <BCardHeader>
                        <BCardTitle>
                            <h2>Equipment</h2>
                        </BCardTitle>
                        <BCardBody id="equipment">
                            <BListGroup>
                                <BListGroupItem>
                                    Helm: {{ props.character.equipment.helm.name }}
                                </BListGroupItem>
                                <BListGroupItem>
                                    Armor: {{ props.character.equipment.armor.name }}
                                </BListGroupItem>
                                <BListGroupItem>
                                    Boots: {{ props.character.equipment.boots.name }}
                                </BListGroupItem>
                                <BListGroupItem>
                                    Weapon: {{ props.character.equipment.weapon.name }}
                                </BListGroupItem>
                            </BListGroup>
                        </BCardBody>
                    </BCardHeader>
                </BCard>
            </BCol>
            <BCol>
                <BCard>
                    <BCardHeader>
                        <BCardTitle>
                            <h2> {{ props.character.name }} </h2>
                        </BCardTitle>
                        <BCardBody id="character">
                            <BListGroup>
                                <BListGroupItem>
                                    Health: {{ props.character.health }}
                                </BListGroupItem>
                                <BListGroupItem>
                                    Attack: {{ props.character.attack }}
                                </BListGroupItem>
                            </BListGroup>
                        </BCardBody>
                    </BCardHeader>
                </BCard>
            </BCol>
            <BCol>
                <BCard>
                    <BCardHeader>
                        <BCardTitle>
                            <h2>Inventory</h2>
                        </BCardTitle>
                        <BCardBody id="inventory">
                            <BListGroup>
                                <BListGroupItem v-for="item in props.character.inventory">
                                    <BButton variant="primary" class="btn-sm">{{item.name}}</BButton>
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
import { BCardGroup, BCard, BCardHeader, BCardBody, BCardTitle, BListGroup, BListGroupItem, BRow, BCol } from 'bootstrap-vue-next';
import { defineProps, onMounted } from 'vue';

const props = defineProps({
    character: {
        type: Object,
        required: true
    }
})

onMounted(() => {
    console.log(props.character);
})

</script>