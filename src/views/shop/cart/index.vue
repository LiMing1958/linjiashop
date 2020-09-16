<template>
    <div class="app-container">


        <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
                  v-loading="listLoading">
            <el-table-column label="用户">
                <template slot-scope="scope">
                    {{scope.row.user.mobile}}
                </template>
            </el-table-column>
            <el-table-column label="商品">
                <template slot-scope="scope">
                    {{scope.row.title}}
                </template>
            </el-table-column>
          <el-table-column label="商品价格">
            <template slot-scope="scope">
              {{formatPrice(scope.row.price)}}
            </template>
          </el-table-column>
            <el-table-column label="产品规格">
                <template slot-scope="scope">
                    {{scope.row.specifications}}
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
</template>

<script src="./cart.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
    @import "src/styles/common.scss";
</style>

