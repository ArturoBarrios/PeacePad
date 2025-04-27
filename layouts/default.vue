<template>
  <div v-if="auth.isReady" class="min-h-screen flex flex-col">
    <header class="flex justify-between items-center px-6 py-4 bg-white">
      <h1 class="text-xl font-bold text-gray-800">PeacePad</h1>
      <div class="flex gap-4">
        <template v-if="auth.user">
          <span class="text-gray-700">Hi, {{ auth.user.name || auth.user.email }}</span>
          <button @click="handleLogout" class="text-sm text-red-600 hover:underline cursor-pointer">
            Logout
          </button>
        </template>
        <template v-else>
          <NuxtLink to="/signup" class="text-sm text-blue-600 hover:underline cursor-pointer">Sign Up</NuxtLink>
          <NuxtLink to="/login" class="text-sm text-blue-600 hover:underline cursor-pointer">Login</NuxtLink>
        </template>
      </div>
    </header>

    <NuxtPage />
  </div>

  <div v-else class="flex items-center justify-center min-h-screen">
    <p class="text-gray-500">Loading...</p>
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import { useAuthStore } from '~/stores/auth'
import { useRouter } from 'vue-router'
import { useRuntimeConfig } from '#imports'

const auth = useAuthStore()
const router = useRouter()
const config = useRuntimeConfig()
const { server } = config.public

onMounted(async () => {
  await auth.fetchUser()
})

const handleLogout = async () => {
  try {
    const response = await $fetch('/graphql', {
      baseURL: server,
      method: 'POST',
      credentials: 'include',
      headers: { 'Content-Type': 'application/json' },
      body: {
        query: `
          mutation {
            logout
          }
        `,
      },
    })

    if (response?.data?.logout) {
      auth.logout()
      router.push('/login')
    } else {
      console.error('Logout failed: server returned false')
    }
  } catch (err) {
    console.error('Logout error:', err)
  }
}
</script>
