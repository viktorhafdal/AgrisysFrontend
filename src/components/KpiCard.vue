<script setup lang="ts">
const props = defineProps<{
  title: string,
  value: any,
}>();
</script>

<template>
  <div class="bg-gray-800 p-4 rounded text-center">
    <div class="text-gray-400 text-sm mb-1">{{ title }}</div>
    <div class="text-3xl font-bold">{{ value }}</div>
  </div>
</template>
