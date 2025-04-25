<script setup>
import { ref, onMounted } from 'vue'

const { $gm } = useNuxtApp()
const mapRef = ref(null)
const searchInput = ref(null)
let map, marker

onMounted(async () => {
  await $gm.load()

  map = new window.google.maps.Map(mapRef.value, {
    center: { lat: 40.749933, lng: -73.98633 },
    zoom: 13
  })

  // Create a marker but keep it hidden for now
  marker = new window.google.maps.Marker({
    map,
    visible: false
  })

  const autocomplete = new window.google.maps.places.Autocomplete(searchInput.value, {
    fields: ['formatted_address', 'geometry', 'name']
  })

  autocomplete.addListener('place_changed', () => {
    const place = autocomplete.getPlace()

    if (!place.geometry || !place.geometry.location) {
      alert('No location data found for that place.')
      return
    }

    // Center the map and show the marker
    map.panTo(place.geometry.location)
    map.setZoom(15)

    marker.setPosition(place.geometry.location)
    marker.setVisible(true)
  })
})
</script>

<template>
  <div class="max-w-5xl mx-auto p-6 space-y-4">
    <input
      ref="searchInput"
      type="text"
      placeholder="Search for an address"
      class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
    />

    <div ref="mapRef" id="map" class="w-full h-[500px] rounded shadow border"></div>
  </div>
</template>
