<template>
    <div class="app-container">
        <div class="block">
          <el-row  :gutter="20">

            <el-col :span="8">
              <el-date-picker
                align="right"
                end-placeholder="发送截至日期"
                range-separator="至"
                size="mini"
                start-placeholder="发送起始日期"
                type="datetimerange"
                v-model="rangeDate"
                value-format="yyyyMMddHHmmss">
              </el-date-picker>
            </el-col>
            <el-col :span="6">
              <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
              <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
            </el-col>
          </el-row>
          <br>
            <el-row>
                <el-col :span="24">
                    <el-button @click.native="clear" icon="el-icon-delete" size="mini" type="danger">{{ $t('button.clear') }}</el-button>
                </el-col>
            </el-row>
        </div>


        <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
                  v-loading="listLoading">
            <el-table-column label="模板编码">
                <template slot-scope="scope">
                    {{scope.row.tplCode}}
                </template>
            </el-table-column>
            <el-table-column label="消息内容">
                <template slot-scope="scope">
                    {{scope.row.content}}
                </template>
            </el-table-column>
            <el-table-column label="接收者">
                <template slot-scope="scope">
                    {{scope.row.receiver}}
                </template>
            </el-table-column>
          <el-table-column label="发送日期">
            <template slot-scope="scope">
              {{scope.row.createTime}}
            </template>
          </el-table-column>
            <el-table-column label="消息类型">
                <template slot-scope="scope">
                    {{scope.row.type==0?"短信":"邮件"}}
                </template>
            </el-table-column>
            <el-table-column label="状态">
              <template slot-scope="scope">
                {{scope.row.state==1?"成功":"失败"}}
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

<script src="./t_message.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
    @import "src/styles/common.scss";
</style>

