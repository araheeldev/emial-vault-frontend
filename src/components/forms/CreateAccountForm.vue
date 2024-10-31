<!-- src/components/CreateAccountForm.vue -->
<script setup lang="ts">
import { defineProps, defineEmits, ref, watch } from 'vue';
import InputField from '@/components/inputs/InputField.vue';
import Button from '@/components/ui/Button.vue';
import PasswordFieldWithTooltip from '@/components/inputs/PasswordFieldWithTooltip.vue';
import Modal from '@/components/layout/Modal.vue';
import { TERMS_AND_CONDITIONS, PRIVACY_POLICY } from '@/helpers/modalContent';
import { validateAccountForm } from '@/helpers/formValidation';

// Props and Emits
const props = defineProps({
  firstName: String,
  lastName: String,
  companyName: String,
  email: String,
  password: String,
  referralCode: String,
  isTermsChecked: Boolean,
});
const emit = defineEmits([
  'update:firstName',
  'update:lastName',
  'update:companyName',
  'update:email',
  'update:password',
  'update:referralCode',
  'update:isTermsChecked',
  'proceedToBilling',
]);

// Local refs to sync with props
const firstName = ref(props.firstName);
const lastName = ref(props.lastName);
const companyName = ref(props.companyName);
const email = ref(props.email);
const password = ref(props.password);
const referralCode = ref(props.referralCode);
const isTermsChecked = ref(props.isTermsChecked);

// Watch for changes in local refs and emit updates
watch(firstName, (val) => emit('update:firstName', val));
watch(lastName, (val) => emit('update:lastName', val));
watch(companyName, (val) => emit('update:companyName', val));
watch(email, (val) => emit('update:email', val));
watch(password, (val) => emit('update:password', val));
watch(referralCode, (val) => emit('update:referralCode', val));
watch(isTermsChecked, (val) => emit('update:isTermsChecked', val));

// Error messages and modal controls
const errors = {
  firstName: ref(''),
  lastName: ref(''),
  companyName: ref(''),
  email: ref(''),
  password: ref(''),
  terms: ref(''),
};
const isTermsModalOpen = ref(false);
const isPrivacyPolicyModalOpen = ref(false);

// Modal handlers
function openTermsModal() {
  isTermsModalOpen.value = true;
}
function openPrivacyPolicyModal() {
  isPrivacyPolicyModalOpen.value = true;
}

// Account creation handler
function handleCreateAccount() {
  const isValid = validateAccountForm({
    firstName,
    lastName,
    companyName,
    email,
    password,
    isTermsChecked,
    errors,
  });

  if (isValid) {
    emit('proceedToBilling');
  }
}
</script>

<template>
  <div class="w-full max-w-md text-center text-textPrimary">
    <h1 class="text-3xl font-bold mb-4 text-start">Create Account</h1>
    <p class="text-textSecondary text-start mb-4">
      Welcome to Email Vault, please enter your details below to create an
      account.
    </p>

    <InputField
      label="First Name *"
      placeholder="Enter your first name"
      type="text"
      v-model="firstName"
      :error="errors.firstName.value"
    />
    <InputField
      label="Last Name *"
      placeholder="Enter your last name"
      type="text"
      v-model="lastName"
      :error="errors.lastName.value"
    />
    <InputField
      label="Company Name *"
      placeholder="Enter your company name"
      type="text"
      v-model="companyName"
      :error="errors.companyName.value"
    />
    <InputField
      label="Email *"
      placeholder="Enter your email"
      type="email"
      v-model="email"
      :error="errors.email.value"
    />
    <PasswordFieldWithTooltip
      v-model="password"
      label="Password *"
      placeholder="Enter your password"
      :error="errors.password.value"
    />

    <InputField
      label="Referral Code"
      placeholder="Enter referral code"
      type="text"
      v-model="referralCode"
    />

    <div class="flex items-center my-4">
      <input
        type="checkbox"
        id="terms"
        v-model="isTermsChecked"
        class="w-4 h-4 text-primary border-gray-300 rounded focus:ring-primary"
      />
      <label
        for="terms"
        class="ml-2 text-sm text-textSecondary"
      >
        I agree to the
        <a
          href="#"
          @click.prevent="openTermsModal"
          class="text-primary underline"
          >Terms and Conditions</a
        >
        and
        <a
          href="#"
          @click.prevent="openPrivacyPolicyModal"
          class="text-primary underline"
          >Privacy Policy</a
        >
      </label>
    </div>
    <p
      v-if="errors.terms"
      class="text-red-600 text-sm text-left mt-1"
    >
      {{ errors.terms }}
    </p>

    <div class="flex justify-end mt-4">
      <Button
        :text="'Continue'"
        @click="handleCreateAccount"
        class="w-full"
      />
    </div>

    <Modal
      :isOpen="isTermsModalOpen"
      title="Terms and Conditions"
      @close="isTermsModalOpen = false"
    >
      <p>{{ TERMS_AND_CONDITIONS }}</p>
    </Modal>

    <Modal
      :isOpen="isPrivacyPolicyModalOpen"
      title="Privacy Policy"
      @close="isPrivacyPolicyModalOpen = false"
    >
      <p>{{ PRIVACY_POLICY }}</p>
    </Modal>
  </div>
</template>
