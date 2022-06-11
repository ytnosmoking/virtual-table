<template>
  <div>
    <el-input v-model="searchParam.id" clearable :title="searchParam.id" />
    <el-input v-model="searchParam.name" clearable :title="searchParam.name" />
    <el-table
      :data="taskData"
      v-loading="taskLoading"
      v-el-table-infinite-scroll="toLoadMore"
      infinite-scroll-disabled="scrollDisabled"
      infinite-scroll-immediate="false"
    >
      <el-table-column label="id" prop="id" show-overflow-tooltip />
      <el-table-column label="name" prop="name" show-overflow-tooltip />
      <el-table-column label="sex" prop="sex" show-overflow-tooltip />
      <el-table-column label="age" prop="age" show-overflow-tooltip />
    </el-table>
  </div>
</template>

<script>
import elTableInfiniteScroll from 'el-table-infinite-scroll';
export default {
  directives: {
    'el-table-infinite-scroll': elTableInfiniteScroll
  },
  data() {
    return {
      taskData: [],
      taskLoading: false,
      scrollDisabled: false,
      searchParam: {
        page_size: 10,
        page_no: 1
      }
    }
  },
  created() {
    this.toGetList();
  },
  methods: {
    toGetList() {
      this.searchParam.page_no = 1; // 初始化 搜索条件 页码
      const param = {};
      Object.keys(this.searchParam).forEach((item) => {
        if (this.searchParam[item] || this.searchParam[item] === 0) {
          param[item] = this.searchParam[item];
        }
      });
      this.scrollDisabled = true;
      this.taskData = []; // 搜索前清空滚动加载获取的数据，避免内容重复
      this.loadList(param);
    },
    toLoadMore() {
      // 监听表格滚动事件
      this.searchParam.page_no++;
      const param = {};
      Object.keys(this.searchParam).forEach((item) => {
        if (this.searchParam[item] || this.searchParam[item] === 0) {
          param[item] = this.searchParam[item];
        }
      });
      this.loadList(param);
    },
    loadList(param) {
      this.taskLoading = true;
      this.getList(param).then((res) => {
        this.taskLoading = false;
        // 没数据的时候，禁用 infinite-scroll-disabled 滚动事件
        if (res.length === 0) return (this.scrollDisabled = true);
        this.scrollDisabled = false;
        this.taskData.push(...res); // 分页请求的数据，追加到任务列表中
      });
    },
    getList(params) {
      const ran = (num = 10000) => Math.random() * num
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          const data = Array(100).fill(1).map(item => ({
            id: `id ${ran()}`,
            name: `name ${ran()}`,
            sex: `sex ${ran()}`,
            age: `age ${ran()}`
          }))
          resolve(data)
        }, 1000)
      })
    }
  }
}
</script>
