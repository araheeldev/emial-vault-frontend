<!-- src/components/Modal.vue -->
<template>
    <div v-if="isOpen" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
        <div class="bg-white p-6 rounded-lg ">
            <div class="flex justify-between items-center border-b pb-3 mb-4">
                <h3 class="text-xl font-semibold">{{ title }}</h3>
                <button @click="closeModal" class="text-gray-600 hover:text-gray-900">&times;</button>
            </div>

            <div class="mb-4 w-[70vw] max-h-[50vh] overflow-y-auto border border-gray-200 rounded-lg p-3">
                <slot />
            </div>

            <div class="text-right">
                <button @click="closeModal"
                    class="px-4 py-2 w-[10vw] bg-primary text-white rounded-md hover:bg-primary-dark">OK</button>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
    isOpen: Boolean,
    title: {
        type: String,
        default: 'Modal Title'
    }
});

const emit = defineEmits(['close']);

function closeModal() {
    emit('close');
}
</script>

<style scoped>
/* Additional modal styles if needed */
</style>