<template>
  <nav class="navbar bg-custom navbar-expand-lg">
    <div class="container-fluid d-flex justify-content-between align-items-center">
      <!-- Logo -->
      <a class="navbar-brand d-flex align-items-center" href="#">
        <img id="container_navlogo" src="/src/assets/logo.png" alt="Logo" width="95" height="95" />
        <span class="text-white ms-2 fw-semibold">D&D Pneus</span>
      </a>

      <!-- Menu -->
      <div v-if="showMenu" class="d-flex align-items-center gap-4">
        <ul class="navbar-nav d-flex flex-row gap-4 mb-0">
          <li class="nav-item"><router-link class="nav-link text-white" to="/home">Início</router-link></li>
          <li class="nav-item"><router-link class="nav-link text-white" to="/routines">Rotinas</router-link></li>
          <li class="nav-item"><router-link class="nav-link text-white" to="/clients">Clientes</router-link></li>
          <li class="nav-item"><router-link class="nav-link text-white" to="/users">Usuários</router-link></li>
        </ul>
        <router-link to="/login" @click.native="logout" class="btn btn-sm btn-outline-light ms-3">Sair</router-link>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { computed } from "vue";
import { useAppStore } from "@/stores/app";
import { useRouter, useRoute } from "vue-router";

const appStore = useAppStore();
const router = useRouter();
const route = useRoute();

const showMenu = computed(() => appStore.authenticated && route.name !== "login");

function logout() {
  localStorage.removeItem("token");
  appStore.setAuthenticated(false);
  router.push("/login");
}
</script>
