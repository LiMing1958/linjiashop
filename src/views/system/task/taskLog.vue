<template>
  <div class="app-container">
    <div class="block">
      <el-row>
        <el-col :span="24">
          <el-button @click.native="back" icon="el-icon-back" size="mini">{{ $t('button.back') }}</el-button>
        </el-col>
      </el-row>
    </div>
    <el-table :data="list" border element-loading-text="Loading" fit highlight-current-row v-loading="listLoading">

      <el-table-column label="任务名">
        <template slot-scope="scope">
          {{scope.row.name}}
        </template>
      </el-table-column>
      <el-table-column label="执行时间">
        <template slot-scope="scope">
          {{scope.row.execAt}}
        </template>
      </el-table-column>
      <el-table-column label="执行结果">
        <template slot-scope="scope">
          {{scope.row.execSuccess === 1 ? '成功' : '失败'}}
        </template>
      </el-table-column>
      <el-table-column label="异常信息">
        <template slot-scope="scope">
          {{scope.row.jobException}}
        </template>
      </el-table-column>

    </el-table>

    <el-pagination
      :current-page.sync="listQuery.page"
      :page-size="listQuery.limit"
      :page-sizes="[10, 20, 50, 100,500]"
      :total="total"
      @current-change="fetchPage"
      @next-click="fetchNext"
      @prev-click="fetchPrev"
      @size-change="changeSize"
      background
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
  </div>
</template>

<script src="./log.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

