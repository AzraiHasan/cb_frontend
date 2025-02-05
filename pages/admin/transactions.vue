<!-- pages/admin/transactions.vue -->
<template>
  <div class="min-h-screen bg-gray-50 p-4">
    <UContainer>
      <div class="flex justify-between items-center mb-6">
        <div>
          <h1 class="text-2xl font-bold text-gray-900">Transaction Monitor</h1>
          <p class="text-gray-600">Monitor and manage all banking transactions</p>
        </div>
      </div>

      <!-- Filters -->
      <UCard class="mb-6">
        <div class="grid md:grid-cols-4 gap-4">
          <UInput v-model="search" icon="i-heroicons-magnifying-glass" placeholder="Search transactions..." />
          <USelect v-model="typeFilter" :options="transactionTypes" placeholder="Transaction type" />
          <USelect v-model="statusFilter" :options="statusOptions" placeholder="Status" />
          <UDatePicker v-model="dateFilter" mode="range" placeholder="Select date range" />
        </div>
      </UCard>

      <!-- Transactions Table -->
      <UCard>
        <UTable :rows="transactions" :columns="columns" :sort="sort" @update:sort="sort = $event">
          <template #amount-data="{ row }">
            <span :class="row.type === 'credit' ? 'text-green-600' : 'text-red-600'">
              {{ formatCurrency(row.amount) }}
            </span>
          </template>

          <template #status-data="{ row }">
            <UBadge :color="getStatusColor(row.status)">
              {{ row.status }}
            </UBadge>
          </template>

          <template #actions-data="{ row }">
            <UDropdown :items="getActionItems(row)">
              <UButton color="gray" variant="ghost" icon="i-heroicons-ellipsis-vertical" />
            </UDropdown>
          </template>
        </UTable>

        <!-- Pagination -->
        <div class="flex justify-between items-center mt-4">
          <p class="text-sm text-gray-600">
            Showing {{ startIndex }} to {{ endIndex }} of {{ totalTransactions }} transactions
          </p>
          <UPagination v-model="currentPage" :total="totalPages" :ui="{ rounded: 'rounded-lg' }" />
        </div>
      </UCard>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
import type { BadgeColor } from '#ui/types'

/* definePageMeta({
  layout: 'admin',
  middleware: ['admin']
}) */

// Table configuration
const columns = [
  { key: 'id', label: 'ID', sortable: true },
  { key: 'timestamp', label: 'Date & Time', sortable: true },
  { key: 'type', label: 'Type' },
  { key: 'amount', label: 'Amount', sortable: true },
  { key: 'accountId', label: 'Account ID' },
  { key: 'status', label: 'Status' },
  { key: 'actions', label: '' }
]

// Filters
const search = ref('')
const typeFilter = ref('')
const statusFilter = ref('')
const dateFilter = ref()

const transactionTypes = [
  { label: 'Credit', value: 'credit' },
  { label: 'Debit', value: 'debit' },
  { label: 'Transfer', value: 'transfer' }
]

const statusOptions = [
  { label: 'Completed', value: 'completed' },
  { label: 'Pending', value: 'pending' },
  { label: 'Failed', value: 'failed' }
]

// Mock data - replace with API calls
const transactions = ref([
  {
    id: 'TXN001',
    timestamp: '2024-02-05 14:30',
    type: 'credit',
    amount: 1500.00,
    accountId: 'ACC123',
    status: 'completed'
  },
  // Add more mock data as needed
])

// Sorting
const sort = ref({
  column: 'timestamp',
  direction: 'desc' as const
})

// Pagination
const currentPage = ref(1)
const itemsPerPage = 10
const totalTransactions = computed(() => transactions.value.length)
const totalPages = computed(() => Math.ceil(totalTransactions.value / itemsPerPage))
const startIndex = computed(() => (currentPage.value - 1) * itemsPerPage + 1)
const endIndex = computed(() => Math.min(currentPage.value * itemsPerPage, totalTransactions.value))

// Utility functions
const formatCurrency = (value: number) => {
  return value.toLocaleString('en-MY', {
    style: 'currency',
    currency: 'MYR'
  })
}

const getStatusColor = (status: string): BadgeColor => {
  const colors: Record<string, BadgeColor> = {
    completed: 'primary',
    pending: 'amber',
    failed: 'red'
  }
  return colors[status] || 'gray'
}

const getActionItems = (transaction: any): any[] => [[
  {
    label: 'View Details',
    icon: 'i-heroicons-eye',
    click: () => viewTransaction(transaction.id)
  },
  {
    label: 'Flag Transaction',
    icon: 'i-heroicons-flag',
    click: () => flagTransaction(transaction.id)
  },
  {
    label: 'Download Receipt',
    icon: 'i-heroicons-document-arrow-down',
    click: () => downloadReceipt(transaction.id)
  }
]]

// Action handlers
const viewTransaction = (id: string) => {
  // Implement view logic
}

const flagTransaction = (id: string) => {
  // Implement flag logic
}

const downloadReceipt = (id: string) => {
  // Implement download logic
}
</script>