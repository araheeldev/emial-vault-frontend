<!-- src/views/CodeVerificationView.vue -->
<script setup lang="ts">
import { ref } from 'vue';
import IllustrationPanel from '@/components/layout/IllustrationPanel.vue';
import Button from '@/components/ui/Button.vue';
import Footer from '@/components/layout/Footer.vue';
import { isCodeComplete } from '@/helpers/formValidation';
import { useAuthStore } from '@/stores/authStore';

const authStore = useAuthStore();

const code = ref(['', '', '', '', '', '']); // Six empty slots for the code
const errorMessage = ref(''); // Error message to display if validation fails

function handleInput(event: Event, index: number) {
    const target = event.target as HTMLInputElement;
    if (target.value.length === 1 && index < 5) {
        const nextInput = document.getElementById(`code-input-${index + 1}`) as HTMLInputElement;
        if (nextInput) nextInput.focus();
    }
}

function handleBackspace(event: KeyboardEvent, index: number) {
    const target = event.target as HTMLInputElement;
    if (event.key === 'Backspace' && index > 0 && !target.value) {
        const prevInput = document.getElementById(`code-input-${index - 1}`) as HTMLInputElement;
        if (prevInput) prevInput.focus();
    }
}

const isCodeFilled = isCodeComplete(code);

function verifyCode() {
    if (isCodeFilled.value) {
        const codeString = code.value.join('');
        errorMessage.value = '';
        authStore.verifyCode(codeString);
    } else {
        errorMessage.value = 'Please fill in all fields before verifying.';
    }
}
</script>

<template>
    <div class="flex flex-col lg:flex-row h-screen font-sans bg-background">
        <div class="flex flex-1 items-center justify-center bg-white p-8 relative">
            <div class="w-full max-w-md text-center text-textPrimary">
                <h1 class="text-3xl font-medium mb-4 text-start">Multi-Factor Authentication</h1>
                <p class="text-textSecondary text-start mb-4">
                    Please enter the confirmation code you see on your authentication app.
                </p>

                <div class="flex justify-center space-x-2 mb-3 h-full">
                    <input v-for="(digit, index) in code" :key="index" v-model="code[index]"
                        @input="handleInput($event, index)" @keydown="handleBackspace($event, index)"
                        :id="`code-input-${index}`" maxlength="1" type="text" class="text-center border rounded-sm focus:outline-none focus:ring-2 focus:ring-primary
                   w-10 h-10 sm:w-12 sm:h-12 md:w-16 md:h-16 text-lg md:text-xl" />
                </div>

                <!-- Error message -->
                <p v-if="errorMessage" class="text-red-500 text-sm mb-3 text-start">{{ errorMessage }}</p>

                <div class="mb-3 flex justify-start">
                    <router-link to="/recovery-code-verification" class="text-primary underline text-sm">Use Recovery
                        Code</router-link>
                </div>

                <div class="flex justify-end">
                    <Button :text="'Verify'" @click="verifyCode" />
                </div>

                <Footer />
            </div>
        </div>

        <IllustrationPanel />
    </div>
</template>
