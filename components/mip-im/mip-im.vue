<template>
  <div
    :style="{'background': background}"
    class="container"
  >
    <mip-im-list :im-list="list"/>
    <mip-im-input
      @send="send"
      @input-extra-event="send"
    />
  </div>

</template>

<style scoped>
.container {
  padding-bottom: .2rem;
}
</style>

<script>
import MipImList from './mip-im-msg/mip-im-list.vue'
import MipImInput from './mip-im-input/mip-im-input.vue'
import mock from './mock.js'
export default {
  components: {
    'mip-im-list': MipImList,
    'mip-im-input': MipImInput
  },
  props: {
    list: {
      type: Array,
      default: mock.list
    },
    customComponents: {
      type: Object,
      default: function () {
        return {}
      }
    }
  },
  created: function () {
    this.$options.components = Object.assign({}, this.$props.customComponents, this.$options.components)
    // this.$options.components = this.$props.customComponents
  },
  mounted () {
    console.log(mock)
    // Create WebSocket connection.
    // const socket = new WebSocket('ws://localhost:8080');

    // // Connection opened
    // socket.addEventListener('open', function (event) {
    //   console.log('hello');
    //   socket.send({a: 1});
    // });

    // // Listen for messages
    // socket.addEventListener('message', function (event) {
    //   console.log('Message from server', event.data);
    // });
    // console.log('This is my first custom component !')
  },
  methods: {
    send (info) {
      console.log(info)
    }
  }
}
</script>
