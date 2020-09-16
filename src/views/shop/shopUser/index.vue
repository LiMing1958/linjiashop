<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="4">
          <el-input placeholder="手机号" size="mini" v-model="listQuery.mobile"></el-input>
        </el-col>
        <el-col :span="4">
          <el-input placeholder="昵称" size="mini"  v-model="listQuery.nickName"></el-input>
        </el-col>
        <el-col :span="8">
          <el-date-picker
            end-placeholder="注册截至日期"
            range-separator="至"
            size="mini"
            start-placeholder="注册开始日期"
            type="daterange"
            v-model="regDate"
            value-format="yyyy-MM-dd">
          </el-date-picker>
        </el-col>
        <el-col :span="8">
          <el-date-picker
            end-placeholder="最近登录截至日期"
            range-separator="至"
            size="mini"
            start-placeholder="最近登录开始日期"
            type="daterange"
            v-model="lastLoginTime"
            value-format="yyyy-MM-dd">
          </el-date-picker>
        </el-col>
      </el-row>
      <el-row  :gutter="20">
        <el-col :span="6">
          <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
          <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
        </el-col>
      </el-row>

    </div>
    <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
              v-loading="listLoading">
      <el-table-column label="ID">
        <template slot-scope="scope">
          {{scope.row.id}}
        </template>
      </el-table-column>
      <el-table-column label="头像">
        <template slot-scope="scope">
          <el-avatar   :src="scope.row.img"></el-avatar>
        </template>
      </el-table-column>
      <el-table-column label="昵称">
        <template slot-scope="scope">
          {{scope.row.nickName}}
        </template>
      </el-table-column>
      <el-table-column label="手机号">
        <template slot-scope="scope">
          <router-link :to="{path:'shopUserDetail?id='+scope.row.id}">
            {{scope.row.mobile}}
          </router-link>

        </template>
      </el-table-column>
      <el-table-column label="性别">
        <template slot-scope="scope">
          {{scope.row.gender}}
        </template>
      </el-table-column>
      <el-table-column label="注册时间">
        <template slot-scope="scope">
          {{scope.row.createTime}}
        </template>
      </el-table-column>
      <el-table-column label="最后登陆时间">
        <template slot-scope="scope">
          {{scope.row.lastLoginTime}}
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

<script src="./shopUser.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

