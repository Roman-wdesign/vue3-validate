<script setup lang="ts">
import { VueFinalModal } from 'vue-final-modal'
import vButton from '@/components/VButton.vue'
defineProps<{
    title?: string
}>()

const emit = defineEmits<{
    (e: 'update:modelValue', modelValue: boolean): void
}>()
</script>

<template>
    <VueFinalModal class="flex justify-center items-center" content-class="flex flex-col bg-white p-4 bg-white "
        @update:model-value="val => emit('update:modelValue', val)">
        <div class="flex items-center h-10">
            <h1 v-if="title" class="text-2xl">
                {{ title }}
            </h1>
            <vButton @click="emit('update:modelValue', false)">
                X
            </vButton>
        </div>
        <slot />
    </VueFinalModal>
</template>
