<!-- layouts/default.vue -->
<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Top Navigation -->
    <nav class="bg-white shadow">
      <div class="mx-auto px-4">
        <div class="flex h-16 justify-between">
          <div class="flex">
            <div class="flex items-center">
              <UButton variant="ghost" class="text-xl font-bold" to="/dashboard">
                MyBank
              </UButton>
            </div>
            <div class="hidden sm:ml-6 sm:flex sm:space-x-8">
              <UButton variant="ghost" to="/dashboard/accounts">
                Accounts
              </UButton>
              <UButton variant="ghost" to="/dashboard/transactions">
                Transactions
              </UButton>
            </div>
          </div>

          <!-- User Dropdown -->
          <div class="flex items-center">
            <UDropdown :items="userMenuItems" :popper="{ placement: 'bottom-end' }">
              <UButton variant="ghost" class="flex items-center">
                <UIcon name="i-heroicons-user-circle" class="h-8 w-8" />
                <UIcon name="i-heroicons-chevron-down" class="ml-2 h-5 w-5" />
              </UButton>
            </UDropdown>
          </div>
        </div>
      </div>
    </nav>

    <!-- Mobile Navigation -->
    <UButton v-show="showMobileMenu" variant="soft" class="fixed bottom-4 right-4 z-50 sm:hidden"
      icon="i-heroicons-bars-3" @click="mobileMenuOpen = true" />

    <USlideover v-model="mobileMenuOpen">
      <div class="flex flex-col h-full">
        <nav class="flex-1 space-y-2 p-4">
          <UButton v-for="item in mobileMenuItems" :key="item.label" block variant="ghost" :to="item.to">
            {{ item.label }}
          </UButton>
        </nav>
      </div>
    </USlideover>

    <!-- Main Content -->
    <main>
      <slot />
    </main>
  </div>
</template>

<script lang="ts" setup>
const mobileMenuOpen = ref(false)
const showMobileMenu = ref(true)

const userMenuItems = [
  [{
    label: 'Settings',
    icon: 'i-heroicons-cog-6-tooth',
    to: '/settings'
  }, {
    label: 'Profile',
    icon: 'i-heroicons-user',
    to: '/settings/profile'
  }],
  [{
    label: 'Sign out',
    icon: 'i-heroicons-arrow-right-on-rectangle',
    click: () => handleSignOut()
  }]
]

const mobileMenuItems = [
  { label: 'Accounts', to: '/dashboard/accounts' },
  { label: 'Transactions', to: '/dashboard/transactions' },
  { label: 'Settings', to: '/settings' }
]

async function handleSignOut() {
  // TODO: Implement sign out logic
  await navigateTo('/auth/login')
}

// Hide mobile menu button when keyboard is shown (mobile devices)
if (process.client) {
  const vh = window.innerHeight
  window.addEventListener('resize', () => {
    showMobileMenu.value = window.innerHeight >= vh * 0.8
  })
}
</script>