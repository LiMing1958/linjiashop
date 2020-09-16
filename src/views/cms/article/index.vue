<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="6">
          <el-input placeholder="标题" size="mini" v-model="listQuery.title"></el-input>
        </el-col>
        <el-col :span="6">
          <el-input placeholder="作者" size="mini" v-model="listQuery.author"></el-input>
        </el-col>
        <el-col :span="8">
          <el-date-picker
            :picker-options="pickerOptions"
            align="right"
            end-placeholder="发布截至日期"
            range-separator="至"
            size="mini"
            start-placeholder="发布起始日期"
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
      <el-row>
        <el-col :span="24">
          <el-button @click.native="add" icon="el-icon-plus" size="mini" type="success">{{ $t('button.add') }}</el-button>
          <el-button @click.native="edit" icon="el-icon-edit"  size="mini" type="primary">{{ $t('button.edit') }}</el-button>
          <el-button @click.native="remove" icon="el-icon-delete"  size="mini" type="danger">{{ $t('button.delete') }}</el-button>
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
      <el-table-column label="标题">
        <template slot-scope="scope">
          {{scope.row.title}}
        </template>
      </el-table-column>
      <el-table-column label="发布日期">
        <template slot-scope="scope">
          {{scope.row.createTime}}
        </template>
      </el-table-column>
      <el-table-column label="作者">
        <template slot-scope="scope">
          {{scope.row.author}}
        </template>
      </el-table-column>
      <el-table-column label="文章配图">
        <template slot-scope="scope">
          <img :src="scope.row.img" style="width:200px;">
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

<script src="./article.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>
