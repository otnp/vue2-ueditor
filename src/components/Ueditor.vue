<template>
  <script ref="ueditor" v-html="inputContent" type="text/plain" style="line-height:20px;"></script>
</template>

<script>
import '../../static/ueditor-vue2/ueditor.config.js'
import '../../static/ueditor-vue2/ueditor.all.js'
import '../../static/ueditor-vue2/lang/zh-cn/zh-cn.js'
export default {
  props: ['inputContent'],
  data () {
    return {
      content: this.inputContent,
      ueditor: null,
      id: 'ueditor-' + Math.random().toString(6).substring(2),
      UE: window.UE || null
    }
  },
  mounted () {
    this.createUeditor()
  },
  destroyed () {
    if (this.ueditor != null) {
      this.ueditor.destroy()
    }
  },
  methods: {
    createUeditor () {
      var that = this
      this.$refs.ueditor.id = that.id
      that.ueditor = that.UE.getEditor(that.id)
      that.ueditor.ready(function () {
        that.ueditor.setContent(that.inputContent)
        that.content = that.ueditor.getContent()
        that.ueditor.addListener('contentChange', function (editor) {
          that.content = that.ueditor.getContent()
          that.$emit('on-content-change', that.content)
        })
      })
    }
  }
}
</script>

<style scoped>
.edui-default{
  line-height: 20px;
}
</style>
