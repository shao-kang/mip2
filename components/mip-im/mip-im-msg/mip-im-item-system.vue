<template>
  <div class="mip-im-item-system">
    <span
      :style="{'background-color':bgColor,'color':textColor}"
      class="mip-im-item-system-text"
    >{{ text }}</span>
  </div>
</template>
<script>
export default {
  props: {
    bgColor: {
      type: String,
      default: 'transparent'
    },
    textColor: {
      type: String,
      default: '#999'
    },
    text: {
      type: String,
      default: ''
    }
  }
}
</script>
<style scoped>
  .mip-im-item-system {
    text-align: center;
    margin-bottom: .15rem;
  }
  .mip-im-item-system-text {
    border-radius: .03rem;
    padding: .05rem .1rem;
    font-size: .12rem;
    line-height: 100%;
    display: inline-block;
  }
</style>
