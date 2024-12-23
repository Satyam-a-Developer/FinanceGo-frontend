<template>
  <div class="min-h-screen flex flex-col p-8 mt-12 w-[100vw]" ref="mainContainer">
    <!-- Loading State -->
    <div v-if="isLoading" class="flex items-center justify-center h-screen">
      <div class="p-4 text-center">
        <p class="text-2xl text-gray-500 animate-pulse">Loading your dashboard...</p>
      </div>
    </div>

    <template v-else>
      <!-- Navigation and Header Section -->
      <div ref="header" class="flex flex-col items-start gap-4 mb-8 text-[3rem] z-10 shadow-md p-4 rounded-lg backdrop-blur opacity-0">
        <h1 class="font-bold text-blue-600">
          Welcome, <span class="text-gray-700">{{ username }}</span>
        </h1>
        <h2 v-if="namedata" class="text-xl font-medium text-gray-600 italic bg-gray-100 px-4 py-2 rounded-md shadow-md">
          Business: {{ namedata }}
        </h2>
      </div>

      <!-- Content Section -->
      <div class="flex-grow mt-4">
        <!-- Error State -->
        <div v-if="error" ref="errorMessage" class="p-4 bg-red-50 border border-red-400 text-red-600 rounded-md shadow-md opacity-0">
          <p>Error: {{ error }}</p>
        </div>

        <!-- Content State -->
        <div v-else class="space-y-6">
          <!-- Business Details -->
          <div class="grid grid-cols-1 md:grid-cols-3 gap-6 p-4">
            <div
              v-for="(box, index) in boxes"
              :key="index"
              :ref="el => boxRefs[index] = el"
              :class="box.class"
              class="gradient-box text-white rounded-lg p-6 shadow-lg transition-transform transform opacity-0">
              <h3 class="text-lg font-semibold">{{ box.title }}</h3>
              <p class="mt-2">{{ messagedata[box.dataKey] }}</p>
            </div>
          </div>
          
          <!-- Chart Section -->
          <div ref="chartContainer" class="opacity-0">
            <line-chart :data="chartData" :options="chartOptions" />
          </div>
          
          <!-- AI Section -->
          <div
            ref="aiSection"
            class="gradient-box bg-gradient-to-r from-blue-400 via-blue-500 to-blue-600 rounded-lg p-6 shadow-lg transition-transform transform opacity-0">
            <h3 class="text-[3rem] font-semibold text-cyan-100">Ask AI for a prediction</h3>
            <div class="flex flex-col gap-4">
              <input 
                v-model="aiQuery"
                type="text" 
                class="mt-4 w-full p-3 rounded-md text-black border-none focus:outline-none focus:ring-2 focus:ring-blue-400"
                placeholder="Type your question here..." 
                @focus="onInputFocus"
                @blur="onInputBlur"
                @keyup.enter="getAIPrediction"
                ref="aiInput"
              />
              <button 
                @click="getAIPrediction"
                :disabled="isLoadingAI"
                class="bg-white text-blue-600 px-4 py-2 rounded-md hover:bg-blue-50 transition-colors duration-200 disabled:opacity-50">
                {{ isLoadingAI ? 'Getting prediction...' : 'Get Prediction' }}
              </button>
              
              <!-- AI Response Section -->
              <div v-if="aiResponse" class="mt-4 bg-white bg-opacity-90 text-gray-800 p-4 rounded-md">
                <p class="font-medium">AI Prediction:</p>
                <p class="mt-2">{{ aiResponse }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { Line } from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement } from 'chart.js';
import gsap from 'gsap';

ChartJS.register(Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement);

const GROQ_API_KEY =  process.env.GROQ_API_KEY;

export default {
  name: 'GradientBoxes',
  components: {
    LineChart: Line
  },
  setup() {
    const boxRefs = ref([]);
    const header = ref(null);
    const chartContainer = ref(null);
    const aiSection = ref(null);
    const aiInput = ref(null);
    const errorMessage = ref(null);
    const mainContainer = ref(null);
    const dataLoaded = ref(false);
    const aiQuery = ref('');
    const aiResponse = ref('');
    const isLoadingAI = ref(false);

    return {
      boxRefs,
      header,
      chartContainer,
      aiSection,
      aiInput,
      errorMessage,
      mainContainer,
      dataLoaded,
      aiQuery,
      aiResponse,
      isLoadingAI
    };
  },
  data() {
    return {
      username: "",
      namedata: "",
      messagedata: null,
      error: null,
      isLoading: true,
      boxes: [
        { title: 'Business Name', dataKey: 'businessName', class: 'bg-gradient-to-r from-blue-400 via-blue-500 to-blue-600' },
        { title: 'Period', dataKey: 'period', class: 'bg-gradient-to-br from-blue-300 to-blue-700' },
        { title: 'Expected Income', dataKey: 'expectedIncome', class: 'bg-gradient-to-t from-sky-400 to-blue-500' },
        { title: 'Actual Income', dataKey: 'actualIncome', class: 'bg-gradient-to-bl from-blue-500 to-blue-900' },
        { title: 'Reason', dataKey: 'reason', class: 'bg-gradient-to-r from-cyan-300 via-blue-400 to-blue-600' },
        { title: 'Category', dataKey: 'category', class: 'bg-gradient-to-tl from-indigo-400 via-blue-500 to-sky-300' }
      ],
      chartData: {
        labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
        datasets: [{
          label: 'Income',
          data: [3000, 4000, 3500, 5000, 4500, 6000, 7000],
          fill: false,
          borderColor: '#42A5F5',
          tension: 0.1
        }]
      },
      chartOptions: {
        responsive: true,
        plugins: {
          legend: { position: 'top' },
          title: { display: true, text: 'Business Income Over Time' }
        }
      }
    };
  },
  methods: {
    async getAIPrediction() {
      if (!this.aiQuery || this.isLoadingAI) return;

      this.isLoadingAI = true;
      try {
        const response = await fetch('https://api.groq.com/openai/v1/chat/completions', {
          method: 'POST',
          headers: {
            'Authorization': `Bearer ${GROQ_API_KEY}`,
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            model: 'mixtral-8x7b-32768',
            messages: [
              {
                role: 'system',
                content: `You are a business analyst AI assistant. Use the following business data to provide insights and predictions:
                Business Name: ${this.messagedata?.businessName}
                Period: ${this.messagedata?.period}
                Expected Income: ${this.messagedata?.expectedIncome}
                Actual Income: ${this.messagedata?.actualIncome}
                Category: ${this.messagedata?.category}`
              },
              {
                role: 'user',
                content: this.aiQuery
              }
            ],
            temperature: 0.7,
            max_tokens: 1000
          })
        });

        if (!response.ok) {
          throw new Error('Failed to get AI prediction');
        }

        const data = await response.json();
        this.aiResponse = data.choices[0].message.content;

        // Animate the response appearance
        gsap.from(this.$el.querySelector('.ai-response'), {
          opacity: 0,
          y: 20,
          duration: 0.5,
          ease: 'power2.out'
        });

      } catch (error) {
        console.error('AI Prediction error:', error);
        this.aiResponse = 'Sorry, I encountered an error while generating the prediction. Please try again.';
      } finally {
        this.isLoadingAI = false;
      }
    },

    initializeAnimations() {
      if (!this.isLoading && this.messagedata) {
        const tl = gsap.timeline();

        tl.to(this.header, {
          opacity: 1,
          y: 0,
          duration: 1,
          ease: 'power3.out'
        });

        this.boxRefs.forEach((box, index) => {
          tl.to(box, {
            opacity: 1,
            y: 0,
            duration: 0.6,
            ease: 'power2.out'
          }, `-=0.4`);
        });

        tl.to(this.chartContainer, {
          opacity: 1,
          y: 0,
          duration: 0.8,
          ease: 'power2.out'
        }, `-=0.2`);

        tl.to(this.aiSection, {
          opacity: 1,
          y: 0,
          duration: 0.8,
          ease: 'power2.out'
        }, `-=0.4`);
      }
    },

    onInputFocus() {
      gsap.to(this.aiInput, {
        scale: 1.02,
        duration: 0.3,
        ease: 'power2.out'
      });
    },

    onInputBlur() {
      gsap.to(this.aiInput, {
        scale: 1,
        duration: 0.3,
        ease: 'power2.out'
      });
    },

    async fetchUserData() {
      try {
        const response = await fetch("http://localhost:3003/dashboard", {
          method: "GET",
          credentials: "include",
        });

        if (!response.ok) {
          const errorData = await response.json();
          throw new Error(errorData.error || "Failed to fetch user data");
        }

        const data = await response.json();
        this.username = data.user.username;
      } catch (error) {
        console.error("Error fetching user data:", error.message);
        throw error;
      }
    },

    async fetchBusinessData() {
      try {
        const response = await fetch("http://localhost:3003/dashboard/business", {
          method: "GET",
          credentials: "include",
        });

        const data = await response.json();

        if (response.ok) {
          this.namedata = data.user.businessName;
          this.messagedata = data.messageData;
        } else {
          throw new Error(data.error || "Failed to fetch business data");
        }
      } catch (error) {
        console.error("Error fetching business data:", error.message);
        throw error;
      }
    }
  },
  async mounted() {
    try {
      await this.fetchUserData();
      await this.fetchBusinessData();
      this.isLoading = false;
      
      this.$nextTick(() => {
        setTimeout(() => {
          this.initializeAnimations();
        }, 100);
      });
    } catch (err) {
      this.error = err.message || "An unexpected error occurred";
      this.isLoading = false;
      
      if (this.errorMessage) {
        gsap.to(this.errorMessage, {
          opacity: 1,
          y: 0,
          duration: 0.5,
          ease: 'power2.out'
        });
      }
    }
  }
};
</script>

<style scoped>
.gradient-box {
  transform: translateY(20px);
}
</style>