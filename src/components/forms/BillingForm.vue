<!-- src/components/BillingForm.vue -->
<script setup lang="ts">
import { defineProps, defineEmits, ref, watch } from 'vue';
import InputField from '@/components/inputs/InputField.vue';
import Button from '@/components/ui/Button.vue';
import { validateBillingForm } from '@/helpers/formValidation';

// Define props to receive initial values
const props = defineProps({
  cardName: String,
  cardNumber: String,
  expirationDate: String,
  cvv: String,
});

// Define emit events for updating parent and navigation
const emit = defineEmits([
  'update:cardName',
  'update:cardNumber',
  'update:expirationDate',
  'update:cvv',
  'backToAccountForm',
  'signUp',
]);

// Local refs to manage form inputs
const cardName = ref(props.cardName);
const cardNumber = ref(props.cardNumber);
const expirationDate = ref(props.expirationDate);
const cvv = ref(props.cvv);

// Watch for changes in local refs and emit updates to parent
watch(cardName, (val) => emit('update:cardName', val));
watch(cardNumber, (val) => emit('update:cardNumber', val));
watch(expirationDate, (val) => emit('update:expirationDate', val));
watch(cvv, (val) => emit('update:cvv', val));

// Error messages for each field
const cardNameError = ref('');
const cardNumberError = ref('');
const expirationDateError = ref('');
const cvvError = ref('');

// Handle form submission for signing up
function handleSignUp() {
  const isValid = validateBillingForm({
    cardName,
    cardNumber,
    expirationDate,
    cvv,
    cardNameError,
    cardNumberError,
    expirationDateError,
    cvvError,
  });

  if (isValid) {
    emit('signUp');
  }
}

// Navigate back to account form
function handleBack() {
  emit('backToAccountForm');
}
</script>

<template>
  <div class="w-full max-w-md text-center text-textPrimary">
    <h1 class="text-3xl font-bold mb-4 text-start">Add Billing Method</h1>
    <p class="text-textSecondary text-start mb-4">
      Please enter your billing details below
    </p>

    <!-- Billing Details Input Fields with Error Handling -->
    <InputField
      label="Name on card *"
      placeholder="Enter name on card"
      type="text"
      v-model="cardName"
      :error="cardNameError"
    />

    <div class="relative">
      <InputField
        label="Card number *"
        placeholder="1234 5678 9101 2314"
        type="text"
        v-model="cardNumber"
        :error="cardNumberError"
      />
    </div>

    <div class="flex space-x-4">
      <InputField
        label="Expiration date *"
        placeholder="MM/YY"
        type="text"
        v-model="expirationDate"
        :error="expirationDateError"
        class="flex-1"
      />
      <InputField
        label="CVV *"
        placeholder="CVV"
        type="text"
        v-model="cvv"
        :error="cvvError"
        class="flex-1"
      />
    </div>

    <!-- Info Text -->
    <div class="flex items-baseline text-left text-sm text-textSecondary my-4">
      <span
        class="flex justify-center items-center min-w-5 min-h-5 bg-primary text-white rounded-full mr-2"
        >i</span
      >
      <p>
        Your credit card will not be charged for 30 days. Cancel anytime in the
        next 30 days free of charge.
      </p>
    </div>

    <!-- Back and Sign Up Buttons -->
    <div class="flex justify-end mt-6">
      <Button
        :text="'Back'"
        @click="handleBack"
        customClass="lg:w-1/3 text-black mr-2 bg-transparent border border-gray-300"
        :isTransparent="true"
      />
      <Button
        :text="'Sign Up'"
        @click="handleSignUp"
        customClass="lg:w-1/3 ml-2"
      />
    </div>
  </div>
</template>

<style scoped>
/* Add any custom styling here if needed */
</style>
