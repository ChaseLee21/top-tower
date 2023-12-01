<template>
    <BRow class="d-flex justify-content-center">
        <BButton v-for="action in actions" variant="primary" class="btn w-auto m-2" @click="handleAction(action)">{{ action.text }}</BButton>
    </BRow>
</template>

<script setup>
import { BRow, BButton } from 'bootstrap-vue-next';
import { defineProps } from 'vue';

const emit = defineEmits(['action']);

const props = defineProps({
    actions: {
        type: Array,
        required: true
    }
})

const handleAction = (action) => {
    emit('action', action);
}



</script>