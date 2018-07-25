<template>
  <div @click="click">
    <mip-img
      :src="img"
      class="mip-im-extra-base-img"/>
    <div
      v-if="!!text"
      class="mip-im-extra-base-text">{{ text }}</div>
  </div>
</template>
<style scoped>
.mip-im-extra-base-img {
  width: .54rem;
  height: .54rem;
  margin: 0 auto;
  padding: .1rem;
  box-sizing: border-box;
  border-radius: 5px;
  border: 1px solid #eee;
  background-color: #fff;
}
.mip-im-extra-base-text {
  margin-top: .1rem;
  font-size: .12rem;
  line-height: 100%;
  color: #555;
}
</style>

<script>
export default {
  props: {
    img: {
      type: String,
      default: ''
    },
    text: {
      type: String,
      default: '文字说明'
    },
    index: {
      type: Number,
      default: 0
    }
  },
  methods: {
    click: function () {
      this.$emit('extra-event', {index: this.index, event: {name: 'click'}})
    }
  }
}
</script>
