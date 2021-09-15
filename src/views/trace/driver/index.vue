<template>
  <div class="app-container">
    <el-row style="margin-bottom: 30px;width: 100%" :gutter="8">
      <el-col :span="4"><el-input v-model="filterText" placeholder="司机ID" /></el-col>
      <el-col :span="2"><el-input v-model="filterText" placeholder="来源系统" /></el-col>
      <el-col :span="2"><el-input v-model="filterText" placeholder="目标系统" /></el-col>
      <el-col :span="4"> <el-date-picker
        v-model="value1"
        type="daterange"
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        style="width: 100%;"
      /></el-col>
      <el-col :span="12">
        <el-button type="primary" icon="el-icon-search">搜索</el-button>
        <el-button>清空</el-button>
        <el-button icon="el-icon-info" @click="openStatePanel">状态</el-button>
        <el-button icon="el-icon-s-promotion" @click="openJumpPanel">跳转到系统</el-button>
      </el-col>
    </el-row>
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >
      <el-table-column align="center" label="司机ID" width="180">
        <template slot-scope="scope">
          {{ scope.$index * 123123123 + 820381038892820 }}
        </template>
      </el-table-column>
      <el-table-column label="请求路由">
        <template slot-scope="scope">
          {{ scope.row.title }}
        </template>
      </el-table-column>
      <el-table-column label="操作人" width="110" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.author }}</span>
        </template>
      </el-table-column>
      <el-table-column class-name="status-col" label="是否成功" width="110" align="center">
        <template slot-scope="scope">
          <el-tag :type="scope.row.status | statusFilter">{{ scope.row.status }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column align="center" prop="created_at" label="操作时间" width="200">
        <template slot-scope="scope">
          <i class="el-icon-time" />
          <span>{{ scope.row.display_time }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" prop="created_at" label="Trace" width="400">
        <template slot-scope="scope">
          <span><a style="color: dodgerblue;" :href="'https://www.bilibili.com/b2b/addCart?gid='+ scope.row.author">{{ scope.row.author }}</a></span>
        </template>
      </el-table-column>
    </el-table>
    <div class="block">
      <el-pagination
        :current-page="currentPage4"
        :page-sizes="[100, 200, 300, 400]"
        :page-size="100"
        layout="total, sizes, prev, pager, next, jumper"
        :total="400"
        style="float: right;padding: 10px;"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
  </div>
</template>

<script>
import { getList } from '@/api/table'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: null,
      listLoading: true,
      driverId: 0
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
      getList().then(response => {
        this.list = response.data.items
        this.listLoading = false
      })
    },
    openJumpPanel() {
      const h = this.$createElement
      this.$msgbox({
        title: '跳转到',
        message: h('p', null, [
          h('span', null, '可以跳转到如下系统'),
          h('br', null, ''),
          h('a', { style: 'color: dodgerblue', attrs: { href: 'https://www.baidu.com/s?wd=' + this.driverId, target: '_blank' }}, 'SpruceDebugger'),
          h('br', null, ''),
          h('a', { style: 'color: dodgerblue', attrs: { href: 'https://www.baidu.com/s?wd=' + this.driverId, target: '_blank' }}, '全景图'),
          h('br', null, ''),
          h('a', { style: 'color: dodgerblue', attrs: { href: 'https://www.baidu.com/s?wd=' + this.driverId, target: '_blank' }}, 'MIS')
        ]),
        showCancelButton: false,
        confirmButtonText: '关闭'
      })
    },
    openStatePanel() {
      const h = this.$createElement
      this.$msgbox({
        title: '司机状态',
        message: h('p', null, [
          h('span', null, '这部分包含了司机在星海和司机系统的基本信息')
          // h('br', null, ''),
          // h('a', { style: 'color: dodgerblue', attrs: { href: 'https://www.baidu.com/s?wd=' + this.driverId, target: '_blank' }}, 'SpruceDebugger'),
          // h('br', null, ''),
          // h('a', { style: 'color: dodgerblue', attrs: { href: 'https://www.baidu.com/s?wd=' + this.driverId, target: '_blank' }}, '全景图'),
          // h('br', null, ''),
          // h('a', { style: 'color: dodgerblue', attrs: { href: 'https://www.baidu.com/s?wd=' + this.driverId, target: '_blank' }}, 'MIS')
        ]),
        showCancelButton: false,
        confirmButtonText: '关闭'
      })
    }
  }
}
</script>
