<template>
    <div>
      <h2 class="text-xl font-semibold mb-4">Tell us about your lifestyle</h2>
      <p class="text-sm text-gray-500 mb-6">This helps us match your daily rhythm to the right home.</p>
  
      <div class="space-y-4">
        <label class="block">
          <span class="text-gray-700">What time do you usually sleep?</span>
          <input type="text" class="mt-1 block w-full border px-4 py-2 rounded" placeholder="e.g. 10:00 PM" />
        </label>
  
        <label class="block">
          <span class="text-gray-700">Do you work or study from home?</span>
          <select class="mt-1 block w-full border px-4 py-2 rounded">
            <option disabled selected>Select an option</option>
            <option>Yes</option>
            <option>Sometimes</option>
            <option>No</option>
          </select>
        </label>
      </div>
    </div>
  </template>
  