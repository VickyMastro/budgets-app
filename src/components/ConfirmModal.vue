<template>
  <q-card style="width: 500px; max-width: 80vw">
    <q-card-section class="row items-center">
      <span class="q-ml-sm text-subtitle1"
        >¿Esta seguro que quiere cerrar sesión?</span
      >
    </q-card-section>

    <q-card-actions align="right">
      <q-btn flat label="Cancelar" color="primary" v-close-popup />

      <q-btn
        label="Cerrar sesión"
        color="negative"
        @click="confirmLogout"
        v-close-popup
      />
    </q-card-actions>
  </q-card>
</template>

<script setup>
import { useRouter } from "vue-router";
import { auth } from "src/boot/firebase";
import { signOut } from "@firebase/auth";

const router = useRouter();

const confirmLogout = async () => {
  try {
    await signOut(auth);

    router.push({ name: "Login" });
  } catch (error) {
    console.error(error);
  }
};
</script>
