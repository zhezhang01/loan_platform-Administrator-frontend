<template>
  <div class="app-container">
    <h4>Bid information</h4>
    <table
      class="table table-striped table-condenseda table-bordered"
      width="100%"
    >
      <tbody>
        <tr>
          <th width="15%">Bid number</th>
          <td width="35%">
            <b>{{ lendDetail.lend.lendNo }}</b>
          </td>
          <th width="15%">Title</th>
          <td width="35%">{{ lendDetail.lend.title }}</td>
        </tr>
        <tr>
          <th>Bid amount</th>
          <td>${{ lendDetail.lend.amount }}</td>
          <th>Investment period</th>
          <td>{{ lendDetail.lend.period }}Mon</td>
        </tr>
        <tr>
          <th>Year rate</th>
          <td>
            Investor:{{ lendDetail.lend.lendYearRate * 100 }}%
            <br />
            Platform:{{ lendDetail.lend.serviceRate * 100 }}%
          </td>
          <th>Repayment way</th>
          <td>{{ lendDetail.lend.param.returnMethod }}</td>
        </tr>

        <tr>
          <th>Minimum investment amount</th>
          <td>${{ lendDetail.lend.lowestAmount }}</td>
          <th>Release date</th>
          <td>{{ lendDetail.lend.publishDate }}</td>
        </tr>
        <tr>
          <th>Start date</th>
          <td>{{ lendDetail.lend.lendStartDate }}</td>
          <th>End date</th>
          <td>{{ lendDetail.lend.lendEndDate }}</td>
        </tr>
        <tr>
          <th>Invested amount</th>
          <td>
            <b>${{ lendDetail.lend.investAmount }}</b>
          </td>
          <th>Investor number</th>
          <td>{{ lendDetail.lend.investNum }}</td>
        </tr>
        <tr>
          <th>Status</th>
          <td>
            <b>{{ lendDetail.lend.param.status }}</b>
          </td>
          <th>Create time</th>
          <td>{{ lendDetail.lend.createTime }}</td>
        </tr>
        <tr>
          <th>Instruction</th>
          <td colspan="3">
            <b>{{ lendDetail.lend.lendInfo }}</b>
          </td>
        </tr>
      </tbody>
    </table>

    <h4>Platform revenue information</h4>
    <table
      class="table table-striped table-condenseda table-bordered"
      width="100%"
    >
      <tbody>
        <tr>
          <th width="15%">Expected profit</th>
          <td width="35%">
            <b>${{ lendDetail.lend.expectAmount }}</b>
          </td>
          <th width="15%">Real time profit</th>
          <td width="35%">${{ lendDetail.lend.realAmount }}</td>
        </tr>
      </tbody>
    </table>

    <h4>Borrower information</h4>
    <table
      class="table table-striped table-condenseda table-bordered"
      width="100%"
    >
      <tbody>
        <tr>
          <th width="15%">Borrower</th>
          <td width="35%">
            <b>{{ lendDetail.borrower.name }}</b>
          </td>
          <th width="15%">Phone</th>
          <td width="35%">{{ lendDetail.borrower.mobile }}</td>
        </tr>
        <tr>
          <th>ID</th>
          <td>{{ lendDetail.borrower.idCard }}</td>
          <th>Gender</th>
          <td>{{ lendDetail.borrower.sex }}</td>
        </tr>
        <tr>
          <th>Age</th>
          <td>{{ lendDetail.borrower.age }}</td>
          <th>Marriage</th>
          <td>{{ lendDetail.borrower.marry }}</td>
        </tr>
        <tr>
          <th>Education</th>
          <td>{{ lendDetail.borrower.education }}</td>
          <th>Industry</th>
          <td>{{ lendDetail.borrower.industry }}</td>
        </tr>
        <tr>
          <th>Monthly income</th>
          <td>{{ lendDetail.borrower.income }}</td>
          <th>Repayment way</th>
          <td>{{ lendDetail.borrower.returnSource }}</td>
        </tr>
        <tr>
          <th>Create time</th>
          <td>{{ lendDetail.borrower.createTime }}</td>
          <th>Status</th>
          <td>{{ lendDetail.borrower.status }}</td>
        </tr>
      </tbody>
    </table>

    <h4>Investment record</h4>
    <el-table :data="lendItemList" stripe style="width: 100%" border>
      <el-table-column type="index" label="Num." width="70" align="center" />
      <el-table-column prop="lendItemNo" label="Investment number" />
      <el-table-column prop="investName" label="Investment user" />
      <el-table-column prop="investAmount" label="Investment amount" />
      <el-table-column label="Year rate">
        <template slot-scope="scope">
          {{ scope.row.lendYearRate * 100 }}%
        </template>
      </el-table-column>
      <el-table-column prop="investTime" label="Invest time" />
      <el-table-column prop="lendStartDate" label="Start date" />
      <el-table-column prop="lendEndDate" label="End date" />
      <el-table-column prop="expectAmount" label="Expected profot" />
      <el-table-column prop="investTime" label="Investment time" />
    </el-table>

    <h4>Repayment plan</h4>
    <el-table :data="lendReturnList" stripe style="width: 100%" border>
      <el-table-column type="index" label="Num." width="70" align="center" />
      <el-table-column prop="currentPeriod" label="Current period" />
      <el-table-column prop="principal" label="principal" />
      <el-table-column prop="interest" label="interest" />
      <el-table-column prop="total" label="principal and interest" />
      <el-table-column prop="returnDate" label="repayment date" width="150" />
      <el-table-column prop="realReturnTime" label="Real repayment date" />
      <el-table-column label="Overdue">
        <template slot-scope="scope">
          <span v-if="scope.row.overdue">
            YES(Overdue amount:${{ scope.row.overdueTotal }})
          </span>
          <span v-else>NO</span>
        </template>
      </el-table-column>
      <el-table-column label="Status" width="80">
        <template slot-scope="scope">
          {{ scope.row.status === 0 ? 'Outstanding' : 'Returned' }}
        </template>
      </el-table-column>
    </el-table>

    <el-row style="text-align:center;margin-top: 40px;">
      <el-button @click="back">
        Return
      </el-button>
    </el-row>
  </div>
</template>
<script>
import lendApi from '@/api/core/lend'
import lendItemApi from '@/api/core/lend-item'
import lendReturnApi from '@/api/core/lend-return'
import '@/styles/show.css'

export default {
  data() {
    return {
      lendDetail: {
        lend: {
          param: {}
        },
        borrower: {}
      },
      lendItemList: [], //Investment list
      lendReturnList: [] //Return plan
    }
  },

  created() {
    if (this.$route.params.id) {
      this.fetchDataById()
      // Investment record
      this.fetchLendItemList()
      // Return plan
      this.fetchLendReturnList()
    }
  },

  methods: {
    fetchDataById() {
      lendApi.show(this.$route.params.id).then(response => {
        this.lendDetail = response.data.lendDetail
      })
    },
    back() {
      this.$router.push({ path: '/core/lend/list' })
    },
    fetchLendItemList() {
      lendItemApi.getList(this.$route.params.id).then(response => {
        this.lendItemList = response.data.list
      })
    },
    fetchLendReturnList() {
      lendReturnApi.getList(this.$route.params.id).then(response => {
        this.lendReturnList = response.data.list
      })
    }
  }
}
</script>
