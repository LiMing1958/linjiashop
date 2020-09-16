<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="6">
          <el-input placeholder="姓名" size="mini" v-model="listQuery.userName"></el-input>
        </el-col>
        <el-col :span="6">
          <el-input placeholder="手机号" size="mini"  v-model="listQuery.mobile"></el-input>
        </el-col>
        <el-col :span="8">
          <el-date-picker
            :picker-options="pickerOptions"
            align="right"
            end-placeholder="邀约截至日期"
            range-separator="至"
            size="mini"
            start-placeholder="邀约开始日期"
            type="datetimerange"
            v-model="rangeDate"
            value-format="yyyyMMddHHmmss">
          </el-date-picker>
        </el-col>
        <el-col :span="4">
          <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
          <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
        </el-col>
      </el-row>
      <br>

    </div>


    <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
    v-loading="listLoading">

      <el-table-column label="ID">
        <template slot-scope="scope">
          {{scope.row.id}}
        </template>
      </el-table-column>
      <el-table-column label="联系人">
        <template slot-scope="scope">
          {{scope.row.userName}}
        </template>
      </el-table-column>
      <el-table-column label="联系手机">
        <template slot-scope="scope">
          {{scope.row.mobile}}
        </template>
      </el-table-column>
      <el-table-column label="email">
        <template slot-scope="scope">
          {{scope.row.email}}
        </template>
      </el-table-column>
      <el-table-column label="邀约时间">
        <template slot-scope="scope">
          {{scope.row.createTime}}
        </template>
      </el-table-column>
      <el-table-column label="备注">
        <template slot-scope="scope">
          {{scope.row.remark}}
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

<script src="./contacts.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

