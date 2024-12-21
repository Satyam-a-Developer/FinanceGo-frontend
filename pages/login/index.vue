<template>
  <div class="h-screen flex items-center justify-center p-4">
    <div class="w-full max-w-md rounded-2xl p-8 overflow-auto">
      <div class="text-center mb-8">
        <h1 class="text-3xl font-bold text-blue-600 inline-block">
          Finance Go
        </h1>
        <span v-if="name" class="ml-4 text-lg text-gray-800"></span>
        <p class="text-gray-500 mt-2">Manage Your Finances Smartly</p>
      </div>

      <form @submit.prevent="handleLogin" class="space-y-6 text-black">
        <div>
          <label for="username" class="block text-sm font-medium text-gray-700">
            Name
          </label>
          <div class="mt-1 relative rounded-md shadow-sm">
            <input
              type="text"
              id="username"
              v-model="name"
              required
              class="pl-3 block w-full rounded-md border border-gray-300 focus:ring-blue-500 focus:border-blue-500 sm:text-sm py-2"
              placeholder="Enter your name"
            />
          </div>
        </div>

        <div>
          <label for="email" class="block text-sm font-medium text-gray-700">
            Email Address
          </label>
          <div class="mt-1 relative rounded-md shadow-sm">
            <input
              type="email"
              id="email"
              v-model="email"
              required
              class="pl-3 block w-full rounded-md border border-gray-300 focus:ring-blue-500 focus:border-blue-500 sm:text-sm py-2"
              placeholder="you@example.com"
            />
          </div>
        </div>

        <div>
          <label for="password" class="block text-sm font-medium text-gray-700">
            Password
          </label>
          <div class="mt-1 relative rounded-md shadow-sm">
            <input
              type="password"
              id="password"
              v-model="password"
              required
              class="pl-3 block w-full rounded-md border border-gray-300 focus:ring-blue-500 focus:border-blue-500 sm:text-sm py-2"
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
            class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500"
          >
            {{ isLoading ? 'Logging in...' : 'Sign In' }}
          </button>
        </div>
      </form>

      <div class="mt-6 text-center">
        <p class="text-sm text-gray-600">
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

import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const name = ref('')
const email = ref('')
const password = ref('')
const errorMessage = ref('')
const isLoading = ref(false)

const router = useRouter()

const handleLogin = async () => {
  errorMessage.value = ''
  console.log('Attempting login with:', { 
    name: name.value,
    email: email.value,
    password: password.value 
  })

  if (!name.value || !email.value || !password.value) {
    errorMessage.value = 'Please fill in all fields'
    return
  }

  isLoading.value = true

  try {
    const response = await fetch('http://localhost:3003/auth/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ 
        username: name.value,
        email: email.value, 
        password: password.value 
      }),
      credentials: 'include' 
    })

    const data = await response.json()

   if (response.ok) {
      // If login successful, store the token
      if (data.token) {
      localStorage.setItem('token', data.token)
      }

 

      console.log('Login successful, navigating to dashboard')
      router.push('/dashboard')
    } else {
      if (Array.isArray(data)) {
        errorMessage.value = data.map(error => error.message).join(', ')
      } else if (data.message) {
        errorMessage.value = data.message
      } else {
        errorMessage.value = 'Invalid credentials. Go to registration page and try again.'
      }
    }
  } catch (error) {
    console.error('Login error:', error)
    errorMessage.value = 'An error occurred during login. Please try again.'
  } 
}


</script>
