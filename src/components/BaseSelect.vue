<script setup lang="ts">
import { ref, computed } from 'vue'


interface DropdownOption {
    value: string;
    text: string;
}

const props = defineProps<{
    options: DropdownOption[];
    modelValue: any;
}>()

const emit = defineEmits<{
    (e: 'update:modelValue', value: string[]): void;
}>()

const isOpen = ref(false) // Переменная для открытия/закрытия дропдауна

// Функция для переключения состояния дропдауна
const toggleDropdown = () => {
    isOpen.value = !isOpen.value
}

// Логика для обновления выбранных элементов при клике на чекбокс
const toggleSelection = (optionValue: string) => {
    const currentValue = [...props.modelValue]
    const index = currentValue.indexOf(optionValue)

    if (index === -1) {
        currentValue.push(optionValue) // Добавляем выбранный элемент
    } else {
        currentValue.splice(index, 1) // Удаляем, если элемент уже выбран
    }

    emit('update:modelValue', currentValue) // Обновляем модель данных
}

// Отображение выбранных элементов
const selectedText = computed(() => {
    return props.modelValue.length ? props.modelValue.join(', ') : 'Выберите тип помещения'
})
</script>

<template>
    <div class="select-container relative w-full cursor-pointer" @click="toggleDropdown">
        <!-- Заголовок дропдауна с выбранными элементами -->
        <div
            class="select-header flex items-center justify-between p-1 border-solid border-1 rounded border-zinc-400  bg-slate-500 after:text-xs after:ml-2.5 after:content-['▼']">
            {{ selectedText }}</div>

        <!-- Выпадающий список с чекбоксами -->
        <div v-if="isOpen"
            class="dropdown-menu absolute top-full w-full left-0 max-h-40 border-solid border-1 rounded z-10 overflow-y-auto border-zinc-400 bg-slate-500"
            @click.stop>
            <ul class="list-none m-0 p-0">
                <li v-for="option in options" :key="option.value" class="dropdown-item flex items-center px-2.5 py-2">
                    <input type="checkbox" :id="option.value" :value="option.value"
                        :checked="props.modelValue.includes(option.value)" @change="toggleSelection(option.value)" />
                    <label :for="option.value">{{ option.text }}</label>
                </li>
            </ul>
        </div>
    </div>
</template>