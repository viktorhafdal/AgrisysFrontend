<template>
<div class="bg-gray-900 text-white min-h-screen flex">
  <!-- Sidebar -->
  <aside class="w-64 bg-gray-800 p-4 space-y-4">
    <h1 class="text-2xl font-bold">AGRISYS</h1>
    <nav class="space-y-2">
      <button class="w-full text-left py-2 px-3 bg-gray-700 rounded hover:bg-gray-600">Dashboard</button>
      <button class="w-full text-left py-2 px-3 hover:bg-gray-700">KPI</button>
      <button class="w-full text-left py-2 px-3 hover:bg-gray-700">Warnings</button>
      <button class="w-full text-left py-2 px-3 hover:bg-gray-700">Settings</button>
    </nav>
  </aside>

  <!-- Main content -->
  <main class="flex-1 p-6 space-y-8">
    <!-- KPI Section -->
    <section>
      <h2 class="text-xl font-semibold mb-4">Farm Status</h2>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
        <KpiCard title="TOTAL PIGS" :value="1740" />
        <KpiCard title="READY FOR SHIPMENT" :value="472" />
        <KpiCard title="OVERWEIGHT" :value="236" />
        <KpiCard title="UNDERPERFORMING" :value="58" />
      </div>
    </section>

    <!-- Graphs -->
    <section class="grid md:grid-cols-2 gap-6">
      <div class="bg-gray-800 p-4 rounded">
        <h3 class="mb-2">Water usage (last 48 hours)</h3>
        <div class="bg-gray-700 rounded">
          <LineGraph />
        </div>
      </div>

      <div class="bg-gray-800 p-4 rounded">
        <h3 class="mb-2">Total feed - last 7 days</h3>
        <div class="bg-gray-700 rounded">
          <BarGraph />
        </div>
      </div>
    </section>

    <!-- Feed Remaining -->
    <section class="grid md:grid-cols-3 gap-4">
      <div class="bg-gray-800 p-4 rounded">
        <h4 class="text-sm text-gray-300">REMAINING TODAY</h4>
        <div>
          <DoughnutGraph />
        </div>
      </div>
      <div class="bg-gray-800 p-4 rounded">
        <h4 class="text-sm text-gray-300">REMAINING YESTERDAY</h4>
        <div>
          <DoughnutGraph2 />
        </div>
      </div>
      <div class="bg-gray-800 p-4 rounded">
        <h4 class="text-sm text-gray-300 mb-2">AUTO REPORTS</h4>
        <div class="space-y-1">
          <div class="flex justify-between bg-gray-700 p-2 rounded">Farrowing Unit Fat <span><i class="pi pi-arrow-circle-right" style="font-size: 24px;"></i></span></div>
          <div class="flex justify-between bg-gray-700 p-2 rounded">Insemination Skinny <span><i class="pi pi-arrow-circle-right" style="font-size: 24px;"></i></span></div>
        </div>
      </div>
    </section>
  </main>
</div>
</template>

<script setup>
const dummyFeedData = 
[
  { day: '22-10', A: 44, B: 30, C: 20, D: 10, E: 5 },
  { day: '23-10', A: 50, B: 35, C: 25, D: 8, E: 3 },
]
</script>
