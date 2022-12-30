<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="toggleLeftDrawer" />

        <q-toolbar-title> Budgets App </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="openMenu" side="left" overlay bordered>
      <q-list>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
        <q-item clickable tag="a" @click="doLogout">
          <q-item-section avatar>
            <q-icon name="logout" color="negative" />
          </q-item-section>

          <q-item-section>
            <q-item-label>Cerrar sesión</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from "vue";
import EssentialLink from "src/components/MenuLink.vue";
import { auth } from "src/boot/firebase";
import { signOut } from "@firebase/auth";
import { useRouter } from "vue-router";

const essentialLinks = [
  {
    title: "Usuario",
    iconColor: "primary",
    icon: "person_outline",
    link: "UserProfile",
  },
  {
    title: "Ingresos",
    iconColor: "positive",
    icon: "trending_up",
    link: "https://github.com/quasarframework",
  },
  {
    title: "Gastos",
    iconColor: "negative",
    icon: "trending_down",
    link: "https://chat.quasar.dev",
  },
  {
    title: "Transferencias",
    iconColor: "primary",
    icon: "sync_alt",
    link: "https://forum.quasar.dev",
  },
  {
    title: "Categorias",
    iconColor: "primary",
    icon: "widgets",
    link: "https://twitter.quasar.dev",
  },
  {
    title: "Cajas",
    iconColor: "primary",
    icon: "wallet",
    link: "https://awesome.quasar.dev",
  },
  {
    title: "Presupuesto",
    iconColor: "primary",
    icon: "leaderboard",
    link: "https://facebook.quasar.dev",
  },
];

const openMenu = ref(false);
const toggleLeftDrawer = () => {
  openMenu.value = !openMenu.value;
};

const router = useRouter();
const doLogout = async () => {
  try {
    await signOut(auth);

    router.push({ name: "Login" });
  } catch (error) {
    console.error(error);
  }
};
</script>
