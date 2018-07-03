<template>
  <div
    :style="{'background': background}"
    class="container"
    >

    <mip-im-list :im-list="list"></mip-im-list>
  </div>

</template>

<style scoped>
.container {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
}
</style>

<script>
import MipImList from './mip-im-msg/mip-im-list.vue'
import mock from './mock.js'
export default {
  props: {
    list: {
      type: Array,
      default: mock.list
    },
    customComponents: {
      type: Object,
      default: function () {return;}
    }
  },
 created: function () {
    this.$options.components = Object.assign({}, this.$props.customComponents, this.$options.components)
    // this.$options.components = this.$props.customComponents
  },
  components: {
    'mip-im-list': MipImList
  },
  mounted () {
    console.log(mock)
    console.log('This is my first custom component !')
  }
}
</script>
