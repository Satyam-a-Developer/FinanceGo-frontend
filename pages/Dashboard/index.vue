<template>
  <div class="min-h-screen flex flex-col p-8 mt-12 w-[100vw]">
    <!-- Navigation and Header Section -->
    <div class="flex flex-col items-start gap-4 mb-8 sticky top-[rem] z-10  shadow-md p-4 rounded-lg backdrop-blur">
      <h1 class="text-4xl font-bold text-blue-600">
        Welcome, <span class="text-gray-700">{{ username }}</span>
      </h1>
      <h2 v-if="namedata" class="text-xl font-medium text-gray-600 italic bg-gray-100 px-4 py-2 rounded-md shadow-md">
        Business: {{ namedata }}
      </h2>
    </div>

    <!-- Content Section -->
    <div class="flex-grow mt-4">
      <!-- Error State -->
      <div v-if="error" class="p-4 bg-red-50 border border-red-400 text-red-600 rounded-md shadow-md">
        <p>Error: {{ error }}</p>
      </div>

      <!-- Loading State -->
      <div v-else-if="isLoading" class="p-4 text-center">
        <p class="text-gray-500 animate-pulse">Loading...</p>
      </div>

      <!-- Content State -->
      <div v-else class="space-y-6">
        <!-- Business Details -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 p-4">
          <div
            class="gradient-box bg-gradient-to-r from-blue-400 via-blue-500 to-blue-600 text-white rounded-lg p-6 shadow-lg transition-transform transform hover:scale-105">
            <h3 class="text-lg font-semibold">Business Name</h3>
            <p class="mt-2">{{ messagedata.businessName }}</p>
          </div>
          <div
            class="gradient-box bg-gradient-to-br from-blue-300 to-blue-700 text-white rounded-lg p-6 shadow-lg transition-transform transform hover:scale-105">
            <h3 class="text-lg font-semibold">Period</h3>
            <p class="mt-2">{{ messagedata.period }}</p>
          </div>
          <div
            class="gradient-box bg-gradient-to-t from-sky-400 to-blue-500 text-white rounded-lg p-6 shadow-lg transition-transform transform hover:scale-105">
            <h3 class="text-lg font-semibold">Expected Income</h3>
            <p class="mt-2">{{ messagedata.expectedIncome }}</p>
          </div>
          <div
            class="gradient-box bg-gradient-to-bl from-blue-500 to-blue-900 text-white rounded-lg p-6 shadow-lg transition-transform transform hover:scale-105">
            <h3 class="text-lg font-semibold">Actual Income</h3>
            <p class="mt-2">{{ messagedata.actualIncome }}</p>
          </div>
          <div
            class="gradient-box bg-gradient-to-r from-cyan-300 via-blue-400 to-blue-600 text-white rounded-lg p-6 shadow-lg transition-transform transform hover:scale-105">
            <h3 class="text-lg font-semibold">Reason</h3>
            <p class="mt-2">{{ messagedata.reason }}</p>
          </div>
          <div
            class="gradient-box bg-gradient-to-tl from-indigo-400 via-blue-500 to-sky-300 text-white rounded-lg p-6 shadow-lg transition-transform transform hover:scale-105">
            <h3 class="text-lg font-semibold">Category</h3>
            <p class="mt-2">{{ messagedata.category }}</p>
          </div>
        </div>
        <!-- Chart Section -->
        <line-chart :data="chartData" :options="chartOptions" />
        <div
  class="gradient-box bg-gradient-to-r from-blue-400 via-blue-500 to-blue-600 rounded-lg p-6 shadow-lg transition-transform transform hover:scale-105 h-[20rem]">
  <h3 class="text-[3rem] font-semibold text-cyan-100">Ask AI for a prediction</h3>
  
  <!-- Input field added below -->
  <input 
    type="text" 
    class="mt-4 w-full p-3 rounded-md text-black border-none focus:outline-none focus:ring-2 focus:ring-blue-400"
    placeholder="Type your question here..." 
  />
</div>
      </div>
    </div>
  </div>
</template>

<script>
import { Line } from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement } from 'chart.js';

// Register chart.js components
ChartJS.register(Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement);

export default {
  name: 'GradientBoxes',
  components: {
    LineChart: Line
  },
  data() {
    return {
      username: "", // Stores the user's username
      namedata: "", // Stores the business name
      messagedata: null, // Stores the business details
      error: null, // Stores error messages
      isLoading: true, // Loading state
      chartData: {
        labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'], // Example months
        datasets: [
          {
            label: 'Income',
            data: [3000, 4000, 3500, 5000, 4500, 6000, 7000], // Example data
            fill: false,
            borderColor: '#42A5F5',
            tension: 0.1
          }
        ]
      },
      chartOptions: {
        responsive: true,
        plugins: {
          legend: {
            position: 'top'
          },
          title: {
            display: true,
            text: 'Business Income Over Time'
          }
        }
      }
    };
  },

  async mounted() {
    try {
      // Fetch user and business data
      await this.fetchUserData();
      await this.fetchBusinessData();
    } catch (err) {
      this.error = err.message || "An unexpected error occurred"; // Handle global errors
    } finally {
      this.isLoading = false; // Disable loading state
    }
  },

  methods: {
    async fetchUserData() {
      try {
        const response = await fetch("http://localhost:3003/dashboard", {
          method: "GET",
          credentials: "include", // Include cookies for authentication
        });

        if (!response.ok) {
          const errorData = await response.json();
          throw new Error(errorData.error || "Failed to fetch user data");
        }

        const data = await response.json();
        this.username = data.user.username; // Assign username from response
      } catch (error) {
        console.error("Error fetching user data:", error.message); // Log error
        throw error; // Rethrow to handle globally
      }
    },

    async fetchBusinessData() {
      try {
        const response = await fetch("http://localhost:3003/dashboard/business", {
          method: "GET",
          credentials: "include", // Include cookies for authentication
        });

        const data = await response.json();
        console.log("Fetched business data:", data); // Debugging log

        if (response.ok) {
          this.namedata = data.user.businessName; // Assign business name
          this.messagedata = data.messageData; // Assign business details
        } else {
          throw new Error(data.error || "Failed to fetch business data");
        }
      } catch (error) {
        console.error("Error fetching business data:", error.message); // Log error
        throw error; // Rethrow to handle globally
      }
    },
  },
};
</script>

<style scoped>
/* Add your styles here */
</style>
