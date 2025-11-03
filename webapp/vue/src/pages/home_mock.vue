<template>
  <v-container fluid class="dashboard-container py-6 px-8">
    <!-- KPIs Superiores -->
    <v-row class="mb-6" justify="space-between">
      <v-col v-for="kpi in kpis" :key="kpi.label" cols="12" sm="6" md="2">
        <v-card class="kpi-card" elevation="4">
          <v-card-text class="text-center">
            <v-icon :icon="kpi.icon" size="32" class="mb-2 text-green-800" />
            <h2 class="text-2xl font-semibold text-green-900">{{ kpi.value }}</h2>
            <p class="text-sm text-green-900 font-medium">{{ kpi.label }}</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Gráficos -->
    <v-row>
      <v-col md="7">
        <v-card class="chart-card" elevation="6">
          <v-card-title class="text-green-900 font-semibold">Rotinas por Status</v-card-title>
          <v-card-text>
            <canvas id="chartStatus"></canvas>
          </v-card-text>
        </v-card>
      </v-col>

      <v-col md="5">
        <v-card class="chart-card" elevation="6">
          <v-card-title class="text-green-900 font-semibold">Desempenho por Colaborador</v-card-title>
          <v-card-text>
            <canvas id="chartColaborador"></canvas>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Rotinas próximas e notificações -->
    <v-row class="mt-6">
      <v-col md="8">
        <v-card class="data-card" elevation="6">
          <v-card-title class="text-green-900 font-semibold">Rotinas próximas do prazo</v-card-title>
          <v-table>
            <thead>
              <tr>
                <th>Cliente</th>
                <th>Rotina</th>
                <th>Prazo</th>
                <th>Responsável</th>
                <th>Status</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in rotinasProximas" :key="item.id">
                <td>{{ item.cliente }}</td>
                <td>{{ item.rotina }}</td>
                <td>{{ item.prazo }}</td>
                <td>{{ item.responsavel }}</td>
                <td>
                  <v-chip :color="getStatusColor(item.status)" dark small>{{ item.status }}</v-chip>
                </td>
              </tr>
            </tbody>
          </v-table>
        </v-card>
      </v-col>

      <v-col md="4">
        <v-card class="feed-card" elevation="6">
          <v-card-title class="text-green-900 font-semibold">Atividades Recentes</v-card-title>
          <v-list density="compact">
            <v-list-item v-for="(log, i) in logs" :key="i">
              <v-list-item-content>
                <v-list-item-title>{{ log.text }}</v-list-item-title>
                <v-list-item-subtitle class="text-xs text-green-800">{{ log.hora }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref } from 'vue'

const kpis = ref([
  { label: 'Rotinas Totais', value: 42, icon: 'mdi-clipboard-text-outline' },
  { label: 'Em andamento', value: 8, icon: 'mdi-progress-clock' },
  { label: 'Concluídas', value: 30, icon: 'mdi-check-circle-outline' },
  { label: 'Atrasadas', value: 4, icon: 'mdi-alert-circle-outline' },
  { label: 'Colaboradores Ativos', value: 12, icon: 'mdi-account-group-outline' }
])

const rotinasProximas = ref([
  { id: 1, cliente: 'Cod3bit', rotina: 'Inspeção final', prazo: '05/11/2025', responsavel: 'João', status: 'Em andamento' },
  { id: 2, cliente: 'TruckCenter', rotina: 'Checklist pneus', prazo: '06/11/2025', responsavel: 'Maria', status: 'Pendente' },
  { id: 3, cliente: 'AutoSul', rotina: 'Revisão mensal', prazo: '07/11/2025', responsavel: 'Pedro', status: 'Concluída' }
])

const logs = ref([
  { text: 'João concluiu “Inspeção final”', hora: 'há 12min' },
  { text: 'Maria pausou “Checklist pneus”', hora: 'há 38min' },
  { text: 'Pedro iniciou “Revisão mensal”', hora: 'há 1h' }
])

const getStatusColor = (status) => {
  switch (status) {
    case 'Concluída': return 'green-darken-2'
    case 'Em andamento': return 'blue-darken-2'
    case 'Pendente': return 'orange-darken-2'
    default: return 'grey'
  }
}
</script>

<style scoped>
.dashboard-container {
  background: transparent;
}

.kpi-card {
  background: rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(10px);
  border-radius: 14px;
  color: #013104;
  transition: all 0.3s ease;
}

.kpi-card:hover {
  transform: translateY(-3px);
  background: rgba(255, 255, 255, 0.18);
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.25);
}

.chart-card, .data-card, .feed-card {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(12px);
  border-radius: 16px;
  color: #013104;
}

.v-table th {
  color: #013104;
}
</style>
