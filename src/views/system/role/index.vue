<template>
  <div class="app-container">
    <div class="block">
      <el-row :gutter="20">
        <el-col :span="6">
          <el-input placeholder="请输入角色名称" size="mini" v-model="listQuery.name"></el-input>
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
          <el-button @click.native="openPermissions" icon="el-icon-setting" size="mini" type="primary">权限配置</el-button>
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
      <el-table-column label="编码">
        <template slot-scope="scope">
          {{scope.row.tips}}
        </template>
      </el-table-column>
      <el-table-column label="所在部门">
        <template slot-scope="scope">
          {{scope.row.deptName}}
        </template>
      </el-table-column>
      <el-table-column label="上级角色">
        <template slot-scope="scope">
          {{scope.row.pName}}
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
      <el-form :model="form" :rules="rules" label-width="80px" ref="form">
        <el-row>
          <el-col :span="12">
            <el-form-item label="编码" prop="tips">
              <el-input minlength=1 v-model="form.tips"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="名称" prop="name">
              <el-input minlength=1 v-model="form.name"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="12">
            <el-form-item label="上级角色">
              <el-input
                @click.native="roleTree.show = !roleTree.show"
                placeholder="请选择上级角色"
                readonly="readonly"
                v-model="form.pName">
              </el-input>
              <el-tree :data="list"
                       :expand-on-click-node="false"
                       :props="roleTree.defaultProps"
                       @node-click="handleRoleNodeClick"
                       class="input-tree"
                       empty-text="暂无数据"
                       v-if="roleTree.show">
              </el-tree>

            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="排序">
              <el-input type="number" v-model="form.num"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="12">
            <el-form-item label="所在部门">
              <el-input
                @click.native="deptTree.show = !deptTree.show"
                placeholder="请选择所在部门"
                readonly="readonly"
                v-model="form.deptName">
              </el-input>
              <el-tree :data="deptList"
                       :expand-on-click-node="false"
                       :props="deptTree.defaultProps"
                       @node-click="handleDeptNodeClick"
                       class="input-tree"
                       empty-text="暂无数据"
                       v-if="deptTree.show">
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


    <el-dialog
      :visible.sync="permissonVisible"
      title="权限配置"
      width="25%">
      <el-form>
        <el-row>
          <el-col :span="12">
            <el-tree
              :data="permissons"
              :default-checked-keys="checkedPermissionKeys"
              :props="defaultProps"
              node-key="id"
              ref="permissonTree"
              show-checkbox>
            </el-tree>

          </el-col>
        </el-row>
        <el-form-item>
          <el-button @click="savePermissions" type="primary">{{ $t('button.submit') }}</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

  </div>
</template>

<script src="./role.js"></script>
<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>
