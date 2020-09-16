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
            <el-table-column label="用户id">
                <template slot-scope="scope">
                    {{scope.row.idUser}}
                </template>
            </el-table-column>
            <el-table-column label="收件人">
                <template slot-scope="scope">
                    {{scope.row.userName}}
                </template>
            </el-table-column>
            <el-table-column label="联系电话">
                <template slot-scope="scope">
                    {{scope.row.mobile}}
                </template>
            </el-table-column>
            <el-table-column label="省">
                <template slot-scope="scope">
                    {{scope.row.province}}
                </template>
            </el-table-column>
            <el-table-column label="市">
                <template slot-scope="scope">
                    {{scope.row.city}}
                </template>
            </el-table-column>
            <el-table-column label="区县">
                <template slot-scope="scope">
                    {{scope.row.district}}
                </template>
            </el-table-column>
            <el-table-column label="详细地址">
                <template slot-scope="scope">
                    {{scope.row.address}}
                </template>
            </el-table-column>
            <el-table-column label="邮政编码">
                <template slot-scope="scope">
                    {{scope.row.postCode}}
                </template>
            </el-table-column>
            <el-table-column label="是否默认">
                <template slot-scope="scope">
                    {{scope.row.isDefault}}
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
                        <el-form-item label="用户id"  >
                            <el-input minlength=1 v-model="form.idUser"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="收件人"  >
                            <el-input minlength=1 v-model="form.userName"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="联系电话"  >
                            <el-input minlength=1 v-model="form.mobile"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="省"  >
                            <el-input minlength=1 v-model="form.province"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="市"  >
                            <el-input minlength=1 v-model="form.city"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="区县"  >
                            <el-input minlength=1 v-model="form.district"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="详细地址"  >
                            <el-input minlength=1 v-model="form.address"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="邮政编码"  >
                            <el-input minlength=1 v-model="form.postCode"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="是否默认"  >
                            <el-input minlength=1 v-model="form.isDefault"></el-input>
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

<script src="./address.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
    @import "src/styles/common.scss";
</style>

