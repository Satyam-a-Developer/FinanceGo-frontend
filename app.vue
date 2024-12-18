<template>
  <div>
    <nav class="fixed top-0 left-0 right-0 z-50 ">
      <div class="container   flex ">
        <div class="wrapper flex bg-[#14213d] p-9  backdrop-blur-md" style=" border-bottom-left-radius: 100px 70px;  border-bottom-right-radius: 100px 70px;">
        

        <div class="flex items-center space-x-4 ">
          <NuxtImg 
            src="/logo2.png" 
            alt="Finance Go Logo" 
            class="h-10 w-10 rounded-full object-cover"
          />
          <h1 class="text-2xl font-bold text-white">Finance Go</h1>
        </div>
        
        <div class="hidden md:flex items-center space-x-6">
          <NuxtLink 
            to="/" 
            class="flex items-center space-x-2 text-white "
          >
            <Icon name="heroicons:home" class="h-5 w-5" />
            <span>Home</span>
          </NuxtLink>
          
          <NuxtLink 
            to="/features" 
            class="flex items-center space-x-2 text-white"
          >
            <Icon name="heroicons:chart-bar" class="h-5 w-5" />
            <span>Features</span>
          </NuxtLink>
          
          <NuxtLink 
            to="/pricing" 
            class="flex items-center space-x-2 text-white"
          >
            <Icon name="heroicons:credit-card" class="h-5 w-5" />
            <span>Pricing</span>
          </NuxtLink>
          
          <NuxtLink 
            to="/login" 
            class="px-4 py-2 bg-blue-600 hover:bg-blue-700 rounded-md transition"
          >
            Login
          </NuxtLink>
        </div>
        
        <!-- Mobile Menu Toggle -->
        <div class="md:hidden">
          <button 
            @click="toggleMobileMenu" 
            class="text-white focus:outline-none"
          >
            <Icon 
              :name="mobileMenuOpen ? 'heroicons:x-mark' : 'heroicons:bars-3'" 
              class="h-6 w-6"
            />
          </button>
        </div>
      </div>
    </div>
      
      <!-- Mobile Menu -->
      <div 
        v-if="mobileMenuOpen" 
        class="md:hidden absolute top-full left-0 right-0 bg-white/90 p-4 space-y-4"
      >
        <NuxtLink 
          to="/" 
          class="block text-gray-300 hover:text-blue-500 transition"
        >
          Home
        </NuxtLink>
        <NuxtLink 
          to="/features" 
          class="block text-gray-300 hover:text-blue-500 transition"
        >
          Features
        </NuxtLink>
        <NuxtLink 
          to="/pricing" 
          class="block text-gray-300 hover:text-blue-500 transition"
        >
          Pricing
        </NuxtLink>
        <NuxtLink 
          to="/login" 
          class="block w-full text-center px-4 py-2 bg-blue-600 hover:bg-blue-700 rounded-md transition"
        >
          Login
        </NuxtLink>
      </div>
    </nav>
    
    <div class="container">
      <NuxtPage />
    </div>
  </div>
</template>

<script>
export default {
  name: 'NuxtPageWrapper',
};
</script>

