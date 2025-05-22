<script setup lang="ts">
defineProps<{
  widgetTitle: string,
  toggle: boolean,
}>()

</script>

<template>
  <div class="bg-gray-800 p-4 rounded" v-if="toggle">
    <h2 class="text-xl font-semibold mb-4">{{ widgetTitle }}</h2>
    <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
      <slot />
    </div>
  </div>
</template>
