<!-- pages/auth/login.vue -->
<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-50">
    <div class="max-w-md w-full space-y-8 p-8 bg-white rounded-lg shadow">
      <div class="text-center">
        <h2 class="text-3xl font-bold text-gray-900">Welcome back</h2>
        <p class="mt-2 text-sm text-gray-600">Sign in to your account</p>
      </div>

      <form @submit.prevent="handleLogin" class="mt-8 space-y-6">
        <UFormGroup label="Email" name="email">
          <UInput v-model="formData.email" type="email" placeholder="Enter your email" :error="errors.email" required />
        </UFormGroup>

        <UFormGroup label="Password" name="password">
          <UInput v-model="formData.password" type="password" placeholder="Enter your password" :error="errors.password"
            required />
        </UFormGroup>

        <div class="flex items-center justify-between">
          <UCheckbox v-model="formData.remember" label="Remember me" name="remember" />
          <UButton variant="link" to="/auth/forgot-password">
            Forgot password?
          </UButton>
        </div>

        <div>
          <UButton type="submit" block :loading="loading">
            Sign in
          </UButton>
        </div>
      </form>
    </div>
  </div>
</template>

<script lang="ts" setup>
const loading = ref(false)
const errors = ref<{ email?: string, password?: string }>({})

const formData = ref<{ email: string, password: string, remember: boolean }>({
  email: '',
  password: '',
  remember: false
})

async function handleLogin() {
  loading.value = true
  errors.value = {}
  
  try {
    // TODO: Integrate with authentication service
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    // On success, redirect to dashboard
    await navigateTo('/dashboard')
  } catch (error) {
    errors.value = {
      email: 'Invalid credentials',
      password: 'Invalid credentials'
    }
  } finally {
    loading.value = false
  }
}
</script>