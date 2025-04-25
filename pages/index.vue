<script setup>
import { ref, onMounted } from 'vue'
import { useAuth } from '~/composables/useAuth'
import HomeLanding from '~/components/HomeLanding.vue'
import HomeDashboard from '~/components/HomeDashboard.vue'

const { user, fetchUser } = useAuth()
const isLoading = ref(true)

onMounted(async () => {
  await fetchUser()
  isLoading.value = false
})
</script>

<template>
  <div>
    <LoadingScreen v-if="isLoading" />
    <component v-else :is="user ? HomeDashboard : HomeLanding" />
  </div>
</template>
