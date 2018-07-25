<template>
  <a
    :data-type="link.type"
    :href="link.url"
    :data-title="link.title"
    class="mip-im-extra-link">
    <mip-im-extra-base
      :img="img"
      :text="text" />
  </a>
</template>
<style scoped>
.mip-im-extra-link {
  display: block;
}
.mip-im-extra-link-img {
  width: .54rem;
  height: .54rem;
  margin: 0 auto;
  border-radius: 5px;
  border: 1px solid #eee;
  background-color: #fff;
}
.mip-im-extra-link-text {
  margin-top: .1rem;
  font-size: .12rem;
  line-height: 100%;
  color: #555;
}

</style>
<script>
import MipImInputExtraBase from './mip-im-extra-base'
export default {
  components: {
    'mip-im-extra-base': MipImInputExtraBase
  },
  props: {
    link: {
      type: Object,
      default: function () {
        return {
          type: '',
          url: null,
          tilte: ''
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
