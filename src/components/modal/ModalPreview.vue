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
        org: '<p class="text-gray-900">Наименование организации / ИП</p>',
        header: '<p class="text-gray-900">Контактный телефон</p>',
    },
})
</script>

<template>
    <VButton @click="() => open()">
        Open Modal
    </VButton>
</template>