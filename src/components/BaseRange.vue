<script setup lang="ts">
import { ref, watch } from 'vue'
const props = defineProps({
    modelValue: {
        type: Object,
        required: true
    }
})

const emit = defineEmits(['update:modelValue'])

const localMin = ref(props.modelValue.min)
const localMax = ref(props.modelValue.max)

watch([localMin, localMax], () => {
    emit('update:modelValue', { min: localMin.value, max: localMax.value })
})
</script>

<template>
    <div class="grid gap-6 mb-6 md:grid-cols-2">
        <div>
            <input
                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                type="number" v-model="localMin" :min="100" :max="399" placeholder="Min" />
        </div>
        <div>
            <input
                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                type="number" v-model="localMax" :min="localMin" :max="400" placeholder="Max" />
        </div>
    </div>
</template>