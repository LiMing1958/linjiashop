<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="6">
          <el-input placeholder="标题" size="mini" v-model="listQuery.title"></el-input>
        </el-col>

        <el-col :span="6">
          <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
          <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
        </el-col>
      </el-row>
      <br>
      <el-row>
        <el-col :span="24">
          <el-button @click.native="add" icon="el-icon-plus" size="mini" type="success" v-permission="['/banner/edit']">{{ $t('button.add') }}</el-button>

          <el-button :disabled="shopCategory.disabled" @click.native="setBanner" icon="el-icon-edit" size="mini" type="success" v-show="shopCategory.show">选中banner</el-button>
          <el-button @click.native="remove" icon="el-icon-delete" size="mini" type="danger" v-permission="['/banner/delete']">{{ $t('button.delete') }}</el-button>
        </el-col>
      </el-row>
    </div>


    <el-table :data="list" @current-change="handleCurrentChange" @row-click="clickRow" border element-loading-text="Loading" fit
              highlight-current-row
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
      <el-table-column label="类别">
        <template slot-scope="scope">
          {{scope.row.type}}
        </template>
      </el-table-column>
      <el-table-column label="打开界面">
        <template slot-scope="scope">
          {{scope.row.page}}
        </template>
      </el-table-column>
      <el-table-column label="参数">
        <template slot-scope="scope">
          {{scope.row.param}}
        </template>
      </el-table-column>
      <el-table-column label="图片">
        <template slot-scope="scope">
          <img :src="scope.row.img" style="width:200px;">
        </template>
      </el-table-column>
    </el-table>

    <el-dialog
      :title="formTitle"
      :visible.sync="formVisible"
      width="70%">
      <el-form :model="form" :rules="rules" label-width="150px" ref="form">
        <el-row>
          <el-col :span="12">
            <el-row>

              <el-col :span="12">
                <el-form-item label="banner图片">
                  <el-upload
                    :action="uploadUrl"
                    :before-upload="handleBeforeUpload"
                    :headers="uploadHeaders"
                    :on-success="handleUploadSuccess"
                    class="upload-demo"
                    drag
                    multiple=false
                  >
                    <i class="el-icon-upload"></i>
                    <div class="el-upload__text">上传图片</div>
                  </el-upload>
                </el-form-item>
              </el-col>
            </el-row>
          </el-col>
          <el-col :span="12">
            <el-row>
              <el-col :span="24">
                <el-form-item label="标题" prop="title">
                  <el-input minlength=1 v-model="form.title"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="24">
                <el-form-item label="打开的界面">
                  <el-input v-model="form.page"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="24">
                <el-form-item label="参数" prop="param">
                  <el-input type="textarea" v-model="form.param"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="24">
                <el-form-item label="类型">
                  <el-select placeholder="请选择" v-model="form.type">
                    <el-option
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                      v-for="item in options">
                    </el-option>
                  </el-select>
                </el-form-item>
              </el-col>
            </el-row>
          </el-col>
        </el-row>





        </el-row>
        <el-form-item>
          <el-button @click="save" type="primary">{{ $t('button.submit') }}</el-button>
          <el-button @click.native="formVisible = false">{{ $t('button.cancel') }}</el-button>
        </el-form-item>

      </el-form>
    </el-dialog>
  </div>
</template>

<script src="./banner.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>
