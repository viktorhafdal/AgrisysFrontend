<script setup lang="ts">
import Chart from 'primevue/chart';
import { ref, onMounted } from "vue";

function generateGraphLabels(hoursBack = 48) {
  const labels: string[] = []

  // round current time down to nearest 2-hour mark
  const now = new Date()
  const currentHour = now.getHours()
  const roundedHour = currentHour - (currentHour % 2)

  const end = new Date(now)
  end.setHours(roundedHour, 0, 0, 0) // set to nearest past 2-hour mark

  for (let i = hoursBack; i >= 0; i -= 2) {
    const labelTime = new Date(end.getTime() - i * 60 * 60 * 1000)

    const isMidnight = labelTime.getHours() === 0
    const labelDate = new Date(labelTime)
    labelDate.setHours(0, 0, 0, 0)

    const today = new Date()
    today.setHours(0, 0, 0, 0)

    let label = ''

    if (isMidnight) {
      label = labelTime.toLocaleDateString(undefined, 
      { 
        day: 'numeric', 
        month: 'short', 
        year:'numeric' 
      })
    } else {
      label = labelTime.toLocaleTimeString(undefined, {
        hour: '2-digit',
        minute: '2-digit',
        hour12: false
      })
    }

    labels.push(label)
  }

  return labels
}

function ariaLabelsLineGraph(timeLabels: string[], data: number[])
{
    let text = ""
    for (let i = 0; i < timeLabels.length; i++){
        text += timeLabels[i] + ": " + data[i] + " litres, "
    }
    return text;
}

const chartLabels = generateGraphLabels();

const chartData = ref();
const chartOptions = ref();

const setChartData = () => {
    const documentStyle = getComputedStyle(document.documentElement);

    return {
        labels: chartLabels,
        datasets: [
            {
                label: 'Litres',
                fill: false,
                borderColor: documentStyle.getPropertyValue('--p-cyan-500'),
                yAxisID: 'y',
                tension: 0.4,
                data: [25, 24, 26, 27, 8, 26, 28, 30, 29, 31, 37, 32, 28, 26, 27, 9, 25, 24, 26, 30, 28, 36, 29, 27, 25, 17]
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
        stacked: false,
        maintainAspectRatio: false,
        aspectRatio: 0.6,
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
                    color: textColorSecondary
                },
                grid: {
                    color: surfaceBorder
                }
            },
            y: {
                type: 'linear',
                display: true,
                position: 'left',
                min: 0,
                max: 40,
                ticks: {
                    color: textColorSecondary
                },
                grid: {
                    color: surfaceBorder
                }
            },
            y1: {
                type: 'linear',
                display: true,
                position: 'right',
                min: 0,
                max: 40,
                ticks: {
                    color: textColorSecondary
                },
                grid: {
                    drawOnChartArea: false, 
                    color: surfaceBorder
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
  <div class="card" role="Widget" :aria-label="`Line Chart that shows water usage for the last 48 hours with litres as Y-axis values and 2 hour intervals as X-values. Data is:
    ${ ariaLabelsLineGraph(chartLabels, [25, 24, 26, 27, 8, 26, 28, 30, 29, 31, 37, 32, 28, 26, 27, 9, 25, 24, 26, 30, 28, 36, 29, 27, 25, 17]) }`">
    <Chart type="line" :data="chartData" :options="chartOptions" class="h-[30rem]" />
  </div>
</template>
