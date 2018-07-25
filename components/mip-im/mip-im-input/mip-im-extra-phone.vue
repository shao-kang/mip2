<template>
  <mip-im-extra-link
    :img="img"
    :text="text"
    :link="{url: 'tel:' + phone}"
  />
</template>
<script>
import MipImExtraLink from './mip-im-extra-link'
export default {
  components: {
    'mip-im-extra-link': MipImExtraLink
  },
  props: {
    phone: {
      type: String,
      default: ''
    },
    img: {
      type: String,
      default: ''
    },
    text: {
      type: String,
      default: '跳转页面'
    }
  }
}
</script>
