<template>
  <div class="app-container">
    <!-- list -->
    <el-table :data="list" stripe>
      <el-table-column type="index" label="Num." width="60" align="center" />
      <el-table-column prop="lendNo" label="Bid id" width="160" />
      <el-table-column prop="amount" label="Bid amount" />
      <el-table-column prop="period" label="Investment period" />
      <el-table-column label="Year rate">
        <template slot-scope="scope">
          {{ scope.row.lendYearRate * 100 }}%
        </template>
      </el-table-column>
      <el-table-column prop="investAmount" label="Invested amount" />
      <el-table-column prop="investNum" label="Investor number" />
      <el-table-column prop="publishDate" label="Release time" width="150" />
      <el-table-column prop="lendStartDate" label="Start date" />
      <el-table-column prop="lendEndDate" label="End date" />
      <el-table-column prop="param.returnMethod" label="Repayment way" />
      <el-table-column prop="param.status" label="status" />

      <el-table-column label="Operation" width="200" align="center">
        <template slot-scope="scope">
          <el-button type="primary" size="mini">
            <router-link :to="'/core/lend/detail/' + scope.row.id">
              Check
            </router-link>
          </el-button>

          <el-button
            v-if="scope.row.status == 1"
            type="warning"
            size="mini"
            @click="makeLoan(scope.row.id)"
          >
            Approval
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import lendApi from '@/api/core/lend'

export default {
  data() {
    return {
      list: null // list
    }
  },

  created() {
    this.fetchData()
  },

  methods: {
    // Add list data
    fetchData() {
      lendApi.getList().then(response => {
        this.list = response.data.list
      })
    },
    makeLoan(id) {
      this.$confirm('Are you sure to approve the loan request?', 'Tips', {
        confirmButtonText: 'Confirm',
        cancelButtonText: 'Cancel',
        type: 'warning'
      })
        .then(() => {
          return lendApi.makeLoan(id)
        })
        .then(response => {
          this.fetchData()
          this.$message({
            type: 'success',
            message: response.message
          })
        })
        .catch(error => {
          console.log('Cancel', error)
          if (error === 'cancel') {
            this.$message({
              type: 'info',
              message: 'Request canceled'
            })
          }
        })
    }
  }
}
</script>
