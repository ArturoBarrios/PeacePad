<template>
  <div class="p-4 space-y-6">
    <div class="flex justify-between items-center">
      <h1 class="text-2xl font-bold">Admin Dashboard</h1>

      <!-- Buttons container -->
      <div class="flex gap-2">
        <NuxtLink
          to="/"
          class="bg-gray-500 text-white px-4 py-2 rounded cursor-pointer hover:bg-gray-700"
        >
          Go Home
        </NuxtLink>

        <NuxtLink
          to="/admin/create-payment"
          class="bg-blue-500 text-white px-4 py-2 rounded cursor-pointer hover:bg-blue-600"
        >
          Create Payment
        </NuxtLink>
      </div>
    </div>

    <div class="overflow-x-auto">
      <table class="min-w-full border border-gray-300">
        <thead class="bg-gray-100">
          <tr>
            <th class="p-2 text-left border-b">User</th>
            <th class="p-2 text-left border-b">Amount</th>
            <th class="p-2 text-left border-b">Due Date</th>
            <th class="p-2 text-left border-b">Profile</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="payment in payments" :key="payment.id" class="hover:bg-gray-50">
            <td class="p-2 border-b">
              <a :href="`/admin/users/${payment.userId}`" class="text-blue-500 hover:underline">
                {{ payment.userName }}
              </a>
            </td>
            <td class="p-2 border-b">
              ${{ payment.amount.toFixed(2) }}
            </td>
            <td class="p-2 border-b">
              {{ payment.dueDate }}
            </td>
            <td class="p-2 border-b">
              {{ payment.profile }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  layout: 'admin',
})

// Mocked data for now — replace with real query later
const payments = ref([
  {
    id: 1,
    userId: 123,
    userName: 'John Doe',
    amount: 120.5,
    dueDate: '2025-05-01',
    profile: 'Standard',
  },
  {
    id: 2,
    userId: 456,
    userName: 'Jane Smith',
    amount: 89.99,
    dueDate: '2025-06-15',
    profile: 'Premium',
  },
])
</script>
