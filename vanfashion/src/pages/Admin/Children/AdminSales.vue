<template>
  <div id="admin-users">
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column prop="id" label="订单ID"></el-table-column>
      <el-table-column prop="userName" label="用户"></el-table-column>
      <el-table-column prop="user_name" label="商品"></el-table-column>
      <el-table-column prop="price" label="价格"></el-table-column>
      <el-table-column prop="orderStatus" label="订单状态">
        <template slot-scope="scope">
          <span v-if="scope.row.orderStatus == 1">待发货</span>
          <span v-if="scope.row.orderStatus == 2">待收货</span>
          <span v-if="scope.row.orderStatus == 3">退货</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="250">
        <template slot-scope="scope">
          <el-button @click="deliverGoode(scope.row)" size="mini">发货</el-button>
          <el-button size="mini">退货</el-button>
          <el-button size="mini">删除订单</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
  import {getAllOrders, updateOrder} from '../../../api/index'
  export default {
    data() {
      return {
        tableData: []
      }
    },
    mounted(){
      this.getUsers();
    },
    methods: {
      // 发货
      async deliverGoode(row){
        console.log(row);
        // return;
        let result = await updateOrder(row.id,2);
        if(result.success_code === 200){
          this.$message({
            type: 'success',
            message: '发货成功'
          });
          row.orderStatus = 2
        }
      },
      async getUsers(){
        const results = await getAllOrders();
        if(results.success_code === 200){
          this.tableData = results.message;
          console.log(this.tableData);
        }
      }
    }
  }
</script>

<style scoped>

</style>
