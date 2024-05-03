<script setup lang="ts">
import { ref } from "vue";

const rawText = ref("");
const outputText = ref("");
let converted;

const convertToCSV = (json: string) => {
  if (json.length === 0) {
    return console.error("text area cannot be empty");
  }

  try {
    converted = JSON.parse(json);
  } catch (error) {
    console.error(error);
    return;
  }

  const values = ref("");

  values.value += Object.keys(converted[0]);
  values.value += "\n";

  converted.forEach((item: any) => {
    values.value += Object.values(item).join(",");
    values.value += "\n";
  });
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
      @click="convertToCSV(rawText)"
    >
      CONVERT
    </button>
  </div>
</template>
