<template>
  <div class="routines-page">
    <h1 class="section-title">Painel de Rotinas</h1>

    <!-- Painel Superior -->
    <div class="dashboard-header">
      <div class="summary-cards">
        <div class="summary-card">
          <p>✅ Concluídas</p>
          <p class="value">{{ summary.concluidas }}</p>
        </div>
        <div class="summary-card">
          <p>🕒 Em andamento</p>
          <p class="value">{{ summary.emAndamento }}</p>
        </div>
        <div class="summary-card">
          <p>⚠️ Pendentes</p>
          <p class="value">{{ summary.pendentes }}</p>
        </div>
      </div>

      <!-- Gráfico -->
      <div class="card chart-card">
        <RoutinesChart :data="summary" />
      </div>
    </div>

    <!-- Grade de Rotinas -->
    <div class="card">
      <RoutinesDatagrid />
    </div>
  </div>
</template>

<script>
import RoutinesDatagrid from "@/components/rotinas/RoutinesDatagrid.vue";
import RoutinesChart from "@/components/rotinas/RoutinesChart.vue";

export default {
  name: "Routines",
  components: { RoutinesDatagrid, RoutinesChart },
  data() {
    return {
      summary: {
        concluidas: 12,
        emAndamento: 5,
        pendentes: 3,
      },
    };
  },
};
</script>

