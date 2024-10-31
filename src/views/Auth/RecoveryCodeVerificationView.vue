<!-- src/views/RecoveryCodeVerificationView.vue -->
<script setup lang="ts">
import { ref } from 'vue';
import { useAuthStore } from '@/stores/authStore';
import IllustrationPanel from '@/components/layout/IllustrationPanel.vue';
import InputField from '@/components/inputs/InputField.vue';
import Button from '@/components/ui/Button.vue';
import Footer from '@/components/layout/Footer.vue';
import { validateRecoveryCode } from '@/helpers/formValidation';

const authStore = useAuthStore();
const recoveryCode = ref('');
const recoveryCodeError = ref('');

function verifyRecoveryCode() {
  const isValid = validateRecoveryCode({ recoveryCode, recoveryCodeError });
  if (isValid) {
    authStore.verifyRecoveryCode(recoveryCode.value);
  } else {
    console.log('Please enter a valid recovery code');
  }
}
</script>

<template>
  <div class="flex flex-col lg:flex-row h-screen font-sans bg-background">
    <div class="flex flex-1 items-center justify-center bg-white p-8 relative">
      <div class="w-full max-w-md text-center text-textPrimary">
        <h1 class="text-3xl font-medium mb-4 text-start">
          Multi-Factor Authentication Complete Here
        </h1>
        <p class="text-textSecondary text-start mb-4">
          Please enter your recovery code.
        </p>

        <!-- Input Field with Error Message if Invalid -->
        <InputField
          label="Recovery Code"
          placeholder="Enter your recovery code"
          type="text"
          v-model="recoveryCode"
          :error="recoveryCodeError"
        />

        <div class="mb-3 flex justify-start">
          <router-link
            to="/code-verification"
            class="text-primary underline text-sm"
            >Use One Time Password</router-link
          >
        </div>

        <div class="flex justify-end">
          <Button
            :text="'Verify'"
            @click="verifyRecoveryCode"
          />
        </div>

        <Footer />
      </div>
    </div>

    <IllustrationPanel />
  </div>
</template>
