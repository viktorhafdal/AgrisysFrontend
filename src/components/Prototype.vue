```vue
<template>
  <div class="p-6 bg-gray-100 min-h-screen">
    <div class="mb-6">
      <h1 class="text-3xl font-bold mb-2">Grise Fodringsdashboard</h1>
      <p class="text-gray-600">Overvåg fodringsmønstre og få advarsler på underernærede grise.</p>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-6 mb-6">
      <Card title="Antal grise med advarsel">
        <p class="text-2xl font-semibold">{{ warningCount }}</p>
      </Card>
      <Card title="Gennemsnitligt foderindtag (seneste 3 dage)">
        <p class="text-2xl font-semibold">{{ averageFeed }} kg</p>
      </Card>
      <Card title="Eksporter data">
        <button @click="exportData" class="bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700">
          Eksportér CSV
        </button>
      </Card>
    </div>

    <div class="bg-white p-4 rounded shadow">
      <h2 class="text-xl font-semibold mb-4">Grise med lavt foderindtag</h2>
      <table class="min-w-full">
        <thead>
          <tr class="bg-gray-200 text-left">
            <th class="px-4 py-2">ID</th>
            <th class="px-4 py-2">Navn</th>
            <th class="px-4 py-2">Gennemsnit (3 dage)</th>
            <th class="px-4 py-2">Advarsel</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="pig in warningPigs" :key="pig.id" class="border-t">
            <td class="px-4 py-2">{{ pig.id }}</td>
            <td class="px-4 py-2">{{ pig.name }}</td>
            <td class="px-4 py-2">{{ pig.avgFeed }} kg</td>
            <td class="px-4 py-2 text-red-600 font-bold">⚠️</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
// import { saveAs } from 'file-saver'

const warningCount = ref(0)
const averageFeed = ref(0)
const warningPigs = ref([])

const dummyData = [
  { id: '001', name: 'Gris A', avgFeed: 1.5 },
  { id: '002', name: 'Gris B', avgFeed: 1.2 },
  { id: '003', name: 'Gris C', avgFeed: 2.8 },
  { id: '004', name: 'Gris D', avgFeed: 3.1 },
  { id: '005', name: 'Gris E', avgFeed: 1.9 }
]

onMounted(() => {
  warningPigs.value = dummyData.filter(p => p.avgFeed < 2.0)
  warningCount.value = warningPigs.value.length
  averageFeed.value = (
    dummyData.reduce((sum, p) => sum + p.avgFeed, 0) / dummyData.length
  ).toFixed(2)
})

function exportData() {
  const csv = 'ID,Navn,Gennemsnit\n' + warningPigs.value.map(p => `${p.id},${p.name},${p.avgFeed}`).join('\n')
  const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' })
  saveAs(blob, 'grise_advarsler.csv')
}
</script>

<style scoped>
table th, table td {
  text-align: left;
}
</style>

<!-- Card Component -->
<script>
export default {
  props: ['title'],
  template: `
    <div class="bg-white p-4 rounded shadow">
      <h3 class="text-lg font-medium mb-2">{{ title }}</h3>
      <slot></slot>
    </div>
  `
}
</script>
```
