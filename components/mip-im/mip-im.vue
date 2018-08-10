<template>
  <div
    :style="{'background-color': imConfig.mainBgColor || '#f1f1f1'}"
    class="mip-im"
  >
    <mip-im-list
      ref="imList"
      :im-list="imList"
      :config="imConfig"
      :custom-msgs="customMsgType"
      class="mip-im-list"/>
    <mip-im-input
      :extra-list="inputExtraList"
      :custom-types="customInputExtraType"
      v-bind="inputConfig"
      @send="sendText"
      @input-extra-event="inputExtraEvent"
    />
  </div>

</template>

<style scoped>
.mip-im {
  min-height: 100vh;
}
.mip-im-list {
  padding-bottom: 1rem;
}
</style>

<script>
import MipImList from './mip-im-msg/mip-im-list.vue'
import MipImInput from './mip-im-input/mip-im-input.vue'
export default {
  components: {
    'mip-im-list': MipImList,
    'mip-im-input': MipImInput
  },
  props: {
    imList: {
      type: Array,
      default () {
        return []
      }
    },
    imConfig: {
      type: Object,
      default () {
        return {
          left: {
            titleColor: '#999',
            bgColor: '#fff',
            bdColor: '#ebebeb',
            textColor: '#333',
            linkColor: '#06356b'
          },
          right: {
            titleColor: '#999',
            bgColor: '#1E5798',
            bdColor: '#1E5798',
            textColor: '#fff',
            linkColor: '#0ff'
          },
          middle: {
            bgColor: 'transparent',
            textColor: '#555'
          },
          systemTime: {
            bgColor: '#d9d9d9',
            textColor: '#fff',
            timeInterval: 300
          },
          mainBgColor: '#f1f1f1'
        }
      }
    },
    customMsgType: {
      type: Object,
      default: function () {
        return {}
      }
    },
    customInputExtraType: {
      type: Object,
      default () {
        return {}
      }
    },
    inputConfig: {
      type: Object,
      default () {
        return {}
      }
    }
  },
  watch: {
    imList (value, old) {
      this.$nextTick(() => {
        this.$refs.imList.moveToLast()
      })
    }
  },
  mounted () {
    this.$refs.imList.moveToLast()
  },
  methods: {
    sendText (info) {
      this.$emit('sendText', info)
    },
    inputExtraEvent (info) {
      this.$emit('inputExtra', info)
    }
  }
}
</script>
