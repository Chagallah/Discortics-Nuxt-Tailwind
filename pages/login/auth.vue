
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
    if (!this.$auth.loggedIn) {
      this.$auth.loginWith('discordapp')
    } else {
      this.$router.push('/dashboard')
    }
  },
}
</script>