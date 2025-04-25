<script setup>
import { ref, computed, watch } from 'vue'
import { useRouter } from 'vue-router'

const props = defineProps({
  profiles: {
    type: Array,
    required: true
  }
})

const router = useRouter()

const currentProfileIndex = ref(0)

const currentProfile = computed(() => props.profiles[currentProfileIndex.value])

const handleProfileChange = (event) => {
  const selected = parseInt(event.target.value)
  if (selected === -1) {
    router.push('/profile/inquiry-details')
  } else {
    currentProfileIndex.value = selected
  }
}
</script>

<template>
  <div>
    <div class="flex justify-between items-center mb-8">
      <h1 class="text-2xl font-bold text-gray-800">Your Peace Profile</h1>

      <select
        class="px-4 py-2 border rounded-md text-sm bg-white"
        @change="handleProfileChange"
        :value="currentProfileIndex"
      >
        <option v-for="(profile, index) in profiles" :key="profile.id" :value="index">
          {{ profile.name }}
        </option>
        <option :value="-1">➕ Create New Profile</option>
      </select>
    </div>

    <div class="rounded-md p-6 bg-white shadow-sm">
      <h2 class="text-xl font-semibold text-gray-700 mb-2">{{ currentProfile.name }}</h2>
      <p class="text-sm text-gray-500">View or update the preferences and matches for this housing profile.</p>

      <NuxtLink
        to="/profile/inquiry-details"
        class="inline-block mt-4 px-4 py-2 bg-blue-600 text-white text-sm rounded hover:bg-blue-700 transition"
      >
        View Details
      </NuxtLink>
    </div>
  </div>
</template>
