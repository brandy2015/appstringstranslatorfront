<template>
  <div id="app">
    <div class="upload-container" v-if="showUpload">
      <el-upload  class="upload" :on-success="onSuccess" drag action="http://localhost/upload" multiple>
        <i class="el-icon-upload"></i>
        <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
        <div class="el-upload__tip" slot="tip">只能上传strings文件，且不超过500kb</div>
      </el-upload>
      <el-table class="file-list" :data="fileList" border>
        <el-table-column fixed prop="filename" label="文件名" ></el-table-column>
        <el-table-column fixed prop="size" label="大小" ></el-table-column>
        <el-table-column fixed prop="createTime" label="上传时间" ></el-table-column>
        <el-table-column fixed="right" label="操作" width="100">
          <template slot-scope="scope">
            <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
            <el-button type="text" size="small">编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <div v-else>
      <div class="operation-panel">
        <el-button>全部选择</el-button>
        <el-button>翻译全部</el-button>
      </div>
      <el-table :data="translatedList" border>
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column fixed prop="key" label="名称" ></el-table-column>
        <el-table-column fixed prop="origin" label="原版翻译" ></el-table-column>
        <el-table-column fixed="right" label="操作" width="100">
          <template slot-scope="scope">
            <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
            <el-button type="text" size="small">编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';

export default {
  name: 'app',
  data() {
    return {
      translatedList: [],
      fileList: [],
    }
  },
  methods: {
    onSuccess(response) {
      window.console.log(response);
      this.translatedList = response;
    },
    handleClick(row) {
      window.console.log(row);
      let self = this;
      axios.get("http://localhost/strings/" + row.id).then(function (response) {
        self.translatedList = response.data;
      });
    }
  },
  components: {
  },
  computed: {
    showUpload(){
      return this.translatedList.length === 0;
    }
  },
  created: function () {
    let self = this;
    axios.get("http://localhost/files").then(function (response) {
      window.console.log(response);
      self.fileList = response.data;
    })
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
  }
  .operation-panel {
    margin-bottom: 30px;
  }
  .upload-container {
    text-align: center;
  }
  .file-list {
    margin-top: 20px;
  }
</style>
