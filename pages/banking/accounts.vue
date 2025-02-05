<!-- pages/banking/accounts.vue -->
<template>
  <div class="min-h-screen bg-gray-50 p-4">
    <UContainer>
      <div class="flex justify-between items-center mb-6">
        <div>
          <h1 class="text-2xl font-bold text-gray-900">My Accounts</h1>
          <p class="text-gray-600">View and manage your bank accounts</p>
        </div>
        <UButton color="primary" icon="i-heroicons-plus">
          Open New Account
        </UButton>
      </div>

      <div class="grid md:grid-cols-2 gap-6">
        <UCard v-for="account in accounts" :key="account.id">
          <template #header>
            <div class="flex items-center justify-between">
              <h2 class="text-lg font-semibold">{{ account.name }}</h2>
              <UBadge :color="account.isDefault ? 'primary' : 'gray'">
                {{ account.isDefault ? 'Default' : 'Secondary' }}
              </UBadge>
            </div>
          </template>

          <div class="space-y-4">
            <div class="flex justify-between items-center">
              <span class="text-gray-600">Balance</span>
              <span class="text-2xl font-bold">{{ formatCurrency(account.balance) }}</span>
            </div>
            <div class="flex justify-between items-center">
              <span class="text-gray-600">Account Number</span>
              <span class="font-medium">{{ account.accountNumber }}</span>
            </div>
            <div class="flex justify-between items-center">
              <span class="text-gray-600">Account Type</span>
              <span>{{ account.type }}</span>
            </div>
          </div>

          <template #footer>
            <div class="flex gap-4 flex-wrap">
              <UButton size="sm" @click="viewTransactions(account.id)">
                View Transactions
              </UButton>
              <UDropdown :items="getAccountActions(account)">
                <UButton size="sm" color="gray" variant="ghost">
                  More Actions
                </UButton>
              </UDropdown>
            </div>
          </template>
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

interface BankAccount {
  id: number
  name: string
  balance: number
  accountNumber: string
  type: string
  isDefault: boolean
}

const accounts = ref<BankAccount[]>([
  {
    id: 1,
    name: 'Primary Savings',
    balance: 25000.50,
    accountNumber: '1234-5678-9012-3456',
    type: 'Savings Account',
    isDefault: true
  },
  {
    id: 2,
    name: 'Business Current',
    balance: 50000.75,
    accountNumber: '9876-5432-1098-7654',
    type: 'Current Account',
    isDefault: false
  }
])

const formatCurrency = (value: number) => {
  return value.toLocaleString('en-MY', {
    style: 'currency',
    currency: 'MYR'
  })
}

const getAccountActions = (account: BankAccount): any[] => [[
  {
    label: 'Set as Default',
    icon: 'i-heroicons-star',
    disabled: account.isDefault,
    click: () => setDefaultAccount(account.id)
  },
  {
    label: 'Account Details',
    icon: 'i-heroicons-document-text',
    click: () => viewAccountDetails(account.id)
  },
  {
    label: 'Download Statement',
    icon: 'i-heroicons-arrow-down-tray',
    click: () => downloadStatement(account.id)
  }
]]

const viewTransactions = (accountId: number) => {
  navigateTo(`/banking/transactions?account=${accountId}`)
}

const setDefaultAccount = (accountId: number) => {
  // Implement set default logic
}

const viewAccountDetails = (accountId: number) => {
  // Implement view details logic
}

const downloadStatement = (accountId: number) => {
  // Implement download logic
}
</script>