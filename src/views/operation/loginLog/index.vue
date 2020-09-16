<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="24">
        <el-col :span="4">
          <el-date-picker placeholder="起始日期" size="mini" style="width: 100%;" type="date" v-model="listQuery.beginTime"
                          value-format="yyyy-MM-dd"></el-date-picker>
        </el-col>
        <el-col :span="4">
          <el-date-picker placeholder="结束日期" size="mini" style="width: 100%;" type="date"  v-model="listQuery.endTime"
                          value-format="yyyy-MM-dd"></el-date-picker>
        </el-col>
        <el-col :span="4">
          <el-input placeholder="日志名称" size="mini" v-model="listQuery.logname"></el-input>
        </el-col>

        <el-col :span="8">
          <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
          <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
          <el-button @click.native="clear" icon="el-icon-delete" size="mini" type="danger">{{ $t('button.clear') }}</el-button>
        </el-col>
      </el-row>
      <br>

    </div>

    <el-table :data="list" border element-loading-text="Loading" fit highlight-current-row v-loading="listLoading">
     <el-table-column type="expand">
       <template slot-scope="props">
         <el-form class="flash-table-expand" inline label-position="left">
           <el-form-item label="用户id">
             <span>{{ props.row.userid }}</span>
           </el-form-item>
           <el-form-item label="日志名称">
             <span>{{ props.row.logname }}</span>
           </el-form-item>
           <el-form-item label="用户">
             <span>{{ props.row.userName }}</span>
           </el-form-item>
           <el-form-item label="IP">
             <span>{{ props.row.ip }}</span>
           </el-form-item>
           <el-form-item label="结果">
             <span>{{ props.row.succeed }}</span>
           </el-form-item>
           <el-form-item label="时间">
             <span>{{ props.row.createTime }}</span>
           </el-form-item>
         </el-form>
       </template>
     </el-table-column>

     <el-table-column
       label="用户"
       prop="userName">
     </el-table-column>
     <el-table-column
       label="IP"
       prop="ip">
     </el-table-column>
     <el-table-column
       label="日志名称"
       prop="logname">
     </el-table-column>
     <el-table-column
       label="时间"
       prop="createTime">
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

<script src="./loginLog.js"></script>
<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>
