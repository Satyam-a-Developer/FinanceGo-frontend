<template>
  <div
    class="bg-dark min-h-screen text-white font-inter flex flex-col items-center"
  >
  <div class="container mx-auto px-6 py-24 min-h-screen max-w-6xl mt-[100px] ml-8">
  <div class="flex flex-col md:flex-row items-center justify-between gap-16">
    <div class="flex-1 space-y-8 text-center md:text-left">
      <h1 class="text-4xl md:text-6xl font-bold text-white leading-tight">
        Transform Your
        <span class="text-blue-500">Financial Future</span>
        <br />
        With Finance Go
      </h1>
      
      <p class="text-gray-300 text-lg md:text-xl">
        Comprehensive financial management platform designed to simplify your
        financial journey, track investments, and make informed decisions.
      </p>
      
      <div class="flex flex-col sm:flex-row gap-4 justify-center md:justify-start">
        <NuxtLink
          to="/signup"
          class="px-6 py-3 bg-blue-600 hover:bg-blue-700 rounded-lg text-white font-semibold transition duration-200"
        >
          Get Started
        </NuxtLink>
        
        <NuxtLink
          to="/features"
          class="px-6 py-3 border border-gray-600 hover:bg-gray-800 rounded-lg text-white transition duration-200"
        >
          Learn More
        </NuxtLink>
      </div>
    </div>

    <div class="flex-1 flex justify-center items-center">
      <NuxtImg
        src="/logo.jpeg"
        width="500"
        height="500"
        alt="Finance Go Dashboard"
        class="w-full max-w-lg rounded-xl shadow-2xl"
      />
    </div>
  </div>
</div>

    <!-- Features Section -->
    <div class="container mx-auto px-6 py-16 max-w-6xl">
      <h2
        ref="fetures"
        class="text-3xl md:text-4xl font-bold text-center mb-12"
      >
        Our Powerful Features
      </h2>

      <div class="grid md:grid-cols-3 gap-8">
        <!-- Budget Tracking -->
        <div
          class="bg-gray-900 p-6 rounded-xl border border-gray-800 hover:border-blue-600 transition"
        >
          <div class="text-blue-500 mb-4 flex justify-center"></div>
          <h3 class="text-xl font-semibold mb-3 text-center">
            Budget Tracking
          </h3>
          <p class="text-gray-400 text-center">
            Effortlessly monitor your income, expenses, and financial health
            with our intuitive tracking tools.
          </p>
        </div>

        <!-- Investment Analysis -->
        <div
          class="bg-gray-900 p-6 rounded-xl border border-gray-800 hover:border-blue-600 transition"
        >
          <div class="text-blue-500 mb-4 flex justify-center"></div>
          <h3 class="text-xl font-semibold mb-3 text-center">
            Investment Insights
          </h3>
          <p class="text-gray-400 text-center">
            Gain deep insights into your investments with comprehensive analysis
            and performance tracking.
          </p>
        </div>

        <!-- Secure Transactions -->
        <div
          class="bg-gray-900 p-6 rounded-xl border border-gray-800 hover:border-blue-600 transition"
        >
          <div class="text-blue-500 mb-4 flex justify-center"></div>
          <h3 class="text-xl font-semibold mb-3 text-center">
            Bank-Level Security
          </h3>
          <p class="text-gray-400 text-center">
            Rest assured with our top-tier security measures protecting your
            financial data.
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  pageTransition: {
    name: "rotate",
  },
});
import { onMounted, ref } from "vue";

const fetures = ref(null);

onMounted(() => {
  const { $gsap, $ScrollTrigger } = useNuxtApp();

  if ($gsap && $ScrollTrigger) {
    $gsap.to(fetures.value, {
      scrollTrigger: {
        trigger: fetures.value,
        start: "top 80%",
        end: "top 20%",
        scrub: true,
      },
      duration: 2,
      scale: 2,
    });
  } else {
    console.error("GSAP or ScrollTrigger is not properly initialized.");
  }
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap");

html,
body {
  background-color: #0a0a0a;
  color: #ffffff;
  font-family: "Inter", sans-serif;
}
</style>
