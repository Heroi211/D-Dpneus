<template>
  <nav class="navbar bg-custom navbar-expand-lg">
    <div class="container-fluid d-flex justify-content-between align-items-center">
      <!-- Logo -->
      <router-link to="/home" class="navbar-brand d-flex align-items-center text-decoration-none" style="cursor: pointer" >
        <img id="container_navlogo" src="/src/assets/logo.png" alt="Logo" width="95" height="95"/>
        <span class="text-white ms-2 fst-italic fw-semibold">
          Olá, {{ userName }}
        </span>
      </router-link>

      <!-- Menu -->
      <div v-if="showMenu" class="d-flex align-items-center gap-4">
        <ul class="navbar-nav d-flex flex-row gap-4 mb-0">
          <li class="nav-item">
            
          </li>
          <li class="nav-item">
            <router-link class="nav-link text-white" to="/routines">Estoque</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link text-white" to="/clients">Vendas</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link text-white" to="/users">Clientes</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link text-white" to="/users">Automação</router-link>
          </li>
        </ul>
        <router-link
          to="/login"
          @click.native="logout"
          class="btn btn-sm btn-outline-light ms-3"
        >
          Sair
        </router-link>
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

// 🔹 Exibe o nome do usuário logado (fallback pra vazio)
const userName = computed(() => appStore.userLogged?.name || "");

// 🔹 Logout padrão
function logout() {
  localStorage.removeItem("token");
  appStore.setAuthenticated(false);
  router.push("/login");
}
</script>
