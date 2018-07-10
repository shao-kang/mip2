<template>
  <div class="mip-im-input-extra-upload-wrapper">
    <mip-im-input-extra-base
      :img="img"
      :text="text"/>
    <input
      ref="fileEle"
      :accept="accept"
      type="file"
      class="mip-im-input-extra-upload-input"
      @change="handleChange"
    >
  </div>
</template>
<style scoped>
.mip-im-input-extra-upload-wrapper {
  position: relative;
  overflow: hidden;
}
.mip-im-input-extra-upload-img {
  width: .54rem;
  height: .54rem;
  margin: 0 auto;
  border-radius: 5px;
  border: 1px solid #eee;
  background-color: #fff;
}
.mip-im-input-extra-upload-text {
  margin-top: .1rem;
  font-size: .12rem;
  line-height: 100%;
  color: #555;
}
.mip-im-input-extra-upload-input {
  opacity: 0;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 100%;
}
</style>
<script>
import MipImInputExtraBase from './mip-im-input-extra-base'
export default {
  components: {
    'mip-im-input-extra-base': MipImInputExtraBase
  },
  props: {
    img: {
      type: String,
      default: ''
    },
    text: {
      type: String,
      default: '上传图片'
    },
    index: {
      type: Number,
      default: 0
    },
    uploadUrl: {
      type: String,
      default: ''
    },
    filename: {
      type: String,
      default: 'img'
    },
    accept: {
      type: String,
      default: 'image/*'
    },
    limit: {
      type: Number,
      default: 100
    }

  },
  computed: {
    limitSize () {
      return this.limit * 1024 * 1024
    }
  },
  methods: {
    click () {
      this.$emit('extra-event', {index: this.index, event: {name: 'click'}})
    },
    handleChange (e) {
      const file = e.target.files[0]
      if (file && file.size > this.limitSize) {
        this.$emit('extra-event', {index: this.index,
          event: {
            name: 'error',
            type: 'sizeLimited',
            message: `您上传的图片${file.name}大小超出限制`
          }})
        return
      }
      if (file) {
        this.preivew(file)
        this.uploadFile(file)
        this.$nextTick(() => {
          this.$refs.fileEle.value = '' // 清空value值 Android下拍照，选择同一个图片的时候无法触发change事件
        })
      }
    },
    uploadFile (file) {
      this.$emit('extra-event', {index: this.index, event: {name: 'beforeUpload'}})
      const formData = new FormData()
      formData.append(this.filename, file)

      fetch(this.uploadUrl, {
        method: 'POST',
        body: formData
      })
        .then(response => {
          if (response.status >= 200 && response.status < 300) {
            return Promise.resolve(response)
          } else {
            return Promise.reject(new Error(response.statusText))
          }
        })
        .then((res) => {
          return res.json()
        })
        .then((data) => {
          this.$emit('extra-event', {index: this.index, event: {name: 'uploadSuccess', data: data}})
        })
        .catch((err) => {
          this.$emit('extra-event', {index: this.index, event: {name: 'uploadFailed', info: err}})
        })
    },
    preivew (file) {
      const reader = new FileReader()
      reader.readAsDataURL(file)
      reader.onload = event => {
        this.$emit('extra-event', {index: this.index, event: {name: 'preview', content: event.target.result}})
      }
    }
  }
}
</script>
