<script setup lang="ts">
import { ref, computed } from "vue";
import Card from "@/components/Card.vue";

enum BackgroundType {
  Color = "color",
  Border = "border",
}

const props = defineProps<{
  modelValue: string;
}>();

const emit = defineEmits<{
  "update:modelValue": [value: string];
}>();

const backgroundColor = computed({
  get: () => props.modelValue,
  set: (value) => emit("update:modelValue", value),
});

const backgroundType = ref<BackgroundType>(BackgroundType.Color);
</script>

<template>
  <Card>
    <div class="color-setting">
      <q-color v-model="backgroundColor" flat no-footer class="color-picker" />
    </div>
  </Card>
</template>

<style scoped>
.card {
  margin-bottom: 1rem;
}

.color-setting {
  display: flex;
  align-items: center;
  gap: 1rem;
  width: 100%;
}

.color-picker ::v-deep {
  max-width: none !important;
  width: 100%;
}

/* RGB/HEXタブを無効化 */
.color-picker ::v-deep .q-tabs {
  display: none;
}

/* カラーピッカーの縦横比を変更 */
.color-picker ::v-deep .q-color-picker__spectrum > div:first-child {
  padding-bottom: 50% !important;
}
</style>
