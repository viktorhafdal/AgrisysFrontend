<script setup lang="ts">
import Chart from 'primevue/chart';
import { ref, onMounted } from "vue";

const getLast7DaysLabels = () => {
    const labels: string[] = []
    const today = new Date()

    for (let i = 6; i >= 0; i--) {
        const date = new Date(today)
        date.setDate(today.getDate() - i)
        labels.push(date.toLocaleDateString(undefined, {
            weekday: 'short', // e.g., Mon, Tue
            month: 'short',   // e.g., May
            day: 'numeric'    // e.g., 6
        }))
    }

    return labels
}


const chartData = ref();
const chartOptions = ref();

const setChartData = () => {
    const documentStyle = getComputedStyle(document.documentElement);

    return {
        labels: getLast7DaysLabels(),
        datasets: [
            {
                label: 'Feed A',
                backgroundColor: documentStyle.getPropertyValue('--p-cyan-500'),
                borderColor: documentStyle.getPropertyValue('--p-cyan-500'),
                data: [53, 41, 65, 32, 60, 29, 56]
            },
            {
                label: 'Feed B',
                backgroundColor: documentStyle.getPropertyValue('--p-rose-500'),
                borderColor: documentStyle.getPropertyValue('--p-rose-500'),
                data: [28, 56, 13, 48, 35, 19, 67]
            },
            {
                label: 'Feed C',
                backgroundColor: documentStyle.getPropertyValue('--p-emerald-500'),
                borderColor: documentStyle.getPropertyValue('--p-emerald-500'),
                data: [44, 39, 31, 51, 60, 63, 28]
            },
            {
                label: 'Feed D',
                backgroundColor: documentStyle.getPropertyValue('--p-amber-500'),
                borderColor: documentStyle.getPropertyValue('--p-amber-500'),
                data: [19, 47, 33, 55, 49, 41, 60]
            },
            {
                label: 'Feed E',
                backgroundColor: documentStyle.getPropertyValue('--p-violet-500'),
                borderColor: documentStyle.getPropertyValue('--p-violet-500'),
                data: [28, 29, 60, 41, 52, 63, 33]
            },
        ]
    };
};

const setChartOptions = () => {
    const documentStyle = getComputedStyle(document.documentElement);
    const textColor = documentStyle.getPropertyValue('--p-text-color');
    const textColorSecondary = documentStyle.getPropertyValue('--p-text-muted-color');
    const surfaceBorder = documentStyle.getPropertyValue('--p-content-border-color');

    return {
        maintainAspectRatio: false,
        aspectRatio: 0.8,
        plugins: {
            legend: {
                labels: {
                    color: textColor
                }
            }
        },
        scales: {
            x: {
                ticks: {
                    color: textColorSecondary,
                    font: {
                        weight: 500
                    }
                },
                grid: {
                    display: false,
                    drawBorder: false
                }
            },
            y: {
                ticks: {
                    color: textColorSecondary
                },
                grid: {
                    color: surfaceBorder,
                    drawBorder: false
                }
            }
        }
    };
}

onMounted(() => {
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});
</script>

<template>
  <div class="card">
      <Chart type="bar" :data="chartData" :options="chartOptions" class="h-[30rem]"  />
  </div>
</template>
