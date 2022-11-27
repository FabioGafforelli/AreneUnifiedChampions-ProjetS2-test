<template>
  <ReloadPWA />
  <img alt="Vue logo" src="./assets/logo.png" />

  <RouterView />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ReloadPWA from "./components/ReloadPWA.vue";
import IconLogo from "./components/icones/Logo.vue";
import { FilmIcon } from "@heroicons/vue/outline";
export default defineComponent({
  name: "App",
  components: {
    ReloadPWA,
    IconLogo, FilmIcon
  },
});
</script>
