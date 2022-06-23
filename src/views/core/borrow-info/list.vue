<template>
  <div class="app-container">
    <!-- list -->
    <el-table :data="list" stripe>
      <el-table-column type="index" label="Num." width="60" align="center" />
      <el-table-column prop="name" label="Borrower name" width="90" />
      <el-table-column prop="mobile" label="Phone" />
      <el-table-column prop="amount" label="Loan amount" />
      <el-table-column label="Loan period" width="90">
        <template slot-scope="scope">{{ scope.row.period }}Mon</template>
      </el-table-column>
      <el-table-column prop="param.returnMethod" label="Repayment" width="150" />
      <el-table-column prop="param.moneyUse" label="Purpose of funds" width="100" />
      <el-table-column label="Year rate" width="90">
        <template slot-scope="scope">
          {{ scope.row.borrowYearRate * 100 }}%
        </template>
      </el-table-column>
      <el-table-column prop="param.status" label="status" width="100" />

      <el-table-column prop="createTime" label="Application time" width="150" />

      <el-table-column label="Operation" width="150" align="center">
        <template slot-scope="scope">
          <el-button type="primary" size="mini">
            <router-link :to="'/core/borrower/info-detail/' + scope.row.id">
              Check
            </router-link>
          </el-button>

          <el-button
            v-if="scope.row.status === 1"
            type="warning"
            size="mini"
            @click="approvalShow(scope.row)"
          >
            Approval
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog title="approval" :visible.sync="dialogVisible" width="490px">
      <el-form label-position="right" label-width="100px">
        <el-form-item label="Pass or not">
          <el-radio-group v-model="borrowInfoApproval.status">
            <el-radio :label="2">PASS</el-radio>
            <el-radio :label="-1">NO PASS</el-radio>
          </el-radio-group>
        </el-form-item>

        <el-form-item v-if="borrowInfoApproval.status == 2" label="Loan name">
          <el-input v-model="borrowInfoApproval.title" />
        </el-form-item>

        <el-form-item v-if="borrowInfoApproval.status == 2" label="Interest start date">
          <el-date-picker
            v-model="borrowInfoApproval.lendStartDate"
            type="date"
            placeholder="Select a start time"
            value-format="yyyy-MM-dd"
          />
        </el-form-item>

        <el-form-item v-if="borrowInfoApproval.status == 2" label="Annual yield">
          <el-input v-model="borrowInfoApproval.lendYearRate">
            <template slot="append">%</template>
          </el-input>
        </el-form-item>

        <el-form-item v-if="borrowInfoApproval.status == 2" label="Service Fee">
          <el-input v-model="borrowInfoApproval.serviceRate">
            <template slot="append">%</template>
          </el-input>
        </el-form-item>

        <el-form-item v-if="borrowInfoApproval.status == 2" label="Loan description">
          <el-input v-model="borrowInfoApproval.lendInfo" type="textarea" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">
          Cancel
        </el-button>
        <el-button type="primary" @click="approvalSubmit">
          Confirm
        </el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import borrowInfoApi from '@/api/core/borrow-info'

export default {
  data() {
    return {
      list: [],
      dialogVisible: false,
      borrowInfoApproval: {
        status: 2,
        serviceRate: 5,
        lendYearRate: 0
      } 
    }
  },

  created() {
    this.fetchData()
  },

  methods: {
    fetchData() {
      borrowInfoApi.getList().then(response => {
        this.list = response.data.list
      })
    },

    approvalShow(row) {
      this.dialogVisible = true
      this.borrowInfoApproval.lendYearRate = row.borrowYearRate * 100
      this.borrowInfoApproval.id = row.id
    },

    approvalSubmit() {
      borrowInfoApi.approval(this.borrowInfoApproval).then(response => {
        this.dialogVisible = false
        this.$message.success(response.message)
        this.fetchData()
      })
    }
  }
}
</script>
