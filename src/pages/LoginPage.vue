<template>
  <q-page class="row justify-center content-center">
    <q-form @submit="onSubmit" class="q-gutter-lg" style="width: 400px">
      <h4 class="text-center text-secondary">Iniciar sesión</h4>

      <q-input
        v-model="email"
        label="Email"
        rounded
        outlined
        type="email"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor, complete este campo',
        ]"
      />

      <q-input
        v-model="password"
        label="Contraseña"
        class="q-mb-md"
        rounded
        outlined
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor, complete este campo',
        ]"
        :type="isPwd ? 'password' : 'text'"
      >
        <template v-slot:append>
          <q-icon
            :name="isPwd ? 'visibility_off' : 'visibility'"
            class="cursor-pointer"
            @click="isPwd = !isPwd"
          />
        </template>
      </q-input>

      <router-link :to="{ name: 'Register' }"
        >¿No tenes usuario? Registrate</router-link
      >

      <div class="flex justify-between">
        <q-btn outline rounded color="primary" @click="doLoginWithGoogle">
          <div class="row items-center">
            <q-icon>
              <img src="~assets/google-logo.svg" alt="" />
            </q-icon>
            <span class="q-ml-sm">Ingresar con google</span>
          </div>
        </q-btn>

        <q-btn
          rounded
          outlined
          label="Ingresar"
          type="submit"
          color="primary"
        />
      </div>
    </q-form>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { useQuasar } from "quasar";
import { auth } from "src/boot/firebase";
import {
  GoogleAuthProvider,
  signInWithEmailAndPassword,
  signInWithPopup,
  signInWithRedirect,
} from "@firebase/auth";
import { useRouter } from "vue-router";

const router = useRouter();

const email = ref("");
const password = ref("");
const isPwd = ref(true);
const $q = useQuasar();

const doLoginWithGoogle = async () => {
  try {
    const provider = new GoogleAuthProvider();
    if ($q.platform.is.desktop) {
      await signInWithPopup(auth, provider);
    } else {
      await signInWithRedirect(auth, provider);
    }

    router.push({ name: "Home" });
    $q.notify({
      color: "green-4",
      textColor: "white",
      icon: "cloud_done",
      message: "Inicio sesión con éxito",
    });
  } catch (error) {
    console.error(error);
  }
};

const onSubmit = async () => {
  try {
    await signInWithEmailAndPassword(auth, email.value, password.value);

    router.push({ name: "Home" });

    $q.notify({
      color: "green-4",
      textColor: "white",
      icon: "check_circle",
      message: "Inicio sesión con éxito",
    });
  } catch (error) {
    if (error.code == "auth/wrong-password") {
      $q.notify({
        color: "red-4",
        textColor: "white",
        icon: "warning",
        message: "Contraseña incorrecta ",
      });
    }
    if (error.code == "auth/user-not-found") {
      $q.notify({
        color: "red-4",
        textColor: "white",
        icon: "warning",
        message: "Email no encontrado ",
      });
    }
    if (error.code == "auth/to-many-request") {
      $q.notify({
        color: "red-4",
        textColor: "white",
        icon: "warning",
        message: "Demasiados intentos, intentelo luego",
      });
    }
    console.error(error);
  }
};
</script>
