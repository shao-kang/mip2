<template>
  <svg
    :class="'mip-im-audio-animation-step' + stepIndex"
    version="1.1"
    class="mip-im-audio-svg"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    x="0px"
    y="0px"
    viewBox="0 0 120 120"
    style="enable-background:new 0 0 120 120;"
    xml:space="preserve">
    <path
      class="mip-im-audio-path0"
      d="M65,86L65,86C79,71.9,79,49.1,65,35l0,0l4.2-4.2c16.4,16.4,16.4,43,0,59.4l0,0L65,86L65,86z" />
    <path
      class="mip-im-audio-path1"
      d="M52.2,73.2L52.2,73.2c7-7,7-18.4,0-25.5l0,0l4.2-4.2l0,0c9.4,9.4,9.4,24.6,0,33.9l0,0L52.2,73.2L52.2,73.2z" />
    <path
      class="mip-im-audio-path2"
      d="M38.5,60.5l4.9-4.9l0,0l0,0c2.7,2.7,2.7,7.2,0,9.9l0,0L38.5,60.5z" />
  </svg>
</template>

<script>
export default {
  props: {
    state: {
      type: String,
      default: 'pause'
    },
    // millisecond
    lifeTime: {
      type: Number,
      default: 600
    },
    // millisecond
    duration: {
      type: Number,
      default: 6000
    }
  },
  data: function () {
    return {
      oState: this.state,
      steps: 3,
      stepIndex: 0,
      oDuration: 0
    }
  },
  watch: {
    state (value, old) {
      this.oState = value
      if (value === 'playing') {
        this.oDuration = 0
        this.playing()
      }
    }
  },
  mounted () {
    if (this.state === 'playing') {
      this.playing()
    };
  },
  methods: {
    playing () {
      let me = this
      clearInterval(me.timerId)
      this.timerId = setInterval(() => {
        me.stepIndex++
        if (me.stepIndex >= this.steps) {
          if (me.oState === 'pause' || me.oDuration > me.duration) {
            me.oDuration = 0
            me.oState = 'pause'
            me.$emit('ended')
            me.stepIndex = 0
            clearInterval(me.timerId)
            return
          }
          me.stepIndex -= me.steps
          me.oDuration += me.lifeTime
        }
      }, me.lifeTime / me.steps)
    }
  }
}
</script>

<style lang="less" scoped>
.mip-im-audio-animation-step0 {
  .mip-im-audio-path0 {
    opacity: 1;
  }
  .mip-im-audio-path1 {
    opacity: 1;
  }
  .mip-im-audio-path2 {
    opacity: 1;
  }
}
.mip-im-audio-animation-step1 {
  .mip-im-audio-path0 {
    opacity: 0;
  }
  .mip-im-audio-path1 {
    opacity: 0;
  }
  .mip-im-audio-path2 {
    opacity: 1;
  }
}
.mip-im-audio-animation-step2 {
  .mip-im-audio-path0 {
    opacity: 0;
  }
  .mip-im-audio-path1 {
    opacity: 1;
  }
  .mip-im-audio-path2 {
    opacity: 1;
  }
}
</style>
