<template>
  <div v-if="$fetchState.pending" id="profile" class="w-full flex flex-row justify-between space-x-4 items-center h-16 p-2">
    <div
      id="profile"
      class="rounded-xl relative flex flex-row space-x-2 items-center justify-between"
    >
      <div class="flex flex-row h-full items-center justify-center" @click="toggleDrop">
        <div class="object-cover bg-gray-600 h-9 w-9 rounded-full">
            </div>
      </div>
      <!-- <div class="text-sm text-indigo-300 px-1 cursor-pointer">
        <div class="text-sm font-semibold px-1 ml-2 bg-gray-600 h-6 w-full rounded-md">
            </div>
      </div> -->
      <div class="text-gray-300 cursor-pointer" @click="toggleDrop"><SVGDown :class="`${dropOpen ? 'transform rotate-180 transition duration-500' : 'transition duration-500'}`" /></div>
      <div :class = "`${dropOpen ? 'block' : 'hidden'} absolute -bottom-36 flex flex-col divide-y divide-gray-600 p-2 rounded-xl w-48 right-0`" style="background: linear-gradient(102.94deg, rgba(255, 255, 255, 0.12) 0%, rgba(255, 255, 255, 0) 232.2%); border: 1px solid #2F2F36;">
          <div class = "flex flex-row items-center py-2 justify-start"><div class="text-sm font-semibold px-1 ml-2 bg-gray-600 h-6 w-full rounded-md">
            </div></div>
          <NuxtLink to = "#" id = "signout" class = "flex flex-row items-center py-2 justify-start"><SVGNewPreServers /> <span class = "px-1">Servers</span></NuxtLink>
          <NuxtLink to = "/logout" id = "signout" @click = "$nuxt.$auth.logout" class = "cursor-pointer flex flex-row items-center py-2 justify-start"><SVGNewSignout /> <span class = "px-1">Sign Out</span></NuxtLink>
      </div>
    </div>
    
  </div>
  <div v-else id="profile" class="w-full flex flex-row justify-between space-x-4 items-center h-16 p-2">
    <div
      id="profile"
      class="rounded-xl relative flex flex-row space-x-2 items-center justify-between"
    >
      <div class="flex flex-row h-full items-center justify-center" @click="toggleDrop">
        <img class="object-cover w-9 h-9 rounded-full" :src="userAvatar" alt="user icon" />
      </div>
      <!-- <div class="text-sm text-indigo-300 px-1 cursor-pointer" @click="toggleDrop">
        <span class = "text-white">Hi,</span> {{ userName }}
      </div> -->
      <div class="text-gray-300 cursor-pointer" @click="toggleDrop"><SVGDown :class="`${dropOpen ? 'transform rotate-180 transition duration-500' : 'transition duration-500'}`" /></div>
      <div :class = "`${dropOpen ? 'block' : 'hidden'} absolute -bottom-36 flex flex-col divide-y divide-gray-600 p-2 rounded-xl w-48 right-0`" style="background: linear-gradient(102.94deg, rgba(255, 255, 255, 0.12) 0%, rgba(255, 255, 255, 0) 232.2%); border: 1px solid #2F2F36;">
          <div class = "flex flex-row items-center py-2 justify-start"><span class = "text-white">Hi,</span> {{ userName }}</div>
          <NuxtLink to = "/dashboard" id = "signout" class = "flex flex-row items-center py-2 justify-start hover:text-blue4f"><SVGNewPreServers /> <span class = "px-1">Servers</span></NuxtLink>
          <NuxtLink to = "/logout" id = "signout" class = "cursor-pointer flex flex-row items-center py-2 justify-start hover:text-blue4f"><SVGNewSignout /> <span class = "px-1">Sign Out</span></NuxtLink>
      </div>
    </div>
    
  </div>
</template>
        <script>

export default {
  data() {
    return {
      dropOpen: false,
       userAvatar: null,
       userName: null// this.$auth.user?.id? 
    }
  },
  async fetch() {
    if(this.$auth.user){
      this.userAvatar = this.$auth.user.avatar ? 'https://cdn.discordapp.com/avatars/' + this.$auth.user.id + '/' + this.$auth.user.avatar + '.webp' : "/img/user_icon.png";
      this.userName = this.$auth.user.username || "User";
      return;
    }
    const token = localStorage.getItem('sessionToken')
    const dataReturned = await this.$api.$request({
      url: 'v2/@me',
      method: 'get',
      headers: { Authorization: `Bearer ${token}` },
    });
    // this.$auth.user = dataReturned.user;
    this.userAvatar = dataReturned.user.avatar ? 'https://cdn.discordapp.com/avatars/' + dataReturned.user.id + '/' + dataReturned.user.avatar + '.webp' : "/img/user_icon.png";
    this.userName = dataReturned.user.username || "User";
    
  },
  methods: {
    toggleDrop() {
      this.dropOpen = !this.dropOpen
    },
    toggleOff() {
        this.dropOpen = false;
    },
  },
}
</script>