<script setup>
import { ref, computed } from 'vue'
import NoProfiles from '~/components/profile/NoProfiles.vue'
import ProfileDashboard from '~/components/profile/ProfileDashboard.vue'

// Source of truth for profiles
const profiles = ref([]) // ← Set this to [] to test empty state

const hasProfiles = computed(() => profiles.value.length > 0)
</script>

<template>
  <div class="max-w-4xl mx-auto px-6 py-10">
    <component
      :is="hasProfiles ? ProfileDashboard : NoProfiles"
      :profiles="profiles"
    />
  </div>
</template>
