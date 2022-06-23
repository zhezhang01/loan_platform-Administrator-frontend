<template>
  <div class="app-container">
    <!-- form -->
    <el-table :data="list" border stripe>
      <el-table-column type="index" width="50" />
      <el-table-column prop="borrowAmount" label="Loan limit" />
      <el-table-column prop="integralStart" label="Start of points interval" />
      <el-table-column prop="integralEnd" label="End of points interval" />
      <el-table-column label="Operation">
        <template slot-scope="scope">
          <router-link
            :to="'/core/integral-grade/edit/' + scope.row.id"
            style="margin-right:5px;"
          >
            <el-button type="primary" size="mini" icon="el-icon-edit">
              Update
            </el-button>
          </router-link>

          <el-button
            type="danger"
            size="mini"
            icon="el-icon-delete"
            @click="removeById(scope.row.id)"
          >
            Delete
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import integralGradeApi from '@/api/core/integral-grade'

export default {
  data() {
    return {
      list: [] //Points grade list
    }
  },

  created() {
    this.fetchData()
  },

  methods: {
    fetchData() {
      integralGradeApi.list().then(response => {
        this.list = response.data.list
      })
    },

    removeById(id) {
      // debugger
      console.log('id', id)

      this.$confirm('This operation will permanently delete this record. Do you want to continue?', 'Tips', {
        confirmButtonText: 'Confirm',
        cancelButtonText: 'Cancel',
        type: 'warning'
      })
        .then(() => {
          return integralGradeApi.removeById(id)
        })
        .then(response => {
          this.$message({
            message: response.message,
            type: 'success'
          })
          this.fetchData()
        })
        .catch(error => {
          console.log('error of catch', error)
          if (error === 'cancel') {
            this.$message({
              type: 'info',
              message: 'Deletion canceled'
            })
          }
        })
    }
  }
}
</script>

<style scoped></style>
