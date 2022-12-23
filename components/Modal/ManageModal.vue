<template>
  <div
    class="
      fixed
      top-0
      bottom-0
      right-0
      left-0
      flex
      justify-center
      z-10
      bg-modalbg
      overflow-auto
      items-center
      rounded
    "
    @click="$emit('close-modal2')"
  >
    <div
      class="
        bg-modalcontent
        text-center
        max-w-accmodalwidth
        min-w-eyemodalminwidth
        modal
        w-1/2
        p-10
        rounded
      "
      @click.stop
    >
      <div class="mb-5 text-left manage_modaltitle">
        Whitelisting & Blacklisting
      </div>
      <p class="text-base font-normal text-acctextcolor text-left">
        Whitelisted Roles
      </p>
      <div class="group relative">
        <div
          class="
            rounded-xl
            border-gray-700 border-2
            hover:border-selectorhighlight
            active:border-selectorhighlight
            wbinputbg
          "
        >
          <div
            id="default-stuff"
            class="my-auto w-full flex flex-row items-center cursor-pointer"
          >
            <div class="h-full items-center"></div>
            <div>
              <div
                v-if="wroledata.length > 0"
                class="flex flex-row justify-center"
              >
                <div v-for="(wrole, index) in wroledata" :key="index">
                  <div class="text-md font-semibold px-2">
                    <div class="my-2 flex flex-row justify-center items-center">
                      <button class="wbBtn px-2 text-wbbtntest">
                        <span class="text-3">{{ wrole.name }}</span>
                        <div @click="deldata('wrole',wrole,wroledata)">
                          <SVGDelete size="10" class="ml-1.5 text-white" />
                        </div>  
                      </button>
                    </div>
                  </div>
                </div>
              </div>
              <div v-else class="m-2">Select</div>
            </div>
            <div
              class="text-gray-300 pl-1 ml-auto mr-1"
              @click="toggleDropWrole"
            >
              <SVGDown
                :class="`${
                  dropOpenWrole
                    ? 'transform rotate-180 transition duration-500'
                    : 'transition duration-500'
                }`"
              />
            </div>
          </div>
          <div
            id="choice-stuff"
            :class="`z-30 bg-gradient-to-l border border-gray-600 rounded-xl w-full divide-y divide-gray-600 h-32 overflow-y-scroll from-discortics-header via-discortics-header to-discortics-header absolute flex flex-col items-start -bottom-32 ${
              dropOpenWrole ? 'block' : 'hidden'
            }`"
          >
            <div
              v-for="role in roledata"
              :key="role.id"
              :class="`flex flex-row items-center hover:bg-gray-800 px-1 py-2 w-full cursor-pointer`"
              @click="role == switchWrole(role)"
            >
              <div
                class="flex flex-row h-full items-center justify-center pl-1"
              ></div>
              <div class="truncate text-md font-semibold px-1">
                {{ role.name }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <p class="text-base font-normal text-acctextcolor text-left">
        Blacklisted Roles
      </p>
      <div class="group relative">
        <div
          class="
            rounded-xl
            wbinputbg
            border-gray-700 border-2
            hover:border-selectorhighlight
            active:border-selectorhighlight
          "
        >
          <div
            id="default-stuff"
            class="my-auto w-full flex flex-row items-center cursor-pointer"
          >
            <div class="h-full items-center"></div>
            <div>
              <div
                v-if="broledata.length > 0"
                class="flex flex-row justify-center"
              >
                <div v-for="(wrole, index) in broledata" :key="index">
                  <div class="text-md font-semibold px-2">
                    <div class="my-2 flex flex-row justify-center items-center">
                      <button class="wbBtn px-2 text-wbbtntest">
                        <span class="text-3">{{ wrole.name }}</span>
                        <div @click="deldata('brole',wrole,broledata)">
                          <SVGDelete size="10" class="ml-1.5 text-white" />
                        </div>
                      </button>
                    </div>
                  </div>
                </div>
              </div>
              <div v-else class="m-2">Select</div>
            </div>
            <div
              class="text-gray-300 pl-1 ml-auto mr-1"
              @click="toggleDropBrole"
            >
              <SVGDown
                :class="`${
                  dropOpenBrole
                    ? 'transform rotate-180 transition duration-500'
                    : 'transition duration-500'
                }`"
              />
            </div>
          </div>
          <div
            id="choice-stuff"
            :class="`z-30 bg-gradient-to-l border border-gray-600 rounded-xl w-full divide-y divide-gray-600 h-32 overflow-y-scroll from-discortics-header via-discortics-header to-discortics-header absolute flex flex-col items-start -bottom-32 ${
              dropOpenBrole ? 'block' : 'hidden'
            }`"
          >
            <div
              v-for="role in roledata"
              :key="role.id"
              :class="`flex flex-row items-center hover:bg-gray-800 px-1 py-2 w-full cursor-pointer`"
              @click="role == switchBrole(role)"
            >
              <div
                class="flex flex-row h-full items-center justify-center pl-1"
              ></div>
              <div class="truncate text-md font-semibold px-1">
                {{ role.name }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <p class="text-base font-normal text-acctextcolor text-left">
        Whitelisted Channels
      </p>
      <div class="group relative">
        <div
          class="
            rounded-xl
            wbinputbg
            border-gray-700 border-2
            hover:border-selectorhighlight
            active:border-selectorhighlight
          "
        >
          <div
            id="default-stuff"
            class="my-auto w-full flex flex-row items-center cursor-pointer"
          >
            <div class="h-full items-center"></div>
            <div>
              <div
                v-if="wchandata.length > 0"
                class="flex flex-row justify-center"
              >
                <div v-for="(wrole, index) in wchandata" :key="index">
                  <div class="text-md font-semibold px-2">
                    <div class="my-2 flex flex-row justify-center items-center">
                      <button class="wbBtn px-2 text-wbbtntest">
                        <span class="text-3">{{ wrole.name }}</span>
                        <div @click="deldata('wchan',wrole,wchandata)">
                          <SVGDelete size="10" class="ml-1.5 text-white" />
                        </div>  
                      </button>
                    </div>
                  </div>
                </div>
              </div>
              <div v-else class="m-2">Select</div>
            </div>
            <div
              class="text-gray-300 pl-1 ml-auto mr-1"
              @click="toggleDropWchannel"
            >
              <SVGDown
                :class="`${
                  dropOpenWchannel
                    ? 'transform rotate-180 transition duration-500'
                    : 'transition duration-500'
                }`"
              />
            </div>
          </div>
          <div
            id="choice-stuff"
            :class="`z-30 bg-gradient-to-l border border-gray-600 rounded-xl w-full divide-y divide-gray-600 h-32 overflow-y-scroll from-discortics-header via-discortics-header to-discortics-header absolute flex flex-col items-start -bottom-32 ${
              dropOpenWchannel ? 'block' : 'hidden'
            }`"
          >
            <div
              v-for="channel in channeldata"
              :key="channel.id"
              :class="`flex flex-row items-center ${
                channel == selectedwchannel ? 'bg-discortics-dropselected ' : ''
              }hover:bg-gray-800 px-1 py-2 w-full cursor-pointer`"
              @click="channel == switchWchan(channel)"
            >
              <div
                class="flex flex-row h-full items-center justify-center pl-1"
              ></div>
              <div class="truncate text-md font-semibold px-1">
                {{ channel.name }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <p class="text-base font-normal text-acctextcolor text-left">
        Blacklisted Channels
      </p>
      <div class="group relative">
        <div
          class="
            rounded-xl
            wbinputbg
            border-gray-700 border-2
            hover:border-selectorhighlight
            active:border-selectorhighlight
          "
        >
          <div
            id="default-stuff"
            class="my-auto w-full flex flex-row items-center cursor-pointer"
          >
            <div class="h-full items-center"></div>
            <div>
              <div
                v-if="bchandata.length > 0"
                class="flex flex-row justify-center"
              >
                <div v-for="(wrole, index) in bchandata" :key="index">
                  <div class="text-md font-semibold px-2">
                    <div class="my-2 flex flex-row justify-center items-center">
                      <button class="wbBtn px-2 text-wbbtntest">
                        <span class="text-3">{{ wrole.name }}</span>
                        <div @click="deldata('bchan',wrole,bchandata)">
                          <SVGDelete size="12" class="ml-1.5 text-white" />
                        </div>
                      </button>
                    </div>
                  </div>
                </div>
              </div>
              <div v-else class="m-2">Select</div>
            </div>
            <div
              class="text-gray-300 pl-1 ml-auto mr-1"
              @click="toggleDropBchannel"
            >
              <SVGDown
                :class="`${
                  dropOpenBchannel
                    ? 'transform rotate-180 transition duration-500'
                    : 'transition duration-500'
                }`"
              />
            </div>
          </div>
          <div
            id="choice-stuff"
            :class="`z-30 bg-gradient-to-l border border-gray-600 rounded-xl w-full divide-y divide-gray-600 h-32 overflow-y-scroll from-discortics-header via-discortics-header to-discortics-header absolute flex flex-col items-start -bottom-32 ${
              dropOpenBchannel ? 'block' : 'hidden'
            }`"
          >
            <div
              v-for="channel in channeldata"
              :key="channel.id"
              :class="`flex flex-row items-center hover:bg-gray-800 px-1 py-2 w-full cursor-pointer`"
              @click="channel == switchBchan(channel)"
            >
              <div
                class="flex flex-row h-full items-center justify-center pl-1"
              ></div>
              <div class="truncate text-md font-semibold px-1">
                {{ channel.name }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="top-8 rounded items-center flex flex-row justify-end">
        <button
          class="
            rounded
            w-question_btn_width
            h-10
            bg-question_addmcq
            mt-6
            text-xs
            text-question_addmcqtext
            items-center
            flex
            font-medium
            justify-center
          "
          @click="onAccept1"
        >
          Save Settings
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    managemodaldata: {
      type: Object,
      default: null,
    },
    channeldata: {
      type: Array,
      default: null,
    },
    roledata: {
      type: Array,
      default: null,
    },
  },
  data() {
    const wroledata = []
    const broledata = []
    const wchandata = []
    const bchandata = []
    return {
      acceptconfirm: '',
      wroledata,
      broledata,
      wchandata,
      bchandata,
      dropOpenWrole: false,
      selectedwrole: this.roledata[0],
      dropOpenBrole: false,
      selectedbrole: this.roledata[0],
      dropOpenWchannel: false,
      selectedwchannel: this.channeldata[0],
      dropOpenBchannel: false,
      selectedbchannel: this.channeldata[0],
    }
  },
  mounted() {
    this.wroledata = this.initialData(
      this.managemodaldata.wroles,
      this.roledata
    )
    this.broledata = this.initialData(
      this.managemodaldata.broles,
      this.roledata
    )
    this.wchandata = this.initialData(
      this.managemodaldata.wchans,
      this.channeldata
    )
    this.bchandata = this.initialData(
      this.managemodaldata.bchans,
      this.channeldata
    )
  },
  methods: {
    async onAccept1() {
      this.$emit('close-modal2')
      const wroleData = [];
      const broleData = [];
      const wchanData = [];
      const bchanData = [];
      this.wroledata.map((item, key) => {
        return wroleData.push(item.id)
      })
      this.broledata.map((item, key) => {
        return broleData.push(item.id)
      })
      this.wchandata.map((item, key) => {
        return wchanData.push(item.id)
      })
      this.bchandata.map((item, key) => {
        return bchanData.push(item.id)
      })
      const localdata = localStorage.getItem('localappdata');
      const initdata = JSON.parse(localdata)
      initdata[this.managemodaldata.ident].wroles = wroleData
      initdata[this.managemodaldata.ident].broles = broleData
      initdata[this.managemodaldata.ident].wchans = wchanData
      initdata[this.managemodaldata.ident].bchans = bchanData
      const senddata = { data: initdata }
        const token = localStorage.getItem('sessionToken')
        await fetch(
          'https://api.discortics.com/v2/application/' + localStorage.getItem('guildID'),
          {
            method: 'POST',
            headers: { 'Content-Type': 'application/json', Authorization: `Bearer ${token}` },
            body:JSON.stringify(senddata) ,
          }
        )
          .then((response) => response.json())
          .then((responseJson) => {
            console.log("responseJson", responseJson)
          })
          .catch((error) => {
            console.log(error)
          })   
    },
    toggleDropWrole() {
      this.dropOpenWrole = !this.dropOpenWrole
    },
    toggleDropBrole() {
      this.dropOpenBrole = !this.dropOpenBrole
    },
    toggleDropWchannel() {
      this.dropOpenWchannel = !this.dropOpenWchannel
    },
    toggleDropBchannel() {
      this.dropOpenBchannel = !this.dropOpenBchannel
    },
    toggleOff() {
      this.dropOpenWrole = false
      this.dropOpenBrole = false
      this.dropOpenWchannel = false
      this.dropOpenBchannel = false
    },
    switchWrole(channel) {
      this.toggleOff()
      let cnt = 0
      this.wroledata.map((item) => {
        if (item === channel) {
          cnt++
        }
        return cnt
      })
      if (cnt === 0) {
        this.wroledata.push(channel)
      }
    },
    switchBrole(channel) {
      this.toggleOff()
      let cnt = 0
      this.broledata.map((item) => {
        if (item === channel) {
          cnt++
        }
        return cnt
      })
      if (cnt === 0) {
        this.broledata.push(channel)
      }
    },
    switchWchan(channel) {
      this.toggleOff()
      let cnt = 0
      this.wchandata.map((item) => {
        if (item === channel) {
          cnt++
        }
        return cnt
      })
      if (cnt === 0) {
        this.wchandata.push(channel)
      }
    },
    switchBchan(channel) {
      this.toggleOff()
      let cnt = 0
      this.bchandata.map((item) => {
        if (item === channel) {
          cnt++
        }
        return cnt
      })
      if (cnt === 0) {
        this.bchandata.push(channel)
      }
    },
    deldata(tp,strdata,arrdata) {
      console.log("dkdkiekd.....", tp, strdata, arrdata)
      arrdata.map((item, index) =>{
        if(item === strdata) {
          arrdata.splice(index,1)
        }
        return arrdata;
      })
      // if(tp === 'wrole') {
      //    this.wroledata = arrdata;
      // }
    },
    initialData(arr1, arr2) {
      const realarr = []
      if (arr1) {
        arr2.map((item) => {
          arr1.map((wrole) => {
            if (item.id === wrole) {
              realarr.push(item)
            }
            return realarr
          })
          return item
        })
      }
      return realarr
    },
  },
}
</script>

<style scoped>
.modal {
  height: fit-content;
  backdrop-filter: blur(48px);
}
</style>