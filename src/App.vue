<template>


  <RouterView />
</template>

<script>
<link rel="manifest" href="/public/manifest.webmanifest">
import IconLogo from "./components/icones/Logo.vue";
import { FilmIcon } from "@heroicons/vue/outline";
export default {
  components: { IconLogo, FilmIcon },
};
</script>
