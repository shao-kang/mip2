<template>
  <mip-im-item-text
    :align="align"
    :bg-color="bgColor"
    :link-color="linkColor"
    :text-color="textColor"
    :avatar="avatar"
    :text="text"
    :avatar-link="avatarLink"
    :triangle="false"
    :bd-color="bdColor"
    type="inflexible">
    <div
      v-if="title"
      slot="top"
      :style="{color: titleColor, 'border-bottom': 'solid 1px ' + bdColor}"
      class="mip-im-item-title"
    >
      {{ title }}
    </div>
    <div
      v-if="btns && btns.length > 0"
      slot="bottom"
      :style="{'border-top': '1px solid ' + bdColor}"
      class="mip-im-item-text-btns"
    >
      <a
        v-for="(btn, index) in btns"
        :key="btn.text"
        :href="btn.url"
        :data-type="btn.type"
        :data-title="btn.title"
        :style="{color: linkColor, 'border-left': index >0 ? '1px solid ' + bdColor : 'none'}"
        class="mip-im-item-text-btn">
        {{ btn.text }}
      </a>
    </div>
  </mip-im-item-text>
</template>

<style scoped>
.mip-im-item-title {
  padding: .1rem .15rem
}
.mip-im-item-text-btns {
  display: flex;
}
.mip-im-item-text-btn {
  width: 0;
  display: block;
  text-align: center;
  padding-top: .1rem;
  padding-bottom: .1rem;
  font-size: .16rem;
  line-height: 100%;
  flex: auto;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
</style>

<script>
import ImItemText from './mip-im-item-text'
export default {
  components: {
    'mip-im-item-text': ImItemText
  },
  props: {
    align: {
      type: String,
      default: 'left'
    },
    bgColor: {
      type: String,
      default: '#fff'
    },
    linkColor: {
      type: String,
      default: '#06356b'
    },
    textColor: {
      type: String,
      default: '#333'
    },

    titleColor: {
      type: String,
      default: '#999'
    },
    title: {
      type: String,
      default: ''
    },
    text: {
      type: [String, Array],
      default: ''
    },
    avatar: {
      type: String,
      default: ''
    },
    avatarLink: {
      type: Object,
      default () {
        return {}
      }
    },
    bdColor: {
      type: String,
      default: '#ebebeb'
    },
    // btns 中一个对象的格式为{url：'aaa', title: 'nihao', type: 'mip', text: 'btn1'}
    //  url 为将要跳转的链接 title 为跳转页面的title type为url类型取值为mip 或空, text: 为按钮中文字
    btns: {
      type: Array,
      default () {
        return []
      }
    }

  }
}
</script>
