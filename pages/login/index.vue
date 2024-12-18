<template>
  <div class="h-screen flex items-center justify-center  p-4">
    <div class="w-full max-w-md   -2xl rounded-2xl p-8">
      <div class="text-center mb-8">
        <h1 class="text-3xl font-bold text-blue-600">Finance Go </h1>
        <p class="text-white-500 mt-2">Manage Your Finances Smartly</p>
      </div>
      
      <form @submit.prevent="handleLogin" class="space-y-6">
        <div>
          <label for="email" class="block text-sm font-medium text-white-700">
            Email Address
          </label>
          <div class="mt-1 relative rounded-md shadow-sm">
            <input
              type="email"
              id="email"
              v-model="email"
              required
              class="pl-3 block w-full rounded-md border border-gray-300 
                     focus:ring-blue-500 focus:border-blue-500 sm:text-sm py-2"
              placeholder="you@example.com"
            />
          </div>
        </div>

        <div>
          <label for="password" class="block text-sm font-medium text-white-700">
            Password
          </label>
          <div class="mt-1 relative rounded-md shadow-sm">
            <input
              type="password"
              id="password"
              v-model="password"
              required
              class="pl-3 block w-full rounded-md border border-gray-300 
                     focus:ring-blue-500 focus:border-blue-500 sm:text-sm py-2"
              placeholder="Enter your password"
            />
          </div>
        </div>

        <div v-if="errorMessage" class="text-red-500 text-sm text-center">
          {{ errorMessage }}
        </div>

        <div>
          <button
            type="submit"
            :disabled="isLoading"
            class="w-full flex justify-center py-2 px-4 border border-transparent 
                   rounded-md shadow-sm text-sm font-medium text-white 
                   bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 
                   focus:ring-offset-2 focus:ring-blue-500"
          >
            {{ isLoading ? 'Logging in...' : 'Sign In' }}
          </button>
        </div>
      </form>

      <div class="mt-6 text-center">
        <p class="text-sm text-white-600">
          Don't have an account? 
          <NuxtLink to="/register" class="font-medium text-blue-600 hover:text-blue-500">
            Register here
          </NuxtLink>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  pageTransition: {
    name: 'rotate'
  }
})
import { ref } from 'vue'
import { useRouter } from 'vue-router'

// Reactive state
const email = ref('')
const password = ref('')
const errorMessage = ref('')
const isLoading = ref(false)

// Router for navigation
const router = useRouter()

// Login handler
const handleLogin = async () => {
  // Reset previous error
  errorMessage.value = ''
  
  // Validate inputs
  if (!email.value || !password.value) {
    errorMessage.value = 'Please enter both email and password'
    return
  }

  // Set loading state
  isLoading.value = true

  try {
    // TODO: Replace with actual authentication logic
    // Example: 
    // const response = await $fetch('/api/login', {
    //   method: 'POST',
    //   body: { email: email.value, password: password.value }
    // })

    // Simulated login (replace with actual authentication)
    if (email.value === 'user@example.com' && password.value === 'password123') {
      // Successful login
      // You might want to set a token or user session here
      router.push('/dashboard')
    } else {
      errorMessage.value = 'Invalid email or password'
    }
  } catch (error) {
    // Handle login error
    errorMessage.value = error.message || 'Login failed. Please try again.'
  } finally {
    // Reset loading state
    isLoading.value = false
  }
}
</script>

<style scoped>
/* Additional custom styles if needed */
</style>