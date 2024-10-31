<script setup lang="ts">
import { EMAIL_SUPPORT } from '@/helpers/constants';
</script>

<template>
  <footer class="text-center text-gray-500 py-4 max-lg:hidden absolute bottom-0">
    © 2023 Email Vault, Inc. All rights reserved |
    <a :href="`mailto:${EMAIL_SUPPORT}`" class="text-primary font-semibold">{{ EMAIL_SUPPORT }}</a>
  </footer>
</template>
