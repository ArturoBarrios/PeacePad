<template>
    <div class="p-6 space-y-6">
      <div class="flex justify-between items-center">
        <h1 class="text-2xl font-bold mb-4">Create Payment</h1>
        <NuxtLink
          to="/admin/dashboard"
          class="bg-red-500 text-white px-4 py-2 rounded cursor-pointer hover:bg-red-800"
        >
          Close
        </NuxtLink>
      </div>
  
      <!-- Select User -->
      <div>
        <label class="block font-semibold mb-2">Select User</label>
        <select v-model="selectedUserId" class="border p-2 rounded w-full">
          <option disabled value="">Select a user</option>
          <option v-for="user in users" :key="user.id" :value="user.id">
            {{ user.email }}
          </option>
        </select>
      </div>
  
      <!-- Profiles -->
      <div v-if="selectedUserProfiles.length" class="space-y-4">
        <h2 class="text-xl font-semibold mt-6">Profiles</h2>
        <div v-for="profile in selectedUserProfiles" :key="profile.id" class="border p-4 rounded shadow cursor-pointer hover:bg-gray-50" @click="selectProfile(profile)">
          <h3 class="font-bold">{{ profile.name }}</h3>
          <p>Address: <a href="#" class="text-blue-500 hover:underline">{{ profile.address }}</a></p>
        </div>
      </div>
  
      <!-- Payment Form -->
      <div v-if="selectedProfile" class="space-y-4 mt-6">
        <h2 class="text-xl font-semibold">Payment Details</h2>
  
        <div>
          <label class="block mb-1">Price</label>
          <input v-model="paymentForm.price" type="text" class="border p-2 rounded w-full" />
        </div>
  
        <div>
          <label class="block mb-1">Currency</label>
          <select v-model="paymentForm.currency" class="border p-2 rounded w-full">
            <option value="USD">USD</option>
            <option value="EUR">EUR</option>
          </select>
        </div>
  
        <div>
          <label class="block mb-1">Status</label>
          <select v-model="paymentForm.status" class="border p-2 rounded w-full">
            <option value="pending">Pending</option>
            <option value="paid">Paid</option>
            <option value="refunded">Refunded</option>
          </select>
        </div>
  
        <div>
          <label class="block mb-1">Payment Type</label>
          <select v-model="paymentForm.type" class="border p-2 rounded w-full">
            <option value="service">Service</option>
            <option value="refund">Refund</option>
            <option value="deposit">Deposit</option>
          </select>
        </div>
  
        <button class="bg-green-500 text-white px-6 py-2 rounded cursor-pointer hover:bg-green-600" @click="savePayment">
          Save Payment
        </button>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  definePageMeta({
    layout: 'admin',
  })
  
  import { ref, computed } from 'vue'
  
  // Dummy users
  const users = ref([
    { id: 1, email: 'john@example.com' },
    { id: 2, email: 'jane@example.com' },
  ])
  
  // Dummy profiles per user
  const profilesByUserId: Record<number, { id: string; name: string; address: string }[]> = {
    1: [
      { id: 'p1', name: 'Moving to College', address: '123 College Ave' },
      { id: 'p2', name: 'First Apartment', address: '456 City Blvd' },
    ],
    2: [
      { id: 'p3', name: 'Relocation', address: '789 Suburb Lane' },
    ],
  }
  
  const selectedUserId = ref('')
  const selectedProfile = ref<null | { id: string; name: string; address: string }>(null)
  
  const selectedUserProfiles = computed(() => {
    if (!selectedUserId.value) return []
    return profilesByUserId[Number(selectedUserId.value)] || []
  })
  
  const paymentForm = ref({
    price: '',
    currency: 'USD',
    status: 'pending',
    type: 'service',
  })
  
  function selectProfile(profile: { id: string; name: string; address: string }) {
    selectedProfile.value = profile
  }
  
  function savePayment() {
    if (!selectedProfile.value) {
      alert('Please select a profile')
      return
    }
  
    console.log('Saving Payment:', {
      userId: selectedUserId.value,
      profileId: selectedProfile.value.id,
      ...paymentForm.value,
    })
  
    // TODO: Hook into real API
    alert('Payment saved (mock)')
  }
  </script>
  