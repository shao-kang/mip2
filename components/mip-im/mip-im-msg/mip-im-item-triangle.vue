<template>
  <div
    :style="{ 'border-top': '1px solid ' + bdColor,
              'border-left': '1px solid ' + bdColor,
              'background-color': bgColor}"
    :class="align==='left'?'mip-im-triangle-left':'mip-im-triangle-right'"/>
</template>

<style scoped>
.mip-im-triangle-left {
  position: absolute;
  left: -.04rem;
  top: .15rem;
  width: .06rem;
  height: .06rem;
  transform: rotate(-45deg);
}
.mip-im-triangle-right {
  position: absolute;
  right: -.04rem;
  top: .15rem;
  width: .06rem;
  height: .06rem;
  transform: rotate(135deg);
}
</style>

<script>
export default {
  props: {
    align: {
      type: String,
      required: true
    },
    bdColor: {
      type: String,
      required: true
    },
    bgColor: {
      type: String,
      required: true
    }
  }
}
</script>
