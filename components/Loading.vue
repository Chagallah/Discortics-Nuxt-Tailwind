<template>
  <div v-if="loading" class="loading fixed z-50 bg-defaultBack">
    <lottie
      :width="480"
      :height="270"
      :options="lottieOptions"
      @animCreated="handleAnimation"
    />
  </div>
</template>
<script>
import lottie from "vue-lottie/src/lottie.vue";
import * as animationData from "@/assets/loading.json";

export default {
  components: {
    lottie,
  },
  data() {
    return {
      loading: false,
      anim: null, // for saving the reference to the animation
      lottieOptions: { animationData: animationData.default, renderer: "canvas" },
    };
  },
  methods: {
    handleAnimation(anim) {
      this.anim = anim;
    },
    start() {
      this.loading = true;
    },
    finish() {
      this.loading = false;
    },
  },
};
</script>

<style>
.loading {
  background: black;
  width: 100%;
  height: 100vh;
  display: grid;
  place-items: center;
  position: absolute;
  top: 0;
  left: 0;
}
</style>