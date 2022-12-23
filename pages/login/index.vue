
<template>
  <div>
    <p class="text-white font-semibold text-center text-4xl mt-12 animate-pulse">
      Redirecting you to Discord...
    </p>
  </div>
</template>


<script>
export default {
  auth: false,
  middleware: ['auth'],
  mounted() {
    this.$nextTick(() => {
      this.$nuxt.$loading.start()
      if (!this.$auth.loggedIn) {
        if (process.browser) {
          window.open('/login/auth')
        }
      } else {
        this.$nuxt.$loading.finish()

        this.$router.push('/dashboard')
      }
    })
  },
}
</script>