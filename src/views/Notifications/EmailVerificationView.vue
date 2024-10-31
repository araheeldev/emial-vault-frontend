<!-- src/views/EmailVerificationView.vue -->
<script setup lang="ts">
import IllustrationPanel from '@/components/layout/IllustrationPanel.vue';
import Footer from '@/components/layout/Footer.vue';

const emailAddress = "sarah.davis@nextgenlabs.com"; // Sample email address, replace with actual data if available

function resendEmail() {
    console.log("Resend email clicked");
}
</script>

<template>
    <div class="flex flex-col lg:flex-row h-screen font-sans bg-background">
        <!-- Left side form container -->
        <div class="flex flex-1 items-center justify-center bg-white p-8 relative">
            <div class="w-full max-w-md text-center text-textPrimary">
                <h1 class="text-3xl font-medium mb-4 text-start">Email Verification</h1>
                <p class="text-textSecondary text-start mb-4">
                    We're almost set! A confirmation email was sent to
                    <span class="font-semibold text-primary">“{{ emailAddress }}”</span>.
                </p>
                <p class="text-textSecondary text-start mb-6">
                    To activate your account, just click on the link in the email. If you can't locate it, please have a
                    look in your spam folder.
                </p>

                <hr class="bg-gray-300 h-px my-4">

                <div class="mb-3">
                    <p class="text-sm">
                        Haven't received any email?
                        <button @click="resendEmail" class="text-primary underline font-semibold">Resend Email</button>
                    </p>
                </div>

                <p class="text-sm text-textSecondary mt-6">
                    Did you enter the wrong email address? Please reach out to us for assistance at
                    <a href="mailto:support@emailvault.app"
                        class="text-primary font-semibold">support@emailvault.app</a>
                </p>

                <Footer />
            </div>
        </div>

        <IllustrationPanel />
    </div>
</template>
