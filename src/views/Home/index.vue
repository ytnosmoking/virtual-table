<template>
  <div>
    <!-- this is Home View
    <svg-icon iconName="dean"></svg-icon>切换角色 -->
    <el-button @click="add">add data</el-button>
    <el-container>
      <el-table :data="tableData" style="width: 100%" height="600">
        <el-table-column prop="date" label="日期" width="180">
        </el-table-column>
        <el-table-column prop="name" label="姓名" width="180">
        </el-table-column>
        <el-table-column prop="address" label="地址"> </el-table-column>
      </el-table>
    </el-container>
  </div>
</template>
<script>
export default {
  name: 'HomeView',
  data() {
    return {
      tableData: []
    }
  },
  mounted() { },
  methods: {
    add() {
      const ran = (num = 10000) => Math.random() * num
      // const r = Math.ceil(ran())

      this.tableData.push(...Array(100).fill(1).map(item => ({
        date: `data ${ran()}`,
        name: `name ${ran()}`,
        address: `address ${ran()}`
      })))
    }
  }
}
</script>
<style lang="less" scoped></style>
