<template>
  <div id="contain" ref="contain" @scroll="scrollListener">
    <div id="table1" ref="table1"></div>
    <el-table
      ref="showTable"
      class="showTable"
      :data="tableData"
      height="800px"
      highlight-current-row
      @row-click="rowClick"
      @current-change="currentChange"
      border
      style="width: 100%"
    >
      <el-table-column v-for="(item, index) in cols" v-bind="item" :key="index">
        <template slot-scope="{ row, $index: rowIndex }">
          <el-input v-if="isSameRow(row)" v-model="row[item.prop]"></el-input>
          <span v-else>{{ row[item.prop] }} -- {{ rowIndex }}</span>
        </template>
      </el-table-column>
      <!-- <el-table-column prop="index" label="序号" width="180"> </el-table-column>
      <el-table-column prop="date" label="日期" width="180"> </el-table-column>
      <el-table-column prop="name" label="姓名" width="180"> </el-table-column>
      <el-table-column prop="address" label="地址"> </el-table-column> -->
    </el-table>
  </div>
</template>
<script>
export default {
  name: 'TestView',
  data() {
    return {
      rowNum: 20,
      tableData: [],
      list: [],
      rowHeight: 40,
      strat: 0,
      end: 20,
      cols: [
        { prop: 'index', label: '序号', width: 180 },
        { prop: 'code', label: 'code', width: 180 },
        { prop: 'date', label: '日期', width: 180 },
        { prop: 'name', label: '姓名', width: 180 },
        { prop: 'name1', label: '姓名1', width: 180 },
        { prop: 'name2', label: '姓名2', width: 180 },
        { prop: 'name3', label: '姓名3', width: 180 },
        { prop: 'name4', label: '姓名4', width: 180 },
        { prop: 'name5', label: '姓名5', width: 180 },
        { prop: 'name6', label: '姓名6', width: 180 },
        { prop: 'name7', label: '姓名7', width: 180 },
        { prop: 'name8', label: '姓名8', width: 180 },
        { prop: 'name9', label: '姓名9', width: 180 },
        { prop: 'address', label: '地址', fixed: 'right', width: 240 }
      ],
      currentRow: null
    }
  },
  created() {
    const ran = (num = 10000) => Math.random() * num
    for (var i = 0; i < 100000; i++) {
      this.list.push({
        code: i,
        date: `周 ${parseInt(ran())}`,
        name: `name ${parseInt(ran())}`,
        name1: `name1 ${parseInt(ran())}`,
        name2: `name2 ${parseInt(ran())}`,
        name3: `name3 ${parseInt(ran())}`,
        name4: `name4 ${parseInt(ran())}`,
        name5: `name5 ${parseInt(ran())}`,
        name6: `name6 ${parseInt(ran())}`,
        name7: `name7 ${parseInt(ran())}`,
        name8: `name8 ${parseInt(ran())}`,
        name9: `name9 ${parseInt(ran())}`,
        address: `address ${parseInt(ran())}`
      })
    }

    // 计算滚动容器高度
    console.log(this.list);
  },
  mounted() {
    this.$refs.contain.style.height = this.rowHeight * this.rowNum + 'px';

    this.$refs.table1.style.height = this.rowHeight * this.list.length + 'px';
    this.$refs.contain.addEventListener('scroll', this.scrollListener);
    this.scrollListener()
  },
  computed: {
    isSameRow() {
      return row => {
        return this.currentRow === row
      }
    }
  },
  methods: {
    showList() {
      const row = this.currentRow
      this.tableData = this.list.slice(this.start, this.end);
      this.$nextTick(() => {
        this.currentRow = row
      })
    },
    scrollListener() {
      // 获取滚动高度
      const scrollTop = this.$refs.contain.scrollTop;
      // 开始的数组索引
      const curStart = Math.floor(scrollTop / this.rowHeight);
      // if (curStart === this.start) {
      //   return
      // }
      this.start = curStart
      // 结束索引
      this.end = this.start + this.rowNum;
      // 绝对定位对相对定位的偏移量
      // this.$refs.showTable.style.top = this.start * this.rowHeight + 'px';
      this.showList();
    },
    currentChange(row, oldRow) {
      console.log(row)
      this.currentRow = row
    },
    rowClick(row, column, event) {

      // console.log(row)
      // console.log(column)
      // console.log(event)
    }

  }

}
</script>
<style lang="less" scoped>
#contain {
  overflow: auto;
  position: relative;
  height: 900px;

  border: 1px solid #c1c1c1;
  // margin: 0 auto;
}
#table1 {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  z-index: -1;
}
.el-table__row {
  height: 40px;
}
.showTable {
  left: 0;
  right: 0;
  top: 0;
  position: sticky;
}
</style>
