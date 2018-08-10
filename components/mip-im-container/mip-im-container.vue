<template>
  <div>
    <mip-im
      :im-list="imList"
      :im-config="imConfig"
      :input-config="inputConfig"
      @sendText="sendText"
      @inputExtra="inputExtra"/>
    <mip-toast
      ref="toast"
      :info-text="toast.content"
      station="center"
    />
  </div>

</template>
<style scoped>
.mip-im-toast {
  top: 50%;
  text-align: center;
}
.mip-im-toast-content {
  background: rgba(0, 0, 0, 0.8);
  display: inline-block;
  border-radius: 4px;
  color: #fff;
  text-align: center;
  white-space: nowrap;
  padding: 0 .17rem;
  line-height: .44rem;
  font-size: 16px;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  max-width: 80%;

}
</style>

<script>
import MipIm from './../mip-im/mip-im'
import MipToast from './../mip-toast/mip-toast'
export default {
  components: {
    'mip-im': MipIm,
    'mip-toast': MipToast
  },
  props: {
    socketUrl: {
      type: String,
      default: ''
    },
    customMsgType: {
      type: Object,
      default () {
        return {}
      }
    },
    customInputExtraType: {
      type: Object,
      default () {
        return {}
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
            textColor: '#999'
          },
          systemTime: {
            bgColor: '#d9d9d9',
            textColor: '#fff',
            timeInterval: 300
          },
          mainBgColor: '#f1f1f1'
        }
      }
    }
  },
  data () {
    return {
      imList: [],
      inputConfig: {
        placeholder: '请输入回复内容'
      },
      role: {},
      isInit: false,
      socektState: true,
      toast: {
        state: false,
        content: ''
      },
      catchMsgs: {}
    }
  },
  mounted () {
    this.socketInit()
    this.$on('sendImg', (event, img) => {
      this.sendImg(img)
    })
  },
  methods: {
    socketInit () {
      // Create WebSocket connection.
      let socket = new WebSocket(this.socketUrl)
      this.socket = socket
      // // // Connection opened
      socket.addEventListener('open', (event) => {
        let json = {method: 'initView'}
        socket.send(JSON.stringify(json))
        this.isInit = true
      })
      this.socketAddEvent()
    },
    socketAddEvent () {
      // // // Listen for messages
      let socket = this.socket
      socket.addEventListener('error', (e) => {
        // handle error event
      })
      socket.addEventListener('close', (e) => {
        // handle error event
        // code 状态码 查看 https://developer.mozilla.org/zh-CN/docs/Web/API/CloseEvent
        // 断线后每隔一秒 尝试重连一次
        setTimeout(() => {
          if (this.isInit) {
            this.reconnect()
          } else {
            this.socketInit()
          }
        }, 1000)
      })
      socket.addEventListener('message', (info) => {
        let data = JSON.parse(info.data)
        console.log(data)
        let event = new CustomEvent(data.method, {detail: data})
        socket.dispatchEvent(event)
      })
      socket.addEventListener('ping', (data) => {
        let msg = {method: 'pong'}
        socket.send(JSON.stringify(msg))
      })
      socket.addEventListener('onInitView', (e) => {
        let data = e.detail.data
        this.role = data.role
        data.headTip && data.headTip.map((item, index) => {
          this.imList.push({type: 'mip-im-item-system', align: 'middle', content: {text: item}, timestamp: 0})
        })
        data.msgList && data.msgList.map((item) => {
          this.imList.push(item)
        })
        if (data.inputConfig !== undefined) {
          this.inputConfig = data.inputConfig
        }
        if (data.countdown) {
          this.$emit('countdown', data.countdown)
        }
      })
      socket.addEventListener('onSendMsg', (e) => {
        if (this.catchMsgs[e.detail.uniqueId] !== undefined) {
          e.detail.info.data.msgList.map((item) => {
            this.imList.push(item)
            this.catchMsgs[e.detail.uniqueId] = null
          })
        }
      })
      socket.addEventListener('onRecvMsg', (e) => {
        let data = e.detail.data
        let length = this.imList.length
        data.msgList.map((item) => {
          // 去重
          let copy = false
          for (let i = 0; i < length; i++) {
            if (this.imList[i].messageId === item.messageId) {
              copy = true
            }
          }
          if (!copy) {
            this.imList.push(item)
          }
        })
        if (data.inputConfig !== undefined) {
          this.inputConfig = Object.assign({}, this.inputConfig, data.inputConfig)
        }
      })
      socket.addEventListener('onLastMsg', (e) => {
        let length = this.imList.length
        let data = e.detail.data
        data.msgList && data.msgList.map((item) => {
          // 去重
          let copy = false
          for (let i = 0; i < length; i++) {
            if (this.imList[i].messageId === item.messageId) {
              copy = true
            }
          }
          if (!copy) {
            this.imList.push(item)
          }
        })
        if (data.inputConfig !== undefined) {
          this.inputConfig = Object.assign({}, this.inputConfig, data.inputConfig)
        }
      })
    },
    reconnect () {
      if (this.socket.readyState < 2) {
        // socket 未断开
        return
      }
      let socket = new WebSocket(this.socketUrl)
      this.socket = socket
      let lastMessage = this.imList[this.imList.length - 1]
      // // // Connection opened
      socket.addEventListener('open', (event) => {
        let msg = {method: 'lastMsg', lastMessageId: lastMessage.messageId}
        socket.send(JSON.stringify(msg))
      })
      this.socketAddEvent()
    },
    sendText (info) {
      let uniqueId = this.getUniqueId(this.questionId)
      let date = Math.ceil(new Date().valueOf() / 1000)
      let msg = {
        method: 'sendMsg',
        uniqueId: uniqueId,
        type: 'mip-im-item-text',
        content: {txt: info.text}
      }
      this.catchMsgs[uniqueId] = {
        // avatar: this.role.user.avatar,
        timestamp: date,
        align: 'right',
        type: 'mip-im-item-text',
        content: {text: info.text}
      }
      if (this.socket.readyState === 1) {
        this.socket.send(JSON.stringify(msg))
        this.msgCheckError(3, uniqueId)
      } else {
        this.error({type: 'socketError'})
      }
    },
    sendImg (info) {
      let uniqueId = this.getUniqueId(this.questionId)
      let date = Math.ceil(new Date().valueOf() / 1000)
      let msg = {
        method: 'sendMsg',
        uniqueId: uniqueId,
        type: 'mip-im-item-img',
        content: info
      }
      this.catchMsgs[uniqueId] = {
        // avatar: this.role.user.avatar,
        timestamp: date,
        align: 'right',
        type: 'mip-im-item-img',
        content: {img: info.img, height: info.height, width: info.width}
      }
      if (this.socket.readyState === 1) {
        this.socket.send(JSON.stringify(msg))
        this.msgCheckError(3, uniqueId)
      } else {
        this.error({type: 'socketError'})
      }
    },
    getUniqueId (questionId) {
      return questionId + '_' + new Date().valueOf() + '_' + Math.ceil(Math.random() * 100000)
    },
    //
    msgCheckError (second, uniqueId) {
      setTimeout(() => {
        if (this.catchMsgs[uniqueId]) {
          this.error({info: '消息发送失败', type: 'sendFalied'})
        }
      }, second * 1000)
    },
    error (value) {
      this.$emit('err', value)
      this.openToast(value.info)
    },
    openToast (content) {
      this.toast.state = true
      this.toast.content = content || '消息发送失败'
      this.$refs.toast.$emit('show')
    },
    inputExtra (info) {
      this.$emit('inputExtra', info)
      if (info.event.name === 'uploadSuccess' && info.event.data.status === 0) {
        let imgInfo = info.event.data.data
        let a = {orientation: imgInfo.orientation, 'picID': imgInfo['picID'], height: imgInfo.height, width: imgInfo.width}
        this.sendImg(a)
      }
      if (info.event.name === 'uploadFailed') {
        this.openToast(' 发送图片失败')
      }
    }
  }
}
</script>
