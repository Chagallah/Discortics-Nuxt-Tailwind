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
    @click="$emit('close-modal3')"
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
      <div class="text-2xl mb-5 text-left">Hold up! You have unsaved changes!</div>
      <div class="top-8 rounded items-center flex flex-row justify-end">
        <button
          class="
            rounded
            w-question_btn_width
            h-10
            mt-6
            text-xs text-white
            items-center
            flex
            justify-center
            font-medium
            text-center
            mr-2
          "
          @click="onReset"
        >
          Reset
        </button>
        <button
          class="
            rounded
            w-question_btn_width
            h-10
            bg-question_reset
            mt-6
            text-xs
            text-question_resettext
            items-center
            flex
            font-medium
            justify-center
          "
          @click="onAccept1"
        >
          Save
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    ident: {
      type: String,
      default: null,
    },
    oldval: {
    //   type: String,
      default: null,
    },
    newval: {
    //   type: String,
      default: null,
    },
    type: {
      type: String,
      default: null,
    },
  },
  data() {
    return {
      acceptconfirm:'',
    }
  },
  methods: {
    async onAccept1() {
      const localdata = localStorage.getItem('localappdata')
      const initdata = JSON.parse(localdata)
      if(this.type === 'appname') {
        initdata[this.ident].name = this.newval
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'appdes') {
        initdata[this.ident].description = this.newval
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'applogging') {
        initdata[this.ident].auto = this.newval.id
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'appemoji') {
        initdata[this.ident].icon = this.newval
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'appaccepted1') {        
        const data = { action: this.newval, role: '' }
        initdata[this.ident].accepted = data
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'appaccepted2') {        
        initdata[this.ident].accepted.role = this.newval.id
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'appdenied1') {     
        const data = { action: this.newval, role: '' }
        initdata[this.ident].denied = data
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'appdenied2') {    
        initdata[this.ident].denied.role = this.newval.id
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'appquestion') {  
        initdata[this.ident].questions = this.newval
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'delquestion') {        
        initdata[this.ident].questions = this.newval
        const senddata = { data: initdata }
        await this.sendData(senddata);
      } else if(this.type === 'appcategory') {        
        await console.log('category')
      }
      this.$emit('submitdata', {
        oldval: this.newval,
        type: this.type,
      })
      this.$emit('close-modal3')
    },
    onReset() {
      this.$emit('submitdata', {
        oldval: this.oldval,
        type: this.type,
      })
      this.$emit('close-modal3')
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
    }
  },
}
</script>

<style scoped>
.modal {
  height: fit-content;
  backdrop-filter: blur(48px);
}
</style>