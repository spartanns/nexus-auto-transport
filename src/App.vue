<script setup>
import { ref, onMounted } from "vue";

const option1 = ref("");
const option2 = ref("");
const option3 = ref("");

const years = ref([]);
const makes = ref([]);
const models = ref([]);

onMounted(async () => {
  try {
    const res = await fetch("https://new.api.nexusautotransport.com/api/vehicles/years");
    const data = await res.json();
    years.value = data.data;
  } catch (err) {
    console.log(err);
  }
});

const fetchMakes = async () => {
  const res = await fetch(`https://new.api.nexusautotransport.com/api/vehicles/makes?year=${option1.value}`);
  const data = await res.json();
  makes.value = data.data;
}

const fetchModels = async () => {
  const res = await fetch(`https://new.api.nexusautotransport.com/api/vehicles/models?year=${option1.value}&make=${option2.value.name}`);
  const data = await res.json();
  models.value = data.data;
}
</script>

<template>
  <div class="bg-slate-800 h-screen w-full flex justify-center items-center gap-4 border border-gray-300 p-4">
    <div class="w-full">
      <label for="select1" class="block text-sm font-medium text-slate-200">Year</label>
      <select id="select1" v-model="option1" @change="fetchMakes"
        class="block w-full mt-1 rounded-md border-gray-300 shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
        <option disabled value="">Please select a year</option>
        <option v-for="option in years" :key="option" :value="option">{{ option }}</option>
      </select>
    </div>

    <div class="w-full">
      <label for="select2" class="block text-sm font-medium text-slate-200">Make</label>
      <select id="select2" v-model="option2" @change="fetchModels" :disabled="!option1"
        class="block w-full mt-1 rounded-md border-gray-300 shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
        <option disabled value="">Please select a make</option>
        <option v-for="option in makes" :key="option" :value="option">{{ option.name }}</option>
      </select>
    </div>

    <div class="w-full">
      <label for="select3" class="block text-sm font-medium text-slate-200">Model</label>
      <select id="select3" v-model="option3" :disabled="!option2"
        class="block w-full mt-1 rounded-md border-gray-300 shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
        <option disabled value="">Please select a model</option>
        <option v-for="option in models" :key="option" :value="option">{{ option.model }}</option>
      </select>
    </div>
  </div>
</template>
