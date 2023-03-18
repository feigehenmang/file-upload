<template>
  <div class="container">
    <h1>文件上传</h1>
    <div ref="drag" id="drag">
      <input type="file" name="file" @change="handleFileChange" />
      <img :src="preview" alt="">
    </div>
    <div>
      <el-button type="primary" @click="handleUpload">上 传</el-button>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      preview: ''
    }
  },
  mounted() {
    this.bindDragEvent('drag',()=>{
      this.preview = window.URL.createObjectURL(this.file)

    })
  },
  methods: {
    bindDragEvent(name,cb) {
      const drag = this.$refs[name]

      drag.addEventListener("dragover", e => {
        drag.style.borderColor = "red"
        e.preventDefault()
      })
      drag.addEventListener("dragleave", e => {
        drag.style.borderColor = "#eee"
        e.preventDefault()
      })
      drag.addEventListener("drop", e => {
          const fileList = e.dataTransfer.files
          drag.style.borderColor = "#eee"
          this.file = fileList[0]; // 先只考虑单文件
          cb && cb()
          e.preventDefault()
        },
        false)
    },
    handleFileChange(e) {
      const [file] = e.target.files;
      if (!file) return;

      // if(file.size>CHUNK_SIZE){
      //   this.$message.error("请选择小于2M的文件");
      //   return;
      // }
      // if(!this.isImage(file)){
      //   this.$message.error("请选择正确的图片格式");
      //   return
      // }

      this.file = file;

    },
    handleUpload() {

    }
  }
}
</script>

<style lang="stylus">
.app>div
  margin 50px
#drag
  height 100px
  border 2px dashed #eee
  line-height 100px
  text-align center
  vertical-align middle
img
  width 50px
.output
  display inline-block
  vertical-align top
  margin-left 30px
  padding 10px
  width 300px
  background  #eee
  img
    width 200px

.cube-container
  width 100px
  overflow hidden
.cube
  width 14px
  height 14px
  line-height 12px;
  border 1px solid black
  background  #eee
  float left
  >.success
    background #67C23A
  >.uploading
    background #409EFF
  >.error
    background #F56C6C

</style>
