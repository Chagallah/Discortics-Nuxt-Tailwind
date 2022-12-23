
<template>
  <div class="h-screen w-full grid place-items-center absolute fixed z-50 bg-defaultBack">
  </div>
</template>

<script>
export default {
  mounted() {
    this.$nextTick(async () => {
      this.$nuxt.$loading.start()
      const token = this.$auth.strategy.token.get()
      const res = await fetch('https://api.discortics.com/v2/signin', {
        method: 'POST',
        headers: { 'content-type': 'application/json' },
        body: JSON.stringify({ token: token.split(' ')[1] }),
      })
      const response = await res.json()
      if (res.status === 200) {
        console.info(response)
        //    await store.commit('dash/set', response.data.token, Date.now() + 3600000)
        localStorage.removeItem('guildID')
        localStorage.setItem('sessionToken', response.token)
        if (window.opener) {
          window.opener.location.reload()
          window.opener.focus()
          this.$nuxt.$loading.finish()
          window.close()
        } else {
          this.$nuxt.$loading.finish()
          this.$router.push('/dashboard')
        }
      } else {
        this.$router.push('/an-error-occured')
      }
    })
  },
}
</script>
