<!-- pages/auth/register/index.vue -->
<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-50">
    <div class="max-w-md w-full space-y-8 p-8 bg-white rounded-lg shadow">
      <div class="text-center">
        <h2 class="text-3xl font-bold text-gray-900">Create Account</h2>
        <p class="mt-2 text-sm text-gray-600">Start your banking journey with us</p>
      </div>

      <div class="py-4">
        <UProgress :value="20" class="w-full" />
        <p class="text-sm text-gray-600 mt-2">Step 1 of 5: Basic Information</p>
      </div>

      <UForm :schema="schema" :state="state" class="space-y-6" @submit="onSubmit">
        <UFormGroup label="Email" name="email">
          <UInput v-model="state.email" type="email" placeholder="Enter your email" />
        </UFormGroup>

        <UFormGroup label="Phone Number" name="phone">
          <UInput v-model="state.phone" type="tel" placeholder="+60 XX-XXXX-XXXX" />
          <template #help>
            This number will be used for account security
          </template>
        </UFormGroup>

        <UFormGroup label="Password" name="password">
          <UInput v-model="state.password" type="password" placeholder="Create a strong password" />
          <template #help>
            Must be at least 8 characters with numbers and special characters
          </template>
        </UFormGroup>

        <UFormGroup label="Confirm Password" name="confirmPassword">
          <UInput v-model="state.confirmPassword" type="password" placeholder="Confirm your password" />
        </UFormGroup>

        <UFormGroup name="termsAccepted">
          <UCheckbox v-model="state.termsAccepted" label="I agree to the Terms of Service and Privacy Policy" />
        </UFormGroup>

        <UFormGroup name="marketingConsent">
          <UCheckbox v-model="state.marketingConsent" label="I agree to receive updates and marketing communications" />
        </UFormGroup>

        <div>
          <UButton type="submit" block :loading="loading">
            Continue
          </UButton>
        </div>

        <p class="text-center text-sm text-gray-600">
          Already have an account?
          <UButton variant="link" to="/auth/login">
            Sign in
          </UButton>
        </p>
      </UForm>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { z } from 'zod'
import type { FormSubmitEvent } from '#ui/types'

const loading = ref(false)

const phoneRegex = /^\+60\d{9,10}$/
const passwordRegex = /^(?=.*[A-Za-z])(?=.*\d)(?=.*[@$!%*#?&])[A-Za-z\d@$!%*#?&]{8,}$/

const schema = z.object({
  email: z.string().email('Please enter a valid email address'),
  phone: z.string().regex(phoneRegex, 'Please enter a valid Malaysian phone number'),
  password: z.string()
    .min(8, 'Password must be at least 8 characters')
    .regex(passwordRegex, 'Password must include letters, numbers, and special characters'),
  confirmPassword: z.string(),
  termsAccepted: z.boolean().refine(val => val === true, 'You must accept the terms to continue'),
  marketingConsent: z.boolean()
}).refine(data => data.password === data.confirmPassword, {
  message: "Passwords don't match",
  path: ['confirmPassword']
})

type Schema = z.output<typeof schema>

const state = reactive({
  email: undefined,
  phone: undefined,
  password: undefined,
  confirmPassword: undefined,
  termsAccepted: false,
  marketingConsent: false
})

async function onSubmit(event: FormSubmitEvent<Schema>) {
  loading.value = true
  
  try {
    // TODO: Submit to registration API
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    // Proceed to review step
    await navigateTo('/auth/register/review')
  } catch (error) {
    console.error('Registration failed:', error)
  } finally {
    loading.value = false
  }
}
</script>