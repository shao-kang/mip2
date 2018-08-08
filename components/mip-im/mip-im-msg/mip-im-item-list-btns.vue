<template>
  <mip-im-item
    :align="align"
    :bg-color="bgColor"
    :avatar="avatar"
    :avatar-link="avatarLink"
    :bd-color="bdColor"
    inflexible>
    <div>
      <div
        :style="{color: titleColor, 'border-bottom': 'solid 1px ' + bdColor}"
        a-if="title"
        class="mip-im-item-title"
      >
        {{ title }}
      </div>
      <a
        v-for="(btn, index) in btns"
        :key="btn.text"
        :href="btn.url"
        :data-type="btn.type"
        :data-title="btn.title"
        :style="{color: linkColor, 'border-bottom': index < btns.length - 1 ? '1px solid ' + bdColor : 'none'}"
        class="mip-im-item-btn">
        {{ btn.text }}
      </a>
    </div>
  </mip-im-item>
</template>

<style scoped>
.mip-im-item-title {
  padding: .1rem .15rem
}
.mip-im-item-btn {
  display: block;
  padding: .1rem .15rem;
  -webkit-box-flex: 1;
  -ms-flex: auto;
  flex: auto;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
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
      default: ''
    },
    titleColor: {
      type: String,
      default: '#999'
    },
    title: {
      type: String,
      default: ''
    },
    btns: {
      type: Array,
      default () {
        return []
      }
    }

  }
}
</script>
