<template>
  <div class="app-container">
    <!-- Search form -->
    <el-form :inline="true" class="demo-form-inline">
      <el-form-item label="Phone">
        <el-input v-model="searchObj.mobile" placeholder="phone number" />
      </el-form-item>

      <el-form-item label="User type">
        <el-select v-model="searchObj.userType" placeholder="Please type" clearable>
          <el-option label="investor" value="1" />
          <el-option label="borrower" value="2" />
        </el-select>
      </el-form-item>

      <el-form-item label="User status">
        <el-select v-model="searchObj.status" placeholder="Please select" clearable>
          <el-option label="Normal" value="1" />
          <el-option label="Locked" value="0" />
        </el-select>
      </el-form-item>
      <el-button type="primary" icon="el-icon-search" @click="fetchData()">
        Search
      </el-button>
      <el-button type="default" @click="resetData()">Clear</el-button>
    </el-form>
    <!-- list -->
    <el-table :data="list" border stripe>
      <el-table-column label="#" width="50">
        <template slot-scope="scope">
          {{ (page - 1) * limit + scope.$index + 1 }}
        </template>
      </el-table-column>

      <el-table-column label="User type" width="100">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.userType === 1" type="success" size="mini">
            investor
          </el-tag>
          <el-tag
            v-else-if="scope.row.userType === 2"
            type="warning"
            size="mini"
          >
            borrower
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="mobile" label="Phone number" />
      <el-table-column prop="name" label="User name" />
      <el-table-column prop="idCard" label="ID number" />
      <el-table-column prop="integral" label="User points" />
      <el-table-column prop="createTime" label="Sign up time" width="100" />
      <el-table-column label="Binding status" width="90">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.bindStatus === 0" type="warning" size="mini">
            Unbound
          </el-tag>
          <el-tag
            v-else-if="scope.row.bindStatus === 1"
            type="success"
            size="mini"
          >
            Bound
          </el-tag>
          <el-tag v-else type="danger" size="mini">Binding failed</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="User status" width="90">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.status === 0" type="danger" size="mini">
            Locked
          </el-tag>
          <el-tag v-else type="success" size="mini">
            Normal
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column label="Operation" align="center" width="200">
        <template slot-scope="scope">
          <el-button
            v-if="scope.row.status == 1"
            type="primary"
            size="mini"
            @click="lock(scope.row.id, 0)"
          >
            Lock
          </el-button>
          <el-button
            v-else
            type="danger"
            size="mini"
            @click="lock(scope.row.id, 1)"
          >
            Unlock
          </el-button>
          <el-button
            type="primary"
            size="mini"
            @click="showLoginRecord(scope.row.id)"
          >
            Sign in Log
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- pagination component -->
    <el-pagination
      @size-change="changePageSize"
      @current-change="changeCurrentPage"
      :current-page="page"
      :page-sizes="[5, 10, 15, 20]"
      :page-size="limit"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
      style="padding:30px 0;"
    ></el-pagination>

    <el-dialog title="User sign in log" :visible.sync="dialogTableVisible">
      <el-table :data="loginRecordList" border stripe>
        <el-table-column type="index" />
        <el-table-column prop="ip" label="IP" />
        <el-table-column prop="createTime" label="Sign in time" />
      </el-table>
    </el-dialog>
  </div>
</template>

<script>
import userInfoApi from '@/api/core/user-info'
import userInfo from '@/api/core/user-info'

export default {
  data() {
    return {
      list: null, // Data list
      total: 0, // Total records in the database
      page: 1, // Default page number
      limit: 5, // Records per page
      searchObj: {}, // query criteria
      loginRecordList: [], // Member sign in log
      dialogTableVisible: false // Whether the log dialog box is displayed
    }
  },

  created() {
    this.fetchData()
  },

  methods: {
    fetchData() {
      userInfoApi
        .getPageList(this.page, this.limit, this.searchObj)
        .then(response => {
          this.list = response.data.pageModel.records
          this.total = response.data.pageModel.total
        })
    },

    // Called when modifying the number of records displayed per page
    changePageSize(size) {
      this.limit = size
      // Retrieve data
      this.fetchData()
    },
    changeCurrentPage(page) {
      this.page = page
      // Retrieve data
      this.fetchData()
    },
    // Clear query criteria
    resetData() {
      // Restore query form
      this.searchObj = {}
      // Get data agains
      this.fetchData()
    },
    // Lock or unlock the member
    lock(id, status) {
      userInfoApi.lock(id, status).then(response => {
        // refresh the page
        this.fetchData()
        this.$message.success(response.message)
      })
    },
    // Display the first 50 user sign in logs
    showLoginRecord(id) {
      this.dialogTableVisible = true
      userInfoApi.getUserLoginRecordTop50(id).then(response => {
        this.loginRecordList = response.data.list
      })
    }
  }
}
</script>
