<template>
  <div class="app-container">
    <div class="block">
      <el-row  :gutter="20">
        <el-col :span="24">
          <el-row>
            <el-col :span="2" style=";margin-top:.5rem;">
              <span style="font-size:.9rem">订单状态：</span>
            </el-col>
            <el-col :span="22">
              <el-button :type="query.button.css.status.all" @click.native="queryByState('all')" size="small" >全部</el-button>
              <el-button  :type="query.button.css.status.unPay" @click.native="queryByState('unPay')" size="mini">待付款 &nbsp;<el-tag size="mini"  type="success">{{query.button.tag.unPay}}</el-tag></el-button>
              <el-button  :type="query.button.css.status.unSend" @click.native="queryByState('unSend')" size="mini">待发货&nbsp;<el-tag size="mini"  type="danger">{{query.button.tag.unSend}}</el-tag></el-button>
              <el-button  :type="query.button.css.status.sended" @click.native="queryByState('sended')" size="mini">已发货&nbsp;<el-tag size="mini"  type="info">{{query.button.tag.sended}}</el-tag></el-button>
              <el-button  :type="query.button.css.status.finished" @click.native="queryByState('finished')" size="mini">已完成&nbsp;<el-tag size="mini"  type="info">{{query.button.tag.finished}}</el-tag></el-button>
              <el-button  :type="query.button.css.status.cancel" @click.native="queryByState('cancel')" size="mini">已取消&nbsp;<el-tag size="mini"  type="info">{{query.button.tag.cancel}}</el-tag></el-button>
              <el-button  :type="query.button.css.status.refundIng" @click.native="queryByState('refundIng')" size="mini">退款中&nbsp;<el-tag size="mini"  type="danger">{{query.button.tag.refundIng}}</el-tag></el-button>
              <el-button  :type="query.button.css.status.refund" @click.native="queryByState('refund')" size="mini">已退款&nbsp;<el-tag size="mini"  type="info">{{query.button.tag.refund}}</el-tag></el-button>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="2" style=";margin-top:.5rem;">
              <span style="font-size:.9rem;">下单日期：</span>
            </el-col>
            <el-col :span="22">
              <el-button :type="query.button.css.date.all" @click.native="queryByDate('all')"  size="small">全部</el-button>
              <el-button  :type="query.button.css.date.today" @click.native="queryByDate('today')"  size="small">今天</el-button>
              <el-button  :type="query.button.css.date.yesterday" @click.native="queryByDate('yesterday')"  size="small">昨天</el-button>
              <el-button  :type="query.button.css.date.seven" @click.native="queryByDate('seven')"  size="small">最近7天</el-button>
              <el-button  :type="query.button.css.date.thirty" @click.native="queryByDate('thirty')"  size="small">最近30天</el-button>
              <el-button  :type="query.button.css.date.month" @click.native="queryByDate('month')"  size="small">本月</el-button>
              <el-button  :type="query.button.css.date.year" @click.native="queryByDate('year')"  size="small">本年</el-button>
              <el-button  :type="query.button.css.date.customer" @click.native="queryByDate('customer')"  size="small">自定义</el-button>
              <el-date-picker
                end-placeholder="截至日期"
                range-separator="至"
                size="mini"
                start-placeholder="开始日期"
                type="daterange"
                v-model="orderDate"
                v-show="query.button.showCustomer"
                value-format="yyyy-MM-dd">
              </el-date-picker>
            </el-col>
          </el-row>
          <br>
        </el-col>
        <el-col :span="4">
          <el-input placeholder="手机号" size="mini" v-model="listQuery.mobile"></el-input>
        </el-col>
        <el-col :span="4">
          <el-input placeholder="订单号" size="mini" v-model="listQuery.orderSn"></el-input>
        </el-col>
        <el-col :span="6">
          <el-button @click.native="search" icon="el-icon-search" size="mini" type="success">{{ $t('button.search') }}</el-button>
          <el-button @click.native="exportXls" icon="el-icon-document" size="mini" type="primary">{{ $t('button.export') }}</el-button>
          <el-button @click.native="reset" icon="el-icon-refresh" size="mini" type="primary">{{ $t('button.reset') }}</el-button>
        </el-col>
      </el-row>
    </div>

    <el-table :data="list" @current-change="handleCurrentChange" border element-loading-text="Loading" fit highlight-current-row
              v-loading="listLoading">
      <el-table-column label="用户">
        <template slot-scope="scope">
          <router-link :to="{path:'shopUserDetail?id='+scope.row.user.id}">
            {{scope.row.user.mobile}}
          </router-link>
        </template>
      </el-table-column>
      <el-table-column label="订单号">
        <template slot-scope="scope">
          <router-link :to="{path:'orderDetail?orderSn='+scope.row.orderSn}">
            {{scope.row.orderSn}}
          </router-link>
        </template>
      </el-table-column>
      <el-table-column label="创建日期">
        <template slot-scope="scope">
          {{scope.row.createTime}}
        </template>
      </el-table-column>
      <el-table-column label="订单状态">
        <template slot-scope="scope">
          {{scope.row.statusName}}
        </template>
      </el-table-column>
      <el-table-column label="支付状态">
        <template slot-scope="scope">
          {{scope.row.payStatusName}}
        </template>
      </el-table-column>
      <el-table-column label="支付方式">
        <template slot-scope="scope">
          {{scope.row.payTypeName}}
        </template>
      </el-table-column>
      <el-table-column label="订单备注">
        <template slot-scope="scope">
          {{scope.row.message}}
        </template>
      </el-table-column>
      <el-table-column label="总金额">
        <template slot-scope="scope">
          {{formatPrice(scope.row.totalPrice)}}
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-dropdown  size="small"   split-button type="primary">

              操作
            <el-dropdown-menu slot="dropdown">
              <!--<el-dropdown-item v-if="scope.row.statusName === '待付款'">修改订单</el-dropdown-item>-->
              <el-dropdown-item @click.native="addComment(scope.row.id)">订单备注</el-dropdown-item>
              <el-dropdown-item @click.native="viewShippingInfo(scope.row)" v-if="scope.row.statusName==='已发货'">物流信息</el-dropdown-item>
              <el-dropdown-item @click.native="openSendOutForm(scope.row.id)" v-if="scope.row.statusName === '待发货'">立即发货</el-dropdown-item>
              <el-dropdown-item v-if="scope.row.payStatusName === '已付款' && scope.row.statusName !== '已退款'&& scope.row.statusName !== '退款中'">立即退款</el-dropdown-item>
              <el-dropdown-item @click.native="viewLog(scope.row.id)">操作日志</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
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
      :visible.sync="logVisible"
      title="订单日志"
      width="40%">
      <el-table
        :data="logs"
        border
        stripe
        style="width: 100%">
        <el-table-column
          label="操作记录"
          prop="descript">
        </el-table-column>
        <el-table-column
          label="操作时间"
          prop="createTime">
        </el-table-column>
      </el-table>
    </el-dialog>
    <el-dialog
      :visible.sync="shipping.show"
      title="发货"
      width="40%">
      <el-form :model="shipping" label-width="200px"  ref="form">
        <el-row>
          <el-col :span="24">
            <el-form-item label="快递公司"  >
              <el-select placeholder="请选择" v-model="shipping.idExpress">
                <el-option
                  :key="item.id"
                  :label="item.name"
                  :value="item.id"
                  v-for="item in expressList">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="快递单号"  >
              <el-input minlength=1 v-model="shipping.shippingSn"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item>
          <el-button @click="sendOut" type="primary">{{ $t('button.submit') }}</el-button>
          <el-button @click.native="shipping.show = false">{{ $t('button.cancel') }}</el-button>
        </el-form-item>

      </el-form>
    </el-dialog>
    <el-dialog
      :visible.sync="shippingInfo.show"
      title="物流信息"
      width="80%">
      <el-form label-width="120px"   ref="form">
        <el-row>
          <el-col :span="24">
            <el-form-item label="快递公司"  >
              <el-select disabled   v-model="shippingInfo.form.idExpress">
                <el-option
                  :key="item.id"
                  :label="item.name"
                  :value="item.id"
                  v-for="item in expressList">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="快递单号"  >
              <el-input readOnly  v-model="shippingInfo.form.shippingSn"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="物流进度">
              <br>
              <el-timeline :reverse="true">
                <el-timeline-item
                  :key="index"
                  :timestamp="activity.acceptTime"
                  v-for="(activity, index) in shippingInfo.form.traces">
                  {{activity.acceptStation}}
                </el-timeline-item>
              </el-timeline>
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item>

          <el-button @click.native="shippingInfo.show = false">{{ $t('button.close') }}</el-button>
        </el-form-item>

      </el-form>
    </el-dialog>

  </div>
</template>

<script src="./order.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

