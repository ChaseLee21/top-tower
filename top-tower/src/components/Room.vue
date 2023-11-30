<template>
    <BRow>
        <h1 id="roomTitle" class="text-center">{{ props.room.title }}</h1>
    </BRow>
    <BRow>
        <p id="roomText" class="text-center">{{ props.room.text }}</p>
    </BRow>
</template>

<script setup>
import { BRow } from 'bootstrap-vue-next'
import { defineProps } from 'vue'

const props = defineProps({
    room: {
        type: Object,
        required: true
    }
})

</script>