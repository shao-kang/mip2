<template>
  <mip-im-item-text
    :align="align"
    :bg-color="bgColor"
    :link-color="linkColor"
    :avatar="avatar"
    :text="text"
    :avatar-link="avatarLink"
    :triangle="false"
    :bd-color="bdColor"
    type="default">
    <div
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
.mip-im-item-text-btns {
  display: flex;
}
.mip-im-item-text-btn {
  width: 0;
  display: block;
  text-align: center;
  padding-top: .1rem;
  padding-bottom: .1rem;
  line-height: 100%;
  flex: auto;
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
      default: '#f0f'
    },
    linkColor: {
      type: String
    },
    text: {
      type: [String, Array],
      default: ''
    },
    avatar: {
      type: String
    },
    avatarLink: {
      type: Object
    },
    bdColor: {
      type: String,
      default: '#000'
    },
    // btns 中一个对象的格式为{url：'aaa', title: 'nihao', type: 'mip'}
    //  url 为将要跳转的链接 title 为跳转页面的title type为url类型取值为mip 或空
    btns: {
      type: Array,
      default: []
    }

  }
}
</script>
