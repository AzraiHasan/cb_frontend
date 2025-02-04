<!-- pages/callback.vue -->
<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-50">
    <UCard>
      <template #header>
        <h3 class="text-lg font-medium">Authentication in Progress</h3>
      </template>

      <div v-if="error" class="text-center">
        <UIcon name="i-heroicons-x-circle" class="text-red-500 h-12 w-12 mx-auto" />
        <p class="mt-4 text-sm text-gray-600">{{ error }}</p>
        <UButton to="/auth/login" class="mt-4">
          Return to Login
        </UButton>
      </div>
      <div v-else class="text-center">
        <UIcon name="i-heroicons-arrow-path" class="h-12 w-12 mx-auto animate-spin" />
        <p class="mt-4 text-sm text-gray-600">Processing your login...</p>
      </div>
    </UCard>
  </div>
</template>

<script lang="ts" setup>
const route = useRoute()
const error = ref<string | null>(null)

onMounted(async () => {
  try {
    const code = route.query.code as string
    if (!code) throw new Error('No authorization code provided')

    // TODO: Exchange code for tokens
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    // Redirect to dashboard on success
    await navigateTo('/dashboard')
  } catch (e) {
    error.value = e instanceof Error ? e.message : 'Authentication failed'
  }
})
</script>