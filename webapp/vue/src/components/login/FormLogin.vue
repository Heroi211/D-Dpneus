<template>
  <div class="login-page" :class="{ 'in-modal': inModal }">
    <v-card class="login-card elevation-8">
      <div class="text-center mb-4">
        <img src="@/assets/PneuVerde.png" alt="D&D Pneus" class="login-logo"  />
      </div>

      <v-card-title class="justify-center">
        <h2 class="login-title">Acesso ao Sistema</h2>
      </v-card-title>

      <v-card-text>
        <v-alert v-if="visivel" color="error" density="compact" class="mb-4">
          Usuário ou senha inválidos
        </v-alert>

        <v-form>
          <v-text-field
            v-model="login"
            label="Usuário"
            prepend-inner-icon="mdi-account"
            variant="outlined"
            density="comfortable"
            hide-details="auto"
            class="mb-3"
          />
          <v-text-field
            v-model="password"
            label="Senha"
            type="password"
            prepend-inner-icon="mdi-lock"
            variant="outlined"
            density="comfortable"
            hide-details="auto"
          />
        </v-form>
      </v-card-text>

      <v-card-actions class="justify-center mt-2">
        <v-btn class="btn-outline" @click="cancelLogin">Cancelar</v-btn>
        <v-btn class="btn-primary" @click="handleLogin" :loading="overlay">
          Confirmar
        </v-btn>
      </v-card-actions>

      <div class="login-links">
        <a @click="goToForgotPassword">Esqueceu a senha?</a>
        <a @click="goToSignUp">Criar conta</a>
      </div>
    </v-card>

    <v-overlay :model-value="overlay" contained>
      <v-progress-circular indeterminate size="64" color="primary" />
    </v-overlay>
  </div>
</template>

<script>
import apiService from "@/services/ApiService";
import { useAppStore } from "@/stores/app";
import { defineComponent, ref } from "vue";
import { useRouter } from "vue-router";
import { processQueue } from "@/services/ApiService";

export default defineComponent({
  name: "FormLogin",
  props: {
    inModal: {
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    const appStore = useAppStore();
    const router = useRouter();

    const overlay = ref(false);
    const login = ref("");
    const password = ref("");
    const visivel = ref(false);

    async function handleLogin() {
      overlay.value = true;
      try {
        const response = await apiService.login(login.value, password.value);
        const token = response.access_token;
        localStorage.setItem("token", token);
        appStore.setAuthenticated(true);

        const user = await apiService.getUserLogged();
        appStore.userLogged = user;

        overlay.value = false;

        if (props.inModal) appStore.closeLoginModal();
        else router.push("/home");

        processQueue(null, token);
      } catch (err) {
        overlay.value = false;
        visivel.value = true;
        processQueue(err, null);
        setTimeout(() => (visivel.value = false), 3000);
      }
    }

    function cancelLogin() {
      if (props.inModal) appStore.closeLoginModal();
    }

    function goToForgotPassword() {
      router.push("/forgotpassword");
    }

    function goToSignUp() {
      router.push("/signup");
    }

    return {
      overlay,
      login,
      password,
      visivel,
      handleLogin,
      cancelLogin,
      goToForgotPassword,
      goToSignUp,
    };
  },
});
</script>
