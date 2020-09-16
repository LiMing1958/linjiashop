<template>
    <div class="app-container">
        <div class="block">
            <el-row>
                <el-col :span="24">
                    <el-button @click.native="add" icon="el-icon-plus"  size="mini" type="success" v-permission="['/category/edit']">{{ $t('button.add') }}</el-button>
                    <el-button @click.native="edit" icon="el-icon-edit"  size="mini" type="primary" v-permission="['/category/edit']">{{ $t('button.edit') }}</el-button>
                    <el-button @click.native="remove" icon="el-icon-delete"  size="mini" type="danger" v-permission="['/category/edit']">{{ $t('button.delete') }}</el-button>
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
          <el-table-column label="Banner管理">
            <template slot-scope="scope">
              <el-button @click.native="bannerMgr(scope.row.id)" icon="el-icon-picture"  size="mini" type="primary">管理</el-button>
            </template>
          </el-table-column>
          <el-table-column label="属性管理">
            <template slot-scope="scope">
              <el-button @click.native="attrKeyMgr(scope.row.id)" icon="el-icon-s-operation"  size="mini" type="primary">管理</el-button>
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
                :title="formTitle"
                :visible.sync="formVisible"
                width="70%">
            <el-form :model="form" label-width="150px"   ref="form">
                <el-row>
                    <el-col :span="12">
                        <el-form-item label="名称"  >
                            <el-input minlength=1 v-model="form.name"></el-input>
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
        :visible.sync="banner.visible"
        title="Banner管理"
        width="70%">
        <el-tabs v-model="banner.activeName">
          <el-tab-pane label="管理" name="first">
            <el-button @click.native="addBanner" icon="el-icon-plus"  size="mini" type="primary">添加</el-button>
            <el-table :data="banner.list" border element-loading-text="Loading" fit highlight-current-row v-loading="listLoading">
              <el-table-column label="标题">
                <template slot-scope="scope">
                  {{scope.row.title}}
                </template>
              </el-table-column>
              <el-table-column label="url">
                <template slot-scope="scope">
                  {{scope.row.url}}
                </template>
              </el-table-column>
              <el-table-column label="图片">
                <template slot-scope="scope">
                  <img :src="apiUrl+ '/file/getImgStream?idFile=' +scope.row.idFile" style="width:100px;">
                </template>
              </el-table-column>
              <el-table-column label="操作">
                <template slot-scope="scope">
                  <el-button @click.native="bannerRemove(scope.row.id)" icon="el-icon-delete"  size="mini" type="danger">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-tab-pane>
          <el-tab-pane label="预览" name="second">
            <el-carousel indicator-position="outside">
              <el-carousel-item :key="item" v-for="item in banner.list">
                <img :src="apiUrl+ '/file/getImgStream?idFile=' +item.idFile" style="width:100%;">
              </el-carousel-item>
            </el-carousel>
          </el-tab-pane>

        </el-tabs>
      </el-dialog>
      <el-dialog
        :visible.sync="attrKey.visible"
        title="属性管理"
        width="70%">
        <el-row>
          <el-col :span="24">
            <el-button @click.native="attrKeyAdd" icon="el-icon-plus"  size="mini" type="success" v-permission="['/category/edit']">{{ $t('button.add') }}</el-button>
          </el-col>
        </el-row>
        <el-table :data="attrKey.list" border element-loading-text="Loading" fit highlight-current-row v-loading="listLoading">
          <el-table-column label="属性名">
            <template slot-scope="scope">
              {{scope.row.attrName}}
            </template>
          </el-table-column>
          <el-table-column label="操作">
            <template slot-scope="scope">
              <el-button @click.native="attrKeyEdit(scope.row)" icon="el-icon-edit"  size="mini" type="primary">编辑</el-button>
              <el-button @click.native="openAttrValDialog(scope.row)" icon="el-icon-edit"  size="mini" type="primary">编辑属性值</el-button>
              <el-button @click.native="attrKeyRemove(scope.row.id)" icon="el-icon-delete"  size="mini" type="danger">删除</el-button>
            </template>
          </el-table-column>
        </el-table>

      </el-dialog>
      <el-dialog
        :visible.sync="attrVal.visible"
        title="属性值管理"
        width="50%">
        <el-row>
          <el-row>
            <el-col :span="24">
              <el-button @click.native="attrValAdd" icon="el-icon-plus"  size="mini" type="success" v-permission="['/category/edit']">{{ $t('button.add') }}</el-button>
            </el-col>
          </el-row>
          <el-table :data="attrVal.list" border element-loading-text="Loading" fit highlight-current-row v-loading="listLoading">
            <el-table-column label="属性值">
              <template slot-scope="scope">
                {{scope.row.attrVal}}
              </template>
            </el-table-column>
            <el-table-column label="操作">
              <template slot-scope="scope">
                <el-button @click.native="attrValEdit(scope.row)" icon="el-icon-edit"  size="mini" type="primary">编辑</el-button>
                <el-button @click.native="attrValRemove(scope.row.id)" icon="el-icon-delete"  size="mini" type="danger">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-row>
      </el-dialog>
    </div>
</template>

<script src="./category.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
    @import "src/styles/common.scss";
</style>

