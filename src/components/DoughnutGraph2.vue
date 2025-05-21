<script setup>
import { ref, onMounted } from "vue";

const chartData = ref();
const chartOptions = ref(null);
const maxFeedKg = 281;

const setChartData = () => {
    const documentStyle = getComputedStyle(document.body);

    return {
        datasets: [
            {
                data: [222.5, Number((maxFeedKg - 222.5).toFixed(1))],
                backgroundColor: [documentStyle.getPropertyValue('--p-cyan-500'), documentStyle.getPropertyValue('--p-gray-500'), documentStyle.getPropertyValue('--p-gray-500')],
                hoverBackgroundColor: [documentStyle.getPropertyValue('--p-cyan-400'), documentStyle.getPropertyValue('--p-gray-400'), documentStyle.getPropertyValue('--p-gray-400')]
            }
        ]
    };
};

const setChartOptions = () => {
    const documentStyle = getComputedStyle(document.documentElement);
    const textColor = documentStyle.getPropertyValue('--p-text-color');

    return {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          labels: {
              color: textColor,
              cutout: '60%',
          },
        },
        tooltip: {
          callbacks: {
            label: function(context) {
              const label = context.label || '';
              const value = context.parsed || 0;
              return label ? `${label}: ${value} kg` : `${value} kg`;
            }
          }
        }
      },
    };
};

const getChartData = () => {
  return chartData.value?.datasets[0]?.data[0] ? `${chartData.value.datasets[0].data[0]} kg <br/>/ ${maxFeedKg} kg` : '';
};

onMounted(() => {
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});
</script>


<template>
  <div class="pt-2">
    <LegendTag label="Feed remaining" bgColor="var(--p-cyan-500)" textColor="#0A0A0A"/>
  </div>
  <div class="pt-2">
    <LegendTag label="Feed consumed" bgColor="var(--p-gray-500)" textColor="#FFF"/>
  </div>
  <div class="relative">
    <div class="card flex justify-center">
      <Chart type="doughnut" :data="chartData" :options="chartOptions" class="w-full h-[300px]" :aria-label="` Doughnut Graph showing remaining feed yesterday, where there has been used 222.5kg out of the maximum of ${maxFeedKg}`" />
      <div class="absolute inset-0 flex items-center justify-center pointer-events-none">
        <span class="text-2xl font-bold text-white" v-html="getChartData()"></span>
      </div>
    </div>
  </div>
</template>
