<template>
  <q-page class="row justify-center content-center">
    <q-form @submit="onSubmit" class="q-gutter-lg" style="width: 400px">
      <h4 class="text-center text-secondary">Registrarse</h4>

      <q-input
        v-model="name"
        label="Nombre"
        rounded
        outlined
        type="text"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor, complete este campo',
        ]"
      />

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
          (val) =>
            (val && val.length > 6) || 'La clave debe superar 6 caracteres',
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

      <router-link :to="{ name: 'Login' }"
        >¿Ya tenes usuario? Inicia sesión</router-link
      >

      <div class="flex justify-end">
        <q-btn
          rounded
          outlined
          label="Crear cuenta"
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
import { createUserWithEmailAndPassword, updateProfile } from "@firebase/auth";

const name = ref("");
const email = ref("");
const password = ref("");
const isPwd = ref(true);
const $q = useQuasar();

const onSubmit = async () => {
  try {
    await createUserWithEmailAndPassword(auth, email.value, password.value);
    await updateProfile(auth.currentUser, { displayName: name.value });
  } catch (error) {
    console.error(error);
  }
  $q.notify({
    color: "green-4",
    textColor: "white",
    icon: "cloud_done",
    message: "Se creo el usuario con éxito",
  });
};
</script>
