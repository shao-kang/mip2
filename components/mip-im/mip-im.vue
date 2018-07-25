<template>
  <div
    :style="{'background': background}"
    class="container"
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
      @input-extra-event="send"
    />
  </div>

</template>

<style scoped>
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
            bgColor: '#f00',
            bdColor: '#f0f',
            textColor: '#f0f',
            linkColor: '#0ff'
          },
          right: {
            bgColor: '#fff',
            bdColor: '#f0f',
            textColor: '',
            linkColor: '#0ff'
          },
          middle: {
            bgColor: 'transparent',
            textColor: '#555'
          },
          systemTime: {
            bgColor: '#d9d9d9',
            textColor: '#fff',
            timeInterval: 60
          }
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
  data () {
    return {}
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
