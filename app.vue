<template>
  <div>
    <nav 
      :class="[
        'fixed transition-all duration-300 z-50',
        isDashboard ? 'left-0 top-0 h-full w-64' : 'top-0 left-0 right-0'
      ]"
    >
      <div 
        ref="navRef"
        :class="[
          'container transition-all duration-300',
          isDashboard ? 'h-full' : 'flex'
        ]"
      >
        <div 
          :class="[
            'wrapper bg-[#14213d] backdrop-blur-md transition-all duration-300',
            isDashboard 
              ? 'h-full w-64 px-4 py-6' 
              : 'px-10 py-5 flex items-center'
          ]"
          :style="!isDashboard ? 'border-bottom-left-radius: 100px 70px; border-bottom-right-radius: 100px 70px;' : ''"
        >
          <div 
            :class="[
              'flex items-center',
              isDashboard ? 'flex-col space-y-4' : 'space-x-4'
            ]"
          >
            <NuxtImg src="/Subject.png" alt="Finance Go Logo" class="h-10 w-10 rounded-full object-cover" />
            <h1 class="text-2xl font-bold text-white">Finance Go</h1>
          </div>

          <div 
            :class="[
              'md:flex items-center space-x-6',
              isDashboard ? 'flex-col space-y-6 mt-10 space-x-0' : 'hidden ml-10'
            ]"
          >
            <NuxtLink 
              to="/" 
              class="flex items-center space-x-2 text-white relative nav-link"
              :class="{ 'active-link': route.path === '/' }"
            >
              <span>Home</span>
            </NuxtLink>
            <NuxtLink 
              to="/pricing" 
              class="flex items-center space-x-2 text-white relative nav-link"
              :class="{ 'active-link': route.path === '/pricing' }"
            >
              <span>Pricing</span>
            </NuxtLink>
            <NuxtLink 
              to="/Dashboard" 
              class="flex items-center space-x-2 text-white relative nav-link"
              :class="{ 'active-link': route.path === '/Dashboard' }"
            >
              <span>Dashboard</span>
            </NuxtLink>
            <NuxtLink 
              v-if="!isLoggedIn" 
              to="/login" 
              :class="[
                'px-4 bg-blue-600 hover:bg-blue-700 rounded-md transition relative nav-link',
                isDashboard ? 'w-full text-center py-2' : '',
                { 'bg-blue-700': route.path === '/login' }
              ]"
            >
              Login
            </NuxtLink>
          </div>

          <div class="md:hidden ml-auto">
            <button @click="toggleMobileMenu" class="text-white focus:outline-none">
              🙂
            </button>
          </div>
        </div>
      </div>

      <div 
        v-if="mobileMenuOpen" 
        class="md:hidden absolute top-full left-0 right-0 bg-[#14213d]/90 p-4 space-y-4"
      >
        <NuxtLink 
          to="/" 
          class="block text-white hover:text-blue-500 transition nav-link"
          :class="{ 'text-blue-500': route.path === '/' }"
        >
          Home
        </NuxtLink>
        <NuxtLink 
          to="/features" 
          class="block text-white hover:text-blue-500 transition nav-link"
          :class="{ 'text-blue-500': route.path === '/service' }"
        >
          Features
        </NuxtLink>
        <NuxtLink 
          to="/pricing" 
          class="block text-white hover:text-blue-500 transition nav-link"
          :class="{ 'text-blue-500': route.path === '/pricing' }"
        >
          Pricing
        </NuxtLink>
        <NuxtLink 
          v-if="!isLoggedIn"
          to="/login"
          class="block w-full text-center px-4 py-2 bg-blue-600 hover:bg-blue-700 rounded-md transition"
          :class="{ 'bg-blue-700': route.path === '/login' }"
        >
          Login
        </NuxtLink> 
      </div>
    </nav>

    <div :class="['container transition-all duration-300', isDashboard ? 'ml-64' : '']">
      <NuxtPage />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
const isDashboard = ref(false);
const mobileMenuOpen = ref(false);
const isLoggedIn = ref(false); // Tracks if the user is logged in
const navRef = ref(null);
const route = useRoute();

const toggleMobileMenu = () => {
  mobileMenuOpen.value = !mobileMenuOpen.value;
};

// Initialize login state on mount
onMounted(() => {
  const { $gsap } = useNuxtApp();

  if ($gsap && !isDashboard.value) {
    $gsap.from(navRef.value, {
      y: -100,
      duration: 1,
      ease: 'power2.out'
    });
  }

  // Check for the presence of the auth token
  const authToken = getCookie('authToken');
  isLoggedIn.value = !!authToken; // If token exists, set `isLoggedIn` to true
});

// Check if a cookie exists
const getCookie = (name) => {
  const cookies = document.cookie.split(';');
  for (let cookie of cookies) {
    const [key, value] = cookie.trim().split('=');
    if (key === name) {
      return value;
    }
  }
  return null;
};
</script>

<style scoped>
.rotate-enter-active,
.rotate-leave-active {
  transition: all 0.4s;
}

.rotate-enter-from,
.rotate-leave-to {
  opacity: 0;
  transform: rotate3d(1, 1, 1, 15deg);
}

.nav-link {
  position: relative;
  transition: all 0.3s ease;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 0;
  height: 2px;
  background-color: #3b82f6; /* Blue underline */
  transition: width 0.3s ease;
}

/* Hover animation for the link */
.nav-link:hover {
  color: #3b82f6; /* Change text color on hover */
  transform: translateY(-2px); /* Subtle upward movement */
}

/* Hover effect for the underline */
.nav-link:hover::after {
  width: 100%; /* Expand the underline on hover */
}

.active-link {
  color: #3b82f6;
}

.active-link::after {
  width: 100%;
}
</style>
