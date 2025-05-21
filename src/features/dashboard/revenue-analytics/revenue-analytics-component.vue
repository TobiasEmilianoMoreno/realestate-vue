<template>
    <div class="revenue-analytics">
        <div class="revenue-analytics__header">
            <h3 class="revenue-analytics__title">Revenue Analytics</h3>
            <select class="revenue-analytics__select" v-model="viewType">
                <option value="Monthly">Monthly</option>
                <option value="Yearly">Yearly</option>
            </select>
        </div>
        <div v-if="error" class="error-message">{{ error }}</div>
        <div v-if="isLoading" class="loading">
            Cargando datos del gráfico…
        </div>
        <div v-if="!error" v-show="!isLoading" class="revenue-analytics__body">
            <canvas ref="chartRef" class="revenue-analytics__chart" height="360"></canvas>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed, watch, onMounted, onBeforeUnmount } from 'vue'
import Chart from 'chart.js/auto'
import crosshairPlugin from 'chartjs-plugin-crosshair'
import { RevenueStats } from './revenue-stats'
import { api } from '@/plugins/axios'

Chart.register(crosshairPlugin)

const chartRef = ref<HTMLCanvasElement | null>(null)
let chartInstance: Chart | null = null

const viewType = ref<'Monthly' | 'Yearly'>('Monthly')
const revenueStats = ref<RevenueStats[]>([])
const isLoading = ref(false)
const error = ref<string | null>(null)

async function loadRevenueStats(): Promise<void> {
    isLoading.value = true
    error.value = null
    try {
        
        const response = await api.get<RevenueStats[]>(`/revenues`, {
            headers: {
                'Authorization': `Bearer ${localStorage.getItem('token')}`
            },
            params: { viewType: viewType.value }
        })
        revenueStats.value = response.data
    } catch {
        error.value = 'No se pudieron cargar los datos de ingresos.'
    } finally {
        isLoading.value = false
    }
}

const chartData = computed(() => {
    const labels = revenueStats.value.map(data => data.label)
    const sales = revenueStats.value.map(data => data.sales)
    const revenue = revenueStats.value.map(data => data.revenue)

    return {
        labels,
        datasets: [
            {
                label: 'No. of Sales',
                data: sales,
                borderColor: '#6B7480',
                backgroundColor: 'transparent',
                pointBackgroundColor: '#6B7480',
                borderWidth: 1.5,
                pointRadius: 4,
                pointHoverRadius: 6,
                tension: 0.4,
                fill: false
            },
            {
                label: 'Revenue',
                data: revenue,
                borderColor: '#10B981',
                backgroundColor: (ctx: any) => {
                    const chart = ctx.chart
                    const { ctx: c, chartArea } = chart
                    if (!chartArea) return 'rgba(16,185,129,0.1)'
                    const grad = c.createLinearGradient(0, chartArea.top, 0, chartArea.bottom)
                    grad.addColorStop(0, 'rgba(20, 226, 158, 0.2)')
                    grad.addColorStop(1, 'rgba(255, 255, 255, 0.05)')
                    return grad
                },
                borderDash: [8, 4],
                borderWidth: 2,
                pointRadius: 0,
                pointHoverRadius: 5,
                pointBackgroundColor: '#10B981',
                tension: 0.4,
                fill: true
            }
        ]
    }
})

const chartOptions = {
    responsive: true,
    maintainAspectRatio: false,
    animation: { duration: 800, easing: 'easeInOutQuad' },
    interaction: { mode: 'index', intersect: false },
    scales: {
        x: {
            type: 'category',
            ticks: { color: '#000000' },
            grid: { display: false }
        },
        y: {
            ticks: { color: '#000000' },
            grid: { color: '#f0f0f0' }
        }
    },
    plugins: {
        legend: {
            position: 'bottom',
            labels: {
                boxWidth: 12,
                padding: 16,
                color: '#000000',
                font: { weight: 'bold' }
            }
        },
        tooltip: {
            enabled: true,
            callbacks: {
                label: (ctx: any) => `${ctx.dataset.label}: ${ctx.parsed.y}`
            }
        },
        title: {
            display: true,
            text: '',
            font: { size: 18 },
            padding: { bottom: 20 }
        },
        crosshair: {
            line: { color: '#94a3b8', width: 1 },
            sync: { enabled: false },
            zoom: { enabled: false },
            snap: { enabled: true }
        }
    }
}

onMounted(async () => {
    await loadRevenueStats()
    if (!chartRef.value) return
    chartInstance = new Chart(chartRef.value.getContext('2d')!, {
        type: 'line',
        data: chartData.value,
        options: chartOptions as any
    })
})

watch(chartData, (newData) => {
    if (!chartInstance) return
    chartInstance.data = newData as any
    chartInstance.update()
})
watch(viewType, async () => {
    await loadRevenueStats()
})

onBeforeUnmount(() => {
    chartInstance?.destroy()
})
</script>
<style lang="scss" scoped>
@use './revenue-analytics.scss';
</style>