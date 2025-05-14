<script setup>
const dummyFeedData = 
[
  { day: '22-10', A: 44, B: 30, C: 20, D: 10, E: 5 },
  { day: '23-10', A: 50, B: 35, C: 25, D: 8, E: 3 },
]

const widgetsPopover = ref();
const showWidgetMenu = (event) => {
    widgetsPopover.value.toggle(event);
}

const showFarmStatus = ref(true);
const showWaterUsage = ref(true);
const showTotalFeed = ref(true);
const showRemainingToday = ref(true);
const showRemainingYesterday = ref(true);
const showAutoReports = ref(true);

const widgetToggles = [
  { label: 'Farm Status', value: showFarmStatus },
  { label: 'Water Usage', value: showWaterUsage },
  { label: 'Total Feed', value: showTotalFeed },
  { label: 'Remaining Today', value: showRemainingToday },
  { label: 'Remaining Yesterday', value: showRemainingYesterday },
  { label: 'Auto Reports', value: showAutoReports }
];
</script>

<template>
<div class="bg-gray-900 text-white min-h-screen flex">
  <!-- Sidebar -->
  <aside class="w-64 bg-gray-800 p-4 space-y-4" aria-label="Sidebar">
    <h1 class="text-2xl font-bold" aria-label="Company name">AGRISYS</h1>
    <nav class="space-y-2">
      <button class="w-full text-left py-2 px-3 bg-gray-700 rounded hover:bg-gray-600" aria-label="Menu item 1: Dashboard">Dashboard</button>
      <button class="w-full text-left py-2 px-3 hover:bg-gray-700" aria-label="Menu item 2: KPI">KPI</button>
      <button class="w-full text-left py-2 px-3 hover:bg-gray-700" aria-label="Menu item 3: Warnings">Warnings</button>
      <button class="w-full text-left py-2 px-3 hover:bg-gray-700" aria-label="Menu item 4: Settings">Settings</button>
    </nav>
  </aside>

  <!-- Main content -->
  <main class="flex-1 p-6 space-y-8">
    
    <!-- Widget toggles -->
    <section>
      <div>
        <Button type="button" label="Widgets" @click="showWidgetMenu" />
        <Popover ref="widgetsPopover">
          <div class="flex flex-col gap-4">
            <div class="card flex justify-between items-center gap-4" v-for="widget in widgetToggles" :key="widget.label">
              <span>{{ widget.label }}</span>
              <ToggleSwitch v-model="widget.value.value">
                <template #handle="{ value }">
                  <i :class="['!text-xs pi', { 'pi-check': widget.value, 'pi-times': !widget.value.value }]" />
                </template>
              </ToggleSwitch>
            </div>
          </div>
        </Popover>
      </div>
    </section>

    <!-- KPI Section -->
    <section v-if="showFarmStatus">
      <div class="bg-gray-800 p-4 rounded">
        <h2 class="text-xl font-semibold mb-4">Farm Status</h2>
        <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
          <KpiCard title="TOTAL PIGS" :value="1740" />
          <KpiCard title="READY FOR SHIPMENT" :value="472" />
          <KpiCard title="OVERWEIGHT" :value="236" />
          <KpiCard title="UNDERPERFORMING" :value="58" />
        </div>
      </div>
    </section>

    <!-- Graphs -->
    <section class="grid md:grid-cols-2 gap-6">
      <div class="bg-gray-800 p-4 rounded" v-if="showWaterUsage">
        <h2 class="text-xl font-semibold mb-4">Water usage (last 48 hours)</h2>
        <div class="bg-gray-700 rounded">
          <LineGraph />
        </div>
      </div>

      <div class="bg-gray-800 p-4 rounded" v-if="showTotalFeed">
        <h2 class="text-xl font-semibold mb-4">Total feed (last 7 days)</h2>
        <div class="bg-gray-700 rounded">
          <BarGraph />
        </div>
      </div>
    </section>

    <!-- Feed Remaining -->
    <section class="grid md:grid-cols-3 gap-4">
      <div class="bg-gray-800 p-4 rounded" v-if="showRemainingToday">
        <h2 class="text-xl font-semibold mb-4">REMAINING TODAY</h2>
        <div>
          <DoughnutGraph />
        </div>
      </div>
      <div class="bg-gray-800 p-4 rounded" v-if="showRemainingYesterday">
        <h2 class="text-xl font-semibold mb-4">REMAINING YESTERDAY</h2>
        <div>
          <DoughnutGraph2 />
        </div>
      </div>
      <div class="bg-gray-800 p-4 rounded" v-if="showAutoReports">
        <h2 class="text-xl font-semibold mb-4"> AUTO REPORTS</h2>
        <div class="space-y-1">
          <div class="flex justify-between bg-gray-700 p-2 rounded">Farrowing Unit Fat <span><i class="pi pi-arrow-circle-right" style="font-size: 24px;"></i></span></div>
          <div class="flex justify-between bg-gray-700 p-2 rounded">Insemination Skinny <span><i class="pi pi-arrow-circle-right" style="font-size: 24px;"></i></span></div>
        </div>
      </div>
    </section>
  </main>
</div>
</template>

<style lang="css" scoped>
.p-button
{
  --p-button-primary-background: #1e2939;
  --p-button-primary-border-color: #1e2939;
  --p-button-primary-color: white;

  --p-button-primary-hover-background: #4a5565;
  --p-button-primary-hover-border-color: #4a5565;
  --p-button-primary-hover-color: white;

  --p-button-primary-active-background: #4a5565;
  --p-button-primary-active-border-color: #4a5565;

}
/* #364153 */
/* #4a5565 */
</style>
