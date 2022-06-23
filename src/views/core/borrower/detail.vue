<template>
  <div class="app-container">
    <el-form label-width="100px" class="form-table">
      <el-row>
        <el-col :span="6">
          <el-form-item label="Status">
            {{ borrower.status }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="Create time">
            {{ borrower.createTime }}
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="6">
          <el-form-item label="Name">
            {{ borrower.name }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="Gender">
            {{ borrower.sex }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="Age">
            {{ borrower.age }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="Phone">
            {{ borrower.mobile }}
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="6">
          <el-form-item label="education">
            {{ borrower.education }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="marriage">
            {{ borrower.marry }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="Industry">
            {{ borrower.industry }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="repayment source">
            {{ borrower.returnSource }}
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="6">
          <el-form-item label="ID number">
            {{ borrower.idCard }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="Contact name">
            {{ borrower.contactsName }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="Contact phone">
            {{ borrower.contactsMobile }}
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="Relationship">
            {{ borrower.contactsRelation }}
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24">
          <el-form-item label="ID1">
            <span v-for="item in borrower.borrowerAttachVOList" :key="item.id">
              <el-image
                v-if="item.imageType == 'idCard1'"
                style="width: 150px;"
                :src="item.imageUrl"
                :preview-src-list="[item.imageUrl]"
              />
            </span>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24">
          <el-form-item label="ID2">
            <span v-for="item in borrower.borrowerAttachVOList" :key="item.id">
              <el-image
                v-if="item.imageType == 'idCard2'"
                style="width: 150px;"
                :src="item.imageUrl"
                :preview-src-list="[item.imageUrl]"
              />
            </span>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24">
          <el-form-item label="House info">
            <span v-for="item in borrower.borrowerAttachVOList" :key="item.id">
              <el-image
                v-if="item.imageType == 'house'"
                style="width: 150px;"
                :src="item.imageUrl"
                :preview-src-list="[item.imageUrl]"
              />
            </span>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24">
          <el-form-item label="Vehicle info">
            <span v-for="item in borrower.borrowerAttachVOList" :key="item.id">
              <el-image
                v-if="item.imageType == 'car'"
                style="width: 150px;"
                :src="item.imageUrl"
                :preview-src-list="[item.imageUrl]"
              />
            </span>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row style="text-align:center">
        <el-button @click="back">
          Return
        </el-button>
      </el-row>
    </el-form>

    <el-form label-width="170px" v-if="borrower.status === '认证中'">
      <el-form-item label="Pass or not">
        <el-radio-group v-model="approvalForm.status">
          <el-radio :label="2">
            PASS
          </el-radio>
          <el-radio :label="-1">
            NO PASS
          </el-radio>
        </el-radio-group>
      </el-form-item>

      <el-form-item v-if="approvalForm.status == 2" label="Initial points">
        <el-input v-model="approvalForm.infoIntegral" style="width: 140px;" />
        <span style="color: indianred">(You can get 30 to 100 points)</span>
      </el-form-item>

      <el-form-item v-if="approvalForm.status == 2" label="Whether the ID card information is correct">
        <el-radio-group v-model="approvalForm.isIdCardOk">
          <el-radio :label="true">
            YES
          </el-radio>
          <el-radio :label="false">
            NO
          </el-radio>
        </el-radio-group>
        <span style="color: indianred">(You can get 30 points)</span>
      </el-form-item>

      <el-form-item v-if="approvalForm.status == 2" label="Whether the vehicle information is correct">
        <el-radio-group v-model="approvalForm.isCarOk">
          <el-radio :label="true">
            YES
          </el-radio>
          <el-radio :label="false">
            NO
          </el-radio>
        </el-radio-group>
        <span style="color: indianred">(You can get 60 points)</span>
      </el-form-item>

      <el-form-item v-if="approvalForm.status == 2" label="Whether the house information is correct">
        <el-radio-group v-model="approvalForm.isHouseOk">
          <el-radio :label="true">
            YES
          </el-radio>
          <el-radio :label="false">
            NO
          </el-radio>
        </el-radio-group>
        <span style="color: indianred">(You can get 100 points)</span>
      </el-form-item>

      <el-row style="text-align:center">
        <el-button type="primary" @click="approvalSubmit()">
          Confirm
        </el-button>
      </el-row>
    </el-form>
  </div>
</template>

<script>
import borrowerApi from '@/api/core/borrower'

export default {
  data() {
    return {
      borrower: {}, //Borrower info
      saveBtnDisabled: false, //Prevent duplicate submissions
      approvalForm: {
        //Approval form
        borrowerId: 0,
        status: 2,
        content: '',
        infoIntegral: 30,
        isIdCardOk: false,
        isHouseOk: false,
        isCarOk: false
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
      borrowerApi.show(this.$route.params.id).then(response => {
        this.borrower = response.data.borrowerDetailVO
      })
    },

    back() {
      // this.$router.push({path: '/core/borrower/list'})
      this.$router.push('/core/borrower/list')
    },

    approvalSubmit() {
      this.saveBtnDisabled = true
      this.approvalForm.borrowerId = this.$route.params.id
      borrowerApi.approval(this.approvalForm).then(response => {
        this.$message.success(response.message)
        this.$router.push('/core/borrower/list')
      })
    }
  }
}
</script>
