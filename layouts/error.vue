<template>
  <div class="error404 text-center">
    <div class="error-content grid grid-cols-2">
      <div class="mt-24 h-3/5 flex">
      <img src="/img/error3.png" alt="error-img" :class="`transition duration-1000 ease-in-out transform ${animating ? 'translate-x-0' : 'translate-x-52'} mx-auto h-3/5 absolute z-40`" />
        <img src="/img/error2.png" alt="error-img" :class="`transition duration-2000 ease-in-out transform ${stillAnimating ? 'translate-y-0' : 'translate-y-2'} mx-auto h-3/5 absolute z-30`" />
        <img src="/img/error1.png" alt="error-img" :class="`transition duration-1000 ease-in-out transform ${animating ? 'translate-x-0' : '-translate-x-52'} mx-auto h-3/5 absolute z-20`" />
      </div>
      <div class="">
        <h1 class="error-number">{{error.statusCode}}</h1>
        <p class="mini-text">Ooops!</p>
        <p class="error-text mb-4 mt-1">Are you lost? Don't Worry!</p>
        <div class="p-8">
          <a
            href="/"
            class="
              bg-gradient-to-r
              hover:bg-gradient-to-tr
              w-48
              mx-auto
              from-discortics-200
              to-discortics-300
              z-10
              p-3
              rounded-md
              transition-all
              block
              duration-700
              ease-in-out
              transform
              hover:-translate-y-1
            "
            >Let's take you Home</a
          >
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  layout: 'errorLayout',
  props: {
    error: {
      type: Object,
      default() {return {statusCode: 400}},
    }
  },
  data() {
    return {
      animating: false,
      stillAnimating: false,
    }
  },
  methods: {
    animateThisShit() {
      this.animating = true;
    },
    stillAnimateThisShit() {
      this.stillAnimating = !this.stillAnimating;
    }
  },
  mounted() {
    this.$nextTick(() => {
      setTimeout(this.animateThisShit, 500)
      setInterval(this.stillAnimateThisShit, 1000)
    })
  }
}
</script>    
<style scoped src = '@/static/css/400.css' />
