<template>
    <div class="max-w-2xl mx-auto p-6 mt-[100px]">
      <form @submit.prevent="handleSubmit" class="space-y-6 text-black">
        <div>
          <label for="businessName " class="form-label">Business Name</label>
          <input
            id="businessName"
            v-model="formData.businessName"
            type="text"
            required
            class="form-input"
          />
        </div>
  
        <div>
          <label for="period" class="form-label">Financial Period</label>
          <input
            id="period"
            v-model="formData.period"
            type="month"
            required
            class="form-input"
          />
        </div>
  
        <div>
          <label for="expectedIncome" class="form-label">Expected Income ($)</label>
          <input
            id="expectedIncome"
            v-model="formData.expectedIncome"
            type="number"
            min="0"
            step="0.01"
            required
            class="form-input"
          />
        </div>
  
        <div>
          <label for="actualIncome" class="form-label">Actual Income ($)</label>
          <input
            id="actualIncome"
            v-model="formData.actualIncome"
            type="number"
            min="0"
            step="0.01"
            required
            class="form-input"
          />
        </div>
  
        <div>
          <label for="reason" class="form-label">Reason for Loss</label>
          <textarea
            id="reason"
            v-model="formData.reason"
            rows="3"
            class="form-input"
            required
          ></textarea>
        </div>
  
        <div>
          <label for="category" class="form-label">Loss Category</label>
          <select
            id="category"
            v-model="formData.category"
            class="form-input"
            required
          >
            <option value="">Select a category</option>
            <option value="operational">Operational</option>
            <option value="market">Market Conditions</option>
            <option value="emergency">Emergency/Crisis</option>
            <option value="seasonal">Seasonal Impact</option>
            <option value="other">Other</option>
          </select>
        </div>
  
        <div class="flex justify-between items-center pt-4">
          <div class="text-lg font-semibold">
            Total Loss: ${{ calculateLoss }}
          </div>
          <button type="submit" class="btn-primary">
            Record Loss
          </button>
        </div>
      </form>
  
      <div v-if="showSuccess" class="mt-4 p-4 bg-green-100 text-green-700 rounded-lg">
        Loss record saved successfully!
      </div>
    </div>
  </template>
  
  <script setup>
  import { useRouter } from 'vue-router'
  const router = useRouter()
  const formData = ref({
    businessName: '',
    period: '',
    expectedIncome: '',
    actualIncome: '',
    reason: '',
    category: ''
  });
  
  const showSuccess = ref(false);
  
  const calculateLoss = computed(() => {
    const expected = Number(formData.value.expectedIncome) || 0;
    const actual = Number(formData.value.actualIncome) || 0;
    return (expected - actual).toFixed(2);
  });
  
  const handleSubmit = () => {
    // Here you would typically send the data to your backend

    fetch('http://localhost:3003/bussinessForm/Form', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    "businessName": formData.value.businessName,
    "period": formData.value.period,
    "expectedIncome": String(formData.value.expectedIncome), // Convert to string
    "actualIncome": String(formData.value.actualIncome), // Convert to string
    "reason": formData.value.reason, // Ensure this field is properly populated
    "category": formData.value.category
  })
});

    console.log('Form submitted:', {
      ...formData.value,
      totalLoss: calculateLoss.value
    });
    
    // Show success message
    showSuccess.value = true;
    router.push('/dashboard')
    setTimeout(() => {
      showSuccess.value = false;
    }, 3000);
  
    // Reset form
    formData.value = {
      businessName: '',
      period: '',
      expectedIncome: '',
      actualIncome: '',
      reason: '',
      category: ''
    };
  };
  </script>