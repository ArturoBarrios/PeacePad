<script setup lang="ts">
import { navigateTo } from '#app'

// Redirect to /profile immediately
navigateTo('/profile', { replace: true })
</script>

<template>
  <!-- Optional fallback if device/network is slow -->
  <div class="flex items-center justify-center min-h-screen">
    <p class="text-gray-500">Redirecting...</p>
  </div>
</template>
