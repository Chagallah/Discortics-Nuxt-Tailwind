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
    @click="$emit('close-modal1')"
  >
    <div
      class="
        bg-modalcontent
        text-center
        max-w-accmodalwidth
        min-w-eyemodalminwidth
        modal
        w-1/2
        p-10
        rounded
      "
      @click.stop
    >
      <div class="text-2xl mb-5 text-left">Accepting Response</div>
      <p class="text-base font-normal text-acctextcolor text-left">
        Custom message (Optional)
      </p>
      <div class="py-2 flex flex-col lg:flex-row items-center mx-auto">
        <div class="group relative w-full">
          <input
            v-model="acceptconfirm"
            class="
              border-gray-500 border-opacity-80
              hover:border-selectorhighlight
              active:border-selectorhighlight
              bg-transparent
              p-2
              border-2
              rounded-md
              w-full
              h-12
            "
          />
          <!-- <div v-if="inputValid" class="font-quicksand pt-2">
            <vs-alert variant="error" no-bg>
              <span>The prefix cannot be <b>blank</b>!</span>
            </vs-alert>
          </div> -->
        </div>
      </div>
      <div class="top-8 rounded items-center flex flex-row justify-end">
        <button
          class="
            rounded
            w-question_btn_width
            h-10
            mt-6
            text-xs text-white
            items-center
            flex
            justify-center
            font-medium
            text-center
            mr-2
          "
          @click="$emit('close-modal1')"
        >
          Cancel
        </button>
        <button
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
          @click="onAccept1"
        >
          Confirm
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    modaldata: {
      type: Array,
      default: null,
    },
  },
  data() {
    return {
      acceptconfirm:'',
    }
  },
  methods: {
    onAccept1() {
      this.$emit('close-modal1')
    },
  },
}
</script>

<style scoped>
.modal {
  height: fit-content;
  backdrop-filter: blur(48px);
}
</style>