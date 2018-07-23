<template>
  <mip-im-item
    :type="type"
    :align="align"
    :avatar="avatar"
    :avatar-link="avatarLink"
    :bg-color="bgColor"
    :bd-color="bdColor">
    <slot name="top" />
    <pre class="im-content-text"><component
      v-for="item in textList"
      :key ="item.text"
      :is="item.url ? 'a': 'span'"
      :href="item.url"
      :data-type="item.type"
      :data-title="item.title"
      :style="{'color': item.url ? linkColor : textColor}"
    >{{ item.text }}</component></pre>
    <slot name="bottom" />
  </mip-im-item>
</template>

<style scoped>
.im-content-text {
  padding: .06rem .12rem;
  font-size: .16rem;
  line-height: .26rem;
  display: block;
  white-space: pre-wrap;
  word-break: break-all;
  word-wrap: break-word;
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
    textColor: {
      type: String,
      default: '#000'
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
    linkColor: {
      type: String,
      default: '#0f0'
    },
    type: {
      type: String,
      default: 'flexible'
    },
    text: {
      type: [String, Array],
      default: ''
    }
  },
  computed: {
    textList: function () {
      if (typeof (this.text) === 'object') {
        return this.text
      }
      return [{text: this.text}]
    }
  }
}
</script>
