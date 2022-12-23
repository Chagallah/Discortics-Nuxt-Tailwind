<template>
  <div class="w-full mx-auto">
    <div class="flex flex-col items-start justify-start space-y-8 w-full">
      <div class="py-2 w-full">
        <div class="py-2 w-full">
          <div class="p-4 flex flex-col items-start space-y-1">
            <span
              class="
                text-2xl
                bg-navCurrent
                text-transparent
                bg-clip-text
                font-bold font-montserrat
              "
              >Configuration</span
            >
            <span class="text-sm">Manage the applications here!</span>
          </div>
        </div>
        <div class="flex flex-col w-full space-y-8 items-stretch justify-start">
          <div class="py-1 lg:py-8 px-4 w-full relative max-w-7xl">
            <div class="py-2 flex flex-col w-full">
              <button @click="addApplication" class="rounded-xl border-4 border-white border-dotted flex justify-center items-center px-6 py-3">
                <div>
                  <SVGPluss class="mb-px" />
                </div>
                &nbsp; Create a new application
              </button>
            </div>
          </div>
        </div>

        <div class="flex flex-col w-full space-y-8 items-stretch justify-start">
          <div
            v-for="(app, index) in apps"
            :key="index"
            class="flex flex-col w-full"
          >
            <div v-for="responsexs in responsex" :key="responsexs">
              <div v-if="responsexs === app.ident">
                <PageAppsApp
                  :title="app.name"
                  :content="app"
                  :roles="roles"
                  :channels="channels"
                  :responses="responsesData[responsexs]"
                />
              </div>
              <div v-else>
                <PageAppsApp
                  :title="app.name"
                  :content="app"
                  :roles="roles"
                  :responses="nodata"
                  :channels="channels"
                />
              </div>
            </div>           
          </div>

          <div class="py-2 w-full">
            <div class="p-4 flex flex-col items-start space-y-1">
              <span
                class="
                  text-2xl
                  bg-navCurrent
                  text-transparent
                  bg-clip-text
                  font-bold font-montserrat
                "
                >Responses</span
              >
              <span class="text-sm">Manage the applications here!</span>
            </div>
          </div>
          <div
            v-for="app in apps"
            :key="app.ident"
            class="flex flex-col w-full"
          >
             <div v-for="responsexs in responsex" :key="responsexs">
              <div v-if="responsexs === app.ident">
                <PageResponsesResponse
                  :title="app.name"
                  :content="app"
                  :roles="roles"
                  :channels="channels"
                  :responses="responsesData[responsexs]"
                />
              </div>
              <div v-else>
                <PageResponsesResponse
                  :title="app.name"
                  :content="app"
                  :roles="roles"
                  :responses="nodata"
                  :channels="channels"
                />
              </div>
            </div>         
          </div>
          
        </div>
      </div>
      <hr class="border-discortics-header w-full" />
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $api, $_ }) {
    const token = localStorage.getItem('sessionToken')
    let apps = await $api.$request({
      url: `v2/application/${localStorage.getItem('guildID')}`,
      method: 'get',
      headers: { Authorization: `Bearer ${token}` },
    })
    const appdata = apps.data
    console.log('====== apps =====>>', appdata)
    localStorage.setItem("localappdata", JSON.stringify(appdata))
    apps = apps.data
    const appsxx = Object.keys(apps)
    const appsyy = Object.values(apps)
    const appsx = appsxx
    const appsy = appsyy
    for (let i = 0; i < appsy.length; ++i) {
      appsy[i].ident = appsx[i]
    }
    const roles = await $api.$request({
      url: `v2/roles/${localStorage.getItem('guildID')}`,
      method: 'get',
      headers: { Authorization: `Bearer ${token}` },
    })
    const channels = await $api.$request({
      url: `v2/channels/${localStorage.getItem('guildID')}`,
      method: 'get',
      headers: { Authorization: `Bearer ${token}` },
    })
    let responses = await $api.$request({
      url: `v2/appresponse/${localStorage.getItem('guildID')}`,
      method: 'get',
      headers: { Authorization: `Bearer ${token}` },
    })
    responses = responses.data
    const responsesx = Object.keys(responses)
    const responsesy = Object.values(responses)
    const mydata = { mydata: 'no' }
    return {
      apps: appsy,
      roles: roles.roles,
      channels: channels.channels,
      responsesData: responses,
      responsex: responsesx,
      responsey: responsesy,
      nodata: mydata,
      appsx,
      appsy,
      appsxx,
      appsyy,
      appdata,
    }
  },
  methods: {
    addApplication() {
      if (!this.toastID) {
        this.toastID = this.$vToastify.info({
          body: 'Hold up! You have unsaved changes!',
          mode: 'prompt',
          answers: {
            Reset: false,
            Save: true,
          },
          canTimeout: false,
          draggable: false,
        })
        this.$vToastify.listen('vtPromptResponse', (payload) => {
          if (this.toastID && payload.id === this.toastID) {
            delete this.toastID
            const value = payload.response
            if (value) {
              this.updateApplication()
            }
          }
        })
      }
    },
    updateApplication() {
      const len = this.appsx.length + 1
      if (len < 10) {
        const localdata = localStorage.getItem('localappdata');
        const initdata = JSON.parse(localdata)
        const appkey = Object.keys(initdata)
        const appval = Object.values(initdata)
        const keys = appkey
        const values = appval
        keys.push('form' + len)
        values.push({ 
          open: false,
          auto: false,
          accepted: false,
          denied: false,
          questions: [],
          name: null,
          description: null,
          icon: null,
          shuffle: false,
          wroles: [],
          broles: [],
          wchans: [],
          bchans: []          
        })
        const result = {}
        keys.forEach((key, i) => (result[key] = values[i]))
        const senddata = { data: result }
        const formReference = {
              open: false,
              auto: false,
              accepted: false,
              denied: false,
              questions: [],
              name: null,
              description: null,
              icon: null,
              shuffle: false,
              wroles: [],
              broles: [],
              wchans: [],
              bchans: []
            }

        for(const form in result){
            const data2 = result[form];
            for(const k in formReference){
                if(data2[k] === undefined) {
                  result[form][k] = formReference[k];
                }
            }
        }
        this.sendData(senddata)
      }
    },
    async sendData(data) {
      const response = await this.$api.request({
        url: `/v2/application/${localStorage.getItem('guildID')}`,
        method: 'post',
        headers: {
          Authorization: `Bearer ${localStorage.getItem('sessionToken')}`,
        },
        data: JSON.stringify(data),
      })
      if (response.status === 200){
        // alert('submit')
      }
    },
  },
  layout: 'dashboard',
}
</script>
