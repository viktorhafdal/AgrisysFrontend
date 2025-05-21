<script setup lang="ts">
defineProps<{
  label: string
  bgColor: string,
  textColor: string,
}>()
</script>

<template>
  <Tag :value="label" :style="{ '--tag-background': bgColor, '--tag-color': textColor }" class="legend-tag" />
</template>

<style scoped>
.legend-tag {
  background-color: var(--tag-background);
  color: var(--tag-color);
}
</style>
