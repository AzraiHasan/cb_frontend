<!-- pages/admin/index.vue -->
<template>
  <div class="min-h-screen bg-gray-50 p-4">
    <UContainer>
      <div class="mb-6">
        <h1 class="text-2xl font-bold text-gray-900">Admin Dashboard</h1>
        <p class="text-gray-600">Overview of banking operations and metrics</p>
      </div>

      <!-- Quick Stats -->
      <div class="grid md:grid-cols-3 gap-6 mb-6">
        <UCard>
          <div class="text-center">
            <div class="text-sm font-medium text-gray-600">Total Customers</div>
            <div class="text-3xl font-bold text-primary mt-1">2,451</div>
            <div class="text-sm text-gray-500 mt-1">+12% from last month</div>
          </div>
        </UCard>

        <UCard>
          <div class="text-center">
            <div class="text-sm font-medium text-gray-600">Active Accounts</div>
            <div class="text-3xl font-bold text-primary mt-1">3,642</div>
            <div class="text-sm text-gray-500 mt-1">+8% from last month</div>
          </div>
        </UCard>

        <UCard>
          <div class="text-center">
            <div class="text-sm font-medium text-gray-600">Daily Transactions</div>
            <div class="text-3xl font-bold text-primary mt-1">1,287</div>
            <div class="text-sm text-gray-500 mt-1">+15% from yesterday</div>
          </div>
        </UCard>
      </div>

      <!-- Main Actions -->
      <div class="grid md:grid-cols-2 gap-6">
        <UCard>
          <template #header>
            <div class="flex items-center justify-between">
              <h2 class="text-lg font-semibold">Customer Management</h2>
              <UBadge color="primary">Priority</UBadge>
            </div>
          </template>

          <div class="space-y-4">
            <p class="text-gray-600">View and manage customer accounts, verify KYC documents, and handle customer
              requests.</p>
            <UButton to="/admin/customers" color="primary" block>
              View Customers
            </UButton>
          </div>
        </UCard>

        <UCard>
          <template #header>
            <div class="flex items-center justify-between">
              <h2 class="text-lg font-semibold">Transaction Monitoring</h2>
              <UBadge color="orange">Active</UBadge>
            </div>
          </template>

          <div class="space-y-4">
            <p class="text-gray-600">Monitor real-time transactions, review flagged activities, and generate reports.
            </p>
            <UButton to="/admin/transactions" color="primary" block>
              View Transactions
            </UButton>
          </div>
        </UCard>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
/* definePageMeta({
  layout: 'banking',
  middleware: ['admin']
}) */
</script>