<script>
import { gsap } from "gsap";
import { heroVideo, smallHeroVideo } from "../utils/index.js";
import { ref, onMounted, onUnmounted } from "vue";

export default {
  name: "Hero",
  setup() {
    const videoSrc = ref(
      window.innerWidth < 760 ? smallHeroVideo : heroVideo
    );

    const updateVideoSrc = () => {
      videoSrc.value = window.innerWidth < 760 ? smallHeroVideo : heroVideo;
    };

    onMounted(() => {
      gsap.to("#hero", { opacity: 1, delay: 2 });
      gsap.to("#cta", {opacity:1,y:-50,delay: 2});
      window.addEventListener("resize", updateVideoSrc);
    });

    onUnmounted(() => {
      window.removeEventListener("resize", updateVideoSrc);
    });

    return { videoSrc };
  },
};
</script>

<template>
  <section class="w-full nav-height bg-black relative">
    <div class="h-5/6 w-full flex-center flex-col">
      <p id="hero" class="hero-title">iPhone 15 Pro</p>
      <div class="md:w-10/12 w-9/12">
        <video class="pointer-events-none" autoplay muted loop playsinline :key="videoSrc">
          <source :src="videoSrc" />
        </video>
      </div>
    </div>

    <div id="cta" class="flex flex-col items-center opacity-0 translate-y-20">
        <a href="#highlights" class="btn">Buy</a>
        <p class="font-normal text-xl">From $199/Month or $999</p>
    </div>
  </section>
</template>
