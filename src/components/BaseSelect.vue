<script setup lang="ts">
import { ref, computed } from 'vue';

const props = defineProps({
    options: {
        type: Array,
        required: true,
    },
    modelValue: {
        type: Array,
        default: () => [],
    },
});

const emit = defineEmits(['update:modelValue']);

const isOpen = ref(false); // Переменная для открытия/закрытия дропдауна

// Функция для переключения состояния дропдауна
const toggleDropdown = () => {
    isOpen.value = !isOpen.value;
};

// Логика для обновления выбранных элементов при клике на чекбокс
const toggleSelection = (optionValue: string) => {
    const currentValue = [...props.modelValue];
    const index = currentValue.indexOf(optionValue);

    if (index === -1) {
        currentValue.push(optionValue); // Добавляем выбранный элемент
    } else {
        currentValue.splice(index, 1); // Удаляем, если элемент уже выбран
    }

    emit('update:modelValue', currentValue); // Обновляем модель данных
};

// Отображение выбранных элементов
const selectedText = computed(() => {
    return props.modelValue.length ? props.modelValue.join(', ') : 'Выберите тип помещения';
});
</script>

<template>
    <div class="select-container" @click="toggleDropdown">
        <!-- Заголовок дропдауна с выбранными элементами -->
        <div class="select-header">{{ selectedText }}</div>

        <!-- Выпадающий список с чекбоксами -->
        <div v-if="isOpen" class="dropdown-menu" @click.stop>
            <ul>
                <li v-for="option in options" :key="option.value" class="dropdown-item">
                    <input type="checkbox" :id="option.value" :value="option.value"
                        :checked="props.modelValue.includes(option.value)" @change="toggleSelection(option.value)" />
                    <label :for="option.value">{{ option.text }}</label>
                </li>
            </ul>
        </div>
    </div>
</template>

<style scoped>
.select-container {
    position: relative;
    width: 50%;
    cursor: pointer;
}

.select-header {
    background-color: #f0f0f0;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.select-header::after {
    content: '▼';
    font-size: 12px;
    margin-left: 10px;
}

.dropdown-menu {
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 4px;
    z-index: 1;
    max-height: 150px;
    overflow-y: auto;
}

ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

.dropdown-item {
    padding: 8px 10px;
    display: flex;
    align-items: center;
}

input[type="checkbox"] {
    margin-right: 10px;
}
</style>
