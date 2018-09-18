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
            :disabled="disabled"
            @focus="onfocus"
            @blur="onblur"
            @click="onclick"
          />
          <div
            v-if="inputState === 'blur'"
            v-show="inputContent === ''"
            class="mip-im-input-extra-button"
            data-stats-baidu-obj="%7B%22type%22%3A%22click%22%2C%22data%22%3A%5B%22_trackEvent%22%2C%22im-input-extra%22%2C%22toggle%22%5D%7D"
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
          <div
            v-if="inputState === 'blur'"
            v-show="inputContent !== ''"
            class="mip-im-input-extra-button mip-im-input-extra-button-span"
            data-stats-baidu-obj="%7B%22type%22%3A%22click%22%2C%22data%22%3A%5B%22_trackEvent%22%2C%22im-input-extra%22%2C%22send%22%5D%7D"
            @click="send">
            发送
          </div>
        </div>
        <div
          v-show="extraState"
          class="mip-im-input-extra">
          <component
            v-for="(item, index) in extraList"
            :key="item.type"
            :is="item.type"
            :index="index"
            v-bind="item.content"
            data-stats-baidu-obj="%7B%22type%22%3A%22click%22%2C%22data%22%3A%5B%22_trackEvent%22%2C%22im-input-extra%22%2C%22item-click%22%5D%7D"
            class="mip-im-input-extra-item"
            @extra-event="extraEvent"
          />
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
  -webkit-appearance: none;

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
  outline: none;
  resize: none;
  background-color: #fff;
  -webkit-appearance: none;
  overflow: hidden;
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
  outline: none;
  word-wrap: break-word;
  -webkit-appearance: none;
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
  width: 0.32rem;
  height: 0.27rem;
  margin: 0.1rem 0 0.1rem 0.07rem;
}
.mip-im-input-extra-button-span {
  font-size: .16rem;
  line-height: .27rem;
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
  justify-content: flex-start;
  flex-wrap: wrap;

}
.mip-im-input-extra-item {
  padding: 0.15rem 0;
  width: 25%;
  flex: none;
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
import MipImExtraUpload from './mip-im-extra-upload.vue'
import MipImExtraLink from './mip-im-extra-link.vue'
import MipImExtraBase from './mip-im-extra-base.vue'
import MipImExtraPhone from './mip-im-extra-phone'
export default {
  components: {
    'mip-im-extra-upload': MipImExtraUpload,
    'mip-im-extra-link': MipImExtraLink,
    'mip-im-extra-phone': MipImExtraPhone,
    'mip-im-extra-base': MipImExtraBase
  },

  props: {
    placeholder: {
      type: String,
      default: '请输入回复内容'
    },
    disabled: {
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
        return []
      }
    }
  },
  data () {
    return {
      // 输入框下方非文字输入
      extraState: false,
      // 输入框状态
      inputState: 'blur',
      bodyStyle: '',
      inputContent: '',
      isMask: false
    }
  },
  computed: {
    inputColor () {
      return this.disabled ? '#ddd' : '#555'
    }
  },
  watch: {
    disabled (value, old) {
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
  methods: {
    extraClick () {
      if (!this.disabled && this.extraList.length > 0) {
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
    onclick () {
      if (this.inputState !== 'focus') {
        this.inputState = 'focus'
      }
    },
    cancle () {
      this.inputContent = ''
    },
    send () {
      if (this.inputContent) {
        this.$emit('send', { type: 'text', text: this.inputContent })
        this.inputContent = ''
      }
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
