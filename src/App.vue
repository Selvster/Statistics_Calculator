<script setup>
import { ref, computed } from 'vue';

const list_items = ref([]);
const list_item = ref('');
const resultsVisible = ref(false);
let Mean = ref(0);
let Median = ref(0);
let Mode = ref(0);
let standard_devation = ref(0);

const addToList = () => {
  list_items.value.push(Number(list_item.value));
  list_item.value = '';
  resultsVisible.value = false;
};

const calculate = () => {
  const sortedList = [...list_items.value].sort((a, b) => a - b);
  calculateStatistics(sortedList);
  resultsVisible.value = true;
};

const calculateStatistics = (sortedList) => {
  Mean.value = (
    list_items.value.reduce((a, b) => a + b, 0) / list_items.value.length
  ).toFixed(2);
  Median.value = sortedList[Math.floor(sortedList.length / 2)];
  Mode.value = sortedList.sort(
    (a, b) =>
      list_items.value.filter((v) => v === a).length -
      list_items.value.filter((v) => v === b).length
  ).pop();
  standard_devation.value = Math.sqrt(
    list_items.value
      .map((x) => Math.pow(x - Mean.value, 2))
      .reduce((a, b) => a + b, 0) / list_items.value.length
  ).toFixed(2);
};


</script>

<template>
  <div>
    <!-- First Calculator Box -->
    <div class="bg-white p-8 rounded shadow-md w-96 mr-4">
      <h2 class="text-2xl font-semibold mb-4 text-blue-500">
        Statistics Calculator
      </h2>

      <form action="#" method="post">
        <div class="mb-4">
          <input
            type="text"
            id="input"
            name="input"
            class="mt-1 mb-2 p-2 w-full border rounded-md bg-white border-blue-500 text-blue-500"
            placeholder="42,53,..."
            v-model="list_item"
          />
          <div
            class="flex items-center bg-blue-500 text-white px-4 py-2 rounded-md"
            v-if="list_items.length > 0"
          >
            <span class="text-2xl font-bold">
              <span v-for="(item, index) in list_items">
                {{ item }}{{ index < list_items.length - 1 ? ', ' : '' }}
              </span>
            </span>
          </div>
        </div>

        <hr />
        <br />
        <button
          @click.prevent="addToList"
          class="bg-blue-500 mr-2 text-white px-4 py-2 rounded-md hover:bg-blue-500 focus:outline-none focus:ring focus:border-blue-300"
        >
          Add to List
        </button>
        <button
          @click.prevent="calculate"
          type="submit"
          class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-500 focus:outline-none focus:ring focus:border-blue-300"
        >
          Calculate
        </button>
      </form>
    </div>
    <h4 class="text-1xl mt-1 font-semibold mb-4 text-white">Supervised By Dr/Fatma Kamal</h4>

    <!-- Second Calculator Box with Results -->
    <div v-if="resultsVisible" class="bg-white p-8 rounded shadow-md w-96 mt-4">
      <h2 class="text-2xl font-semibold mb-4 text-blue-500">Results</h2>
      <p class="text-1xl font-semibold mb-4 text-blue-500">Mean: {{ Mean }}</p>
      <p class="text-1xl font-semibold mb-4 text-blue-500">Median: {{ Median }}</p>
      <p class="text-1xl font-semibold mb-4 text-blue-500">Mode: {{ Mode }}</p>
      <p class="text-1xl font-semibold mb-4 text-blue-500">Standard Devation : {{ standard_devation }}</p>
    </div>
  </div>

</template>
