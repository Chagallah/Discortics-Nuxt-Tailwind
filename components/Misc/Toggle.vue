<template>
  <div
    :class="`w-20 cursor-pointer relative transition duration-300 ease-in-out w-12 h-8 flex items-center ${
      toggled ? 'bg-toggleon bg-toggleonbg relative' : 'bg-toggleoff bg-none'
    } rounded-full`"
    @click="toggle"
  >
    <div
      :class="`p-px flex justify-center items-center transition duration-300 ease-in-out transform absolute w-5 h-5 rounded-full ${
        toggled
          ? 'translate-x-5 absolute left-9 bg-toggleon'
          : 'translate-x-0 bg-white opacity-20'
      } left-1 top-1 bottom-1 my-auto`"
    ></div>
    <div class="toggletext">
      <span v-if="toggled" class="absolute left-2.5 text-toggleopencolor">OPEN</span>
      <span v-else class="absolute left-7 text-xs">CLOSE</span>
    </div>  
  </div>
</template>
<script>
export default {
  props: {
    enabled: {
      type: Boolean,
      default() {
        return false
      },
    },
  },
  data() {
    return {
      toggled: this.enabled,
    }
  },
  methods: {
    toggle() {
      this.toggled = !this.toggled
    },
    toggleOn() {
      this.toggled = true
    },
    toggleOff() {
      this.toggled = false
    },
    getState() {
      return this.toggled
    },
  },
}
</script>