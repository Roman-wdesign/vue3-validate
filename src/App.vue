<script setup lang="ts">
import { ModalsContainer, useModal } from 'vue-final-modal'
import ModalConfirm from '@/components/ModalConfirm.vue'
const { open, close } = useModal({
  component: ModalConfirm,
  attrs: {
    title: 'Заполните заявку, чтобы стать резидентом',
    onConfirm() {
      close()
    },
  },
  slots: {
    default: '<p>UseModal: The content of the modal</p>',
  },
})
</script>

<template>
  <VButton @click="() => open()">
    <button type="button"
      class="px-5 py-2.5 text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm">
      Open Modal</button>

  </VButton>
  <ModalsContainer />
</template>

