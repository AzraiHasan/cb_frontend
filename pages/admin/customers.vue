<!-- pages/admin/customers.vue -->
<template>
  <div class="min-h-screen bg-gray-50 p-4">
    <UContainer>
      <div class="flex justify-between items-center mb-6">
        <div>
          <h1 class="text-2xl font-bold text-gray-900">Customer Management</h1>
          <p class="text-gray-600">View and manage customer accounts</p>
        </div>
        <UButton color="primary" icon="i-heroicons-plus">
          Add Customer
        </UButton>
      </div>

      <!-- Filters -->
      <UCard class="mb-6">
        <div class="grid md:grid-cols-3 gap-4">
          <UInput v-model="search" icon="i-heroicons-magnifying-glass" placeholder="Search customers..." />
          <USelect v-model="statusFilter" :options="statusOptions" placeholder="Filter by status" />
          <USelect v-model="accountTypeFilter" :options="accountTypeOptions" placeholder="Filter by account type" />
        </div>
      </UCard>

      <!-- Customer Table -->
      <UCard>
        <UTable :rows="customers" :columns="columns" :sort="sort" @update:sort="sort = $event">
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
            Showing {{ startIndex }} to {{ endIndex }} of {{ totalCustomers }} customers
          </p>
          <UPagination v-model="currentPage" :total="totalPages" :ui="{ rounded: 'rounded-lg' }" />
        </div>
      </UCard>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
/* definePageMeta({
  layout: 'admin',
  middleware: ['admin']
}) */

// Table configuration
const columns = [
  { key: 'id', label: 'ID', sortable: true },
  { key: 'name', label: 'Customer Name', sortable: true },
  { key: 'email', label: 'Email' },
  { key: 'accountType', label: 'Account Type' },
  { key: 'status', label: 'Status' },
  { key: 'lastLogin', label: 'Last Login', sortable: true },
  { key: 'actions', label: '' }
]

// Filters
const search = ref('')
const statusFilter = ref('')
const accountTypeFilter = ref('')

const statusOptions = [
  { label: 'Active', value: 'active' },
  { label: 'Pending', value: 'pending' },
  { label: 'Suspended', value: 'suspended' }
]

const accountTypeOptions = [
  { label: 'Personal', value: 'personal' },
  { label: 'Business', value: 'business' }
]

// Mock data - replace with API calls
const customers = ref([
  {
    id: 1,
    name: 'John Doe',
    email: 'john@example.com',
    accountType: 'personal',
    status: 'active',
    lastLogin: '2024-02-05 14:30'
  },
  // Add more mock data as needed
])

// Sorting
const sort = ref({
  column: 'id',
  direction: 'asc' as const
})

// Pagination
const currentPage = ref(1)
const itemsPerPage = 10
const totalCustomers = computed(() => customers.value.length)
const totalPages = computed(() => Math.ceil(totalCustomers.value / itemsPerPage))
const startIndex = computed(() => (currentPage.value - 1) * itemsPerPage + 1)
const endIndex = computed(() => Math.min(currentPage.value * itemsPerPage, totalCustomers.value))

// Utility functions
import type { BadgeColor } from '#ui/types'

const getStatusColor = (status: string): BadgeColor => {
  const colors: Record<string, BadgeColor> = {
    active: 'primary',
    pending: 'amber',
    suspended: 'red'
  }
  return colors[status] || 'gray'
}

const getActionItems = (customer: any): any[] => [[
  {
    label: 'View Details',
    icon: 'i-heroicons-eye',
    click: () => viewCustomer(customer.id)
  },
  {
    label: 'Edit',
    icon: 'i-heroicons-pencil-square',
    click: () => editCustomer(customer.id)
  },
  {
    label: 'Suspend Account',
    icon: 'i-heroicons-lock-closed',
    click: () => suspendCustomer(customer.id)
  }
]]

// Action handlers
const viewCustomer = (id: number) => {
  // Implement view logic
}

const editCustomer = (id: number) => {
  // Implement edit logic
}

const suspendCustomer = (id: number) => {
  // Implement suspend logic
}
</script>