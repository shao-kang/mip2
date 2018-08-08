<template>
  <div
    :class="'im-item-container-' + align"
    :style="{'background': background}"
    class="im-item-container">
    <a
      :href="avatarLink && avatarLink.url"
      :data-type="avatarLink && avatarLink.type"
      :data-title="avatarLink && avatarLink.title"
      class="im-item-avator-link">
      <mip-img
        :src="avatar"
        class="im-item-avator" />
    </a>

    <div
      :class="'im-item-container-' + align"
      class="im-item-content-wrapper"
    >
      <div
        :class="type ==='flexible' ? 'im-item-content-flex' : 'im-item-content'"
        :style="{'background-color': bgColor, 'border': '1px solid ' + bdColor}"
      >
        <mip-im-triangle
          v-if="triangle"
          :align="align"
          :bd-color="bdColor"
          :bg-color="bgColor"
        />
        <slot />

      </div>
      <div
        v-if="type ==='flexible'"
        class="im-item-content-space"/>
    </div>
    <div class="im-item-space" />
  </div>
</template>

<style scoped>
.im-item-container {
  display: flex;
  padding: 0 0.07rem;
  margin-bottom: .15rem;
}
.im-item-avator-link {
  display: block;
  margin-left: .1rem;
  margin-right: .1rem;
  height: .4rem;
  width: .4rem;
}
.im-item-container-left {
  flex-direction: row;
}
.im-item-container-right {
  flex-direction: row-reverse;
}
.im-item-avator {
  flex: 0 0 .4rem;
  height: .4rem;
  width: .4rem;
  border-radius: 50%;
  overflow: hidden;
}
.im-item-content-wrapper {
  flex: auto;
  width: 0;
  display: flex;
  padding-left: .06rem;
  padding-right: .06rem;
}
.im-item-content {
  flex: 1 1 0;
  width:0;
  position: relative;
  border-radius: 5px;
}
.im-item-content-flex {
  flex: auto;
  max-width: 100%;
  position: relative;
  box-sizing: border-box;
  border-radius: 5px;

}
.im-item-content-space {
  flex: 10000 0 0;
}
.im-item-space {
  flex: 0 0 .44rem;
  padding: 0 .1rem;
}

</style>

<script>
import ImTriangle from './mip-im-item-triangle.vue'

export default {
  components: {
    'mip-im-triangle': ImTriangle
  },
  props: {
    type: {
      type: String,
      default: 'inflexible'
    },
    align: {
      type: String,
      default: 'left'
    },
    bgColor: {
      type: String,
      default: '#fff'
    },
    triangle: {
      type: Boolean,
      default: true
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
        return {
          url: '',
          type: '',
          title: ''
        }
      }
    }
  }
}
</script>
