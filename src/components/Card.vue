<script setup lang="ts"></script>

<template>
  <q-card v-bind="$attrs" class="custom-rounded-card q-pa-lg">
    <slot />
  </q-card>
</template>

<style scoped lang="scss">
.custom-rounded-card {
  border-radius: 12px !important;
  overflow: hidden;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}
</style>
