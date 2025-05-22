<script setup>
  import { ref } from 'vue'
  import { onMounted } from 'vue'
  import Sidebar from './Sidebar.vue';

  const sidebarLinks =
  [
    ['Dashboard', '/'],
    ['KPI', '/kpi'],
    ['Warnings', '/warnings'],
    ['Settings', '/settings']
  ];
  
  const isSidebarOpen = ref(false)

  const handleClickOutside = (event) => {
    const sidebar = document.querySelector('aside[aria-label="Sidebar"]')
    const toggleButton = event.target.closest('button[aria-label="Toggle menu"]')

    if (isSidebarOpen.value && sidebar && !sidebar.contains(event.target) && !toggleButton) {
      isSidebarOpen.value = false
    }
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

  onMounted(() => {
    document.addEventListener('click', handleClickOutside)
  })

  onUnmounted(() => {
    document.removeEventListener('click', handleClickOutside)
  })
</script>

<template>
  <div class="bg-gray-900 text-white min-h-screen flex">
    <Sidebar title="AGRISYS" :toggle="isSidebarOpen" :links="sidebarLinks" />

    <main class="flex-1 p-6 space-y-8">
      <section>
        <div>
          <div class="flex flex-row fixed z-1000">
            <Button class="p-4 text-white bg-gray-800 me-2" @click="isSidebarOpen = !isSidebarOpen"
              aria-label="Toggle menu" type="button" icon="pi pi-bars" />
            <WidgetSelector :widget-toggles="widgetToggles" />
          </div>
        </div>
      </section>

      <div class="h-1"></div>

      <section>
        <WidgetWrapper title="Farm Status" :toggle="showFarmStatus">
          <KpiCard title="TOTAL PIGS" :value="1740" />
          <KpiCard title="READY FOR SHIPMENT" :value="472" />
          <KpiCard title="OVERWEIGHT" :value="236" />
          <KpiCard title="UNDERPERFORMING" :value="58" />
        </WidgetWrapper>
      </section>

      <section class="grid md:grid-cols-2 gap-6">
        <StylableWidgetWrapper title="Water usage (last 48 hours)" :toggle="showWaterUsage"
          styling-classes="bg-gray-700 rounded">
          <LineGraph />
        </StylableWidgetWrapper>

        <StylableWidgetWrapper title="Total feed (last 7 days)" :toggle="showTotalFeed"
          styling-classes="bg-gray-700 rounded">
          <BarGraph />
        </StylableWidgetWrapper>
      </section>

      <section class="grid md:grid-cols-3 gap-4">
        <StylableWidgetWrapper title="FEED REMAINING TODAY" :toggle="showRemainingToday">
          <DoughnutGraph />
        </StylableWidgetWrapper>

        <StylableWidgetWrapper title="FEED REMAINING YESTERDAY" :toggle="showRemainingYesterday">
          <DoughnutGraph2 />
        </StylableWidgetWrapper>

        <StylableWidgetWrapper title="AUTO REPORTS" :toggle="showAutoReports">
          <div class="space-y-1">
            <div class="flex justify-between bg-gray-700 p-2 rounded">
              Farrowing Unit Fat
              <span>
                <i class="pi pi-arrow-circle-right" style="font-size: 24px;"></i>
              </span>
            </div>
            <div class="flex justify-between bg-gray-700 p-2 rounded">
              Insemination Skinny
              <span>
                <i class="pi pi-arrow-circle-right" style="font-size: 24px;"></i>
              </span>
            </div>
          </div>
        </StylableWidgetWrapper>
      </section>
    </main>
  </div>
</template>
