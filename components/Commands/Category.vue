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
      "
    >
      <div class="px-8">
        <div class="pt-2 flex flex-row justify-between">
          <div class="py-2 flex flex-col justify-center">
            <p class="text-2xl pt-2 pb-1 font-semibold font-montserrat capitalize">
              {{ title || `Form ${content.ident.substring(4, 5)}` }}
            </p>
            <p class="text-sm pt-1 pb-2 font-quicksand">
              Contains
              {{
                content
                  ? Object.keys(content).length-2
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
              <MiscTogglegradent :id="'id'" :enabled="!content.disabled" @subdata="subdata" :typedata="content" type="category" :checkdata="null"/>
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
      <div class="flex flex-col items-stretch space-y-4 w-full">
        <!-- Commands -->
        <div class="py-2 w-full">
          <div >
            <div v-for="(contents, index) in this.arrkey" :key="index">
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
                  <div class="flex flex-row justify-between items-center">
                    <div class="flex flex-row justify-center items-center">
                      <div>
                        <span class="font-montserrat font-semibold text-xl capitalize">{{ contents }}</span>
                      </div>
                    </div>

                    <div :class="`${
                            !contentdata[contents].disabled ? '' : 'opacity-50'
                          } flex flex-row items-center m-2`">
                      <div class="p-2">
                        <MiscTogglegradent :id="contents" :typedata="contentdata[contents]" @subdata="subdata" :enabled="!contentdata[contents].disabled" type="commands" :checkdata="contentdata"/>
                      </div>
                      <div class="p-2 mr-5">
                        <button class="rounded manage"  @click="showManage(contentdata[contents])">
                        Manage
                        </button>
                    </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <ManagemodalCommand
      :managemodaldata="content"
      :channeldata="channels"
      :roledata="roles"
      :modalData="modalDatas"
      v-show="showManageModal"
      @close-modal="showManageModal = false"
    />
    <PopupModal
      :ident="content.ident"
      :oldval="oldval"
      :newval="newval"
      :type="apptype"
      v-show="showModal3"
      @submitdata="submitdatas"
      @close-modal3="showModal3 = false"
    />
  </div>
</template>

<script>
import ManagemodalCommand from '~/components/Modal/ManageCommand.vue'
import PopupModal from '~/components/Modal/PopupModal.vue'
export default {
  name: 'IndexNew',
  components: {
    ManagemodalCommand,
    PopupModal,
  },
  data() {
    const contentkey = []
    const contentval = []
    const arrkey = []
    const arrval = []
    const showManageModal = false
    const modalDatas = null
    const contentdata = {}
    const oldcategorytoggle = false
    const categorytoggle = false
    const oldcommandtoggle = false
    const commandtoggle = false
    const showModal3 = false
    const oldval = null
    const newval = null
    const apptype = ''
    return {
      toggled: false,
      contentkey,
      contentval,
      arrkey,
      arrval,
      showManageModal,
      modalDatas,
      contentdata,
      oldcategorytoggle,
      categorytoggle,
      oldcommandtoggle,
      commandtoggle,
      showModal3,
      oldval, 
      newval,
      apptype
    }
  },
  fetch() {
  },
  mounted() {
    this.contentdata = this.content
    this.oldcategorytoggle = this.content.disabled
    this.categorytoggle = this.content.disabled
    this.contentkey = Object.keys(this.content)
    this.contentval = Object.values(this.content)
    this.contentkey.map((item, index) => {
      if(item !== 'disabled' & item !== 'ident') {
        this.arrkey.push(item)
      }
      return this.arrkey
    })
    this.contentval.map((item, index) => {
      if(typeof(item) !== 'string' & typeof(item) !== 'boolean') {
        this.arrval.push(item)
      }
      return this.arrval
    })
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
    showManage(data) {
      if(!data.disabled) {
        this.showManageModal = true
        this.modalDatas = data
      }      
    },
    subdata(value) {
      if(value.type === 'category') {
        this.contentdata = value.typedata
        // this.categorytoggle = this.contentdata.disabled 
        this.categorytoggle = value.toggledata
        if(this.oldcategorytoggle !== this.categorytoggle) {
          this.oldval = this.oldcategorytoggle
          this.newval = this.categorytoggle
          this.apptype = 'appcategory'
          this.showModal3 = true
          this.oldcategorytoggle = this.categorytoggle
        }
      } else if(value.type === 'commands') {
        this.oldcommandtoggle = value.typedata.disabled
        this.contentdata[value.id].disabled = value.toggledata
        this.commandtoggle = value.toggledata
        if(this.oldcommandtoggle !== this.commandtoggle) {
          this.showModal3 = true
          this.oldcommandtoggle = this.commandtoggle
        }
      }
      
    },
    submitdatas(value) {
      if(value.type === 'appcategory') {
      console.log('toggle', this.contentdata.disabled, value.oldval)
      //  this.contentdata.disabled = value.oldval
      //   // data.disabled = !this.toggled
      //   if(this.contentdata.disabled) {
      //       for(const formdata in this.contentdata) {
      //           if(typeof this.contentdata[formdata] !== 'boolean' && typeof this.contentdata[formdata] !== 'string') {
      //               this.contentdata[formdata].disabled = true
      //           } 
      //       }
      //   } else {
      //       for(const formdata in this.contentdata) {
      //           if(typeof this.contentdata[formdata] !== 'boolean' && typeof this.contentdata[formdata] !== 'string') {
      //               this.contentdata[formdata].disabled = false
      //           } 
      //       }
      //   }
      // console.log('toggle', this.contentdata)
        // this.newval = value.oldval
        // this.oldval = value.oldval
      }
    }
  },
}
</script>
