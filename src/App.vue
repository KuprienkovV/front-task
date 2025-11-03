<template>
  <div class="max-w-lg mx-auto py-20">
    <AgeInput 
      label="Samuel is"
      image-url="student.jpg"
      caption="hours old"
      v-model="inputValue"
    />
  </div>
</template>
<script setup lang="ts">
import AgeInput from "@/components/NumericInput.vue";
import { ref } from "vue";

const inputValue = ref('7');
</script>
