<script setup>
import { ref } from 'vue'

const email = ref('')
const isSending = ref(false)
const sent = ref(false)
const error = ref('')
const successMessage = ref('Thanks! We’ll keep you updated.')
const config = useRuntimeConfig()

const handleSubmit = async () => {
  error.value = ''

  if (!email.value.trim()) {
    error.value = 'Please enter an email address.'
    return
  }

  isSending.value = true
  const { server } = useRuntimeConfig().public
  console.log("server", server)
  try {
    const response = await $fetch('/graphql', {
  baseURL: server, // ✅ full baseURL overrides Nuxt default
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: {
    query: `
      mutation SendTestEmail($to: String!) {
        sendTestEmail(to: $to)
      }
    `,
    variables: { to: email.value },
  },
})


    if (response?.data?.sendTestEmail === true) {
      sent.value = true
    } else {
      error.value = 'Something went wrong. Please try again.'
    }
  } catch (err) {
    console.error('[Email Error]', err)
    error.value = 'Failed to send email. Try again later.'
  } finally {
    isSending.value = false
  }
}
</script>


<template>
  <section class="min-h-screen flex items-center justify-center bg-gray-50">
    <div class="max-w-2xl mx-auto text-center px-6 py-12">
      <h1 class="text-4xl font-bold tracking-tight text-gray-800 sm:text-5xl">
        Welcome to PeacePad
      </h1>
      <p class="mt-4 text-lg text-gray-600">
        Helping students and international renters find peaceful, noise-free homes.
      </p>

      <div v-if="!sent" class="mt-8 flex flex-col sm:flex-row gap-3 justify-center">
        <input
          type="email"
          v-model="email"
          :disabled="isSending"
          placeholder="Your email address"
          class="w-full sm:w-72 px-4 py-2 rounded-md border border-gray-300 shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none"
        />
        <button
          @click="handleSubmit"
          :disabled="isSending"
          class="px-6 py-2 rounded-md font-medium transition text-white cursor-pointer"
          :class="isSending ? 'bg-blue-400 cursor-wait' : 'bg-blue-600 hover:bg-blue-700'"
        >
          <span v-if="!isSending">Stay in Touch</span>
          <span v-else>Sending...</span>
        </button>
      </div>

      <div class="mt-4 h-5 text-sm">
  <transition name="fade">
    <p v-if="error" class="text-red-600">{{ error }}</p>
  </transition>
</div>



      <transition name="fade">
        <div v-if="sent" class="mt-10 p-4 rounded-md bg-green-100 text-green-700 font-medium">
          {{ successMessage }}
        </div>
      </transition>

    </div>
  </section>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
