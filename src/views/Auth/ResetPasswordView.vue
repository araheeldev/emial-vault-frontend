<!-- src/views/ResetPasswordView.vue -->
<script setup lang="ts">
import { ref } from 'vue';
import { useAuthStore } from '@/stores/authStore';
import InputField from '@/components/inputs/InputField.vue';
import PasswordFieldWithTooltip from '@/components/inputs/PasswordFieldWithTooltip.vue';
import Button from '@/components/ui/Button.vue';
import IllustrationPanel from '@/components/layout/IllustrationPanel.vue';
import Footer from '@/components/layout/Footer.vue';
import { validateEmailField } from '@/helpers/formValidation';

const authStore = useAuthStore();
const stage = ref(1); // 1 for email input, 2 for password reset
const email = ref('');
const emailError = ref('');
const newPassword = ref('');
const agreement = ref(false);

function proceedToPasswordReset() {
    if (validateEmailField(email, emailError)) {
        authStore.resetPassword(email.value); // Call the resetPassword method
        stage.value = 2; // Move to the next stage
    }
}

function submitNewPassword() {
    if (newPassword.value && agreement.value) {
        console.log('New password set:', newPassword.value);
    }
}
</script>

<template>
    <div class="flex flex-col lg:flex-row h-screen font-sans bg-background">
        <div class="flex flex-1 items-center justify-center bg-white p-8 relative">
            <div class="w-full max-w-md text-center text-textPrimary">
                <h1 class="text-3xl font-bold mb-4 text-start">Reset Password</h1>

                <template v-if="stage === 1">
                    <p class="text-textSecondary text-start mb-4 text-sm">
                        Having trouble logging in to your account? Please enter your email below to reset your password.
                    </p>
                    <InputField placeholder="Email" type="email" v-model="email" :error="emailError" />
                    <Button :text="'Reset password'" class="w-full mt-4" @click="proceedToPasswordReset" />
                </template>

                <!-- Stage 2: Enter New Password -->
                <template v-else>
                    <p class="text-textSecondary text-start mb-4">
                        Please enter your new password
                    </p>
                    <PasswordFieldWithTooltip v-model="newPassword" label="Password"
                        placeholder="Enter your new password" />
                    <div class="flex items-center mt-4 mb-6">
                        <input type="checkbox" id="agreement" v-model="agreement"
                            class="w-4 h-4 text-primary border-gray-300 rounded focus:ring-primary">
                        <label for="agreement" class="ml-2 text-sm text-textSecondary">
                            I agree to the <a href="#" class="text-primary underline">Terms and Conditions</a> and <a
                                href="#" class="text-primary underline">Privacy Policy</a>
                        </label>
                    </div>

                    <div class="flex justify-end">
                        <Button :text="'Continue'" @click="submitNewPassword" />
                    </div>
                </template>

                <hr class="bg-primary h-0.5 my-3">
                <p class="text-sm">
                    Already have an account?
                    <router-link to="/signin" class="text-primary">Sign In</router-link>
                </p>
            </div>

            <Footer />
        </div>

        <IllustrationPanel />
    </div>
</template>
