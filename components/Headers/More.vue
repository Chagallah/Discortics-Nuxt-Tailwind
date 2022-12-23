<template>
  <div class="w-full flex flex-row justify-between space-x-4 items-center h-16 p-2">
    <div
      class="rounded-xl relative flex flex-row space-x-2 items-center justify-between"
    >
      <span :class="`${dropOpen ? 'text-blue4f' : 'text-base'} capitalize font-quicksand font-medium text-graye1 hover:text-blue4f transition-all cursor-pointer`" @click="toggleDrop" @mouseover="toggleOn"> More</span>

      <!-- <div class="text-gray-300 cursor-pointer" @click="toggleDrop"><SVGDown :class="`${dropOpen ? 'transform rotate-180 transition duration-500' : 'transition duration-500'}`" /></div> -->
      <div :class = "`${dropOpen ? 'block' : 'hidden'} absolute -bottom-48 flex flex-col p-2 rounded-xl w-48 right-0`" style="background: linear-gradient(102.94deg, rgba(255, 255, 255, 0.18) 0%, rgba(255, 255, 255, 0) 232.2%); border: 1px solid #2F2F36;" @mouseover="toggleOn" @mouseout="toggleOff">
          <a href="#" class="flex flex-row items-center py-2 justify-start hover:text-blue4f transition-all" target="_blank"><span class = "px-1">Status </span><span class="bg-blue-100 text-black text-xs font-medium inline-flex items-center px-2.5 py-0.5 rounded dark:bg-blue-200 dark:text-blue-800">  Soon</span></a>
          <a href="https://discortics.com/servers" class="flex flex-row items-center py-2 justify-start hover:text-blue4f transition-all" target="_blank"><span class = "px-1">Server List</span></a>
          <a href="https://discortics.com/privacy" class="flex flex-row items-center py-2 justify-start hover:text-blue4f transition-all" target="_blank"><span class = "px-1">Privacy Policy</span></a>
          <a href="https://discortics.com/refund" class="flex flex-row items-center py-2 justify-start hover:text-blue4f transition-all" target="_blank"><span class = "px-1">Refund Policy</span></a>
      </div>
    </div>
    
  </div>
</template>
        <script>

export default {
  data() {
    return {
      dropOpen: false,
    }
  },
  methods: {
    toggleDrop() {
      this.dropOpen = !this.dropOpen
    },
    toggleOff() {
        this.dropOpen = false;
    },
    toggleOn() {
        this.dropOpen = true;
    },
  },
}
</script>
