<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div style="margin-left:auto;margin-right:auto;width:1200px;">
    <v-ueditor
     :inputContent="cont"
        v-model="cont"
        @on-content-change="change">
        </v-ueditor>
      </div>  
  </div>
</template>

<script>
import Ueditor from './Ueditor.vue'

export default {
  name: 'hello',
  data () {
    return {
      msg: 'Vue2-Ueditor',
      cont: '<p>hello ueditor~~</p>'
    }
  },
  methods: {
    change (val) {
      this.cont = val
    }
  },
  components: {
    'v-ueditor': Ueditor
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
