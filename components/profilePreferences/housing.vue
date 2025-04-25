<template>
    <div>
      <h2 class="text-xl font-semibold mb-4">Tell us about your housing needs</h2>
      <p class="text-sm text-gray-500 mb-6">Help us understand the type of place you're looking for.</p>
  
      <div class="space-y-4">
        <label class="block">
          <span class="text-gray-700">Ideal monthly budget</span>
          <input type="number" class="mt-1 block w-full border px-4 py-2 rounded" placeholder="e.g. 900" />
        </label>
  
        <label class="block">
          <span class="text-gray-700">Move-in date</span>
          <input type="date" class="mt-1 block w-full border px-4 py-2 rounded" />
        </label>
  
        <label class="block">
          <span class="text-gray-700">Private bathroom required?</span>
          <select class="mt-1 block w-full border px-4 py-2 rounded">
            <option disabled selected>Select one</option>
            <option>Yes</option>
            <option>Preferably</option>
            <option>No</option>
          </select>
        </label>
      </div>
    </div>
  </template>
  