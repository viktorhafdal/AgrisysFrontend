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
                data: [156.2, 124.8],
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
        plugins: {
            legend: {
                labels: {
                    cutout: '60%',
                    color: textColor
                },
            }
        }
    };
};

const getChartData = () => {
  return chartData.value?.datasets[0]?.data[0] ? `${chartData.value.datasets[0].data[0]} <br/>/ ${maxFeedKg}` : '';
};

onMounted(() => {
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});
</script>


<template>
  <div class="relative">
    <div class="card flex justify-center">
      <Chart type="doughnut" :data="chartData" :options="chartOptions" class="w-full md:w-[30rem] " :aria-label="` Doughnut Graph showing remaining feed today, where there has been used 156.2kg out of the maximum of ${maxFeedKg}`" />
      <div class="absolute inset-0 flex items-center justify-center pointer-events-none">
        <span class="text-2xl font-bold text-white" v-html="getChartData()"></span>
      </div>
    </div>
  </div>
</template>
