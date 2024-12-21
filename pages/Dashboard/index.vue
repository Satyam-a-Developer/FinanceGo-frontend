<template>
  <div class="min-h-screen flex flex-col p-8">
    <!-- Navigation and Header Section -->
    <div class="flex items-center gap-6 mb-8">
      <NuxtLink 
        to="/businessForm" 
        class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700 transition-colors">
        Fill the Form
      </NuxtLink>
      <h1 class="text-3xl font-bold text-blue-600">Welcome</h1>
    </div>

    <!-- Content Section -->
    <div class="mt-4">
      <!-- Error State -->
      <div v-if="error" class="p-4 bg-red-100 border border-red-400 text-red-700 rounded-md">
        <p>Error: {{ error }}</p>
      </div>

      <!-- Loading State -->
      <div v-else-if="isLoading" class="p-4">
        <p class="text-gray-600">Loading...</p>
      </div>

      <!-- Content State -->
      <div v-else class="space-y-4">
        <h1 class="text-3xl font-bold text-gray-800">{{ username }}</h1>
        <h2 v-if="namedata" class="text-2xl font-semibold text-gray-700">
          Business: {{ namedata }}
        </h2>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      namedata : '',
      error: null,
      isLoading: true
    };
  },

  async mounted() {
    try {
      await this.fetchUserData();
      await this.fetchBusinessData();
    } catch (err) {
      this.error = err.message || 'An unexpected error occurred';
    } finally {
      this.isLoading = false;
    }
  },

  methods: {
    async fetchUserData() {
      const response = await fetch('http://localhost:3003/dashboard', {
        method: 'GET',
        credentials: 'include',
      });

      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.error || 'Failed to fetch user data');
      }

      const data = await response.json();
      this.username = data.user.username;
    },

    async fetchBusinessData() {
      const response = await fetch('http://localhost:3003/dashboard/business', {
        method: 'GET',
        credentials: 'include',
      });


      const data = await response.json();
      this.namedata = data.user.businessName;
    }
  }
};
</script>

<style scoped>
/* Additional custom styles can be added here if needed */
</style>