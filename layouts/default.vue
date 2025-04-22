<template>
    <div>
      <header class="flex justify-between items-center px-6 py-4 bg-white shadow-md">
        <h1 class="text-xl font-bold text-gray-800">PeacePad</h1>
        <div class="flex gap-4">
          <template v-if="user">
            <span class="text-gray-700">Hi, {{ user.name || user.email }}</span>
            <button @click="logout" class="text-sm text-red-600 hover:underline cursor-pointer">
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
  import { useAuth } from '~/composables/useAuth'
  
  const { user, fetchUser } = useAuth()
  
  onNuxtReady(() => {
    fetchUser()
  })
  
  const logout = () => {
    document.cookie = 'token=; Max-Age=0; path=/'
    user.value = null
  }
  </script>
  
  