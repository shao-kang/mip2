<template>
  <div @touchmove="onscroll">
    <mip-fixed
      v-if="isMask"
      type="bottom"
      class="mip-im-input-mask"
      @click="maskClick"/>
    <mip-fixed type="bottom">
      <div
        class="mip-im-input">
        <div
          v-if="inputState === 'focus'"
          class="mip-im-input-input-focus-btn" >
          <div
            class="mip-im-input-input-focus-btn-cancle"
            @click="cancle">取消</div>
          <div
            class="mip-im-input-input-focus-btn-send"
            @click="send">发送</div>
        </div>
        <div class="mip-im-input-main">
          <textarea
            :class="inputState === 'blur' ? 'mip-im-input-input-blur': 'mip-im-input-input-focus'"
            v-model="inputContent"
            :placeholder="placeholder"
            @focus="onfocus"
            @blur="onblur"
          />
          <div
            v-if="inputState === 'blur'"
            class="mip-im-input-extra-button"
            @click="extraClick">

            <svg
              t="1531226140598"
              class="mip-im-input-extra-button-svg"
              viewBox="0 0 1024 1024"
              version="1.1"
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
            >
              <path
                :fill="inputColor"
                d="M511.509333 128C299.861333 128 127.658667 300.181333 127.658667 511.850667c0 211.669333 172.202667 383.850667 383.850666 383.850666 211.669333 0 383.850667-172.181333 383.850667-383.850666C895.36 300.181333 723.178667 128 511.509333 128m0 831.701333c-246.954667 0-447.850667-200.917333-447.850666-447.850666C63.658667 264.896 264.533333 64 511.509333 64 758.442667 64 959.36 264.896 959.36 511.850667c0 246.933333-200.917333 447.850667-447.850667 447.850666"
              />
              <path
                :fill="inputColor"
                d="M490.666667 469.333333h-141.717334c-15.978667 0-28.949333 14.314667-28.949333 32 0 17.664 12.970667 32 28.949333 32H490.666667v141.717334c0 15.978667 14.314667 28.949333 32 28.949333s32-12.970667 32-28.949333V533.333333h141.717333c15.978667 0 28.949333-14.336 28.949333-32 0-17.685333-12.970667-32-28.949333-32H554.666667v-141.717333c0-15.978667-14.314667-28.949333-32-28.949333s-32 12.970667-32 28.949333V469.333333z"
              />
            </svg>
          </div>

        </div>
        <div
          v-show="extraState"
          class="mip-im-input-extra">
          <component
            v-for="(item, index) in extraList"
            :key="item.viewType"
            :is="item.type"
            :index="index"
            class="mip-im-input-extra-item"
            @extra-event="extraEvent"
          />
          <div :class="'mip-im-input-extra-space-' + (4 - extraList.length % 4)" />
        </div>
      </div>
    </mip-fixed>
  </div>

</template>
<style scoped>
.mip-im-input-mask {
  height: 200%;
  height: 200vh;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.4);
  -webkit-tap-highlight-color: transparent;
}

.mip-im-input {
  min-height: 0.48rem;
  background-color: #f8f8f8;
  width: 100%;
}
.mip-im-input-main {
  padding-right: 0.17rem;
  padding-left: 0.17rem;
  overflow: hidden;
  display: flex;
}
.mip-im-input-input-blur {
  flex: auto;
  box-sizing: border-box;
  border-radius: 0.03rem;
  height: 0.36rem;
  padding: 0 0.08rem;
  border: 1px solid #eee;
  font-size: 0.14rem;
  line-height: 0.36rem;
  margin-top: 0.06rem;
  margin-bottom: 0.06rem;
  vertical-align: middle;
  word-wrap: normal;
  word-break: normal;
  outline: 0;
  background-color: #fff;
  -webkit-appearance: none;
}
.mip-im-input-input-focus {
  margin-top: 0.15rem;
  margin-bottom: 0.2rem;
  padding: 0.12rem 0.18rem;
  border: 1px solid #e1e1e1;
  font-size: 15px;
  width: 100%;
  height: 0.94rem;
  resize: none;
  word-wrap: break-word;
}
.mip-im-input-input-focus-btn {
  margin-top: 0.1rem;
  display: flex;
  padding: 0.1rem 0.17rem 0;
  justify-content: space-between;
  color: #555;
  line-height: 100%;
}
.mip-im-input-input-focus-btn-cancle {
  width: 40%;
  padding: 0.1rem;
}
.mip-im-input-input-focus-btn-send {
  width: 40%;
  padding: 0.1rem;
  text-align: right;
}
.mip-im-input-extra-button {
  width: 0.27rem;
  height: 0.27rem;
  margin: 0.1rem 0 0.1rem 0.07rem;
}
.mip-im-input-extra-button-svg {
  width: 100%;
  height: 100%
}
.mip-im-input-extra {
  display: flex;
  text-align: center;
  color: #555;
  padding: 0 0.17rem;
  border-top: 1px solid #ebebeb;
  justify-content: space-between;
}
.mip-im-input-extra-item {
  padding: 0.15rem 0;
  width: 0;
  flex: 1 1 auto;
}
.mip-im-input-extra-space-1 {
  flex: 1 1 auto;
  width: 0;
}
.mip-im-input-extra-space-2 {
  flex: 2 2 auto;
  width: 0;
}
.mip-im-input-extra-space-3 {
  flex: 1 1 auto;
  width: 0;
}
.mip-im-input-real-textarea {
  margin-top: 0.15rem;
  padding: 0.12rem 0.18rem;
  border: 1px solid #e1e1e1;
  font-size: 15px;
  overflow: hidden;
  width: 100%;
  height: 0.94rem;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  resize: none;
  word-wrap: break-word;
}
</style>
<script>
import MipImInputExtraUpload from './mip-im-input-extra-upload.vue'
import MipImInputExtraLink from './mip-im-input-extra-link.vue'
import mipImInputExtraBase from './mip-im-input-extra-base.vue'
export default {
  components: {
    'mip-input-upload': MipImInputExtraUpload,
    'mip-input-link': MipImInputExtraLink,
    'mip-input-base': mipImInputExtraBase
  },

  props: {
    placeholder: {
      type: String,
      default: 'ddd'
    },
    disable: {
      type: Boolean,
      default: false
    },
    customTypes: {
      type: Object,
      default () {
        return {}
      }
    },
    value: {
      type: String,
      default: ''
    },
    extraList: {
      type: Array,
      default () {
        return [
          { type: 'mip-input-upload', content: {} },
          { type: 'mip-input-link', content: {} },
          { type: 'mip-input-base', content: {} }
        ]
      }
    }
  },
  data () {
    return {
      // 输入框下方非文字输入
      extraState: true,
      // 输入框状态
      inputState: 'blur',
      bodyStyle: '',
      inputContent: '',
      isMask: true
    }
  },
  computed: {
    inputColor () {
      return this.disable ? '#ddd' : '#555'
    }
  },
  watch: {
    disable (value, old) {
      if (value === true) {
        this.extraState = false
        this.inputContent = ''
      }
    },
    value (value, old) {
      this.inputContent = value
    }
  },
  created () {
    this.$options.components = Object.assign(
      {},
      this.$props.customTypes,
      this.$options.components
    )
  },
  mounted () {
    // navigator.getUserMedia()
  },
  methods: {
    extraClick () {
      if (!this.disable) {
        this.extraState = !this.extraState
        this.isMask = this.extraState
      }
    },
    extraEvent (info) {
      this.$emit('input-extra-event', info)
    },
    maskClick () {
      this.isMask = false
      this.extraState = false
    },
    onfocus () {
      this.inputState = 'focus'
      this.extraState = false
      this.isMask = true
    },
    cancle () {
      this.inputContent = ''
    },
    send () {
      this.$emit('send', { type: 'text', text: this.inputContent })
    },
    onblur () {
      this.inputState = 'blur'
      this.isMask = false
    },
    onscroll (e) {
      if (this.inputState === 'focus') {
        e.preventDefault()
      }
    }
  }
}
</script>
