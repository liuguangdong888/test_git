<template>
  <div>
    <!-- <div>
     <div>
       <span id="printTest">liudong</span>
     </div>
    <span>
      <van-button type="primary"  v-print="'#printTest'">打印</van-button>
      <van-button type="warning" @click="dialogVisible = false">取消</van-button>
    </span>
    </div>-->
    <!-- <div ref="context">
      <span>打印</span>
      <div class="no-print">不打印</div>
      <div>你好啊111</div>
    </div>
    <span>
      <van-button type="primary" @click="dayin">打印</van-button>
    </span> -->
    <el-col style="padding: 10px 0 20px;">
        <el-button
          class="pull-right"
          icon="el-icon-upload"
          type="primary"
          size="mini"
          @click="importFile()"
        >批量导入</el-button>
        <el-button
          class="pull-right right-10"
          icon="el-icon-download"
          type="primary"
          size="mini"
          @click="downloadFile('档案模板')"
        >模板下载</el-button>
        <el-button
          size="mini"
          type="primary"
          icon="el-icon-plus"
          class="pull-right"
          @click="addRow"
        >新增</el-button>
        <div class="pull-right">
          <el-input
            placeholder="请输入编码,名称"
            prefix-icon="el-icon-search"
            v-model="FinQueryParams.archiveFilter"
            size="mini"
          ></el-input>
        </div>
      </el-col>
  </div>
</template>

<script>
export default {
  name: 'Home',
  methods: {
    dayin () {
      this.$print(this.$refs.context)
    },
    importFile () {
      this.$refs.refFileUpload.open()
    },
    downloadFile (name) {
      const requestConfig = {
        headers: {
          'Content-Type': 'application/json;application/octet-stream'
        }
      }
      this.$axios.post(this.downLoadUrl, requestConfig, {
        responseType: 'blob'
      }).then(res => {
        // 处理返回的文件流
        const content = res.data
        const blob = new Blob([content])
        var date =
          new Date().getFullYear() +
          '' +
          (new Date().getMonth() + 1) +
          '' +
          new Date().getDate()
        const fileName = date + name + '.xlsx'
        if ('download' in document.createElement('a')) {
          // 非IE下载
          const elink = document.createElement('a')
          elink.download = fileName
          elink.style.display = 'none'
          elink.href = URL.createObjectURL(blob)
          document.body.appendChild(elink)
          elink.click()
          URL.revokeObjectURL(elink.href) // 释放URL 对象
          document.body.removeChild(elink)
        } else {
          // IE10+下载
          navigator.msSaveBlob(blob, fileName)
        }
      })
    }

  }
}
</script>
<style lang="less" scoped>
/deep/ * {
  font-size: 14px;
}
/deep/.van-buttons {
  color: red;
}
</style>
