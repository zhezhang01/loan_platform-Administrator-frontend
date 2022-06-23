<template>
  <div class="app-container">
    <!--Search list-->
    <el-form :inline="true">
      <el-form-item label="Key word">
        <el-input v-model="keyword" placeholder="Name/Phone/ID" />
      </el-form-item>
      <el-button type="primary" icon="el-icon-search" @click="fetchData()">
        Search
      </el-button>
      <el-button type="default" @click="resetData()">
        Clear
      </el-button>
    </el-form>

    <!-- list -->
    <el-table :data="list" stripe>
      <el-table-column label="Num." width="70" align="center">
        <template slot-scope="scope">
          {{ (page - 1) * limit + scope.$index + 1 }}
        </template>
      </el-table-column>

      <el-table-column prop="name" label="Name" />
      <el-table-column prop="mobile" label="Phone" />
      <el-table-column prop="idCard" label="ID number" width="200" />
      <el-table-column label="Gender" width="80">
        <template slot-scope="scope">
          {{ scope.row.sex === 1 ? 'male' : 'female' }}
        </template>
      </el-table-column>

      <el-table-column prop="age" label="age" width="80" />

      <el-table-column label="marriage" width="120">
        <template slot-scope="scope">
          {{ scope.row.marry ? 'Yes' : 'No' }}
        </template>
      </el-table-column>

      <el-table-column label="status" width="170">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.status === 0" type="info" size="mini">
            Not certified
          </el-tag>
          <el-tag v-if="scope.row.status === 1" type="warning" size="mini">
            under certification
          </el-tag>
          <el-tag v-if="scope.row.status === 2" type="success" size="mini">
            Authentication succeeded
          </el-tag>
          <el-tag v-if="scope.row.status === -1" type="danger" size="mini">
            Authentication failed
          </el-tag>
        </template>
      </el-table-column>

      <el-table-column prop="createTime" label="Application time" width="160" />

      <el-table-column label="Operation" width="200" align="center">
        <template slot-scope="scope">
          <router-link :to="'/core/borrower/detail/' + scope.row.id">
            <el-button v-if="scope.row.status === 1" type="warning" size="mini">
              Approval
            </el-button>
            <el-button v-else type="primary" size="mini">
              Check
            </el-button>
          </router-link>
        </template>
      </el-table-column>
    </el-table>

    <!-- pagination component -->
    <el-pagination
      :current-page="page"
      :total="total"
      :page-size="limit"
      :page-sizes="[2, 10, 20]"
      style="padding: 30px 0; "
      layout="total, sizes, prev, pager, next, jumper"
      @size-change="changePageSize"
      @current-change="changeCurrentPage"
    />
  </div>
</template>

<script>
import borrowerApi from '@/api/core/borrower'

export default {
  data() {
    return {
      list: null, // banner list
      total: 0, // Total records in the database
      page: 1, // Default page number
      limit: 10, // Records per page
      keyword: '' // Query form object
    }
  },

  // Lifecycle function: the memory is ready and the page has not been rendered
  created() {
    this.fetchData()
  },

  methods: {
    // Load banner list data
    fetchData() {
      borrowerApi
        .getPageList(this.page, this.limit, this.keyword)
        .then(response => {
          this.list = response.data.pageModel.records
          this.total = response.data.pageModel.total
        })
    },

    // When the number of records per page changes, the size: callback parameter indicates the currently selected number of records per page
    changePageSize(size) {
      this.limit = size
      this.fetchData()
    },

    // Change page number, page: callback parameter, indicating the currently selected "page number"
    changeCurrentPage(page) {
      this.page = page
      this.fetchData()
    },

    // reset the form
    resetData() {
      this.keyword = ''
      this.fetchData()
    }
  }
}
</script>
