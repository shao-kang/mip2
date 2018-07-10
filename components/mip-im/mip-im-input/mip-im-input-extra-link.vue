<template>
  <a
    :data-type="link.type"
    :href="link.url"
    :data-title="link.title"
    class="mip-im-input-extra-link">
    <mip-im-input-extra-base
      :img="img"
      :text="text" />
  </a>
</template>
<style scoped>
.mip-im-input-extra-link {
  display: block;
}
.mip-im-input-extra-link-img {
  width: .54rem;
  height: .54rem;
  margin: 0 auto;
  border-radius: 5px;
  border: 1px solid #eee;
  background-color: #fff;
}
.mip-im-input-extra-link-text {
  margin-top: .1rem;
  font-size: .12rem;
  line-height: 100%;
  color: #555;
}

</style>
<script>
import MipImInputExtraBase from './mip-im-input-extra-base'
export default {
  components: {
    'mip-im-input-extra-base': MipImInputExtraBase
  },
  props: {
    link: {
      type: Object,
      default: function () {
        return {
          type: 'mip',
          url: 'http://www.baidu.com',
          tilte: 'baidu'
        }
      }
    },
    img: {
      type: String,
      default: ''
    },
    text: {
      type: String,
      default: '跳转页面'
    },
    index: {
      type: Number,
      default: 0
    }
  }

}
</script>
