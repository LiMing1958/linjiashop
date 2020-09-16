<template>
  <div class="app-container">
    <div class="block">
      <el-button @click.native="add" icon="el-icon-plus" size="mini"  type="success">{{ $t('button.add') }}</el-button>
    </div>

    <tree-table
    :data="data"
    :expandAll="expandAll"
    border
    highlight-current-row>
      <el-table-column label="简称" >
        <template slot-scope="scope">
          <el-button @click="edit(scope.row)" type="text">{{scope.row.simplename}}</el-button>

        </template>
      </el-table-column>
      <el-table-column label="全称" >
        <template slot-scope="scope">
          <span >{{scope.row.fullname}}</span>
        </template>
      </el-table-column>
      <el-table-column label="顺序" >
        <template slot-scope="scope">
          <span >{{scope.row.num}}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" >
        <template slot-scope="scope">
          <el-button @click="remove(scope.row)" type="text">删除</el-button>
        </template>
      </el-table-column>
    </tree-table>

    <el-dialog
      :title="formTitle"
      :visible.sync="formVisible"
      width="70%">
      <el-form :model="form" :rules="rules" label-width="120px" ref="form">
        <el-row>
          <el-col :span="12">
            <el-form-item label="名称" prop="simplename">
              <el-input minlength=1 v-model="form.simplename"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="全称" prop="fullname">
              <el-input minlength=1  v-model="form.fullname"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="12">
            <el-form-item label="排序" prop="num">
              <el-input type="number" v-model="form.num"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="父部门" >
              <el-input
                @click.native="showTree = !showTree"
                placeholder="请选择父部门"
                readonly="readonly"
                v-model="form.pname">
              </el-input>
              <el-tree :data="data"
                       :expand-on-click-node="false"
                       :props="defaultProps"
                       @node-click="handleNodeClick"
                       class="input-tree"
                       empty-text="暂无数据"
                       v-if="showTree">
              </el-tree>

            </el-form-item>
          </el-col>


        </el-row>
        <el-form-item>
          <el-button @click="save" type="primary">{{ $t('button.submit') }}</el-button>
          <el-button @click.native="formVisible = false">{{ $t('button.cancel') }}</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script src="./dept.js"></script>
<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>
