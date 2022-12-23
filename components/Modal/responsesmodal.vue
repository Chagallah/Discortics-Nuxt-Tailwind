<template>
  <div
    class="
      fixed
      top-0
      bottom-0
      right-0
      left-0
      flex
      justify-center
      z-10
      bg-modalbg
      overflow-auto
      items-center
      rounded
    "
    @click="$emit('close-modal')"
  >
    <div
      class="
        bg-modalcontent
        text-center
        max-w-eyemodalwidth
        min-w-eyemodalminwidth
        modal
        w-1/2
        p-10
        rounded
      "
      @click.stop
    >
      <div class="text-2xl mb-5 text-left">Questions</div>
      <div
        v-for="(modaldata, index) in modaldata"
        :key="index"
        class="relative mb-2"
      >
        <Modalcontent :modaldata="modaldata" :num="index"/>
      </div> 
      <div class="top-8 rounded items-center flex flex-row justify-end">
        <button
          class="
            rounded
            w-question_btn_width
            h-10
            bg-question_addmcq
            mt-6
            text-xs
            text-question_addmcqtext
            items-center
            flex
            justify-center
            font-medium
            text-center
            mr-2
          "
          @click="onAccept"
        >
          Accept
        </button>
        <button
          @click="$emit('close-modal')"
          class="
            rounded
            w-question_btn_width
            h-10
            bg-question_reset
            mt-6
            text-xs
            text-question_resettext
            items-center
            flex
            font-medium
            justify-center
          "
        >
          Deny
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Modalcontent from '~/components/Modal/Modalcontent.vue'
export default {
  components: {
    Modalcontent,
  },
  props: {
    modaldata: {
      type: Array,
      default: null,
    },
  },
   data() {
    return {
      // modaldata : this.modaldata,
    }
  },
  methods: {
    onAccept() {
      this.$emit('close-modal');
    }
  },
}
</script>

<style scoped>
.modal {
  height: fit-content;
  backdrop-filter: blur(48px);
}
</style>