<!-- src/views/EmailSentView.vue -->
<script setup lang="ts">
import IllustrationPanel from '@/components/layout/IllustrationPanel.vue';
import Footer from '@/components/layout/Footer.vue';

const emailAddress = "sarah.davis@nextgenlabs.com"; // Placeholder email address

function resendEmail() {
    console.log("Resend email clicked");
    // Logic to resend the password reset email
}
</script>

<template>
  <div class="flex flex-col lg:flex-row h-screen font-sans bg-background">
    <div class="flex flex-1 items-center justify-center bg-white p-8 relative">
      <div class="w-full max-w-md text-center text-textPrimary">
        <h1 class="text-3xl font-medium mb-4 text-start">Email Sent</h1>
        <p class="text-textSecondary text-start mb-4">
          A password reset link has been sent to
          <span class="font-semibold text-primary">“{{ emailAddress }}”</span>.
        </p>
        <p class="text-textSecondary text-start mb-6">
          Just click on the link in that email to complete the password reset process.
        </p>

        <hr class="bg-gray-300 h-px my-4">

        <div class="mb-3">
          <p class="text-sm">
            Haven't received any email?
            <button @click="resendEmail" class="text-primary underline font-semibold">Resend Email</button>
          </p>
        </div>

        <Footer />
      </div>
    </div>

    <IllustrationPanel />
  </div>
</template>
