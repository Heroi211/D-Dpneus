<template>
  <v-navigation-drawer
    app
    v-model="drawer"
    :permanent="!isMobile"
    class="dd-sidebar"
  >
    <v-list nav dense>
      <v-list-item class="sidebar-logo">
        <img src="@/assets/logo.png" alt="D&D Pneus" class="logo-img" />
      </v-list-item>

      <v-divider class="my-3"></v-divider>

      <v-list-item
        v-for="item in menuItems"
        :key="item.title"
        :to="item.to"
        link
        rounded="xl"
        class="sidebar-item"
      >
        <v-icon class="mr-3">{{ item.icon }}</v-icon>
        <v-list-item-title>{{ item.title }}</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from "vue";


const drawer = ref(true);
const isMobile = ref(false);

const checkIsMobile = () => {
  isMobile.value = window.innerWidth <= 960;
};

onMounted(() => {
  checkIsMobile();
  window.addEventListener("resize", checkIsMobile);
});
onBeforeUnmount(() => {
  window.removeEventListener("resize", checkIsMobile);
});

const menuItems = [
  { title: "Dashboard", icon: "mdi-view-dashboard-outline", to: "/home" },
  { title: "Usuários", icon: "mdi-account-group-outline", to: "/usuarios" },
  { title: "Tarefas", icon: "mdi-clipboard-list-outline", to: "/tarefas" },
  { title: "Relatórios", icon: "mdi-chart-bar", to: "/relatorios" },
  { title: "Configurações", icon: "mdi-cog-outline", to: "/configuracoes" },
];
</script>


