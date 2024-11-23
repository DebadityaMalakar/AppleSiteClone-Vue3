<script setup>
import Hero from "./components/Hero.vue";
import Navbar from "./components/Navbar.vue";
import Highlights from "./components/Highlights.vue";
import Model from "./components/Model.vue";

// export default {
//   components : {
//     Hero,
//     Navbar
//   }
// }
</script>

<template>
  <main class="bg-black">
    <Navbar />
    <Hero />
    <Highlights />
    <Model />
  </main>
</template>