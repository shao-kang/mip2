<template>
  <div>
    <template v-for="(im, index) in imList">
      <mip-im-item-system
        v-if="im.timestamp > 0 && ( index === 0 || im.timestamp -imList[index-1].timestamp > config.systemTime.timeInterval)"
        :key="im.messageId"
        :bg-color="config&&config.systemTime&&config.systemTime.bgColor"
        :text-color="config&&config.systemTime&&config.systemTime.textColor"
        :text="formatDate(im.timestamp, 'yyyy年MM月dd日 hh:mm')"/>
      <component
        ref="imItem"
        :is="im.type"
        :key="im.messageId"
        v-bind="im.content"
        :align="im.align"
        :avatar="im.avatar"
        :bg-color="config[im.align]&&config[im.align].bgColor"
        :bd-color="config[im.align]&&config[im.align].bdColor"
        :link-color="config[im.align]&&config[im.align].linkColor"
        :avatar-link="im.avatarLink"/>
    </template>
  </div>
</template>
<script>
import ImItemSystem from './mip-im-item-system.vue'
import ImItemText from './mip-im-item-text.vue'
import ImItemTextImg from './mip-im-item-text-img.vue'
import ImItemTextBtns from './mip-im-item-text-btns.vue'
import ImItemImg from './mip-im-item-img.vue'
import ImItemAudio from './mip-im-item-audio.vue'
import ImItemListBtns from './mip-im-item-list-btns'
export default {
  components: {
    'mip-im-item-img': ImItemImg,
    'mip-im-item-list-btns': ImItemListBtns,
    'mip-im-item-text': ImItemText,
    'mip-im-item-text-img': ImItemTextImg,
    'mip-im-item-audio': ImItemAudio,
    'mip-im-item-system': ImItemSystem,
    'mip-im-item-text-btns': ImItemTextBtns
  },
  props: {
    imList: {
      type: Array,
      default () {
        return []
      }
    },
    config: {
      type: Object,
      default () {
        return {
          left: {
            bgColor: '#ff0',
            bdColor: '#f0f',
            textColor: '',
            linkColor: '#0ff'
          },
          right: {
            bgColor: '#fff',
            bdColor: '#f0f',
            textColor: '',
            linkColor: '#00f'
          },
          middle: {
            bgColor: 'transparent',
            textColor: '#555'
          },
          systemTime: {
            bgColor: '#d9d9d9',
            textColor: '#fff',
            timeInterval: 300
          }
        }
      }
    },
    customMsgs: {
      type: Object,
      default () {
        return {}
      }
    }
  },
  created: function () {
    this.$options.components = Object.assign({}, this.$props.customMsgs, this.$options.components)
  },
  methods: {
    moveToLast () {
      this.$nextTick(() => {
        if (this.$refs.imItem && this.$refs.imItem.length > 0) {
          let last = this.$refs.imItem.length - 1
          this.$refs.imItem[last].$el && this.$refs.imItem[last].$el.scrollIntoView()
        }
      })
    },
    formatDate (seconds, fmt) {
      let date = new Date(seconds * 1000)
      let o = {
        'M+': date.getMonth() + 1,
        'd+': date.getDate(),
        'h+': date.getHours(),
        'm+': date.getMinutes(),
        's+': date.getSeconds(),
        'q+': Math.floor((date.getMonth() + 3) / 3),
        'S': date.getMilliseconds()
      }
      if (/(y+)/.test(fmt)) {
        fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
      }
      for (let k in o) {
        if (new RegExp('(' + k + ')').test(fmt)) {
          fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1)
            ? (o[k]) : (('00' + o[k]).substr(('' + o[k]).length)))
        }
      }
      return fmt
    }
  }
}
</script>
