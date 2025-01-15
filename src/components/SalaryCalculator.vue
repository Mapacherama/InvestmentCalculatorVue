<template>
    <div class="min-h-screen flex flex-col items-center justify-center bg-gray-100">
      <div class="w-full max-w-md bg-white p-6 rounded shadow-md">
        <h1 class="text-2xl font-bold text-center mb-4">Salary Allocation Calculator</h1>
  
        <!-- Form Section -->
        <form @submit.prevent="calculateAllocation" class="space-y-4">
          <div>
            <label class="block text-gray-700">Monthly Salary (€):</label>
            <input
              type="number"
              v-model="salary"
              class="w-full px-3 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter your monthly salary"
              required
            />
          </div>
  
          <div>
            <label class="block text-gray-700">Percentage for ETFs:</label>
            <input
              type="number"
              v-model="percentages.ETFs"
              class="w-full px-3 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter percentage"
              required
            />
          </div>
  
          <div>
            <label class="block text-gray-700">Percentage for Silver/Gold:</label>
            <input
              type="number"
              v-model="percentages['Silver/Gold']"
              class="w-full px-3 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter percentage"
              required
            />
          </div>
  
          <div>
            <label class="block text-gray-700">Percentage for Microsoft/NVIDIA:</label>
            <input
              type="number"
              v-model="percentages['Microsoft/NVIDIA']"
              class="w-full px-3 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter percentage"
              required
            />
          </div>
  
          <div>
            <label class="block text-gray-700">Percentage for Groceries:</label>
            <input
              type="number"
              v-model="percentages.Groceries"
              class="w-full px-3 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter percentage"
              required
            />
          </div>
  
          <button
            type="submit"
            class="w-full bg-blue-500 text-white py-2 rounded hover:bg-blue-600"
          >
            Calculate
          </button>
        </form>
  
        <!-- Results Section -->
        <div v-if="result" class="mt-6">
          <h2 class="text-xl font-semibold text-center">Allocation Breakdown</h2>
          <ul class="mt-4 space-y-2">
            <li
              v-for="(amount, category) in result"
              :key="category"
              class="flex justify-between bg-gray-50 p-2 rounded shadow"
            >
              <span>{{ category }}</span>
              <span>€{{ amount }}</span>
            </li>
          </ul>
        </div>
  
        <!-- Error Section -->
        <div v-if="error" class="mt-4 text-red-500 text-center">
          {{ error }}
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        salary: "", // Monthly salary input
        percentages: {
          ETFs: "",
          "Silver/Gold": "",
          "Microsoft/NVIDIA": "",
          Groceries: "",
        }, // Input percentages
        result: null, // API response data
        error: null, // Error message
      };
    },
    methods: {
      async calculateAllocation() {
        try {
          this.error = null; // Clear any previous error
  
          const response = await fetch("http://127.0.0.1:5000/calculate", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              salary: this.salary,
              percentages: this.percentages,
            }),
          });
  
          if (!response.ok) {
            throw new Error(`Error: ${response.status} ${response.statusText}`);
          }
  
          this.result = await response.json(); // Parse the JSON response
        } catch (error) {
          this.error = error.message || "An unexpected error occurred.";
          this.result = null; // Clear results if there's an error
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* Optional custom styling */
  </style>
  