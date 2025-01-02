<template>
  <div class="uplink-container">
    <div style="color:#909399;margin-bottom: 30px">
      當前用户：{{ name }};
      用户角色: {{ userType }}
    </div>
    <div>
      <el-form ref="form" :model="tracedata" label-width="80px" size="mini" style="">
        <el-form-item v-show="userType!='養殖戶'&userType!='消費者'" label="溯源碼:" style="width: 300px" label-width="120px">
          <el-input v-model="tracedata.traceability_code" />
        </el-form-item>

        <div v-show="userType=='養殖戶'">
          <el-form-item label="牛隻編號:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Farmer_input.Fa_fruitName" />
          </el-form-item>
          <el-form-item label="產地:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Farmer_input.Fa_origin" />
          </el-form-item>
          <el-form-item label="出生時間:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Farmer_input.Fa_plantTime" />
          </el-form-item>
          <el-form-item label="屠宰時間:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Farmer_input.Fa_pickingTime" />
          </el-form-item>
          <el-form-item label="養殖戶名稱:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Farmer_input.Fa_farmerName" />
          </el-form-item>
        </div>
        <div v-show="userType=='加工廠'">
          <el-form-item label="商品名稱:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Factory_input.Fac_productName" />
          </el-form-item>
          <el-form-item label="生產批次:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Factory_input.Fac_productionbatch" />
          </el-form-item>
          <el-form-item label="生產時間:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Factory_input.Fac_productionTime" />
          </el-form-item>
          <el-form-item label="工廠名與廠址:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Factory_input.Fac_factoryName" />
          </el-form-item>
          <el-form-item label="聯絡電話:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Factory_input.Fac_contactNumber" />
          </el-form-item>
        </div>
        <div v-show="userType=='物流公司'">
          <el-form-item label="姓名:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Driver_input.Dr_name" />
          </el-form-item>
          <el-form-item label="年齡:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Driver_input.Dr_age" />
          </el-form-item>
          <el-form-item label="聯絡電話:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Driver_input.Dr_phone" />
          </el-form-item>
          <el-form-item label="車牌號:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Driver_input.Dr_carNumber" />
          </el-form-item>
          <el-form-item label="運輸工具:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Driver_input.Dr_transport" />
          </el-form-item>
        </div>
        <div v-show="userType=='商店'">
          <el-form-item label="入庫時間:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Shop_input.Sh_storeTime" />
          </el-form-item>
          <el-form-item label="賣出時間:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Shop_input.Sh_sellTime" />
          </el-form-item>
          <el-form-item label="商店名稱:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Shop_input.Sh_shopName" />
          </el-form-item>
          <el-form-item label="商店地址:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Shop_input.Sh_shopAddress" />
          </el-form-item>
          <el-form-item label="商店電話:" style="width: 300px" label-width="120px">
            <el-input v-model="tracedata.Shop_input.Sh_shopPhone" />
          </el-form-item>
        </div>
      </el-form>
      <span slot="footer" style="color: gray;" class="dialog-footer">
        <el-button v-show="userType != '消費者'" type="primary" plain style="margin-left: 220px;" @click="submittracedata()">提 交</el-button>
      </span>
      <span v-show="userType == '消費者'" slot="footer" style="color: gray;" class="dialog-footer">
        消費者沒有權限輸入！請使用溯源功能!
      </span>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import { uplink } from '@/api/trace'

export default {
  name: 'Uplink',
  data() {
    return {
      tracedata: {
        traceability_code: '',
        Farmer_input: {
          Fa_fruitName: '',
          Fa_origin: '',
          Fa_plantTime: '',
          Fa_pickingTime: '',
          Fa_farmerName: ''
        },
        Factory_input: {
          Fac_productName: '',
          Fac_productionbatch: '',
          Fac_productionTime: '',
          Fac_factoryName: '',
          Fac_contactNumber: ''
        },
        Driver_input: {
          Dr_name: '',
          Dr_age: '',
          Dr_phone: '',
          Dr_carNumber: '',
          Dr_transport: ''
        },
        Shop_input: {
          Sh_storeTime: '',
          Sh_sellTime: '',
          Sh_shopName: '',
          Sh_shopAddress: '',
          Sh_shopPhone: ''
        }
      },
      loading: false
    }
  },
  computed: {
    ...mapGetters([
      'name',
      'userType'
    ])
  },
  methods: {
    submittracedata() {
      console.log(this.tracedata)
      const loading = this.$loading({
        lock: true,
        text: '資料上鏈中...',
        spinner: 'el-icon-loading',
        background: 'rgba(0, 0, 0, 0.7)'
      })
      var formData = new FormData()
      formData.append('traceability_code', this.tracedata.traceability_code)
      // 根据不同的用户给arg1、arg2、arg3..赋值,
      switch (this.userType) {
        case '養殖戶':
          formData.append('arg1', this.tracedata.Farmer_input.Fa_fruitName)
          formData.append('arg2', this.tracedata.Farmer_input.Fa_origin)
          formData.append('arg3', this.tracedata.Farmer_input.Fa_plantTime)
          formData.append('arg4', this.tracedata.Farmer_input.Fa_pickingTime)
          formData.append('arg5', this.tracedata.Farmer_input.Fa_farmerName)
          break
        case '加工廠':
          formData.append('arg1', this.tracedata.Factory_input.Fac_productName)
          formData.append('arg2', this.tracedata.Factory_input.Fac_productionbatch)
          formData.append('arg3', this.tracedata.Factory_input.Fac_productionTime)
          formData.append('arg4', this.tracedata.Factory_input.Fac_factoryName)
          formData.append('arg5', this.tracedata.Factory_input.Fac_contactNumber)
          break
        case '物流公司':
          formData.append('arg1', this.tracedata.Driver_input.Dr_name)
          formData.append('arg2', this.tracedata.Driver_input.Dr_age)
          formData.append('arg3', this.tracedata.Driver_input.Dr_phone)
          formData.append('arg4', this.tracedata.Driver_input.Dr_carNumber)
          formData.append('arg5', this.tracedata.Driver_input.Dr_transport)
          break
        case '商店':
          formData.append('arg1', this.tracedata.Shop_input.Sh_storeTime)
          formData.append('arg2', this.tracedata.Shop_input.Sh_sellTime)
          formData.append('arg3', this.tracedata.Shop_input.Sh_shopName)
          formData.append('arg4', this.tracedata.Shop_input.Sh_shopAddress)
          formData.append('arg5', this.tracedata.Shop_input.Sh_shopPhone)
          break
      }
      uplink(formData).then(res => {
        if (res.code === 200) {
          loading.close()
          this.$message({
            message: '上鏈成功，交易ID：' + res.txid + '\n溯源碼：' + res.traceability_code,
            type: 'success'
          })
        } else {
          loading.close()
          this.$message({
            message: '上鏈失敗',
            type: 'error'
          })
        }
      }).catch(err => {
        loading.close()
        console.log(err)
      })
    }
  }
}

</script>

<style lang="scss" scoped>
.uplink {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
