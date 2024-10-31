<!-- src/views/Billing/AddBillingView.vue -->
<script setup lang="ts">
import BillingForm from '@/components/forms/BillingForm.vue';
import IllustrationPanel from '@/components/layout/IllustrationPanel.vue';
import Footer from '@/components/layout/Footer.vue';
</script>

<template>
  <div class="flex flex-col lg:flex-row min-h-screen font-sans bg-background">
    <div class="flex flex-1 items-center justify-center bg-white p-8 relative">
      <BillingForm />
      <Footer />
    </div>

    <IllustrationPanel />
  </div>
</template>
