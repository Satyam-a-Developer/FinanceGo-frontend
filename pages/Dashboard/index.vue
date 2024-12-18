<template>
  <div class="space-y-10">
    <div class="h-screen flex justify-center items-center">
      <h1 class="text-4xl font-bold">Scroll Down to Animate</h1>
    </div>
    <div ref="box" class="h-20 w-20 bg-blue-500 mx-auto"></div>
    <div class="h-screen"></div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const box = ref(null);

onMounted(() => {
  const { $gsap, $ScrollTrigger } = useNuxtApp();

  if ($gsap && $ScrollTrigger) {
    $gsap.to(box.value, {
      scrollTrigger: {
        trigger: box.value, // Element to trigger the animation
        start: 'top 80%', // When the top of the element hits 80% of the viewport
        end: 'top 20%', // When the top of the element hits 20% of the viewport
        scrub: true, // Smoothly animate on scroll
      },
      x: 300,
      rotation: 360,
      duration: 2,
    });
  } else {
    console.error('GSAP or ScrollTrigger is not properly initialized.');
  }
});
</script>
