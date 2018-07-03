<template>
  <mip-im-item
    :align="align"
    :bg-color="bgColor"
    :avatar="avatar"
    :avatar-link="avatarLink"
    :triangle="false"
    :bd-color="bdColor">
    <mip-img
      class="im-content-img"
      popup
      src="https://www.mipengine.org/static/img/sample_01.jpg" />
  </mip-im-item>
</template>

<style scoped>
.im-content-img {
  display: block;
  width: auto;
  margin: -1px;
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
      type: String
    },
    avatarLink: {
      type: Object
    },
    img: {
      type: String,
      default: ''
    }

  }
}
</script>
