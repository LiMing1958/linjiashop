<template>
  <div class="app-container">
    <div class="block">
      <el-tabs @tab-click="changeGroup" type="card" v-model="actvieGroup">
        <el-tab-pane
          :key="item.name"
          :label="item.name"
          :name="item.value"
          v-for="(item, index) in cfgGroups"
        >
          {{item.content}}
        </el-tab-pane>
      </el-tabs>
      <div v-show="actvieGroup !== 'all'">
        <el-form   label-width="160px">
        <el-form-item
          :key="item.cfgName"
          :label="item.cfgDesc"
          v-for="(item, index) in cfgList">
          <el-input type="textarea" v-model="cfg[item.cfgName]"  ></el-input>
        </el-form-item>
          <br>
          <el-form-item>
            <el-button @click="saveGroup" type="primary">{{ $t('button.submit') }}</el-button>
          </el-form-item>
        </el-form>
      </div>
      <div v-show="actvieGroup === 'all'">
        <el-row :gutter="20">
          <el-col :span="6">
            <el-input :placeholder="$t('config.name')" size="mini" v-model="listQuery.cfgName"></el-input>
          </el-col>
          <el-col :span="6">
            <el-input :placeholder="$t('config.value')" size="mini" v-model="listQuery.cfgValue"></el-input>
          </el-col>
          <el-col :span="6">
            <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}
            </el-button>
            <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}
            </el-button>
          </el-col>
        </el-row>
        <br>
        <el-row>
          <el-col :span="24">
            <el-button @click.native="add" icon="el-icon-plus" size="mini" type="success">{{ $t('button.add') }}
            </el-button>
            <el-button @click.native="edit" icon="el-icon-edit" size="mini" type="primary">{{ $t('button.edit') }}
            </el-button>
            <el-button @click.native="remove" icon="el-icon-delete" size="mini" type="danger">{{ $t('button.delete') }}
            </el-button>
            <el-button @click.native="exportXls" icon="el-icon-document" size="mini" type="info">{{ $t('button.export') }}
            </el-button>
          </el-col>
        </el-row>


        <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
                  v-loading="listLoading">

          <el-table-column label="ID">
            <template slot-scope="scope">
              {{scope.row.id}}
            </template>
          </el-table-column>
          <el-table-column :label="$t('config.name')">
            <template slot-scope="scope">
              {{scope.row.cfgName}}
            </template>
          </el-table-column>
          <el-table-column :label="$t('config.value')">
            <template slot-scope="scope">
              {{scope.row.cfgValue}}
            </template>
          </el-table-column>
          <el-table-column :label="$t('config.descript')">
            <template slot-scope="scope">
              {{scope.row.cfgDesc}}
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
      <el-dialog
        :title="formTitle"
        :visible.sync="formVisible"
        width="70%">
        <el-form :model="form" :rules="rules" label-width="150px" ref="form">
          <el-row>
            <el-col :span="12">
              <el-form-item :label="$t('config.name')" prop="cfgName">
                <el-input minlength=1 v-model="form.cfgName"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item :label="$t('config.value')" prop="cfgValue">
                <el-input minlength=1 v-model="form.cfgValue"></el-input>
              </el-form-item>
            </el-col>


            <el-col :span="12">
              <el-form-item :label="$t('config.descript')">
                <el-input v-model="form.cfgDesc"></el-input>
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
  </div>
</template>

<script src="./cfg.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

