<template>
  <div class="app-container">
    <div style="margin-bottom: 10px;">
      <!-- Excel import button -->
      <el-button
        @click="dialogVisible = true"
        type="primary"
        size="mini"
        icon="el-icon-download"
      >
        Import Excel
      </el-button>

      <!-- Excel Output button -->
      <el-button
        @click="exportData"
        type="primary"
        size="mini"
        icon="el-icon-upload2"
      >
        Output Excel
      </el-button>
    </div>

    <!-- dialog -->
    <el-dialog title="Data dictionary import" :visible.sync="dialogVisible" width="30%">
      <el-form>
        <el-form-item label="Select Excel file">
          <el-upload
            :auto-upload="true"
            :multiple="false"
            :limit="1"
            :on-exceed="fileUploadExceed"
            :on-success="fileUploadSuccess"
            :on-error="fileUploadError"
            :action="BASE_API + '/admin/core/dict/import'"
            name="file"
            accept="application/vnd.ms-excel,application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
          >
            <el-button size="small" type="primary">Click to upload</el-button>
          </el-upload>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">
          Cancel
        </el-button>
      </div>
    </el-dialog>

    <el-table :data="list" border row-key="id" lazy :load="load">
      <el-table-column label="Name" align="left" prop="name" />
      <el-table-column label="Code" prop="dictCode" />
      <el-table-column label="Value" align="left" prop="value" />
    </el-table>
  </div>
</template>

<script>
import dictApi from '@/api/core/dict'
export default {
  data() {
    return {
      dialogVisible: false,
      BASE_API: process.env.VUE_APP_BASE_API,
      list: []
    }
  },

  created() {
    this.fetchData()
  },

  methods: {
    fetchData() {
      dictApi.listByParentId(1).then(response => {
        this.list = response.data.list
      })
    },

    // more than one file
    fileUploadExceed() {
      this.$message.warning('You can only select one file')
    },

    fileUploadSuccess(response) {
      if (response.code === 0) {
        //Success
        this.$message.success('Success to import data')
        this.dialogVisible = false
      } else {
        //Failure
        this.$message.error(response.message)
      }
    },

    fileUploadError(error) {
      this.$message.error('Fail to import data')
    },

    // Output Excel file
    exportData() {
      window.location.href = this.BASE_API + '/admin/core/dict/export'
    },

    load(tree, treeNode, resolve) {
      // Fetch data
      dictApi.listByParentId(tree.id).then(response => {
        resolve(response.data.list)
      })
    }
  }
}
</script>
