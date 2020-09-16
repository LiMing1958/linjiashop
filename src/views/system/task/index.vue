<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="6">
          <el-input placeholder="请输入任务名" size="mini" v-model="listQuery.name"></el-input>
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
      <el-table-column label="任务名">
        <template slot-scope="scope">
          {{scope.row.name}}
        </template>
      </el-table-column>
      <el-table-column label="执行类" width="300">
        <template slot-scope="scope">
          {{scope.row.jobClass}}
        </template>
      </el-table-column>
      <el-table-column label="定时规则">
        <template slot-scope="scope">
          {{scope.row.cron}}
        </template>
      </el-table-column>

      <el-table-column label="说明">
        <template slot-scope="scope">
          {{scope.row.note}}
        </template>
      </el-table-column>

      <el-table-column label="最近执行时间">
        <template slot-scope="scope">
          {{scope.row.execAt}}
        </template>
      </el-table-column>

      <el-table-column label="最近执行结果">
        <template slot-scope="scope">
          {{scope.row.execResult}}
        </template>
      </el-table-column>
      <el-table-column label="操作" width="200">
        <template slot-scope="scope">
        <el-button @click.native="viewLog(scope.row.id)" icon="el-icon-log" size="mini">查看日志</el-button>
          <el-button @click.native="enable(scope.row.id)" icon="el-icon-log" size="mini" type="success"
                     v-if="scope.row.disabled===true">启用</el-button>
          <el-button @click.native="disable(scope.row.id)" icon="el-icon-log" size="mini" type="danger"
                     v-if="scope.row.disabled===false">禁用</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog
      :title="formTitle"
      :visible.sync="formVisible"
      width="70%">
      <el-form :model="form" :rules="rules" label-width="80px" ref="form">
        <el-row>
          <el-col :span="12">
            <el-form-item label="任务名" prop="name">
              <el-input v-model="form.name"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="定时规则" prop="cron">
              <el-input v-model="form.cron"></el-input>
            </el-form-item>
          </el-col>


          <el-col :span="12">
            <el-form-item label="执行类" prop="jobClass">
              <el-input type="textarea" v-model="form.jobClass"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="任务说明" prop="cfgDesc">
              <el-input type="textarea" v-model="form.note"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="执行参数">
              <el-input type="textarea" v-model="form.data"></el-input>
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

<script src="./task.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

