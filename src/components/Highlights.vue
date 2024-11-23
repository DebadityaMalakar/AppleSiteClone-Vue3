<script>
import { gsap } from "gsap";
import { onMounted } from "vue";
import { watchImg, rightImg } from "../utils/index.js";
import VideoCarousel from "./VideoCarousel.vue";

export default {
  name: "Highlights",
  components: {
    VideoCarousel,
  },
  data() {
    return {
      watchImg,
      rightImg,
    };
  },
  setup() {
    onMounted(() => {
      gsap.to("#title", { opacity: 1, y: 0 });
      gsap.to(".link", { opacity: 1, y: 0, duration: 1, stagger: 0.25 });
    });
  },
};
</script>

<template>
  <section id="highlights" class="w-screen overflow-hidden h-full common-padding bg-zinc">
    <div class="screen-max-width">
      <div class="mb-12 w-full items-end md:flex justify-between">
        <h1 id="title" class="section-heading">Get the highlights</h1>
        <div class="flex flex-wrap items-end gap-5">
          <p class="link">
            Watch the film
            <img :src="watchImg" alt="watch" />
          </p>
        </div>
        <div class="flex flex-wrap items-end gap-5">
          <p class="link">
            Watch the event
            <img :src="rightImg" alt="event" />
          </p>
        </div>
      </div>
      <VideoCarousel />
    </div>
  </section>
</template>
