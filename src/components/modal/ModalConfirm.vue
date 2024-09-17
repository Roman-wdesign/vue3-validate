<script setup lang="ts">
import { VueFinalModal } from 'vue-final-modal'
import { useForm, useField } from 'vee-validate'
import * as yup from 'yup'

import vButton from '@/components/VButton.vue'


import BaseSelect from '@/components/BaseSelect.vue'

const roomTypeOptions = [
    { value: 'склады', text: 'Склады' },
    { value: 'офисные', text: 'Офисные' },
    { value: 'производственные', text: 'Производственные' },
]

// Схема валидации
const validationSchema = yup.object({
    roomType: yup.array().required('Необходимо выбрать хотя бы один тип помещения'),

})


const { handleSubmit, resetForm, errors, meta, values } = useForm({
    validationSchema,
    initialValues: {
        roomType: [] as string[],
    },
})

// Поля с валидацией
const { value: roomType, errorMessage: roomTypeError, meta: roomTypeMeta } = useField<string[]>('roomType')






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
        <form>

            <div class="mb-10 flex items-center h-10">
                <h1 v-if="title" class="text-2xl">
                    {{ title }}
                </h1>
            </div>

            <div class="mb-10">

                <input type="text"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                    placeholder="Наименование организации / ИП" required />
            </div>
            <div class="mb-10">

                <input type="text"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                    placeholder="Контактный телефон" required />
            </div>
            <div class="mb-10">
                <div
                    class="dropdown bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                    <label>Тип помещения</label>
                    <BaseSelect v-model="roomType" :options="roomTypeOptions" />
                    <span v-if="roomTypeError && roomTypeMeta.touched">{{ roomTypeError }}</span>
                </div>
            </div>
            <div class="mb-10">

                <input type="text"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                    placeholder="Адрес" required />
            </div>

            <label for="email" class="block mb-4 w-full text-sm font-medium text-gray-900 dark:text-sky-900">Площадь
                помещения
                (м2)</label>
            <div class="grid gap-6 mb-6 md:grid-cols-2">

                <div class="mb-10">

                    <input type="text"
                        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="от 120" required />
                </div>
                <div class="mb-10">

                    <input type="text"
                        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="до 400" required />
                </div>
                <div class="mb-10">

                    <input type="text"
                        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="с" required />
                </div>
                <div class="mb-10">

                    <input type="text" id="confirm_password"
                        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="по" required />
                </div>
            </div>

            <vButton type="submit"
                class="text-white bg-sky-950 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-full text-sm w-full sm:w-auto px-12 py-2.5 text-center dark:bg-sky-900 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                Отправить</vButton>
        </form>
    </VueFinalModal>
</template>
