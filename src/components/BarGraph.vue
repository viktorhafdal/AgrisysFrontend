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

const chartlabels = getLast7DaysLabels();

function ariaLabelsBarGraph(timeLabels: string[], dataA: number[], dataB: number[], dataC: number[], dataD: number[], dataE: number[])
{
    let text = ""
    for (let i = 0; i < timeLabels.length; i++){
        text += timeLabels[i] + ": Feed A: " + dataA[i] + " kg, Feed B: " + dataB[i] + " kg, Feed C: " + dataC[i] + " kg, Feed D: " + dataD[i] + " kg, Feed E: " + dataE[i] + "kg, "
    }
    return text;
}


const chartData = ref();
const chartOptions = ref();

const setChartData = () => {
    const documentStyle = getComputedStyle(document.documentElement);

    return {
        labels: chartlabels,
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
            },
            tooltip: {
                callbacks: {
                    label: function(context: any) {
                        return `${context.dataset.label}: ${context.parsed.y}kg`;
                    }
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
        <div role="Widget" :aria-label="`Bar chart that shows feed usage for the last 7 days with kg feed as Y-axis values and 1 day intervals as X-values. Data is:
        ${ ariaLabelsBarGraph(chartlabels, [53, 41, 65, 32, 60, 29, 56], [28, 56, 13, 48, 35, 19, 67], [44, 39, 31, 51, 60, 63, 28], [19, 47, 33, 55, 49, 41, 60], [28, 29, 60, 41, 52, 63, 33]) }`">
                <Chart type="bar" :data="chartData" :options="chartOptions" class="h-[30rem]"/>
        </div>
    </div>
</template>
