<script setup lang="ts">
import HelloWorldCard from "@/components/HelloWorldCard.vue";
import CalculatorCard from "@/components/CalculatorCard.vue";
</script>

<template>
  <div class="app-container">
    <HelloWorldCard />
    <CalculatorCard />
  </div>
</template>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}
</style>
