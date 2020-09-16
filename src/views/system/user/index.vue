<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="6">
          <el-input placeholder="请输入帐号" size="mini" v-model="listQuery.account"></el-input>
        </el-col>
        <el-col :span="6">
          <el-input placeholder="请输入姓名" size="mini" v-model="listQuery.name"></el-input>
        </el-col>
        <el-col :span="6">
          <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
          <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
        </el-col>
      </el-row>
      <br>
      <el-row>
        <el-col :span="24">
          <el-button @click.native="add" icon="el-icon-plus" size="mini" type="success" v-permission="['/mgr/add']">
            {{$t('button.add') }}
          </el-button>
          <el-button @click.native="edit" icon="el-icon-edit" size="mini" type="primary" v-permission="['/mgr/edit']">
            {{$t('button.edit') }}
          </el-button>
          <el-button @click.native="remove" icon="el-icon-delete" size="mini" type="danger" v-permission="['/mgr/delete']">
            {{$t('button.delete') }}
          </el-button>
          <el-button @click.native="openRole" icon="el-icon-role" size="mini" type="info">角色分配</el-button>
        </el-col>
      </el-row>
    </div>


    <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
    v-loading="listLoading">

      <el-table-column label="账号">
        <template slot-scope="scope">
          {{scope.row.account}}
        </template>
      </el-table-column>
      <el-table-column label="姓名">
        <template slot-scope="scope">
          {{scope.row.name}}
        </template>
      </el-table-column>
      <el-table-column label="性别">
        <template slot-scope="scope">
          {{scope.row.sexName}}
        </template>
      </el-table-column>
      <el-table-column label="角色">
        <template slot-scope="scope">
          {{scope.row.roleName}}
        </template>
      </el-table-column>
      <el-table-column label="部门">
        <template slot-scope="scope">
          {{scope.row.deptName}}
        </template>
      </el-table-column>
      <el-table-column label="邮箱">
        <template slot-scope="scope">
          {{scope.row.email}}
        </template>
      </el-table-column>
      <el-table-column label="电话">
        <template slot-scope="scope">
          {{scope.row.phone}}
        </template>
      </el-table-column>
      <el-table-column label="创建时间">
        <template slot-scope="scope">
          {{scope.row.createtime}}
        </template>
      </el-table-column>
      <el-table-column label="状态">
        <template slot-scope="scope">
          {{scope.row.statusName}}
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
            <el-form-item label="账户" prop="account">
              <el-input minlength=1 v-model="form.account"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="姓名" prop="name">
              <el-input minlength=1  v-model="form.name"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="12">
            <el-form-item label="性别">
              <el-radio-group v-model="form.sex">
                <el-radio :label="1">男</el-radio>
                <el-radio :label="2">女</el-radio>
              </el-radio-group>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="邮箱" prop="email">
              <el-input v-model="form.email"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12" v-show="isAdd">
            <el-form-item label="密码" prop="password">
              <el-input type="password"  v-model="form.password"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12" v-show="isAdd">
            <el-form-item label="确认密码" prop="rePassword">
              <el-input type="password"  v-model="form.rePassword"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="电话" prop="phone">
              <el-input v-model="form.phone"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="所属部门" >
              <el-input
                @click.native="deptTree.show  = !deptTree.show"
                placeholder="请选择所属部门"
                readonly="readonly"
                v-model="form.deptName">
              </el-input>
              <el-tree :data="deptTree.data"
                       :expand-on-click-node="false"
                       :props="deptTree.defaultProps"
                       @node-click="handleNodeClick"
                       class="input-tree"
                       empty-text="暂无数据"
                       v-if="deptTree.show">
              </el-tree>

            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="是否启用" prop="status">
              <el-switch v-model="form.status"></el-switch>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="出生日期">
                <el-date-picker placeholder="选择日期" style="width: 100%;" type="date" v-model="form.birthday">

                </el-date-picker>
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item>
          <el-button @click="saveUser" type="primary">{{ $t('button.submit') }}</el-button>
          <el-button @click.native="formVisible = false">{{ $t('button.cancel') }}</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <el-dialog
      :visible.sync="roleDialog.visible"
      title="角色分配"
      width="25%">
      <el-form>
        <el-row>
          <el-col :span="12">
            <el-tree
              :data="roleDialog.roles"
              :default-checked-keys="roleDialog.checkedRoleKeys"
              :props="roleDialog.defaultProps"
              node-key="id"
              ref="roleTree"
              show-checkbox>
            </el-tree>

          </el-col>
        </el-row>
        <el-form-item>
          <el-button @click="setRole" type="primary">{{ $t('button.submit') }}</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script src="./user.js"></script>
<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

