<script setup lang="ts">
import { VueFinalModal } from 'vue-final-modal'
import { useForm, useField } from 'vee-validate'
import * as yup from 'yup'

import vButton from '@/components/VButton.vue'
import BaseSelect from '@/components/BaseSelect.vue'
import BaseRange from '@/components/BaseRange.vue'


defineProps<{
    title?: string
}>()

const emit = defineEmits<{
    (e: 'update:modelValue', modelValue: boolean): void
}>()

interface RoomArea {
    min: number
    max: number
}

interface OrganizationName {
    organizationName: string
}

interface Phonenumber {
    phoneNumber: string
}

interface Address {
    address: string
}


// Выпадающий список
const roomTypeOptions = [
    { value: 'склады', text: 'Склады' },
    { value: 'офисные', text: 'Офисные' },
    { value: 'производственные', text: 'Производственные' },
]



// Метод для удаления лишних символов перед валидацией
const formatPhoneNumberForValidation = (phone: string) => {
    return phone.replace(/\s|\(|\)|-/g, ''); // Убираем пробелы, скобки и дефисы
}







// Схема валидации
const validationSchema = yup.object({
    phoneNumber: yup
        .string()
        .transform((value) => formatPhoneNumberForValidation(value))
        .required('Поле обязательно для заполнения'),
    roomType: yup.array().required('Необходимо выбрать хотя бы один тип помещения'),
    roomArea: yup.object().shape({
        min: yup.number().min(0).max(120).required('Минимальное значение площади обязательно'),
        max: yup
            .number()
            .min(yup.ref('min'), 'Максимальное значение должно быть больше минимального')
            .max(400, 'Максимальное значение площади — 400')
            .required('Максимальное значение площади обязательно'),
    }),
    organizationName: yup.string().required('Поле обязательно для заполнения'),
    address: yup.string().required('Поле обязательно для заполнения'),
})




// Используем useForm для управления формой и ее состоянием
const { handleSubmit, resetForm, errors, meta, values } = useForm({
    validationSchema,
    initialValues: {
        roomType: [] as string[],
        roomArea: { min: 100, max: 400 },
        organizationName: '',
        phoneNumber: '',
        address: ''
    },
})


// Поля с валидацией
const { value: roomType, errorMessage: roomTypeError, meta: roomTypeMeta } = useField<string[]>('roomType')
const { value: roomArea, errorMessage: roomAreaError, meta: roomAreaMeta } = useField<RoomArea>('roomArea')
const { value: organizationName, errorMessage: orgError, meta: orgMeta } = useField<OrganizationName>('organizationName')
const { value: phoneNumber, errorMessage: phoneError, meta: phoneMeta } = useField<Phonenumber>('phoneNumber')
const { value: address, errorMessage: addressError, meta: addressMeta } = useField<Address>('address')





// написание маски телефона 
const formatPhoneNumber = (event: any) => {
    const input = event.target.value.replace(/\D/g, '')
    const match = input.match(/(\d{3})(\d{3})(\d{4})/)

    if (match) {
        event.target.value = `+7 (${match[1]}) ${match[2]}-${match[3]}`
    }

    phoneNumber.value = event.target.value
}



// Отправка формы
const onSubmit = handleSubmit((values) => {
    console.log('Отправлено:', values);
    resetForm();  // Сброс формы после отправки
});


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
                <span v-if="orgError && orgMeta.touched">{{ orgError }}</span>
                <input type="text" v-model="organizationName"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                    placeholder="Наименование организации / ИП" required />
            </div>
            <div class="mb-10">
                <span v-if="phoneError && phoneMeta.touched">{{ phoneError }}</span>
                <input @input="formatPhoneNumber" v-model="phoneNumber" type="text"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                    placeholder="Контактный телефон +7 (___) ___-____" required />
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
                <span v-if="addressError && addressMeta.touched">{{ addressError }}</span>
                <input v-model="address" type="text"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                    placeholder="Адрес" required />
            </div>

            <label for="text" class="block mb-4 w-full text-sm font-medium text-gray-900 dark:text-sky-900">Площадь
                помещения
                (м2)</label>
            <BaseRange v-model="roomArea" />
            <span v-if="roomAreaError?.min && roomAreaMeta.touched">{{ roomAreaError?.min }}</span>
            <span v-if="roomAreaError?.max && roomAreaMeta.touched">{{ roomAreaError?.max }}</span>
            <label for="text" class="block mb-4 w-full text-sm font-medium text-gray-900 dark:text-sky-900">Дата
                начала аренды</label>
            <div class="grid gap-6 mb-6 md:grid-cols-2">
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

            <vButton @submit="onSubmit" type="submit"
                class="text-white bg-sky-950 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-full text-sm w-full sm:w-auto px-12 py-2.5 text-center dark:bg-sky-900 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                Отправить</vButton>
        </form>
    </VueFinalModal>
</template>
