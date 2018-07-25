<template>
  <mip-im-item
    :align="align"
    :bg-color="bgColor"
    :avatar="avatar"
    :avatar-link="avatarLink"
    :bd-color="bdColor"
    type="flexible">
    <div class="mip-im-item-audio">
      <mip-im-audio
        :duration="totalTime *1000"
        :class="'mip-im-item-audio-icon-' + align"
        :state="state"
        @ended="()=>{state='pause'}"
        @click.native="clickAudio"
      />
      <div :class="'mip-im-item-audio-total-time-' + align">{{ totalTime }}''</div>
      <audio
        ref="audio"
        @ended="()=>{state='pause'}">
        <source
          v-for="source in sourceList"
          :key="source.src"
          v-bind="source" >
      </audio>
    </div>
  </mip-im-item>

</template>
<script>
import ImAudio from './mip-im-audio-animation.vue'
import ImItem from './mip-im-item'
export default {
  components: {
    'mip-im-audio': ImAudio,
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
      default: null
    },
    avatarLink: {
      type: Object,
      default () { return {} }
    },
    totalTime: {
      type: Number,
      default: 0
    },
    audioSource: {
      type: Array,
      default: function () {
        return []
      }
    }
  },
  data: function () {
    return {
      state: 'pause'
    }
  },
  computed: {
    sourceList: function () {
      if (typeof (this.audioSource) === 'object') {
        return this.audioSource
      }
      return [{src: this.audioSource}]
    }
  },
  watch: {
    state: function (value) {
      if (this.state === 'playing') {
        this.$refs.audio.play()
      } else {
        this.$refs.audio.pause()
        this.$refs.audio.currentTime = 0
      }
    }
  },
  methods: {
    clickAudio: function () {
      if (this.state === 'pause') {
        this.state = 'playing'
        this.$refs.audio.play()
      } else {
        this.state = 'pause'
        this.$refs.audio.pause()
        this.$refs.audio.currentTime = 0
      }
    }
  }
}
</script>
<style lang="less" scoped>
  .mip-im-item-audio {
    width: .38rem;
    height: .38rem;
  }
  .mip-im-item-audio-icon-right {
    transform: rotate(180deg);
  }
  .mip-im-item-audio-total-time {
    position: absolute;
    top: 0;
    top: 50%;
    height: 100%;
    line-height: .38rem;
    margin-top: -50%;
  }
  .mip-im-item-audio-total-time-right {
    .mip-im-item-audio-total-time;
    right: 120%;
  }
  .mip-im-item-audio-total-time-left {
    .mip-im-item-audio-total-time;
    left: 120%;
  }

</style>
