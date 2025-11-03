<template>
  <v-footer id="app-footer" padless>
    <v-container fluid class="text-center py-2">
      <v-row no-gutters>
        <v-col>
          <small class="white--text">
            © {{ currentYear }} D&D Pneus — Desenvolvido por Cod3bit
          </small>
        </v-col>
      </v-row>
    </v-container>
  </v-footer>
</template>

<script>
export default {
  name: "AppFooter",
  data() {
    return {
      currentYear: new Date().getFullYear(),
    };
  },
};
</script>
