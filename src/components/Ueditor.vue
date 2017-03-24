<template>
  <script ref="ueditor" v-html="inputcontent" type="text/plain" style="line-height:20px;"></script>
</template>

<script>
export default {
  props: {
    inputcontent: {
      type: String,
      default: '请输入内容'
    }
  },
  data () {
    return {
      content: '',
      ueditor: null,
      id: 'ueditor-' + Math.random().toString(6).substring(2),
      ueditorState: false
    }
  },
  mounted () {
    this.loadJS()
  },
  destroyed () {
    if (this.ueditorState) {
      this.ueditor.destroy()
      this.ueditor = null
    }
  },
  methods: {
    loadJS () {
      if (window.UE === undefined || window.UE === null) {
        let confScript = document.createElement('script')
        confScript.type = 'text/javascript'
        confScript.src = '/static/ueditor-vue2/ueditor.config.js'
        confScript.id = 'ueditor-conf-script'

        let ueditorScript = document.createElement('script')
        ueditorScript.type = 'text/javascript'
        ueditorScript.src = '/static/ueditor-vue2/ueditor.all.js'
        ueditorScript.id = 'ueditor-script'

        let langScript = document.createElement('script')
        langScript.type = 'text/javascript'
        langScript.src = '/static/ueditor-vue2/lang/zh-cn/zh-cn.js'
        langScript.id = 'ueditor-script'

        let s = document.getElementsByTagName('head')[0]
        s.appendChild(confScript)
        confScript.onload = () => {
          s.appendChild(ueditorScript)
          ueditorScript.onload = () => {
            s.appendChild(langScript)
            langScript.onload = () => {
              this.createUeditor()
            }
          }
        }
      } else {
        this.createUeditor()
      }
    },
    createUeditor () {
      this.content = this.inputcontent
      this.$nextTick(() => {
        var that = this
        this.$refs.ueditor.id = that.id
        this.ueditor = window.UE.getEditor(that.id)
        this.ueditor.ready(function () {
          this.content = this.ueditor.getContent()
          this.ueditorState = true
          this.setContent(this.inputcontent)
          this.ueditor.addListener('contentChange', function (editor) {
            this.content = this.ueditor.getContent()
            this.$emit('on-content-change', this.content)
          }.bind(this))
        }.bind(this))
      })
    },
    setContent (val) {
      if (this.content === val) {
        return true
      }
      this.$nextTick(() => {
        console.log(this.ueditorState)
        if (this.ueditorState) {
          this.ueditor.setContent(val)
          this.content = val
        }
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
