<template>
  <div style="width: 100%; height: 260px;">
    <Bar :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup>
import { Bar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

const props = defineProps({
  data: {
    type: Object,
    required: true
  }
})

const chartData = {
  labels: ['Concluídas', 'Em andamento', 'Pendentes'],
  datasets: [
    {
      label: 'Rotinas',
      data: [
        props.data.concluidas,
        props.data.emAndamento,
        props.data.pendentes
      ],
      backgroundColor: ['#16a34a', '#f59e0b', '#ef4444'],
      borderRadius: 6,
    },
  ],
}

const chartOptions = {
  responsive: true,
  plugins: {
    legend: { display: false },
  },
  scales: {
    y: { beginAtZero: true, ticks: { stepSize: 1 } },
    x: { grid: { display: false } },
  },
}
</script>
