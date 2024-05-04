<script setup lang="ts">
import { computed, ref } from "vue";
const rawText = ref("");
const outputText = ref("");
let converted;
const hasError = computed(
  () => outputText.value === "" || outputText.value === "undefined"
);
const convertToCSV = (json: string) => {
  outputText.value = "";

  if (json.length === 0) {
    return console.error("text area cannot be empty");
  }

  try {
    converted = JSON.parse(json);
  } catch (error) {
    return console.error(error);
  }

  const flattenObject = (obj: object, prefix = "") => {
    const result = {};
    for (const key in obj) {
      const value = obj[key];
      const newKey = prefix ? `${prefix}.${key}` : key;
      if (typeof value === "object" && value !== null) {
        Object.assign(result, flattenObject(value, newKey));
      } else {
        result[newKey] = value as string;
      }
    }
    return result;
  };

  const flattenedObject = flattenObject(converted);

  let headers = Object.keys(flattenedObject);
  let rows = [Object.values(flattenedObject)];

  outputText.value = [headers, ...rows].map((row) => row.join(",")).join("\n");
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
        <textarea
          class="p-8 w-96 h-96 shadow-lg"
          v-model="rawText"
          :class="[
            hasError
              ? 'outline-red-500 outline-2'
              : 'outline-green-500 outline-2',
          ]"
        ></textarea>
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
