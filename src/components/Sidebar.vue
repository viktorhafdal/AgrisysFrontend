<script setup lang="ts">
defineProps<{
  title: string
  toggle: boolean,
  links: [string, string][]
}>()
</script>

<template>
  <transition name="slide">
    <aside v-if="toggle"
      class="fixed md:static top-0 left-0 z-1001 w-64 h-full md:h-auto bg-gray-800 p-4 space-y-4 transition-all"
      aria-label="Sidebar">
      <h1 class="text-2xl font-bold" aria-label="Company name">{{ title }}</h1>
      <nav class="space-y-2">
        <a v-for="(link, index) in links" :key="index"
          class="w-full text-left py-2 px-3 bg-gray-800 rounded hover:bg-gray-600 block" :href="link[1]">
          {{ link[0] }}
        </a>
      </nav>
    </aside>
  </transition>
</template>
