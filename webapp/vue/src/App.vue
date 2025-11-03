<template>
  <v-app>
    <!-- Exibe a barra e o footer apenas se não for página de login -->
    <template v-if="!hideNavbar">
      <v-app-bar id="app_bar" dark>
        <AppNav />
      </v-app-bar>
    </template>

    <v-main :class="{'login-page': hideNavbar}">
      <router-view />
    </v-main>

    <template v-if="!hideNavbar">
      <AppFooter />
    </template>

    <LoginModal />
  </v-app>
</template>

<script setup>
import { computed } from "vue";
import { useRoute } from "vue-router";
import AppFooter from "@/components/AppFooter.vue";
import AppNav from "@/components/AppNav.vue";
import LoginModal from "@/components/login/ModalLogin.vue";

const route = useRoute();
const hideNavbar = computed(() => route.meta.hideNavbar);
</script>
