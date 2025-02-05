<!-- pages/banking/transactions.vue -->
<template>
  <div class="min-h-screen bg-gray-50 p-4">
    <UContainer>
      <div class="mb-6">
        <h1 class="text-2xl font-bold text-gray-900">Transaction History</h1>
        <p class="text-gray-600">View and track your transactions</p>
      </div>

      <!-- Filters -->
      <UCard class="mb-6">
        <div class="grid md:grid-cols-3 gap-4">
          <USelect v-model="selectedAccount" :options="accountOptions" placeholder="Select account" />
          <UDatePicker v-model="dateRange" mode="range" placeholder="Date range" />
          <UInput v-model="search" icon="i-heroicons-magnifying-glass" placeholder="Search transactions..." />
        </div>
      </UCard>

      <!-- Transaction List -->
      <UCard>
        <div v-for="(group, date) in groupedTransactions" :key="date" class="mb-6 last:mb-0">
          <div class="flex items-center justify-between mb-4">
            <h3 class="font-medium text-gray-900">{{ formatDate(date) }}</h3>
            <span class="text-sm text-gray-500">{{ group.length }} transactions</span>
          </div>

          <div class="space-y-4">
            <div v-for="transaction in group" :key="transaction.id"
              class="flex items-center justify-between p-4 bg-gray-50 rounded-lg hover:bg-gray-100 transition-colors">
              <div class="flex items-center gap-4">
                <UIcon :name="getTransactionIcon(transaction.type)" class="text-2xl"
                  :class="getTransactionIconColor(transaction.type)" />
                <div>
                  <p class="font-medium">{{ transaction.description }}</p>
                  <p class="text-sm text-gray-500">{{ transaction.reference }}</p>
                </div>
              </div>
              <div class="text-right">
                <p class="font-medium" :class="transaction.type === 'credit' ? 'text-green-600' : 'text-red-600'">
                  {{ transaction.type === 'credit' ? '+' : '-' }}{{ formatCurrency(transaction.amount) }}
                </p>
                <p class="text-sm text-gray-500">{{ formatTime(transaction.timestamp) }}</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Pagination -->
        <div class="flex justify-between items-center mt-6">
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
import { format, parseISO } from 'date-fns'

/* definePageMeta({
  layout: 'banking',
  middleware: ['auth']
}) */

interface Transaction {
  id: string
  timestamp: string
  type: 'credit' | 'debit'
  amount: number
  description: string
  reference: string
}

// Filters
const selectedAccount = ref('')
const dateRange = ref()
const search = ref('')

const accountOptions = [
  { label: 'Savings Account (1234)', value: '1234' },
  { label: 'Current Account (5678)', value: '5678' }
]

// Mock transactions data
const transactions = ref<Transaction[]>([
  {
    id: 'TXN001',
    timestamp: '2024-02-05T14:30:00',
    type: 'credit',
    amount: 1500.00,
    description: 'Salary Credit',
    reference: 'SALARY/FEB2024/ABC'
  },
  {
    id: 'TXN002',
    timestamp: '2024-02-05T12:15:00',
    type: 'debit',
    amount: 50.00,
    description: 'Online Purchase',
    reference: 'POS/AMAZON/12345'
  }
])

// Group transactions by date
const groupedTransactions = computed(() => {
  const groups: Record<string, Transaction[]> = {}
  transactions.value.forEach(transaction => {
    const date = transaction.timestamp.split('T')[0]
    if (!groups[date]) {
      groups[date] = []
    }
    groups[date].push(transaction)
  })
  return groups
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

const formatDate = (dateString: string) => {
  return format(parseISO(dateString), 'MMMM d, yyyy')
}

const formatTime = (dateTimeString: string) => {
  return format(parseISO(dateTimeString), 'h:mm a')
}

const getTransactionIcon = (type: string) => {
  return type === 'credit' ? 'i-heroicons-arrow-down-left' : 'i-heroicons-arrow-up-right'
}

const getTransactionIconColor = (type: string) => {
  return type === 'credit' ? 'text-green-600' : 'text-red-600'
}
</script>