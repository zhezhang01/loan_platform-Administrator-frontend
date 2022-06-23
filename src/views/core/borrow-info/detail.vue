<template>
  <div class="app-container">
    <h4>Loan information</h4>
    <table
      class="table table-striped table-condenseda table-bordered"
      width="100%"
    >
      <tbody>
        <tr>
          <th width="15%">Loan amount</th>
          <td width="35%">${{ borrowInfoDetail.borrowInfo.amount }}</td>
          <th width="15%">Loan term</th>
          <td width="35%">{{ borrowInfoDetail.borrowInfo.period }}Mon</td>
        </tr>
        <tr>
          <th>Year rate</th>
          <td>{{ borrowInfoDetail.borrowInfo.borrowYearRate * 100 }}%</td>
          <th>Repayment method</th>
          <td>{{ borrowInfoDetail.borrowInfo.param.returnMethod }}</td>
        </tr>

        <tr>
          <th>Purpose of funds</th>
          <td>{{ borrowInfoDetail.borrowInfo.param.moneyUse }}</td>
          <th>Status</th>
          <td>{{ borrowInfoDetail.borrowInfo.param.status }}</td>
        </tr>
        <tr>
          <th>Create time</th>
          <td>{{ borrowInfoDetail.borrowInfo.createTime }}</td>
          <th></th>
          <td></td>
        </tr>
      </tbody>
    </table>

    <h4>Borrower info</h4>
    <table
      class="table table-striped table-condenseda table-bordered"
      width="100%"
    >
      <tbody>
        <tr>
          <th width="15%">Borrower</th>
          <td width="35%">
            <b>{{ borrowInfoDetail.borrower.name }}</b>
          </td>
          <th width="15%">Phone</th>
          <td width="35%">{{ borrowInfoDetail.borrower.mobile }}</td>
        </tr>
        <tr>
          <th>ID</th>
          <td>{{ borrowInfoDetail.borrower.idCard }}</td>
          <th>Gender</th>
          <td>{{ borrowInfoDetail.borrower.sex }}</td>
        </tr>
        <tr>
          <th>Age</th>
          <td>{{ borrowInfoDetail.borrower.age }}</td>
          <th>Marriage</th>
          <td>{{ borrowInfoDetail.borrower.marry }}</td>
        </tr>
        <tr>
          <th>Education</th>
          <td>{{ borrowInfoDetail.borrower.education }}</td>
          <th>Industry</th>
          <td>{{ borrowInfoDetail.borrower.industry }}</td>
        </tr>
        <tr>
          <th>Monthly income</th>
          <td>{{ borrowInfoDetail.borrower.income }}</td>
          <th>Source of repayment</th>
          <td>{{ borrowInfoDetail.borrower.returnSource }}</td>
        </tr>
        <tr>
          <th>Create time</th>
          <td>{{ borrowInfoDetail.borrower.createTime }}</td>
          <th>Status</th>
          <td>{{ borrowInfoDetail.borrower.status }}</td>
        </tr>
      </tbody>
    </table>

    <el-row style="text-align:center;margin-top: 40px;">
      <el-button @click="back">
        Return
      </el-button>
    </el-row>
  </div>
</template>

<script>
import borrowInfoApi from '@/api/core/borrow-info'
import '@/styles/show.css'

export default {
  data() {
    return {
      borrowInfoDetail: {
        borrowInfo: {
          param: {}
        },
        borrower: {}
      }
    }
  },

  created() {
    if (this.$route.params.id) {
      this.fetchDataById()
    }
  },

  methods: {
    fetchDataById() {
      borrowInfoApi.show(this.$route.params.id).then(response => {
        this.borrowInfoDetail = response.data.borrowInfoDetail
      })
    },

    back() {
      this.$router.push('/core/borrower/info-list')
    }
  }
}
</script>
