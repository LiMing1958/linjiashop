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

      <el-table-column label="名称" >
        <template slot-scope="scope">
          <el-button @click="edit(scope.row)" type="text">{{scope.row.name}}</el-button>
        </template>
      </el-table-column>
      <el-table-column label="编码" >
        <template slot-scope="scope">
          <span >{{scope.row.code}}</span>
        </template>
      </el-table-column>
      <el-table-column label="组件" >
        <template slot-scope="scope">
          <span >{{scope.row.component}}</span>
        </template>
      </el-table-column>
      <el-table-column label="是否是菜单" >
        <template slot-scope="scope">
          <span >{{scope.row.isMenuName}}</span>
        </template>
      </el-table-column>
        <el-table-column label="URL">
          <template slot-scope="scope">
            <span >{{scope.row.url}}</span>
          </template>
      </el-table-column>
      <el-table-column label="是否启用">
        <template slot-scope="scope">
          <span >{{scope.row.statusName}}</span>
        </template>
      </el-table-column>
      <el-table-column label="是否隐藏">
        <template slot-scope="scope">
          <span >{{scope.row.hidden}}</span>
        </template>
      </el-table-column>
      <el-table-column label="顺序">
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
              <el-form-item label="名称" prop="name">
                <el-input minlength=1 v-model="form.name"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="请求地址" prop="url">
                <el-input minlength=1  v-model="form.url"></el-input>
              </el-form-item>
            </el-col>

            <el-col :span="12">
              <el-form-item label="是否是菜单">
                <el-radio-group v-model="form.ismenu">
                  <el-radio :label="1">是</el-radio>
                  <el-radio :label="0">否</el-radio>
                </el-radio-group>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="是否启用">
                <el-radio-group v-model="form.status">
                  <el-radio :label="1">是</el-radio>
                  <el-radio :label="0">否</el-radio>
                </el-radio-group>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="菜单编号" prop="code">
                <el-input v-model="form.code"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="组件">
                <el-input v-model="form.component"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="是否隐藏">
                <el-radio-group v-model="form.hidden">
                  <el-radio :label="true">是</el-radio>
                  <el-radio :label="false">否</el-radio>
                </el-radio-group>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="图标" >
                <el-input v-model="form.icon"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="排序" prop="num">
                <el-input type="number" v-model="form.num"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="父菜单" >
                <el-input
                  @click.native="showTree = !showTree"
                  placeholder="请选择父菜单"
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

<script src="./menu.js"></script>
<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>
