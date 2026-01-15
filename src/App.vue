<script setup lang="ts">
import { ref } from "vue";
import GitHubButton from "@/components/GitHubButton.vue";
import HelloWorldCard from "@/components/HelloWorldCard.vue";
import CalculatorCard from "@/components/CalculatorCard.vue";
import WindowSettingsCard from "@/components/WindowSettingsCard.vue";

const backgroundColor = ref<string>("#324d70");
</script>

<template>
  <GitHubButton />
  <div class="app-container" :style="{ backgroundColor }">
    <HelloWorldCard />
    <CalculatorCard />
    <WindowSettingsCard v-model="backgroundColor" />
  </div>
</template>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding: 2rem;
  transition: background-color 0.3s ease;
  width: 100%;
  min-height: 100vh;
  align-items: center;
}
</style>
