<template>
        <button
            :class="['lg:w-1/3 px-4 py-2 mt-4 rounded-md  transition duration-300 text-sm', 
                    { 'bg-primary hover:bg-purple-700 text-white': !isTransparent },
                    customClass]"
        >
            {{ text }}
        </button>
</template>

<script setup lang="ts">
const props = defineProps({
    text: String,
    isTransparent: {
        type: Boolean,
        default: false,
    },
    customClass: {
        type: String,
        default: '',
    }
});
</script>
