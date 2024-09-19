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
        rentStart: '<label for="text" class="block mb-4 w-full text-sm font-medium text-gray-900 dark:text-sky-900">Дата начала аренды</ label >',
        sqOffice: '<label for="text" class="block mb-4 w-full text-sm font-medium text-gray-900 dark:text-sky-900">Площадь помещения (м2)</label>',
        typeOffice: '<label>Тип помещения</label>'
    },
})
</script>

<template>
    <VButton @click="() => open()">
        Open Modal
    </VButton>
</template>