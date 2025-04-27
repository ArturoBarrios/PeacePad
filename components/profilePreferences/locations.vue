<script setup>
import { ref } from 'vue'
import GoogleMap from '~/components/GoogleMap.vue'

const locations = ref([])
const latestPlace = ref(null)

const handlePlaceSelected = (place) => {
  latestPlace.value = place
}

const addLocation = () => {
  if (latestPlace.value) {
    locations.value.push(latestPlace.value)
    latestPlace.value = null
  }
}
</script>

<template>
  <div class="max-w-7xl mx-auto p-6">
    <div class="flex flex-col lg:flex-row gap-8">
      <!-- Left: Map Section -->
      <div class="flex-1">
        <GoogleMap @place-selected="handlePlaceSelected" />

        <button
          @click="addLocation"
          :disabled="!latestPlace"
          class="w-full mt-4 px-4 py-2 rounded font-medium transition text-white
                 disabled:bg-gray-400 bg-blue-600 hover:bg-blue-700 disabled:cursor-not-allowed"
        >
          Add Location
        </button>
      </div>

      <!-- Right: Saved Locations List -->
      <div class="flex-1">
        <h2 class="text-2xl font-bold mb-4">Saved Locations</h2>

        <div v-if="locations.length" class="space-y-4">
          <div
            v-for="(loc, index) in locations"
            :key="index"
            class="p-4 bg-white rounded shadow flex flex-col"
          >
            <div class="font-semibold">{{ loc.name }}</div>
            <div class="text-sm text-gray-500">{{ loc.address }}</div>
            <div class="text-sm text-gray-400 mt-1">
              ({{ loc.lat.toFixed(4) }}, {{ loc.lng.toFixed(4) }})
            </div>
          </div>
        </div>

        <p v-else class="text-gray-500">No locations added yet.</p>
      </div>
    </div>
  </div>
</template>
