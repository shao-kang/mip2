<template>
  <mip-im-item
    :align="align"
    :bg-color="bgColor"
    :avatar="avatar"
    :avatar-link="avatarLink"
    :triangle="false"
    :bd-color="bdColor"
    inflexible>
    <mip-img
      :height="height > 0 && height / width > 0.2 ? height : false"
      :width="width > 0 && height / width > 0.2 ? width : false"
      :layout="height > 0 && width > 0 && height / width > 0.2 ? 'responsive' : 'container'"
      :src="img"
      class="im-content-img"
      popup
    />
  </mip-im-item>
</template>

<style scoped>
.im-content-img {
  display: block;
  width: auto;
  position: relative;
  border-radius: 5px;
  overflow: hidden;
}
</style>

<script>
import ImItem from './mip-im-item'
export default {
  components: {
    'mip-im-item': ImItem
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
    bdColor: {
      type: String,
      default: '#fff'
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
    img: {
      type: String,
      default: ''
    },
    height: {
      type: Number,
      default: 100
    },
    width: {
      type: Number,
      default: 100
    }

  }
}
</script>
