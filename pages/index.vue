<template>
    <div
        class="bg-dark min-h-screen text-white font-inter flex flex-col items-center"
    >
        <div
            class="container mx-auto px-6 py-24 min-h-screen max-w-6xl mt-[100px]"
        >
            <div class="flex flex-col items-center justify-center gap-16">
                <div class="flex-1 space-y-8 text-center max-w-3xl">
                    <h1
                        ref="title"
                        class="text-4xl md:text-6xl font-bold text-white leading-tight"
                    >
                        <span class="inline-block opacity-0"
                            >Transform Your</span
                        >
                        <span class="text-blue-500 inline-block opacity-0"
                            >Financial Future</span
                        >
                        <br />
                        <span class="inline-block opacity-0"
                            >With Finance Go</span
                        >
                    </h1>

                    <p
                        ref="description"
                        class="text-gray-300 text-lg md:text-xl opacity-0"
                    >
                        Comprehensive financial management platform designed to
                        simplify your financial journey, track investments, and
                        make informed decisions.
                    </p>

                    <div
                        ref="buttons"
                        class="flex flex-col sm:flex-row gap-4 justify-center opacity-0"
                    >
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

                <div
                    ref="visualization"
                    class="flex-1 flex justify-center items-center opacity-0 relative w-full max-w-2xl"
                >
                    <!-- Animated Chart Background -->
                    <div
                        class="absolute inset-0 bg-gradient-to-b from-blue-500/10 to-transparent rounded-2xl"
                    ></div>

                    <!-- Financial Stats Grid -->
                    <div
                        class="grid grid-cols-2 gap-6 p-8 relative z-10 w-full"
                    >
                        <div
                            class="bg-gray-800/50 p-6 rounded-xl backdrop-blur-sm border border-gray-700 transform hover:scale-105 transition-transform"
                        >
                            <h3
                                class="text-lg font-semibold text-blue-400 mb-2"
                            >
                                Portfolio Value
                            </h3>
                            <div
                                ref="portfolioValue"
                                class="text-3xl font-bold"
                            >
                                $0
                            </div>
                            <div class="text-green-400 text-sm mt-2">
                                +12.5% ↑
                            </div>
                        </div>

                        <div
                            class="bg-gray-800/50 p-6 rounded-xl backdrop-blur-sm border border-gray-700 transform hover:scale-105 transition-transform"
                        >
                            <h3
                                class="text-lg font-semibold text-blue-400 mb-2"
                            >
                                Assets
                            </h3>
                            <div ref="assetCount" class="text-3xl font-bold">
                                0
                            </div>
                            <div class="text-blue-400 text-sm mt-2">
                                Diversified
                            </div>
                        </div>

                        <div
                            class="bg-gray-800/50 p-6 rounded-xl backdrop-blur-sm border border-gray-700 transform hover:scale-105 transition-transform"
                        >
                            <h3
                                class="text-lg font-semibold text-blue-400 mb-2"
                            >
                                ROI
                            </h3>
                            <div ref="roiValue" class="text-3xl font-bold">
                                0%
                            </div>
                            <div class="text-green-400 text-sm mt-2">
                                Above Market
                            </div>
                        </div>

                        <div
                            class="bg-gray-800/50 p-6 rounded-xl backdrop-blur-sm border border-gray-700 transform hover:scale-105 transition-transform"
                        >
                            <h3
                                class="text-lg font-semibold text-blue-400 mb-2"
                            >
                                Risk Score
                            </h3>
                            <div ref="riskScore" class="text-3xl font-bold">
                                0
                            </div>
                            <div class="text-yellow-400 text-sm mt-2">
                                Balanced
                            </div>
                        </div>
                    </div>

                    <!-- Floating Elements -->
                    <div
                        ref="floatingElements"
                        class="absolute inset-0 pointer-events-none"
                    >
                        <div
                            class="absolute top-0 right-0 w-96 h-20 bg-blue-500/20 rounded-full blur-xl"
                        ></div>
                        <div
                            class="absolute bottom-0 left-0 w-96 h-32 bg-purple-500/20 rounded-full blur-xl"
                        ></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Features Section -->
        <div class="container mx-auto px-6 py-16 max-w-6xl">
            <h2
                ref="features"
                class="text-3xl md:text-4xl font-bold text-center mb-12 opacity-0"
            >
                Our Powerful Features
            </h2>

            <div ref="featureCards" class="grid md:grid-cols-3 gap-8">
                <!-- Budget Tracking -->
                <div
                    class="bg-gray-900 p-6 rounded-xl border border-gray-800 hover:border-blue-600 transition opacity-0"
                >
                    <div class="text-blue-500 mb-4 flex justify-center"></div>
                    <h3 class="text-xl font-semibold mb-3 text-center">
                        Budget Tracking
                    </h3>
                    <p class="text-gray-400 text-center">
                        Effortlessly monitor your income, expenses, and
                        financial health with our intuitive tracking tools.
                    </p>
                </div>

                <!-- Investment Analysis -->
                <div
                    class="bg-gray-900 p-6 rounded-xl border border-gray-800 hover:border-blue-600 transition opacity-0"
                >
                    <div class="text-blue-500 mb-4 flex justify-center"></div>
                    <h3 class="text-xl font-semibold mb-3 text-center">
                        Investment Insights
                    </h3>
                    <p class="text-gray-400 text-center">
                        Gain deep insights into your investments with
                        comprehensive analysis and performance tracking.
                    </p>
                </div>

                <!-- Secure Transactions -->
                <div
                    class="bg-gray-900 p-6 rounded-xl border border-gray-800 hover:border-blue-600 transition opacity-0"
                >
                    <div class="text-blue-500 mb-4 flex justify-center"></div>
                    <h3 class="text-xl font-semibold mb-3 text-center">
                        Bank-Level Security
                    </h3>
                    <p class="text-gray-400 text-center">
                        Rest assured with our top-tier security measures
                        protecting your financial data.
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

const title = ref(null);
const description = ref(null);
const buttons = ref(null);
const visualization = ref(null);
const features = ref(null);
const featureCards = ref(null);
const portfolioValue = ref(null);
const assetCount = ref(null);
const roiValue = ref(null);
const riskScore = ref(null);
const floatingElements = ref(null);

onMounted(() => {
    const { $gsap, $ScrollTrigger } = useNuxtApp();

    if ($gsap && $ScrollTrigger) {
        // Hero section animations
        const tl = $gsap.timeline();

        // Animate title words separately
        tl.to(title.value.children, {
            y: 0,
            opacity: 1,
            duration: 0.8,
            stagger: 0.2,
            ease: "power3.out",
        });

        // Fade in description
        tl.to(
            description.value,
            {
                opacity: 1,
                y: 0,
                duration: 0.8,
                ease: "power2.out",
            },
            "-=0.4",
        );

        // Fade in buttons with slight bounce
        tl.to(
            buttons.value,
            {
                opacity: 1,
                y: 0,
                duration: 0.6,
                ease: "back.out(1.7)",
            },
            "-=0.4",
        );

        // Animate the visualization section
        tl.to(
            visualization.value,
            {
                opacity: 1,
                duration: 1,
                ease: "power2.out",
            },
            "-=0.2",
        );

        // Animate stats with counting effect
        $gsap.to(portfolioValue.value, {
            innerText: 248756,
            duration: 2,
            snap: { innerText: 1 },
            ease: "power2.out",
            prefix: "$",
        });

        $gsap.to(assetCount.value, {
            innerText: 156,
            duration: 2,
            snap: { innerText: 1 },
            ease: "power2.out",
        });

        $gsap.to(roiValue.value, {
            innerText: 24,
            duration: 2,
            snap: { innerText: 1 },
            ease: "power2.out",
            suffix: "%",
        });

        $gsap.to(riskScore.value, {
            innerText: 85,
            duration: 2,
            snap: { innerText: 1 },
            ease: "power2.out",
        });

        // Animate floating elements
        $gsap.to(floatingElements.value.children, {
            y: -20,
            duration: 2,
            ease: "none",
            yoyo: true,
            repeat: -1,
            stagger: 0.5,
        });

        // Features section animations
        $gsap.to(features.value, {
            scrollTrigger: {
                trigger: features.value,
                start: "top 80%",
                end: "top 50%",
                scrub: false,
            },
            opacity: 1,
            y: 0,
            duration: 1,
            ease: "power2.out",
        });

        // Animate feature cards
        const cards = Array.from(featureCards.value.children);
        $gsap.to(cards, {
            scrollTrigger: {
                trigger: featureCards.value,
                start: "top 80%",
                end: "top 50%",
                scrub: false,
            },
            opacity: 1,
            y: 0,
            duration: 0.8,
            stagger: 0.2,
            ease: "power2.out",
        });

        // Add hover effect to feature cards
        Array.from(featureCards.value.children).forEach((card) => {
            card.addEventListener("mouseenter", () => {
                $gsap.to(card, {
                    scale: 1.05,
                    duration: 0.3,
                    ease: "power2.out",
                });
            });

            card.addEventListener("mouseleave", () => {
                $gsap.to(card, {
                    scale: 1,
                    duration: 0.3,
                    ease: "power2.out",
                });
            });
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
