<template>
  <div class="min-h-screen flex flex-col w-[100vw]" ref="mainContainer">
      <!-- Loading State -->
      <div v-if="isLoading" class="flex items-center justify-center h-screen">
          <div class="p-4 text-center">
              <p class="text-2xl text-gray-500 animate-pulse">
                  Loading your dashboard...
              </p>
          </div>
      </div>

      <!-- Main Content -->
      <div v-else class="flex flex-col">
          <!-- Navigation and Header Section -->
          <div
              ref="header"
              class="flex flex-col items-center bg-[#14213d] gap-4 mb-8 text-[3rem] z-10 shadow-lg p-4 backdrop-blur opacity-0"
          >
              <h1 class="font-bold text-blue-600 mt-[5rem]">
                  Welcome, <span class="text-gray-700">{{ username }}</span>
              </h1>
              <h2
                  v-if="namedata"
                  class="text-xl font-medium text-gray-600 italic bg-gray-100 px-4 py-2 rounded-md shadow-md"
              >
                  Business: {{ namedata }}
              </h2>
          </div>

          <!-- Content Section -->
          <div class="flex-grow mt-4 px-4">
              <!-- Error State -->
              <div
                  v-if="error"
                  ref="errorMessage"
                  class="p-4 bg-red-50 border border-red-400 text-red-600 rounded-md shadow-md opacity-0"
              >
                  <p>Error: not data-base is here </p>
              </div>

              <!-- Content State -->
              <div v-else class="space-y-6">
                  <!-- Business Details -->
                  <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                      <div
                          v-for="(box, index) in boxes"
                          :key="index"
                          :ref="(el) => (boxRefs[index] = el)"
                          :class="box.class"
                          class="gradient-box text-white rounded-lg p-6 shadow-lg transition-transform transform opacity-0"
                      >
                          <h3 class="text-lg font-semibold">{{ box.title }}</h3>
                          <p class="mt-2">{{ messagedata?.[box.dataKey] ?? 'N/A' }}</p>
                      </div>
                  </div>

                  <!-- Chart Section -->
                  <div ref="chartContainer" class="opacity-0">
                      <line-chart :data="chartData" :options="chartOptions" />
                  </div>

                  <!-- AI Section -->
                  <div
                      ref="aiSection"
                      class="gradient-box bg-gradient-to-r from-blue-400 via-blue-500 to-blue-600 rounded-lg p-6 shadow-lg transition-transform transform opacity-0"
                  >
                      <h3 class="text-[3rem] font-semibold text-cyan-100">
                          Ask AI for a Prediction
                      </h3>
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
                              class="bg-white text-blue-600 px-4 py-2 rounded-md hover:bg-blue-50 transition-colors duration-200 disabled:opacity-50"
                          >
                              {{ isLoadingAI ? 'Getting Prediction...' : 'Get Prediction' }}
                          </button>

                          <!-- AI Response Section -->
                          <div
                              v-if="aiResponse"
                              class="mt-4 bg-white bg-opacity-90 text-gray-800 p-4 rounded-md"
                          >
                              <p class="font-medium">AI Prediction:</p>
                              <p class="mt-2">{{ aiResponse }}</p>
                          </div>
                      </div>
                  </div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { Line } from 'vue-chartjs';
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  CategoryScale,
  LinearScale,
  PointElement,
} from 'chart.js';
import gsap from 'gsap';

ChartJS.register(Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement);

const GROQ_API_KEY = process.env.GROQ_API_KEY;

export default {
  name: 'GradientBoxes',
  components: {
      LineChart: Line,
  },
  setup() {
      const boxRefs = ref([]);
      const header = ref(null);
      const chartContainer = ref(null);
      const aiSection = ref(null);
      const aiInput = ref(null);
      const errorMessage = ref(null);
      const mainContainer = ref(null);
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
          aiQuery,
          aiResponse,
          isLoadingAI,
      };
  },
  data() {
      return {
          username: '',
          namedata: '',
          messagedata: null,
          error: null,
          isLoading: true,
          boxes: [
              { title: 'Business Name', dataKey: 'businessName', class: 'bg-gradient-to-r from-blue-400 via-blue-500 to-blue-600' },
              { title: 'Period', dataKey: 'period', class: 'bg-gradient-to-br from-blue-300 to-blue-700' },
              { title: 'Expected Income', dataKey: 'expectedIncome', class: 'bg-gradient-to-t from-sky-400 to-blue-500' },
              { title: 'Actual Income', dataKey: 'actualIncome', class: 'bg-gradient-to-bl from-blue-500 to-blue-900' },
              { title: 'Reason', dataKey: 'reason', class: 'bg-gradient-to-r from-cyan-300 via-blue-400 to-blue-600' },
              { title: 'Category', dataKey: 'category', class: 'bg-gradient-to-tl from-indigo-400 via-blue-500 to-sky-300' },
          ],
          chartData: {
              labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
              datasets: [
                  {
                      label: 'Income',
                      data: [3000, 4000, 3500, 5000, 4500, 6000, 7000],
                      fill: false,
                      borderColor: '#42A5F5',
                      tension: 0.1,
                  },
              ],
          },
          chartOptions: {
              responsive: true,
              plugins: {
                  legend: { position: 'top' },
                  title: { display: true, text: 'Business Income Over Time' },
              },
          },
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
                      Authorization: `Bearer ${GROQ_API_KEY}`,
                      'Content-Type': 'application/json',
                  },
                  body: JSON.stringify({
                      model: 'mixtral-8x7b-32768',
                      messages: [
                          {
                              role: 'system',
                              content: `You are a business analyst AI assistant. Use the following business data to provide insights and predictions:
                              Business Name: ${this.messagedata?.businessName ?? 'N/A'}
                              Period: ${this.messagedata?.period ?? 'N/A'}
                              Expected Income: ${this.messagedata?.expectedIncome ?? 'N/A'}
                              Actual Income: ${this.messagedata?.actualIncome ?? 'N/A'}
                              Category: ${this.messagedata?.category ?? 'N/A'}`,
                          },
                          { role: 'user', content: this.aiQuery },
                      ],
                      temperature: 0.7,
                      max_tokens: 1000,
                  }),
              });

              if (!response.ok) throw new Error('Failed to get AI prediction');
              const data = await response.json();
              this.aiResponse = data.choices[0].message.content;
          } catch (error) {
              console.error('AI Prediction error:', error);
              this.aiResponse = 'Sorry, I encountered an error while generating the prediction. Please try again.';
          } finally {
              this.isLoadingAI = false;
          }
      },

      initializeAnimations() {
          const tl = gsap.timeline();
          const elements = [
              { ref: this.header, delay: 0 },
              ...this.boxRefs.map((box, i) => ({ ref: box, delay: i * 0.1 })),
              { ref: this.chartContainer, delay: 0.2 },
              { ref: this.aiSection, delay: 0.4 },
          ];

          elements.forEach(({ ref, delay }) => {
              if (ref) {
                  tl.to(ref, {
                      opacity: 1,
                      y: 0,
                      duration: 0.6,
                      ease: 'power2.out',
                  }, delay);
              }
          });

          if (this.error && this.errorMessage) {
              tl.to(this.errorMessage, {
                  opacity: 1,
                  y: 0,
                  duration: 0.5,
                  ease: 'power2.out',
              });
          }
      },

      onInputFocus() {
          gsap.to(this.aiInput, { scale: 1.02, duration: 0.3, ease: 'power2.out' });
      },

      onInputBlur() {
          gsap.to(this.aiInput, { scale: 1, duration: 0.3, ease: 'power2.out' });
      },

      async fetchData() {
          try {
              const [userResponse, businessResponse] = await Promise.all([
                  fetch('http://localhost:3003/dashboard', { method: 'GET', credentials: 'include' }),
                  fetch('http://localhost:3003/dashboard/business', { method: 'GET', credentials: 'include' }),
              ]);

              if (!userResponse.ok || !businessResponse.ok) {
                  const errorData = await (userResponse.ok ? businessResponse : userResponse).json();
                  throw new Error(errorData.error || 'Failed to fetch data');
              }

              const userData = await userResponse.json();
              const businessData = await businessResponse.json();

              this.username = userData.user.username;
              this.namedata = businessData.user.businessName;
              this.messagedata = businessData.messageData;
          } catch (error) {
              this.error = error.message || 'An unexpected error occurred';
          } finally {
              this.isLoading = false;
              this.$nextTick(() => this.initializeAnimations());
          }
      },
  },
  mounted() {
      this.fetchData();
  },
};
</script>

<style scoped>
.gradient-box {
  transform: translateY(20px);
}
</style>
