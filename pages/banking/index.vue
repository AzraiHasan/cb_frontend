<!-- pages/banking/index.vue -->
<template>
  <div class="min-h-screen bg-gray-50 p-4">
    <UContainer>
      <div class="mb-6">
        <h1 class="text-2xl font-bold text-gray-900">Welcome Back, {{ userName }}</h1>
        <p class="text-gray-600">Manage your accounts and transactions</p>
      </div>

      <!-- Account Summary -->
      <UCard class="mb-6">
        <template #header>
          <div class="flex items-center justify-between">
            <h2 class="text-lg font-semibold">Main Account</h2>
            <UBadge color="primary">Active</UBadge>
          </div>
        </template>

        <div class="space-y-4">
          <div class="flex justify-between items-center">
            <span class="text-gray-600">Available Balance</span>
            <span class="text-2xl font-bold">RM {{ formatCurrency(balance) }}</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-600">Account Number</span>
            <span class="font-medium">{{ accountNumber }}</span>
          </div>
        </div>

        <template #footer>
          <div class="flex gap-4">
            <UButton to="/banking/transactions" color="primary">View Transactions</UButton>
            <UButton to="/banking/accounts" variant="ghost">Manage Accounts</UButton>
          </div>
        </template>
      </UCard>

      <!-- Quick Actions -->
      <div class="grid md:grid-cols-3 gap-6">
        <UCard class="text-center hover:shadow-lg transition-shadow">
          <div class="p-4">
            <div class="mb-4">
              <UIcon name="i-heroicons-arrow-up-on-square" class="text-4xl text-primary" />
            </div>
            <h3 class="font-semibold mb-2">Deposit</h3>
            <p class="text-sm text-gray-600 mb-4">Make a deposit to your account</p>
            <UButton size="sm" color="primary" block>Deposit Funds</UButton>
          </div>
        </UCard>

        <UCard class="text-center hover:shadow-lg transition-shadow">
          <div class="p-4">
            <div class="mb-4">
              <UIcon name="i-heroicons-arrow-path" class="text-4xl text-orange-500" />
            </div>
            <h3 class="font-semibold mb-2">Transfer</h3>
            <p class="text-sm text-gray-600 mb-4">Transfer between accounts</p>
            <UButton size="sm" color="primary" block>Transfer Money</UButton>
          </div>
        </UCard>

        <UCard class="text-center hover:shadow-lg transition-shadow">
          <div class="p-4">
            <div class="mb-4">
              <UIcon name="i-heroicons-document-text" class="text-4xl text-green-500" />
            </div>
            <h3 class="font-semibold mb-2">Statements</h3>
            <p class="text-sm text-gray-600 mb-4">View monthly statements</p>
            <UButton size="sm" color="primary" block>View Statements</UButton>
          </div>
        </UCard>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
/* definePageMeta({
  layout: 'banking',
  middleware: ['auth']
}) */

// Mock data - replace with actual data from your store/API
const userName = ref('John Doe')
const balance = ref(25000.50)
const accountNumber = ref('1234-5678-9012-3456')

const formatCurrency = (value: number) => {
  return value.toLocaleString('en-MY', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  })
}
</script>