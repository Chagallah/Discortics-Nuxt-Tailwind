<template>
  <div>
    <div
      class="
        w-full
        h-12
        items-center
        bg-question
        flex flex-row
        justify-between
        bg-question_grad
        overflow-hidden
        rounded-xl
      "
    >
      <div class="flex flex-row p-2">
        <div
          class="
            absolute
            left-0
            top-0
            bg-indigo-400
            rounded-xl
            h-12
            w-7
            flex flex-row
            items-center
            justify-center
          "
        >
          <span class="text-xl">{{num + 1}}</span>
        </div>
        <div class="ml-10 text-question_color">
          {{
            typeof modaldata[0] === 'string'
              ? modaldata[0]
              : modaldata[0].question
          }}
        </div>
      </div>
      <div class="items-center text-xl absolute right-10" @click="toggle">
        <span v-if="toggled">
          <SVGDelete size="20" class="text-red transition duration-300" />
        </span>
        <span v-else>
          <SVGDelete
            size="20"
            class="text-red transform rotate-45 transition duration-300"
          />
        </span>
      </div>

      <button
        class="
          bg-indigo-400
          rounded-xl
          h-16
          w-24
          flex flex-row
          items-center
          justify-center
          bg-question_closebtn
          backdrop-blur-xl
        "
        @click="toggle"
        style="-webkit-filter: blur(24px)"
      ></button>
    </div>

    <div
      v-if="(typeof modaldata[0] !== 'string')"
      :class="`${toggled ? 'block' : 'hidden'} flex justify-between transition duration-300`"
    >
      <div class="top-8 rounded items-center flex flex-row">
        <SVGArrow size="20" class="mx-4 my-1" />
        <div class="relative">
          <button
            class="
              rounded
              w-24
              h-7
              mr-2
              mt-6
              text-xs
              text-question_btntext
              items-center
              flex
              justify-between
            "
          >
            {{ modaldata[1] }}
          </button>
        </div>
      </div>
    </div>
    <div v-else :class="`${toggled ? 'block' : 'hidden transition duration-300'} flex justify-between`">
      <div class="top-8 rounded items-center flex flex-row">
        <SVGArrow size="20" class="mx-4 my-1" />
        <div class="relative">
          <button
            class="
              rounded
              w-fit
              h-7
              mr-2
              mt-6
              text-xs
              text-question_btntext
              items-center
              flex
              justify-between
              items-center
            "
          >
            {{ modaldata[1] }}
          </button>
        </div>
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
    num: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      toggled: false,
    }
  },
  methods: {
    toggle() {
      this.toggled = !this.toggled
    },
  },
}
</script>

<style scoped>
</style>