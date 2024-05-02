<script setup lang="ts">
import { ref } from "vue";

const rawText = ref("");
const outputText = ref("");
const testJSON = ref([
  {
    Name: "John Doe",
    Age: 35,
    City: "New York",
  },
  {
    Name: "Jane Smith",
    Age: 28,
    City: "Los Angeles",
  },
  {
    Name: "Michael Johnson",
    Age: 42,
    City: "Chicago",
  },
]);
const convertToCSV = (json: any) => {
  if (json.length === 0) {
    return console.error("text area cannot be empty");
  }
  const headers = Object.keys(json[0]);
  const values = ref("");
  // console.log(Object.values(json[0]));

  for (let i = 0; i < Object.keys(json).length; i++) {
    console.log(Object.values(json[i]));
    values.value += Object.values(json[i]).join("\n");

    console.log(values.value);
  }
  outputText.value += headers;
  outputText.value += values.value;
};
</script>

<template>
  <div
    class="flex flex-col items-center justify-center min-h-screen bg-teal-100"
  >
    <div class="grid grid-cols-2 gap-4">
      <div class="w-96">
        <h3 class="w-fit text-xl font-bold text-center">
          Paste your JSON data here
        </h3>
        <textarea class="p-8 w-96 h-96 shadow-lg" v-model="rawText"></textarea>
      </div>
      <div class="w-96">
        <h3 class="w-fit text-xl font-bold text-center">Here's your result</h3>
        <textarea
          class="p-8 w-96 h-96 shadow-lg"
          v-model="outputText"
        ></textarea>
      </div>
    </div>
    <button
      class="w-fit h-12 bg-black text-white rounded-lg p-2 m-2"
      @click="convertToCSV(testJSON)"
    >
      CONVERT
    </button>
  </div>
</template>
