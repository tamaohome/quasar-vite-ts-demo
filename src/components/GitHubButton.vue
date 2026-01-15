<script setup lang="ts">
const repositoryUrl = "https://github.com/tamaohome/quasar-vite-ts-demo";
</script>

<template>
  <a :href="repositoryUrl" target="_blank" rel="noopener noreferrer" class="github-link">
    <i class="fab fa-github"></i>
  </a>
</template>

<style scoped lang="scss">
@import "@/quasar-variables.sass";

.github-link {
  position: fixed;
  top: 1.5rem;
  right: 1.5rem;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  text-decoration: none;
  transition: all 0.2s;

  i {
    color: #fff;
    font-size: 3rem;
  }

  &:hover i,
  &:focus i,
  &:focus-visible i {
    color: $warning;
  }
}
</style>
