<template>
  <mip-im
    :im-list="imList"
    :im-config="imConfig"
    :input-config="inputConfig"
    @sendText="sendText"
    @inputExtra="sendText"/>
</template>

<script>
import MipIm from './../mip-im/mip-im'
export default {
  components: {'mip-im': MipIm},
  props: {
    socketUrl: {
      type: String,
      default: ''
    },
    questionId: {
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
        var json = {method: 'initView'}
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

        data.msgList && data.msgList.map((item) => {
          this.imList.push(item)
        })
        if (data.inputConfig !== undefined) {
          this.inputConfig = data.inputConfig
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
        this.msgCheckError(2, uniqueId)
      } else {
        this.$emit('error', {type: 'info'})
      }
    },
    sendImg (info) {
      let uniqueId = this.getUniqueId(this.questionId)
      let date = Math.ceil(new Date().valueOf() / 1000)
      let msg = {
        method: 'sendMsg',
        uniqueId: uniqueId,
        type: 'mip-im-item-img',
        content: {picID: info.img, orientation: 0}
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
        this.msgCheckError(2, uniqueId)
      } else {
        this.$emit('error', {type: 'info'})
      }
    },
    getUniqueId (questionId) {
      return questionId + '_' + new Date().valueOf() + '_' + Math.ceil(Math.random() * 100000)
    },
    //
    msgCheckError (second, uniqueId) {
      setTimeout(() => {
        if (this.catchMsgs[uniqueId]) {
          this.$emit('err', {info: '消息发送失败', type: 'sendFalied'})
        }
      }, second * 1000)
    }
  }
}
</script>
