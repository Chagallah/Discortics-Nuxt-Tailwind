<template>
  <div class="py-1 px-4 w-full relative max-w-7xl">
    <div
      class="
        max-w-7xl
        form-card
        w-full
        relative
        overflow-hidden
        flex flex-col
        justify-center
        overflow-hidden
      "
    >
      <div class="px-8">
        <div class="gradient-circle w-24 h-24 absolute right-28 -top-16" />
        <div class="pt-2 flex flex-row justify-between">
          <div class="py-2 flex flex-col justify-center">
            <p class="text-2xl pt-2 pb-1 font-semibold font-montserrat capitalize">
              {{ title || `Form ${content.ident.substring(4, 5)}` }}
            </p>
            <p class="text-sm pt-1 pb-2">
              Contains
              {{
                content.questions
                  ? content.questions.filter((x) => typeof x === 'string')
                      .length
                  : 0
              }}
              description and
              {{
                content.questions
                  ? content.questions.filter((x) => typeof x !== 'string')
                      .length
                  : 0
              }}
              mcqs based questions
            </p>
          </div>
          <div class="flex flex-row items-center">
            <div class="p-2">
              <MiscToggle :enabled="content.open" />
            </div>
            <div class="p-2">
              <button class="p-2 w-8 h-8 rounded-full bg-gray-600" @click="delApplication(content.ident)">
                <SVGRemove size="16" class="text-red" />
              </button>
            </div>
            <div class="p-2" @click="toggle">
              <button v-if="toggled" class="p-2 w-8 h-8">
                <SVGDown
                  size="20"
                  class="text-red transform rotate-180 transition duration-500"
                />
              </button>
              <button v-else class="p-2 w-8 h-8">
                <SVGDown size="20" class="text-red transition duration-500" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div
      :class="`${
        toggled ? '' : 'hidden'
      } overflow-hidden transition-all flex flex-col justify-start duration-500 ease-in-out -bottom-24 mt-2`"
    >    
      <!-- Questions part, Responses -->
      <div class="flex flex-col items-stretch space-y-4 w-full">
        <!-- Responses -->
        <div class="py-2 w-full">
          <div v-if="responses.mydata === 'no'" class="flex justify-center">
            <img src="../../../assets/img/image 1.png" alt="no response" />
          </div>
          <div v-else>
            <div v-for="(response, index) in responses.applicants" :key="index">
              <!-- +++++++++++++++ avatar dev +++++++++++++++++++ -->
              <div
                class="
                  max-w-7xl
                  form-card
                  w-full
                  relative
                  overflow-hidden
                  flex flex-col
                  justify-center
                  mt-3
                "
              >
                <div class="px-8">
                  <div class="pt-2 flex flex-row justify-between items-center">
                    <div class="flex flex-row justify-center items-center">
                      <div class="pb-2 flex flex-col justify-center flex">
                        <div
                          class="
                            w-14
                            border-8
                            rounded-full
                            border-transparent
                            ring-2
                          "
                        >
                          <img
                            :src="responses.users[response].avatar"
                            alt="avatar"
                            class="
                              shadow-lg
                              rounded-full
                              max-w-full
                              h-auto
                              align-middle
                              border-none
                            "
                          />
                        </div>
                      </div>
                      <div class="ml-7">
                        <span>{{ responses.users[response].username }}</span>
                      </div>
                    </div>

                    <div class="flex flex-row items-center mb-2">
                      <button
                        class="
                          rounded
                          w-question_btn_width
                          h-10
                          bg-question_addmcq
                          text-xs
                          text-question_addmcqtext
                          items-center
                          flex
                          justify-center
                          font-medium
                          text-center
                          mr-2
                        "
                        @click="showeyeModal1"
                      >
                        Accept
                      </button>
                      <button
                        class="
                          rounded
                          w-question_btn_width
                          h-10
                          bg-question_reset
                          text-xs
                          text-question_resettext
                          items-center
                          flex
                          font-medium
                          justify-center
                          mr-9
                        "
                      >
                        Deny
                      </button>
                      <div class="mr-7">
                        <button
                          class="p-2 w-8 h-8"
                          @click="showeyeModal(responses.responses[response])"
                        >
                          <SVGEye size="16" class="text-red" />
                        </button>
                      </div>
                      <div class="mr-7">
                        <button class="p-2 w-8 h-8 rounded-full bg-gray-600">
                          <SVGRemove size="16" class="text-red" />
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <!-- +++++++++++++++ avatar dev +++++++++++++++++++ -->
            </div>
          </div>
        </div>
      </div>
    </div>
    <Responsesmodal
      :modaldata="modaldata"
      v-show="showModal"
      @close-modal="showModal = false"
    />
    <Acceptmodal v-show="showModal1" @close-modal1="showModal1 = false" />
    <Managemodal
      :managemodaldata="content"
      :channeldata="channels"
      :roledata="roles"
      v-show="showModal2"
      @close-modal2="showModal2 = false"
    />
  </div>
</template>

<script>
import { Picker } from 'emoji-mart-vue'
import InputTag from 'vue-input-tag'
import Vue from 'vue'
import Responsesmodal from '~/components/Modal/responsesmodal.vue'
import Acceptmodal from '~/components/Modal/Acceptmodal.vue'
import Managemodal from '~/components/Modal/ManageModal.vue'
Vue.component('picker', Picker)
Vue.component('input-tag', InputTag)
export default {
  name: 'IndexNew',
  components: {
    Responsesmodal,
    Acceptmodal,
    Managemodal,
  },
  data() {
    const langs = ['add', 'remove']
    const langsdenied = ['add', 'remove']
    const showModal = false
    const showModal1 = false
    const showModal2 = false
    const modaldata = []
    const appname = ''
    const desdata = ''
    const olddesdata = ''
    const desnum = 0
    const tagque = {}
    const oldappname = ''
    return {
      langs,
      langsdenied,
      selected: langs[0],
      oldSelected: langs[0],

      selectedAccepted: this.roles[0],
      oldSelectedAccepted: this.roles[0],

      selectedDenied: this.roles[0],
      oldSelectedDenied: this.roles[0],

      denied_selected: langs[0],
      logging_selected: [],
      oldlogging_selected: [],
      denied_oldSelected: langs[0],

      dropOpen2: false,
      dropOpenAccepted: false,
      dropOpenDenied: false,
      dropOpen_denied: false,
      dropOpen_logging: false,

      toggled: false,
      toggled_sub: false,

      showModal,
      showModal1,
      showModal2,
      modaldata,

      appname,
      oldappname,
      desdata,
      olddesdata,
      desnum,
      tagque,
    }
  },
  fetch() {
    let lang = this.langs[0]
    // ************* accepted *************
    if (!this.content.accepted) {
      this.selected = this.langs[0]
      lang = 'add'
    } else {
      lang = this.content.accepted.action
    }
    this.selected = this.langs.find((item) => item === lang)
    this.oldSelected = this.selected

    // *************** denied *****************
    let langdenied = this.langsdenied[0]
    if (!this.content.denied) {
      this.denied_selected = this.langsdenied[0]
      langdenied = 'add'
    } else {
      langdenied = this.content.denied.action
    }
    this.denied_selected = this.langsdenied.find((item) => item === langdenied)
    this.denied_oldSelected = this.denied_selected

    // *************** logging *****************
    if(this.content.auto) {
      this.channels.map((item, index) => {
        if(item.id === this.content.auto) {
          this.logging_selected = item;
          this.oldlogging_selected = this.logging_selected;
        }
        return item;
      })
    } else {
      this.logging_selected = {id:'111', name:'disabled'}
      this.oldlogging_selected = this.logging_selected
    }

    // ******************* accepted role ***********************
    if (!this.content.accepted) {
      this.selectedAccepted = ''
    } else {
      this.roles.map((item, index) => {
        if (item.id !== this.content.accepted.role) {
          this.selectedAccepted = this.roles[index]
        }
        return this.oldSelectedAccepted
      })
    }
    this.oldSelectedAccepted = this.selectedAccepted

    // ******************* denied role ***********************
    if (!this.content.denied) {
      this.selectedDenied = ''
    } else {
      this.roles.map((item, index) => {
        if (item.id === this.content.denied.role) {
          this.selectedDenied = this.roles[index]
        }
        return this.selectedDenied
      })
    }
    this.oldSelectedDenied = this.selectedDenied
    this.appname = this.content.name
    this.oldappname = this.appname
    console.log('content====>', this.content)
    console.log('channels========>', this.channels)
  },
  mounted() {
    const token = localStorage.getItem('sessionToken')
    fetch(
      'https://api.discortics.com/v2/emotes/' + localStorage.getItem('guildID'),
      {
        method: 'get',
        headers: { Authorization: `Bearer ${token}` },
      }
    )
      .then((response) => response.json())
      .then((responseJson) => {
        const data = responseJson.emotes
        data.map((item, index) => {
          if(item.id === this.content.icon) {
            this.emotes = item.icon
          } 
          return this.emotes
        })
        // this.emotes = responseJson.emotes
        this.emotes1 = data[Math.floor(Math.random()*data.length)].icon
        this.emotes2 = data[Math.floor(Math.random()*data.length)].icon
        this.emotes3 = data[Math.floor(Math.random()*data.length)].icon
        this.emotes4 = data[Math.floor(Math.random()*data.length)].icon
      })
      .catch((error) => {
        console.log(error)
      })

    if (this.content.description) {
      this.desdata = this.content.description
      this.olddesdata = this.desdata
      this.desnum = this.content.description.length
    }
    if(this.content.icon) {
      if(this.content.icon.length === 18) {
        this.oldemoteVal = this.emotes
      } else {
        this.oldemoteVal = this.content.icon
      }      
    } 
  },
  props: {
    title: {
      type: String,
      default: null,
    },
    roles: {
      type: Array,
      default: null,
    },
    channels: {
      type: Array,
      default: null,
    },
    content: {
      type: Object,
      default: null,
    },
    responses: {
      type: Object,
      default: null,
    },
  },
  methods: {
    toggle() {
      this.toggled = !this.toggled
    },
    showeyeModal(data) {
      if (data !== undefined) {
        this.modaldata = data
        this.showModal = true
      }
    },
    showeyeModal1() {
      this.showModal1 = true
    },
    showManage() {
      this.showModal2 = true
    },
    valueChanged(ident, oldval, newval, type) {
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
              this.updateData(ident, oldval, newval, type)
            } else if(!value){
              if(type === 'appemoji') {
                this.emoteVal = this.oldemoteVal
              } else if(type === 'applogging') {
                this.logging_selected = this.oldlogging_selected
              } else if(type === 'appaccrole') {
                this.selected = this.oldSelected
              } else if( type === 'appaccepted') {
                this.selectedAccepted = this.oldSelectedAccepted
              } else if(type === 'appdenrole') {
                this.denied_selected = this.denied_oldSelected
              } else if(type === 'appdenied') {
                this.oldSelectedDenied = this.selectedDenied
              } 
            }
          }
        })
      }
    },
    updateData(ident, oldval, newval, type) {
      console.log(type)
      
    },
  },
}
</script>
