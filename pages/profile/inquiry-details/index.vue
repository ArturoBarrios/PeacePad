<script setup>
import { ref, computed } from 'vue'
import Lifestyle from '~/components/profilePreferences/lifestyle.vue'
import Housing from '~/components/profilePreferences/housing.vue'
import Noise from '~/components/profilePreferences/noise.vue'
import Priorities from '~/components/profilePreferences/priorities.vue'
import Locations from '~/components/profilePreferences/locations.vue'

const componentMap = {
  lifestyle: Lifestyle,
  housing: Housing,
  noise: Noise,
  priorities: Priorities,
  locations: Locations
}

const currentStep = ref(0)

const steps = [
  { title: 'Lifestyle', key: 'lifestyle' },
  { title: 'Housing', key: 'housing' },
  { title: 'Noise Sensitivity', key: 'noise' },
  { title: 'Priorities', key: 'priorities' },
  { title: 'Locations', key: 'locations' }
]

const isFirst = computed(() => currentStep.value === 0)
const isLast = computed(() => currentStep.value === steps.length - 1)

const goToStep = (index) => {
  currentStep.value = index
}

const nextStep = () => {
  if (!isLast.value) currentStep.value++
}

const prevStep = () => {
  if (!isFirst.value) currentStep.value--
}

const saveStep = () => {
  // TODO: Integrate with backend GraphQL mutation
  console.log(`Saving data for step: ${steps[currentStep.value].key}`)
}
</script>
<template>
  <div class="max-w-3xl mx-auto p-6 space-y-6">
    <!-- Top Action Bar: Close and Save (parallel, spread) -->
    <div class="flex justify-between items-center">
      <router-link
        to="/profile"
        class="bg-red-500 hover:bg-red-600 text-white text-sm font-medium px-6 py-2 rounded transition cursor-pointer"
      >
        Close
      </router-link>

      <button
        @click="saveStep"
        class="cursor-pointer px-6 py-2 rounded text-sm bg-green-500 text-white hover:bg-green-600"
      >
        Save
      </button>
    </div>

    <!-- Step Header Navigation -->
    <div class="flex flex-wrap justify-center gap-4 border-b pb-6">
      <template v-for="(step, index) in steps" :key="step.key">
        <button
          class="cursor-pointer text-sm sm:text-base font-medium focus:outline-none px-3 py-2 rounded transition"
          :class="{
            'text-blue-600 border-b-2 border-blue-600': currentStep === index,
            'text-gray-500 hover:text-blue-500': currentStep !== index
          }"
          @click="goToStep(index)"
        >
          {{ index + 1 }}. {{ step.title }}
        </button>
      </template>
    </div>

    <!-- Step Navigation Buttons: Back and Next (centered) -->
    <div class="flex justify-end gap-4">
      <button
        @click="prevStep"
        :disabled="isFirst"
        class="cursor-pointer px-6 py-2 rounded text-sm bg-gray-100 hover:bg-gray-200 disabled:opacity-50"
      >
        Back
      </button>

      <button
        @click="nextStep"
        :disabled="isLast"
        class="cursor-pointer px-6 py-2 rounded text-sm bg-blue-500 text-white hover:bg-blue-600 disabled:opacity-50"
      >
        Next
      </button>
    </div>

    <!-- Step Form Section -->
    <div class="min-h-[200px]">
      <component :is="componentMap[steps[currentStep].key]" />
    </div>
  </div>
</template>
