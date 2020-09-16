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
            <el-table-column label="编号">
                <template slot-scope="scope">
                    {{scope.row.code}}
                </template>
            </el-table-column>
            <el-table-column label="标题">
                <template slot-scope="scope">
                    {{scope.row.title}}
                </template>
            </el-table-column>
            <el-table-column label="内容">
                <template slot-scope="scope">
                    {{scope.row.content}}
                </template>
            </el-table-column>
            <el-table-column label="发送条件">
                <template slot-scope="scope">
                    {{scope.row.cond}}
                </template>
            </el-table-column>
            <el-table-column label="发送器">
                <template slot-scope="scope">
                    {{scope.row.messageSender.name}}
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
                        <el-form-item label="编号"  >
                            <el-input minlength=1 v-model="form.code"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="标题"  >
                            <el-input minlength=1 v-model="form.title"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="内容"  >
                            <el-input minlength=1 v-model="form.content"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="发送条件"  >
                            <el-input minlength=1 v-model="form.cond"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="发送器"  >
                          <el-select  filterable placeholder="请选择" v-model="form.idMessageSender">
                            <el-option
                              :key="item.id"
                              :label="item.name"
                              :value="item.id"
                              v-for="item in sendList"

                            >
                            </el-option>
                          </el-select>

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

<script src="./t_message_template.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
    @import "src/styles/common.scss";
</style>

