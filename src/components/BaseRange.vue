<script setup lang="ts">
import { ref, watch } from 'vue'
const props = defineProps({
    modelValue: {
        type: Object,
        required: true
    }
});

const emit = defineEmits(['update:modelValue']);

const localMin = ref(props.modelValue.min);
const localMax = ref(props.modelValue.max);

watch([localMin, localMax], () => {
    emit('update:modelValue', { min: localMin.value, max: localMax.value });
});
</script>

<template>
    <div>
        <input type="number" v-model="localMin" :min="0" :max="120" placeholder="Min" />
        <input type="number" v-model="localMax" :min="localMin" :max="400" placeholder="Max" />
    </div>
</template>