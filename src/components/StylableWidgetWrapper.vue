<script setup lang="ts">
defineProps<{
  widgetTitle: string,
  toggle: boolean,
  stylingClasses?: string,
}>()

</script>

<template>
  <div class="bg-gray-800 p-4 rounded" v-if="toggle">
    <h2 class="text-xl font-semibold mb-4">{{ widgetTitle }}</h2>
    <div :class="stylingClasses">
      <slot />
    </div>
  </div>
</template>
