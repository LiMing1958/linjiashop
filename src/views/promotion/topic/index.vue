<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="6">
            <el-select    filterable placeholder="请选择是否禁用" size="mini" v-model="listQuery.disabled">
              <el-option
                :key="item.value"
                :label="item.label"
                :value="item.value"
                v-for="item in options">
              </el-option>
            </el-select>

        </el-col>
        <el-col :span="10">
          <el-date-picker
            align="right"
            end-placeholder="发送截至日期"
            range-separator="至"
            size="mini"
            start-placeholder="发送起始日期"
            type="datetimerange"
            v-model="rangeDate"
            value-format="yyyyMMddHHmmss">
          </el-date-picker>
        </el-col>
        <el-col :span="6">
          <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
          <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
        </el-col>
      </el-row>
      <br>
      <el-row>
        <el-col :span="24">
          <el-button @click.native="add" icon="el-icon-plus" size="mini" type="success" v-permission="['/topic/edit']">{{ $t('button.add') }}
          </el-button>
          <el-button @click.native="remove" icon="el-icon-delete" size="mini" type="danger" v-permission="['/topic/delete']">{{ $t('button.delete') }}
          </el-button>
        </el-col>
      </el-row>
    </div>


    <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
              v-loading="listLoading">
      <el-table-column label="标题">
        <template slot-scope="scope">
          {{scope.row.title}}
        </template>
      </el-table-column>
      <el-table-column label="专题详情">
        <template slot-scope="scope">
          <img :src="apiUrl+ '/file/getImgStream?idFile=' +scope.row.article.img"
               @click="viewArticle(scope.row.article)" style="width:50px !important;cursor:pointer;" title="查看专题详情">
        </template>
      </el-table-column>
      <el-table-column label="阅读量">
        <template slot-scope="scope">
          {{scope.row.pv}}
        </template>
      </el-table-column>
      <el-table-column label="是否禁用">
        <template slot-scope="scope">
          <el-switch
            @change="changeDisabled(scope.row)"
            v-model="scope.row.disabled"
          >
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column label="创建日期">
        <template slot-scope="scope">
          {{scope.row.createTime}}
        </template>
      </el-table-column>
      <el-table-column label="最近维护日期">
        <template slot-scope="scope">
          {{scope.row.modifyTime}}
        </template>
      </el-table-column>
      <el-table-column>
        <template slot-scope="scope">
          <el-button @click.native="view(scope.row.id)" circle icon="el-icon-view" size="mini" type="primary"></el-button>
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
    <el-dialog
      :title="article.title"
      :visible.sync="showArticle"
      width="80%">
      <p v-html="article.content"></p>
    </el-dialog>
    <el-dialog
      :title="formTitle"
      :visible.sync="formVisible"
      width="70%">
      <el-form :model="form" :rules="rules" label-width="150px" ref="form">
        <el-row>
          <el-col :span="12">
            <el-form-item label="标题">
              <el-input minlength=4 placeholder="请输入标题" v-model="form.title"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="专题文章">
              <el-select
                :loading="searchLoading"
                :remote-method="searchArticle"
                filterable
                no-data-text="无数据,可在CMS管理-文章管理中新建专题文章"
                placeholder="请输入文章关键字搜索2"
                remote
                v-model="form.idArticle">
                <el-option
                  :key="item.id"
                  :label="item.title"
                  :value="item.id"
                  v-for="item in articleList">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="商品列表">
              <el-select
                :loading="searchLoading"
                :remote-method="searchGoods"
                @change="changeGoods"
                filterable
                multiple
                placeholder="请输入商品关键字搜索"
                remote
                v-model="form.idGoodsList">
                <el-option
                  :key="item.id"
                  :label="item.name"
                  :value="item.id"
                  v-for="item in goodsList">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item>
          <el-button @click="save" type="primary">{{ $t('button.submit') }}</el-button>
          <el-button @click.native="formVisible = false">{{ $t('button.cancel') }}</el-button>
        </el-form-item>

      </el-form>
    </el-dialog>
    <el-dialog
      :visible.sync="showTopic"
      title="专题预览"
      width="450px">
      <p v-html="topicDetail"></p>


    </el-dialog>
  </div>
</template>

<script src="./topic.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";

</style>

