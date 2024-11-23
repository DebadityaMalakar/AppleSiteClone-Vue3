<template>
  <div>
    <div class="flex items-center">
      <div
        v-for="(list, i) in hightlightsSlides"
        :key="list.id"
        id="slider"
        class="sm:pr-20 pr-10"
      >
        <div class="video-carousel_container">
          <div class="w-full h-full flex-center rounded-3xl overflow-hidden bg-black">
            <video
              id="video"
              :class="{ 'translate-x-44': list.id === 2 }"
              class="pointer-events-none"
              playsinline
              preload="auto"
              muted
              ref="videoRefs"
              @ended="() => i !== 3 ? handleProcess('video-end', i) : handleProcess('video-last')"
              @play="() => handleVideoPlay()"
              @loadedmetadata="(e) => handleLoadedMetaData(i, e)"
            >
              <source :src="list.video" type="video/mp4" />
            </video>
          </div>

          <div class="absolute top-12 left-[5%] z-10">
            <p
              v-for="(text, index) in list.textLists"
              :key="index"
              class="md:text-2xl text-xl font-medium"
            >
              {{ text }}
            </p>
          </div>
        </div>
      </div>
    </div>

    <div class="relative flex-center mt-10">
      <div class="flex-center py-5 px-7 bg-gray-300 backdrop-blur rounded-full">
        <span
          v-for="(_, i) in videoRefs"
          :key="i"
          class="mx-2 w-3 h-3 bg-gray-200 rounded-full relative cursor-pointer"
          ref="videoDivRefs"
        >
          <span
            class="absolute h-full w-full rounded-full"
            ref="videoSpanRefs"
          />
        </span>
      </div>

      <button class="control-btn">
        <img
          :src="isLastVideo ? replayImg : !isPlaying ? playImg : pauseImg"
          :alt="isLastVideo ? 'replay' : !isPlaying ? 'play' : 'pause'"
          @click="isLastVideo 
            ? handleProcess('video-reset')
            : !isPlaying
              ? handleProcess('play')
              : handleProcess('pause')"
        />
      </button>
    </div>
  </div>
</template>

<script>
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
gsap.registerPlugin(ScrollTrigger);

import { hightlightsSlides } from "../constants";
import { pauseImg, playImg, replayImg } from "../utils";

export default {
  name: "VideoCarousel",
  
  data() {
    return {
      videoState: {
        isEnd: false,
        startPlay: false,
        videoId: 0,
        isLastVideo: false,
        isPlaying: false,
      },
      loadedData: [],
      videoRefs: [],
      videoDivRefs: [],
      videoSpanRefs: [],
      hightlightsSlides,
      pauseImg,
      playImg,
      replayImg
    };
  },

  computed: {
    isLastVideo() {
      return this.videoState.isLastVideo;
    },
    isPlaying() {
      return this.videoState.isPlaying;
    },
    videoId() {
      return this.videoState.videoId;
    },
    startPlay() {
      return this.videoState.startPlay;
    }
  },

  watch: {
    'videoState.isEnd'() {
      this.updateSliderAnimation();
    },
    'videoState.videoId'() {
      this.updateSliderAnimation();
      this.setupVideoProgress();
    },
    loadedData: {
      handler(newVal) {
        if (newVal.length > 3) {
          this.updateVideoPlayback();
        }
      },
      deep: true
    }
  },

  mounted() {
    this.initializeGSAP();
  },

  methods: {
    initializeGSAP() {
      gsap.to("#video", {
        scrollTrigger: {
          trigger: "#video",
          toggleActions: "restart none none none",
        },
        onComplete: () => {
          this.videoState.startPlay = true;
          this.videoState.isPlaying = true;
        },
      });
    },

    updateSliderAnimation() {
      gsap.to("#slider", {
        transform: `translateX(${-100 * this.videoState.videoId}%)`,
        duration: 2,
        ease: "power2.inOut",
      });
    },

    setupVideoProgress() {
      let currentProgress = 0;
      const span = this.$refs.videoSpanRefs;

      if (span && span[this.videoState.videoId]) {
        let anim = gsap.to(span[this.videoState.videoId], {
          onUpdate: () => {
            const progress = Math.ceil(anim.progress() * 100);

            if (progress !== currentProgress) {
              currentProgress = progress;

              gsap.to(this.$refs.videoDivRefs[this.videoState.videoId], {
                width: window.innerWidth < 760 
                  ? "10vw" 
                  : window.innerWidth < 1200 
                    ? "10vw" 
                    : "4vw",
              });

              gsap.to(span[this.videoState.videoId], {
                width: `${currentProgress}%`,
                backgroundColor: "white",
              });
            }
          },
          onComplete: () => {
            if (this.videoState.isPlaying) {
              gsap.to(this.$refs.videoDivRefs[this.videoState.videoId], {
                width: "12px",
              });
              gsap.to(span[this.videoState.videoId], {
                backgroundColor: "#afafaf",
              });
            }
          },
        });

        if (this.videoState.videoId === 0) {
          anim.restart();
        }

        const animUpdate = () => {
          if (this.$refs.videoRefs[this.videoState.videoId]) {
            anim.progress(
              this.$refs.videoRefs[this.videoState.videoId].currentTime /
                hightlightsSlides[this.videoState.videoId].videoDuration
            );
          }
        };

        if (this.videoState.isPlaying) {
          gsap.ticker.add(animUpdate);
        } else {
          gsap.ticker.remove(animUpdate);
        }
      }
    },

    updateVideoPlayback() {
      if (this.loadedData.length > 3) {
        if (!this.videoState.isPlaying) {
          this.$refs.videoRefs[this.videoState.videoId].pause();
        } else if (this.videoState.startPlay) {
          this.$refs.videoRefs[this.videoState.videoId].play();
        }
      }
    },

    handleProcess(type, i) {
      switch (type) {
        case "video-end":
          this.videoState.isEnd = true;
          this.videoState.videoId = i + 1;
          break;

        case "video-last":
          this.videoState.isLastVideo = true;
          break;

        case "video-reset":
          this.videoState.videoId = 0;
          this.videoState.isLastVideo = false;
          break;

        case "pause":
        case "play":
          this.videoState.isPlaying = !this.videoState.isPlaying;
          break;

        default:
          return;
      }
    },

    handleVideoPlay() {
      this.videoState.isPlaying = true;
    },

    handleLoadedMetaData(i, event) {
      this.loadedData.push(event);
    }
  }
};
</script>
