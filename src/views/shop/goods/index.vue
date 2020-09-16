<template>
  <div class="app-container">
    <div class="block">
        <el-row  :gutter="20">
          <el-col :span="6">
            <el-input placeholder="商品名称" size="mini" v-model="listQuery.name"></el-input>
          </el-col>

          <el-col :span="6">
            <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
            <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
          </el-col>
        </el-row> <br>
      <el-row>
        <el-col :span="24">
          <el-button @click.native="add" icon="el-icon-plus" size="mini" type="success" v-permission="['/goodsEdit']">{{ $t('button.add') }}
          </el-button>
        </el-col>
      </el-row>
    </div>


    <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
              v-loading="listLoading">
      <el-table-column label="名称">
        <template slot-scope="scope">
          {{scope.row.name}}
        </template>
      </el-table-column>
      <el-table-column label="小图">
        <template slot-scope="scope">
          <img :src="apiUrl+ '/file/getImgStream?idFile=' +scope.row.pic" style="width:50px;">

        </template>
      </el-table-column>

      <el-table-column label="类别">
        <template slot-scope="scope">
          {{scope.row.category.name}}
        </template>
      </el-table-column>
      <el-table-column label="产品简介">
        <template slot-scope="scope">
          {{scope.row.descript}}
        </template>
      </el-table-column>
      <el-table-column label="上架">
        <template slot-scope="scope">
          <el-switch
            @change="changeIsOnSale(scope.row)"
            v-model="scope.row.isOnSale"
          >
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column label="是否新品推荐">
        <template slot-scope="scope">
          <el-tag type="success" v-show="scope.row.isNew">是</el-tag>
          <el-tag type="warning"  v-show="!scope.row.isNew">否</el-tag>

        </template>
      </el-table-column>
      <el-table-column label="是否人气商品">
        <template slot-scope="scope">
          <el-tag type="success" v-show="scope.row.isHot">是</el-tag>
          <el-tag type="warning"  v-show="!scope.row.isHot">否</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button @click.native="edit(scope.row.id)" circle icon="el-icon-edit" size="mini" type="primary">
          </el-button>
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

<script src="./goods.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

