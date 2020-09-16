<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="6">
          <el-input placeholder="文件名" size="mini" v-model="listQuery.originalFileName"></el-input>
        </el-col>

        <el-col :span="6">
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
      <el-table-column label="文件名">
        <template slot-scope="scope">
          {{scope.row.originalFileName}}
        </template>
      </el-table-column>
      <el-table-column label="上传日期">
        <template slot-scope="scope">
          {{scope.row.createTime}}
        </template>
      </el-table-column>
      <el-table-column labe="下载">
        <template slot-scope="scope">
          <el-button @click.native="download(scope.row.id,scope.row.realFileName)" icon="el-icon-log" size="mini">下载</el-button>
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

<script src="./file.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

