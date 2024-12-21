<template>
  <div class="h-screen flex flex-row mt-[5rem] ml-[5rem] p-4">
    <NuxtLink 
          to="/bussinessForm"
          class="block text-white hover:text-blue-500 transition nav-link"
        >
          Fill the form
        </NuxtLink>
    <h1 class="text-3xl font-bold text-blue-600 inline-block mr-10">Welcome</h1>
    <div v-if="error" class="error">
      <p>Error: login in to create a new account</p>
    </div>
    <div v-else>
      <h1 v-if="!username">Loading...</h1>
      <h1 v-else class="text-[30px] font-bold"> {{ username }}</h1>
    </div>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '', // To store the username
      error: '', // To store error messages
    };
  },
  async mounted() {
    try {
  
    
      const response = await fetch('http://localhost:3003/dashboard', {
        method: 'GET',
        credentials: 'include',
      });

      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.error || 'Failed to fetch dashboard data');
      }

      const data = await response.json();
      this.username = data.user.username;
    } catch (err) {
      this.error = err.message;
    }
  },
};
</script>

<style>
.error {
  color: red;
}
</style>
