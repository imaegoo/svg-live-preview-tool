<template>
  <div id="app">
    <h1>SVG 在线预览工具</h1>
    <p>无后端 SVG 在线预览工具，只需拖入 SVG 文件，轻松预览！</p>
    <el-upload
        class="upload"
        ref="upload"
        drag
        multiple
        action=""
        accept=".svg"
        :show-file-list="false"
        :http-request="uploadFile">
      <i class="el-icon-upload"></i>
      <div class="el-upload__text">将 SVG 文件拖到此处，或<em>点击上传</em></div>
    </el-upload>
    <div class="svg-list">
      <div class="svg-item" v-for="svg in svgs" :key="svg.title">
        <div class="svg-inline" v-html="svg.data"></div>
        <el-input class="svg-title" v-model="svg.title" readonly></el-input>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      svgs: []
    }
  },
  methods: {
    async uploadFile (e) {
      try {
        if (!/\.svg$/.test(e.file.name)) {
          throw new Error(e.file.name + ' 不是 SVG 文件！')
        }
        this.svgs.push({
          title: e.file.name,
          data: await this.readAsText(e.file)
        })
      } catch (e) {
        console.error(e)
      }
    },
    readAsText (file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader()
        reader.readAsText(file)
        reader.onloadend = () => {
          if (reader.error) {
            reject(reader.error)
          } else {
            resolve(reader.result)
          }
        }
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
#app .svg-list {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
}
#app .svg-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  margin: 10px;
}
#app .svg-inline {
  width: 100px;
  height: 100px;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}
#app .svg-title input {
  border: none;
}
</style>
