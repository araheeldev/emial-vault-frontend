<!-- src/components/forms/SignInForm.vue -->
<script setup lang="ts">
import InputField from '@/components/inputs/InputField.vue';
import Button from '@/components/ui/Button.vue';
import { ref } from 'vue';
import { useAuthStore } from '@/stores/authStore';
import { validateSignInForm } from '@/helpers/formValidation';

const email = ref('');
const password = ref('');
const emailError = ref('');
const passwordError = ref('');
const authStore = useAuthStore();

function handleSignIn() {
  const isValid = validateSignInForm({ email, password, emailError, passwordError });
  if (isValid) {
    authStore.login(email.value, password.value);
  }
}
</script>

<template>
  <div class="w-full max-w-md text-center text-textPrimary">
    <h1 class="text-2xl font-bold mb-4 text-primary">Sign In</h1>
    <p class="mb-6 text-textSecondary text-start">
      Welcome to Email Vault, please enter your login credentials below to start using the application.
    </p>

    <InputField label="Email" placeholder="Enter your email" type="email" v-model="email" :error="emailError" />
    <InputField label="Password" placeholder="Enter your password" type="password" v-model="password" :error="passwordError" :showToggle="true" />

    <div class="text-right mt-2">
      <a href="#" class="text-primary">Forgot password?</a>
    </div>

    <div class="flex justify-end mt-4">
      <Button :text="'Sign In'" @click="handleSignIn" />
    </div>

    <div class="mt-4">
      <span>Don't have an account?
        <router-link to="/create-account" class="text-primary underline">Create account</router-link>
      </span>
    </div>
  </div>
</template>
