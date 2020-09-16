<template>
    <div class="app-container">
        <div class="block">
            <el-row>
                <el-col :span="24">
                    <el-button @click.native="add" icon="el-icon-plus"  size="mini" type="success">{{ $t('button.add') }}</el-button>
                    <el-button @click.native="edit" icon="el-icon-edit"  size="mini" type="primary">{{ $t('button.edit') }}</el-button>
                    <el-button @click.native="remove" icon="el-icon-delete"  size="mini" type="danger">{{ $t('button.delete') }}</el-button>
                </el-col>
            </el-row>
        </div>


        <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
                  v-loading="listLoading">
            <el-table-column label="公司名称">
                <template slot-scope="scope">
                    {{scope.row.name}}
                </template>
            </el-table-column>
            <el-table-column label="公司编码">
                <template slot-scope="scope">
                    {{scope.row.code}}
                </template>
            </el-table-column>
            <el-table-column label="是否禁用">
                <template slot-scope="scope">
                  <el-switch
                    @change="changeDisabled(scope.row)"
                    v-model="scope.row.disabled"
                  >
                  </el-switch>
                </template>
            </el-table-column>
            <el-table-column label="排序">
                <template slot-scope="scope">
                    {{scope.row.sort}}
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
                        <el-form-item label="公司名称"  >
                            <el-input minlength=1 v-model="form.name"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="公司编码"  >
                            <el-input minlength=1 v-model="form.code"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="是否禁用"  >
                          <el-radio-group v-model="form.disabled">
                            <el-radio :label="false">否</el-radio>
                            <el-radio :label="true">是</el-radio>
                          </el-radio-group>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="排序"  >
                            <el-input-number minlength=1 v-model="form.sort"></el-input-number>
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

<script src="./express.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
    @import "src/styles/common.scss";
</style>

