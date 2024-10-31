<script setup lang="ts">
import SignInForm from '@/components/forms/SignInForm.vue';
import IllustrationPanel from '@/components/layout/IllustrationPanel.vue';
import Footer from '@/components/layout/Footer.vue';

</script>

<template>
  <div class="flex flex-col lg:flex-row h-screen font-sans bg-background">
    <div class="flex flex-1 items-center justify-center bg-white p-8 relative">
      <SignInForm />
      <Footer />
    </div>
    <IllustrationPanel />
  </div>
</template>