<template>
    <div>
      <h2 class="text-xl font-semibold mb-4">How sensitive are you to noise?</h2>
      <p class="text-sm text-gray-500 mb-6">We'll use this to filter out apartments that don't fit your quiet preferences.</p>
  
      <div class="space-y-4">
        <label class="block">
          <span class="text-gray-700">How much does street noise bother you?</span>
          <select class="mt-1 block w-full border px-4 py-2 rounded">
            <option disabled selected>Select one</option>
            <option>Not at all</option>
            <option>Somewhat</option>
            <option>Very much</option>
          </select>
        </label>
  
        <label class="block">
          <span class="text-gray-700">How much do neighbor sounds (TV, music, footsteps) bother you?</span>
          <select class="mt-1 block w-full border px-4 py-2 rounded">
            <option disabled selected>Select one</option>
            <option>Not at all</option>
            <option>Somewhat</option>
            <option>Very much</option>
          </select>
        </label>
      </div>
    </div>
  </template>
  