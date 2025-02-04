<!-- pages/dashboard/accounts/index.vue -->
<template>
  <div class="space-y-6 p-6">
    <!-- Welcome Section for New Users -->
    <UCard v-if="isNewUser" class="bg-primary-50">
      <template #header>
        <div class="flex items-center justify-between">
          <h3 class="text-lg font-medium">Welcome to Your Core Banking Dashboard</h3>
          <UButton variant="ghost" icon="i-heroicons-x-mark" @click="dismissWelcome" />
        </div>
      </template>

      <p class="text-sm text-gray-600 mb-4">
        Get started by opening your first account. Choose from our core banking services designed to meet your banking
        needs.
      </p>

      <UButton @click="startAccountOpening">
        Open an account
      </UButton>
    </UCard>

    <!-- Account List Section -->
    <div>
      <div class="flex justify-between items-center mb-4">
        <h2 class="text-xl font-semibold">Your Accounts</h2>
        <UButton v-if="accounts.length > 0" icon="i-heroicons-plus" @click="startAccountOpening">
          Open New Account
        </UButton>
      </div>

      <div v-if="accounts.length === 0" class="text-center py-12">
        <UIcon name="i-heroicons-bank-notes" class="h-12 w-12 mx-auto text-gray-400 mb-4" />
        <h3 class="text-lg font-medium text-gray-900 mb-2">
          No Services Yet
        </h3>
        <p class="text-sm text-gray-600 mb-4">
          Start your digital core banking service by creating your first account
        </p>
        <UButton @click="startAccountOpening">
          Open an Account
        </UButton>
      </div>

      <div v-else class="grid gap-4 md:grid-cols-2 lg:grid-cols-3">
        <UCard v-for="account in accounts" :key="account.id" class="hover:shadow-lg transition-shadow"
          @click="navigateTo(`/dashboard/accounts/${account.id}`)">
          <template #header>
            <div class="flex items-center justify-between">
              <h3 class="font-medium">{{ account.name }}</h3>
              <UBadge :color="account.status === 'active' ? 'emerald' : 'amber'"
                :variant="account.status === 'active' ? 'solid' : 'outline'">
                {{ account.status }}
              </UBadge>
            </div>
          </template>

          <p class="text-2xl font-semibold mb-2">
            RM {{ formatAmount(account.balance) }}
          </p>
          <p class="text-sm text-gray-600">
            Account: {{ account.accountNumber }}
          </p>
        </UCard>
      </div>
    </div>

    <!-- Account Opening Modal -->
    <UModal v-model="showAccountModal">
      <UCard>
        <template #header>
          <h3 class="text-lg font-medium">Open New Account</h3>
        </template>

        <UForm :schema="schema" :state="formState" @submit="handleAccountCreation">
          <UFormGroup label="Account Type" name="accountType">
            <USelect v-model="formState.accountType" :options="accountTypes" />
          </UFormGroup>

          <UFormGroup label="Initial Deposit (RM)" name="initialDeposit">
            <UInput v-model="formState.initialDeposit" type="number" min="0" />
          </UFormGroup>

          <div class="flex justify-end gap-2 mt-4">
            <UButton variant="ghost" @click="showAccountModal = false">
              Cancel
            </UButton>
            <UButton type="submit" :loading="creating">
              Create Account
            </UButton>
          </div>
        </UForm>
      </UCard>
    </UModal>
  </div>
</template>

<script lang="ts" setup>
import { z } from 'zod'
import type { FormSubmitEvent } from '#ui/types'

// Types
interface Account {
  id: string
  name: string
  accountNumber: string
  balance: number
  status: 'active' | 'pending' | 'frozen'
  type: 'savings' | 'current' | 'fixed_deposit'
}

interface AccountType {
  label: string
  value: Account['type']
}

// State
const isNewUser = ref(true)
const showAccountModal = ref(false)
const creating = ref(false)

// Mock data - Replace with API calls
const accounts = ref<Account[]>([
  /* {
    id: '1',
    name: 'Basic Savings',
    accountNumber: '1234567890',
    balance: 1000.00,
    status: 'active',
    type: 'savings'
  } */
])

const accountTypes: AccountType[] = [
  { label: 'Savings Account', value: 'savings' },
  { label: 'Current Account', value: 'current' },
  { label: 'Fixed Deposit', value: 'fixed_deposit' }
]

// Form Schema
const schema = z.object({
  accountType: z.enum(['savings', 'current', 'fixed_deposit'] as const),
  initialDeposit: z.number().min(0, 'Initial deposit must be positive')
})

type Schema = z.output<typeof schema>

const formState = reactive({
  accountType: undefined as Account['type'] | undefined,
  initialDeposit: undefined as number | undefined
})

// Functions
function dismissWelcome() {
  isNewUser.value = false
  // TODO: Save preference to user settings
}

function startAccountOpening() {
  showAccountModal.value = true
}

function formatAmount(amount: number) {
  return amount.toLocaleString('en-MY', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  })
}

async function handleAccountCreation(event: FormSubmitEvent<Schema>) {
  creating.value = true
  try {
    // TODO: API call to create account
    const newAccount: Account = {
      id: String(accounts.value.length + 1),
      name: `${event.data.accountType.charAt(0).toUpperCase() + event.data.accountType.slice(1)} Account`,
      accountNumber: Math.random().toString().slice(2, 12),
      balance: event.data.initialDeposit,
      status: 'pending',
      type: event.data.accountType
    }
    
    accounts.value.push(newAccount)
    showAccountModal.value = false
  } catch (error) {
    console.error('Failed to create account:', error)
  } finally {
    creating.value = false
  }
}

// Lifecycle
onMounted(async () => {
  // TODO: Fetch user's accounts
  // const response = await fetch('/api/accounts')
  // accounts.value = await response.json()
})
</script>