<script setup lang="ts">
defineProps<{
  label: string
  color: string
}>()
</script>

<template>
  <Tag :value="label" :style="{ '--tag-background': color }" class="legend-tag" />
</template>

<style scoped>
.legend-tag {
  background-color: var(--tag-background);
  color: white;
}
</style>
