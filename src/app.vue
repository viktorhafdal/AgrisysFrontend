<script setup lang="ts">
useHead({
  title: 'Agrisys Frontend',
  meta: [
    { name: 'description', content: 'A dashboard app for pigs' },
    {charset: 'utf-8'}
  ],
  htmlAttrs: {
    lang: 'en'
  }
})
</script>

<template>
  <div>
    <NuxtRouteAnnouncer />
    <NuxtPage />
  </div>
</template>
