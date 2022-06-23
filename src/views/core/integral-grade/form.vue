<template>
  <div class="app-container">
    <!-- Input list -->
    <el-form label-width="120px">
      <el-form-item label="Loan limit">
        <el-input-number v-model="integralGrade.borrowAmount" :min="0" />
      </el-form-item>
      <el-form-item label="Start of points interval">
        <el-input-number v-model="integralGrade.integralStart" :min="0" />
      </el-form-item>
      <el-form-item label="End of points interval">
        <el-input-number v-model="integralGrade.integralEnd" :min="0" />
      </el-form-item>
      <el-form-item>
        <el-button
          :disabled="saveBtnDisabled"
          type="primary"
          @click="saveOrUpdate()"
        >
          Save
        </el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import integralGradeApi from '@/api/core/integral-grade'

export default {
  data() {
    return {
      saveBtnDisabled: false, //Whether to disable the Save button to prevent the form from being submitted repeatedly
      integralGrade: {}
    }
  },

  created() {
    //When the ID attribute exists in the route, it is the callback form, and the interface of callback data needs to be called
    if (this.$route.params.id) {
      this.fetchById(this.$route.params.id)
    }
  },

  methods: {
    fetchById(id) {
      integralGradeApi.getById(id).then(response => {
        this.integralGrade = response.data.record
      })
    },

    //Save or update
    saveOrUpdate() {
      //Disable save button
      this.saveBtnDisabled = true

      if (!this.integralGrade.id) {
        //Call add interface
        this.saveData()
      } else {
        //Call update interface
        this.updateData()
      }
    },

    saveData() {
      integralGradeApi.save(this.integralGrade).then(response => {
        // this.$message({
        //   type: 'success',
        //   message: response.message
        // })
        this.$message.success(response.message)
        this.$router.push('/core/integral-grade/list')
      })
    },

    updateData() {
      integralGradeApi.updateById(this.integralGrade).then(response => {
        this.$message.success(response.message)
        this.$router.push('/core/integral-grade/list')
      })
    }
  }
}
</script>
