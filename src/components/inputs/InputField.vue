<!-- src/components/inputs/InputField.vue -->
<template>
    <div class="mb-4">
        <label class="block text-left font-normal mb-1 text-textSecondary">{{ label }}</label>
        <div class="relative">
            <input :type="type" :placeholder="placeholder" :value="modelValue" @input="onInput"
                class="w-full px-4 py-2 border rounded-md focus:ring-2 focus:ring-primary focus:border-primary focus:outline-none" />
            <p v-if="error" class="text-red-500 text-sm mt-1 text-start">{{ error }}</p>
        </div>
    </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';

const { label, placeholder, modelValue, type, error } = defineProps({
    label: String,
    placeholder: String,
    modelValue: [String, Number],
    type: {
        type: String,
        default: 'text',
    },
    error: {
        type: String,
        default: '',
    },
});

const emit = defineEmits(['update:modelValue']);

function onInput(event: Event) {
    const target = event.target as HTMLInputElement;
    emit('update:modelValue', target.value);
}
</script>
