<script setup lang="ts">
import { useModal } from 'vue-final-modal'
import ModalConfirm from '@/components/modal/ModalConfirm.vue'
import VButton from '@/components/VButton.vue'

const { open } = useModal({
    component: ModalConfirm,
    attrs: {
        title: 'Заполните заявку, чтобы стать резидентом',
    },
    slots: {
        default: '<p>The content of the modal</p>',
    },
})
</script>

<template>
    <VButton @click="() => open()">
        Open Modal
    </VButton>
</template>