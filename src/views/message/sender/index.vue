<template>
    <div class="app-container">
        <div class="block">
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
            <el-table-column label="名称">
                <template slot-scope="scope">
                    {{scope.row.name}}
                </template>
            </el-table-column>
            <el-table-column label="发送类">
                <template slot-scope="scope">
                    {{scope.row.className}}
                </template>
            </el-table-column>
          <el-table-column label="运营商模板编号">
            <template slot-scope="scope">
              {{scope.row.tplCode}}
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
            <el-form :model="form" :rules="rules" label-width="150px" ref="form">
                <el-row>
                    <el-col :span="12">
                        <el-form-item label="名称"  >
                            <el-input minlength=1 v-model="form.name"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="发送类"  >
                            <el-input minlength=1 v-model="form.className"></el-input>
                        </el-form-item>
                    </el-col>
                  <el-col :span="12">
                    <el-form-item label="运营商模板编号"  >
                      <el-input minlength=1 v-model="form.tplCode"></el-input>
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

<script src="./t_message_sender.js"></script>

<style lang="scss" rel="stylesheet/scss" scoped>
    @import "@/styles/common.scss";
</style>

