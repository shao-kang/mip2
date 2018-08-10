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
        v-for="(btn, index) in links"
        :key="btn.text"
        :href="btn.url"
        :data-type="btn.type"
        :data-title="btn.title"
        :style="{color: linkColor, 'border-bottom': index < links.length - 1 ? '1px solid ' + bdColor : 'none'}"
        class="mip-im-item-link">
        {{ btn.text }}
      </a>
      <div
        v-if="btns && btns.length > 0"
        slot="bottom"
        :style="{'border-top': '1px solid ' + bdColor}"
        class="mip-im-item-text-btns"
      >
        <a
          v-for="(btn, index) in btns"
          :key="btn.text"
          :href="btn.url"
          :data-type="btn.type"
          :data-title="btn.title"
          :style="{color: linkColor, 'border-left': index >0 ? '1px solid ' + bdColor : 'none'}"
          class="mip-im-item-text-btn">
          {{ btn.text }}
        </a>
      </div>
    </div>
  </mip-im-item>
</template>

<style scoped>
.mip-im-item-title {
  padding: .1rem .15rem
}
.mip-im-item-link {
  display: block;
  padding: .1rem .15rem;
  -webkit-box-flex: 1;
  -ms-flex: auto;
  flex: auto;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.mip-im-item-text-btns {
  display: flex;
}
.mip-im-item-text-btn {
  width: 0;
  display: block;
  text-align: center;
  padding: .1rem;
  font-size: .16rem;
  line-height: 100%;
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
      default: '#fff'
    },
    bdColor: {
      type: String,
      default: '#ebebeb'
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
      default: '#06356b'
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
    },
    links: {
      type: Array,
      default () {
        return []
      }
    }

  }
}
</script>
