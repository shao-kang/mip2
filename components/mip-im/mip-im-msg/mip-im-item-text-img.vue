<template>
  <mip-im-item-text
    :align="align"
    :bg-color="bgColor"
    :triangle="false"
    :avatar="avatar"
    :avatar-link="avatarLink"
    :text="text"
    :text-color="textColor"
    :link-color="linkColor"
    :bd-color="bdColor"
    type="inflexible">
    <mip-img
      :slot="position"
      :src="img"
      :layout="height > 0 && width > 0 ? 'responsive' : 'container'"
      :height="height"
      :width="width"
      class="mip-im-item-text-img"
      popup />
  </mip-im-item-text>
</template>

<style scoped>
.mip-im-item-text-img {
  display: block;
  width: auto;
  border-radius: 0 0 5px 5px;
  overflow: hidden;
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
    textColor: {
      type: String,
      default: '#333'
    },
    bdColor: {
      type: String,
      default: '#ebebeb'
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
    linkColor: {
      type: String,
      default: ''
    },
    text: {
      type: [String, Array],
      default: ''
    },
    // 两种类型 bottom top
    position: {
      type: String,
      default: 'bottom'
    },
    img: {
      type: String,
      default: ''
    },
    height: {
      type: Number,
      default: 0
    },
    width: {
      type: Number,
      default: 0
    }
  }
}
</script>
