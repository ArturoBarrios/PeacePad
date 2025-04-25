<template>
  <div>
    <header class="flex justify-between items-center px-6 py-4 bg-white ">
      <h1 class="text-xl font-bold text-gray-800">PeacePad</h1>
      <div class="flex gap-4">
        <template v-if="user">
          <span class="text-gray-700">Hi, {{ user.name || user.email }}</span>
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
</template>

<script setup>
import { onNuxtReady } from '#app'
import { useRouter } from 'vue-router'
import { useRuntimeConfig } from '#imports'
import { useAuth } from '~/composables/useAuth'

const { user, fetchUser } = useAuth()
const config = useRuntimeConfig()
const router = useRouter()
const { server } = config.public

onNuxtReady(async () => {
  await fetchUser()
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
        `
      }
    })

    if (response?.data?.logout) {
      user.value = null
      router.push('/login')
    } else {
      console.error('Logout failed: server returned false')
    }
  } catch (err) {
    console.error('Logout error:', err)
  }
}
</script>
