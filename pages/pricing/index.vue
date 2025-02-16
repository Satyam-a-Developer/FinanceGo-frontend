<template>
  <div class="w-full py-12 bg-gray-900 flex justify-center items-center min-h-screen">
    <div class="max-w-5xl mx-auto px-4 mt-10">
      <div class="text-center mb-12 opacity-0" ref="headerRef">
        <h2 class="text-3xl font-bold text-white mb-4">Choose Your Plan</h2>
        <p class="text-gray-400">Select the perfect plan for your financial needs</p>
      </div>

      <div class="text-center mb-6">
        <button 
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" 
          @click="connectWallet">
          Connect Wallet
        </button>
      </div>

      <div class="grid md:grid-cols-2 gap-8">
        <div
          v-for="(plan, index) in plans"
          :key="plan.name"
          :ref="el => cardRefs[index] = el"
          class="relative overflow-hidden rounded-lg border bg-card text-card-foreground shadow-sm transition-all duration-200 cursor-pointer opacity-0"
          :class="{
            'border-blue-500 border-2 scale-105': selectedPlan === plan.name,
            'border-blue-500 border': plan.popular && selectedPlan !== plan.name,
            'border-gray-700': !plan.popular && selectedPlan !== plan.name
          }"
          @click="selectPlan(plan.name)"
          @mouseenter="onCardHover(index)"
          @mouseleave="onCardLeave(index)"
        >
          <div v-if="plan.popular" class="absolute top-4 right-4">
            <span class="bg-blue-500 text-white text-xs font-semibold px-3 py-1 rounded-full">
              Popular
            </span>
          </div>

          <div class="p-6">
            <h3 class="text-2xl font-bold text-white">{{ plan.name }}</h3>
            <p class="text-gray-400 mt-2">{{ plan.description }}</p>
          </div>

          <div class="p-6 pt-0">
            <div class="mb-6">
              <span class="text-4xl font-bold text-white">{{ plan.price }}</span>
              <span class="text-gray-400 ml-2">/month</span>
            </div>

            <ul class="space-y-3">
              <li
                v-for="(feature, featureIndex) in plan.features"
                :key="feature"
                :ref="el => featureRefs[`${index}-${featureIndex}`] = el"
                class="flex items-center gap-3 opacity-0"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 text-green-500"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <polyline points="20 6 9 17 4 12" />
                </svg>
                <span class="text-gray-300">{{ feature }}</span>
              </li>
            </ul>
          </div>

          <div class="p-6">
            <NuxtLink
              :to="plan.link"
              class="w-full py-3 rounded-lg font-semibold transition-colors text-center block"
              :class="plan.popular
                ? 'bg-blue-600 hover:bg-blue-700 text-white'
                : 'bg-gray-700 hover:bg-gray-600 text-white'"
            >
              {{ plan.buttonText }}
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { Connection, clusterApiUrl, PublicKey, Transaction, SystemProgram } from '@solana/web3.js';
import gsap from 'gsap';

const selectedPlan = ref(null);
const headerRef = ref(null);
const cardRefs = ref([]);
const featureRefs = ref({});
const walletConnected = ref(false);
const userWallet = ref(null);

const selectPlan = async (planName) => {
  selectedPlan.value = planName;

  cardRefs.value.forEach((card, index) => {
    gsap.to(card, {
      scale: selectedPlan.value === plans[index].name ? 1.05 : 1,
      duration: 0.3,
      ease: 'power2.out',
    });
  });

  if (!walletConnected.value) {
    alert('Please connect your wallet first.');
    return;
  }

  const plan = plans.find((p) => p.name === planName);
  const lamports = plan.price.replace('₹', '') * 100000; // Convert price to lamports

  await sendSolanaTransaction(lamports);
};

const sendSolanaTransaction = async (lamports) => {
  try {
    const connection = new Connection(clusterApiUrl('devnet'));
    const recipientPublicKey = new PublicKey('<RECIPIENT_PUBLIC_KEY>');
    const senderPublicKey = userWallet.value;

    const transaction = new Transaction().add(
      SystemProgram.transfer({
        fromPubkey: senderPublicKey,
        toPubkey: recipientPublicKey,
        lamports,
      }),
    );

    const { signature } = await window.solana.signAndSendTransaction(transaction);
    await connection.confirmTransaction(signature, 'processed');

    alert('Transaction successful!');
  } catch (error) {
    console.error('Transaction failed', error);
    alert('Transaction failed. Please try again.');
  }
};

const connectWallet = async () => {
  if (window.solana && window.solana.isPhantom) {
    try {
      const wallet = await window.solana.connect();
      walletConnected.value = true;
      userWallet.value = new PublicKey(wallet.publicKey);
      alert('Wallet connected!');
    } catch (error) {
      console.error('Wallet connection failed', error);
    }
  } else {
    alert('Phantom Wallet is not installed. Please install it to continue.');
  }
};

const onCardHover = (index) => {
  if (selectedPlan.value === plans[index].name) return;

  gsap.to(cardRefs.value[index], {
    y: -10,
    duration: 0.3,
    ease: 'power2.out',
  });
};

const onCardLeave = (index) => {
  if (selectedPlan.value === plans[index].name) return;

  gsap.to(cardRefs.value[index], {
    y: 0,
    duration: 0.3,
    ease: 'power2.out',
  });
};

onMounted(() => {
  gsap.to(headerRef.value, {
    opacity: 1,
    y: 0,
    duration: 1,
    ease: 'power2.out',
  });

  cardRefs.value.forEach((card, index) => {
    gsap.to(card, {
      opacity: 1,
      x: 0,
      duration: 0.8,
      delay: 0.2 + index * 0.2,
      ease: 'power2.out',
    });
  });

  Object.entries(featureRefs.value).forEach(([key, element], index) => {``
    gsap.to(element, {
      opacity: 1,
      x: 0,
      duration: 0.5,
      delay: 0.8 + index * 0.1,
      ease: 'power2.out',
    });
  });
});

const plans = [
  {
    name: 'Basic Plan',
    price: '₹500',
    description: 'Perfect for individuals starting their financial journey',
    features: [
      'Basic Portfolio Tracking',
      'Monthly Financial Reports',
      'Email Support',
      '2 Investment Categories',
      'Basic Analytics Dashboard',
    ],
    buttonText: 'Start Basic Plan',
    link: '/signup?plan=basic',
    popular: false,
  },
  {
    name: 'Premium Plan',
    price: '₹1,000',
    description: 'Ideal for serious investors and traders',
    features: [
      'Advanced Portfolio Tracking',
      'Real-time Financial Reports',
      'Priority 24/7 Support',
      'Unlimited Investment Categories',
      'Advanced Analytics & Insights',
      'Tax Reports Generation',
      'Multiple Portfolio Management',
    ],
    buttonText: 'Start Premium Plan',
    link: '/signup?plan=premium',
    popular: true,
  },
];
</script>
