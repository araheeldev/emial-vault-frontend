<!-- src/views/CreateAccountView.vue -->
<script setup lang="ts">
import { ref } from 'vue';
import CreateAccountForm from '@/components/forms/CreateAccountForm.vue';
import BillingForm from '@/components/forms/BillingForm.vue';
import { useAuthStore } from '@/stores/authStore';
import IllustrationPanel from '@/components/layout/IllustrationPanel.vue';
import Footer from '@/components/layout/Footer.vue';

// State to track form sections
const showBillingForm = ref(false);

// Initialize the auth store
const authStore = useAuthStore();

// User Info Data
const firstName = ref('');
const lastName = ref('');
const companyName = ref('');
const email = ref('');
const password = ref('');
const referralCode = ref('');
const isTermsChecked = ref(false);

// Billing Info Data
const cardName = ref('');
const cardNumber = ref('');
const expirationDate = ref('');
const cvv = ref('');

// Error handling could also be added here if necessary for form validation
const errors = ref({});

// Proceed to Billing form
function proceedToBilling() {
  showBillingForm.value = true;
}

// Go back to Account form
function goBackToAccountForm() {
  showBillingForm.value = false;
}

// Handle account creation
function handleCreateAccount() {
  authStore.createAccount({
    // User Info
    firstName: firstName.value,
    lastName: lastName.value,
    companyName: companyName.value,
    email: email.value,
    password: password.value,
    referralCode: referralCode.value,
    isTermsChecked: isTermsChecked.value,
    // Billing Info
    cardName: cardName.value,
    cardNumber: cardNumber.value,
    expirationDate: expirationDate.value,
    cvv: cvv.value,
  });
}
</script>

<template>
  <div class="flex flex-col lg:flex-row min-h-screen font-sans bg-background">
    <div class="flex flex-1 items-center justify-center bg-white p-8 relative">
      <template v-if="!showBillingForm">
        <!-- Pass user data as props to CreateAccountForm -->
        <CreateAccountForm
          :first-name="firstName"
          :last-name="lastName"
          :company-name="companyName"
          :email="email"
          :password="password"
          :referral-code="referralCode"
          :is-terms-checked="isTermsChecked"
          @update:firstName="firstName = $event"
          @update:lastName="lastName = $event"
          @update:companyName="companyName = $event"
          @update:email="email = $event"
          @update:password="password = $event"
          @update:referralCode="referralCode = $event"
          @update:isTermsChecked="isTermsChecked = $event"
          @proceedToBilling="proceedToBilling"
        />
      </template>

      <template v-else>
        <!-- Pass billing data as props to BillingForm -->
        <BillingForm
          :card-name="cardName"
          :card-number="cardNumber"
          :expiration-date="expirationDate"
          :cvv="cvv"
          @update:cardName="cardName = $event"
          @update:cardNumber="cardNumber = $event"
          @update:expirationDate="expirationDate = $event"
          @update:cvv="cvv = $event"
          @backToAccountForm="goBackToAccountForm"
          @signUp="handleCreateAccount"
        />
      </template>

      <Footer />
    </div>
    <IllustrationPanel />
  </div>
</template>
