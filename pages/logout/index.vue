
<template>
  <div class="h-screen w-full grid place-items-center absolute fixed z-50 bg-defaultBack">
  </div>
</template>


<script>
export default {
  auth: false,
  middleware: ['auth'],
  mounted() {
      this.$nuxt.$loading.start()
      localStorage.clear();
      this.$auth.logout();
      window.location.reload(true);
      this.$nuxt.$loading.finish()
      this.$router.push('/');
  },
}
</script>