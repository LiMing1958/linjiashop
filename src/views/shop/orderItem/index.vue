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
            <el-table-column label="所属订单id">
                <template slot-scope="scope">
                    {{scope.row.idOrder}}
                </template>
            </el-table-column>
            <el-table-column label="商品id">
                <template slot-scope="scope">
                    {{scope.row.idGoods}}
                </template>
            </el-table-column>
            <el-table-column label="单价">
                <template slot-scope="scope">
                    {{scope.row.price}}
                </template>
            </el-table-column>
            <el-table-column label="数量">
                <template slot-scope="scope">
                    {{scope.row.amount}}
                </template>
            </el-table-column>
            <el-table-column label="合计">
                <template slot-scope="scope">
                    {{scope.row.totalPrice}}
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
                        <el-form-item label="所属订单id"  >
                            <el-input minlength=1 v-model="form.idOrder"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="商品id"  >
                            <el-input minlength=1 v-model="form.idGoods"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="单价"  >
                            <el-input minlength=1 v-model="form.price"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="数量"  >
                            <el-input minlength=1 v-model="form.amount"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="合计"  >
                            <el-input minlength=1 v-model="form.totalPrice"></el-input>
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

<script src="./orderItem.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
    @import "src/styles/common.scss";
</style>

