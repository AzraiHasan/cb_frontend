<!-- pages/auth/register/review.vue -->
<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-50">
    <div class="max-w-2xl w-full space-y-8 p-8 bg-white rounded-lg shadow">
      <div class="text-center">
        <h2 class="text-3xl font-bold text-gray-900">Review Your Information</h2>
        <p class="mt-2 text-sm text-gray-600">Please review your details before proceeding</p>
      </div>

      <div class="divide-y divide-gray-200">
        <!-- Basic Information -->
        <div class="py-4">
          <div class="flex justify-between items-center mb-4">
            <h3 class="text-lg font-medium">Basic Information</h3>
            <UButton variant="ghost" icon="i-heroicons-pencil-square" @click="navigateTo('/auth/register')">
              Edit
            </UButton>
          </div>
          <dl class="grid grid-cols-1 gap-x-4 gap-y-4 sm:grid-cols-2">
            <div>
              <dt class="text-sm font-medium text-gray-500">Email</dt>
              <dd class="mt-1 text-sm text-gray-900">{{ state.email }}</dd>
            </div>
            <div>
              <dt class="text-sm font-medium text-gray-500">Phone</dt>
              <dd class="mt-1 text-sm text-gray-900">{{ state.phone }}</dd>
            </div>
          </dl>
        </div>

        <!-- Marketing Preferences -->
        <div class="py-4">
          <h3 class="text-lg font-medium mb-4">Preferences</h3>
          <dl class="space-y-2">
            <div>
              <dt class="text-sm font-medium text-gray-500">Marketing Communications</dt>
              <dd class="mt-1 text-sm text-gray-900">
                {{ state.marketingConsent ? 'Opted in' : 'Opted out' }}
              </dd>
            </div>
          </dl>
        </div>

        <!-- Confirmation -->
        <div class="py-4">
          <div class="flex gap-4">
            <UButton variant="outline" @click="navigateTo('/auth/register')">
              Edit Information
            </UButton>

            <UButton :loading="loading" @click="handleConfirm">
              Create Account
            </UButton>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
const loading = ref(false)

// TODO: Get this from your state management
const state = reactive({
  email: 'user@example.com',
  phone: '+60123456789',
  marketingConsent: true
})

async function handleConfirm() {
  loading.value = true
  try {
    // TODO: Submit final registration
    await new Promise(resolve => setTimeout(resolve, 1000))
    await navigateTo('/dashboard/accounts')
  } catch (error) {
    console.error('Registration failed:', error)
  } finally {
    loading.value = false
  }
}
</script>