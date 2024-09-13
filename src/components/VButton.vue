<script setup lang="ts">
export interface Props {
    disabled?: boolean,
    buttonClass?: string
}

const props = defineProps<Props>()
</script>
<template>
    <button
        class="w-auto font-bold py-2 px-4 rounded disabled:opacity-50 disabled:cursor-not-allowed text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300  text-sm"
        :disabled="props.disabled" type="button">
        <slot></slot>
    </button>
</template>