<template>
    <div class="relative mb-4">
        <label class="block text-left font-normal mb-1 text-textSecondary">{{ label }}</label>
        <div class="relative">
            <input
                :type="showPassword ? 'text' : 'password'"
                :placeholder="placeholder"
                :value="password"
                @input="onInput"
                @focus="showTooltip"
                @blur="hideTooltip"
                class="w-full px-4 py-2 border rounded-md focus:ring-2 focus:ring-primary focus:border-primary focus:outline-none"
            />
            <button
                @click="togglePasswordVisibility"
                type="button"
                class="absolute right-3 top-1/2 transform -translate-y-1/2 text-primary"
            >
                {{ showPassword ? 'Hide' : 'Show' }}
            </button>
        </div>

        <!-- Tooltip for Password Requirements -->
        <div
            v-if="isTooltipVisible"
            class="absolute top-full left-0 mt-2
                   lg:top-0 lg:left-full lg:mt-0 lg:ml-2
                   p-4 bg-white border border-gray-300 rounded shadow-lg w-64 text-left z-50"
        >
            <p class="font-semibold mb-2">Please fulfill the below requirements:</p>
            <ul>
                <li :class="{ 'text-green-600': requirements.length, 'text-red-600': !requirements.length }">
                    <span v-if="requirements.length">✔</span>
                    <span v-else>✘</span> At least 8 characters in length
                </li>
                <li :class="{ 'text-green-600': requirements.uppercase, 'text-red-600': !requirements.uppercase }">
                    <span v-if="requirements.uppercase">✔</span>
                    <span v-else>✘</span> At least 1 uppercase character
                </li>
                <li :class="{ 'text-green-600': requirements.lowercase, 'text-red-600': !requirements.lowercase }">
                    <span v-if="requirements.lowercase">✔</span>
                    <span v-else>✘</span> At least 1 lowercase character
                </li>
                <li :class="{ 'text-green-600': requirements.number, 'text-red-600': !requirements.number }">
                    <span v-if="requirements.number">✔</span>
                    <span v-else>✘</span> At least 1 number
                </li>
                <li :class="{ 'text-green-600': requirements.specialChar, 'text-red-600': !requirements.specialChar }">
                    <span v-if="requirements.specialChar">✔</span>
                    <span v-else>✘</span> At least 1 special character
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, defineProps, defineEmits, watch } from 'vue';
import debounce from 'lodash.debounce';
import { validatePassword } from '@/helpers/formValidation';

const props = defineProps({
    label: {
        type: String,
        default: 'Password *',
    },
    placeholder: {
        type: String,
        default: 'Enter your password',
    },
    modelValue: {
        type: String,
        default: '',
    },
});

const emit = defineEmits(['update:modelValue']);

// Reactive variables
const password = ref(props.modelValue);
const requirements = ref({
    length: false,
    uppercase: false,
    lowercase: false,
    number: false,
    specialChar: false,
});
const showPassword = ref(false);
const isTooltipVisible = ref(false); // Controls tooltip visibility

// Debounced function to update requirements
const updateRequirements = debounce(() => {
    requirements.value = validatePassword(password.value);
}, 100);

// Watch for password changes and emit updates
watch(password, (newPassword) => {
    updateRequirements();
    emit('update:modelValue', newPassword);
});

// Toggle password visibility
function togglePasswordVisibility() {
    showPassword.value = !showPassword.value;
}

// Show tooltip when input is focused
function showTooltip() {
    isTooltipVisible.value = true;
}

// Hide tooltip when input loses focus
function hideTooltip() {
    isTooltipVisible.value = false;
}

function onInput(event: Event) {
    const target = event.target as HTMLInputElement;
    password.value = target.value;
}
</script>
