<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="6">
          <el-input placeholder="请输入规格名名称" size="mini" v-model="listQuery.name"></el-input>
        </el-col>
        <el-col :span="6">
          <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
          <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
        </el-col>
      </el-row>
      <br>
      <el-row>
        <el-col :span="24">
          <el-button @click.native="add" icon="el-icon-plus" size="mini" type="success">{{ $t('button.add') }}</el-button>
          <el-button @click.native="edit" icon="el-icon-edit" size="mini" type="primary">{{ $t('button.edit') }}</el-button>
          <el-button @click.native="remove" icon="el-icon-delete" size="mini" type="danger">{{ $t('button.delete') }}</el-button>
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
      <el-table-column label="名称">
        <template slot-scope="scope">
          {{scope.row.attrName}}
        </template>
      </el-table-column>
      <el-table-column label="详情">
        <template slot-scope="scope">
          {{ getDetails(scope.row.attrVals)}}
        </template>
      </el-table-column>

    </el-table>


    <el-dialog
      :title="formTitle"
      :visible.sync="formVisible"
      width="60%">
      <el-form :model="form" :rules="rules" label-width="80px" ref="form">

        <el-form-item label="名称" prop="name">
          <el-input minlength=1  v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item
          :key="rec.key"
          :label="'规格名' + (index+1)"
          :prop="'details.' + index + '.value'"
          :rules="{
            required: true, message: '不能为空', trigger: 'blur'
          }"
          v-for="(rec, index) in form.details"
        >
          <el-col :span="10">
          <el-input placeholder="值" v-model="rec.key"></el-input>
          </el-col>
          <el-col :span="1" class="line">&nbsp; </el-col>
          <el-col :span="10">
          <el-input placeholder="名称" v-model="rec.value"></el-input>
          </el-col>
          <el-col :span="3">&nbsp;
          <el-button @click.prevent="removeDetail(rec)" icon="el-icon-delete" type="danger" >{{ $t('button.delete')
            }}</el-button>
          </el-col>
        </el-form-item>

        <el-form-item>
          <el-button @click="save" type="primary">{{ $t('button.submit') }}</el-button>
          <el-button @click="addDetail">新增规格名</el-button>
          <el-button @click.native="formVisible = false">{{ $t('button.cancel') }}</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

  </div>
</template>

<script src="./attribute.js"></script>
<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>
