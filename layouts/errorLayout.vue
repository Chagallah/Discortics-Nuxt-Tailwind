<script>
import Navigation from '@/data/Navigation'
import HeaderNew from '~/components/Layout/HeaderNew.vue'
export default {
    components: { HeaderNew },
    data() {
        return {
            Navigation
        };
    },
    head() {
        return {
            title: this.title,
            meta: [
                {
                    hid: "description",
                    name: "description",
                    content: "My custom description",
                },
            ],
        };
    }
}
</script>

<template>
  <div class="flex flex-col min-h-screen font-quicksand tracking-wide bg-discortics-100 text-white">
    <HeaderNew />
    <main class = "flex-1 bg-defaultBack bg-repeat-round">
      <Nuxt />
    </main>
  </div>
</template>
