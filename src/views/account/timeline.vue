<template>
  <div class="app-container">
    <div class="user">
      <strong>{{ user.name }}</strong><br>
      <small>{{ user.dept }} &nbsp;&nbsp; {{ user.roles }}</small>
    </div>
    <el-row class="user-content" style="">
      <el-col :span="6" class="profile">
        <img class="user-avatar" src="@/assets/img/avatar.gif"><br>
        <p><span class="title"><i class="el-icon-phone"></i>&nbsp;&nbsp;{{ user.phone }}</span></p>
        <p><span class="title"><i class="el-icon-message"></i>&nbsp;&nbsp;{{ user.email }}</span></p>
        <p><span class="title"><i class="el-icon-open"></i>&nbsp;&nbsp;{{ user.status == '1'?'启用' : '禁用' }}</span></p>
        <p><span class="title"><i class="el-icon-location-outline"></i>&nbsp;&nbsp;上海市浦东大道290弄</span></p>
      </el-col>
      <el-col :span="18" style="padding-left:10px;">
        <el-tabs @tab-click="handleClick" v-model="activeName">
          <el-tab-pane label="个人资料" name="profile"></el-tab-pane>
          <el-tab-pane label="最近活动" name="timeline"></el-tab-pane>
          <el-tab-pane label="修改密码" name="updatePwd"></el-tab-pane>
        </el-tabs>
        <el-timeline :reverse="reverse">
          <el-timeline-item
            :key="index"
            :timestamp="activity.createTime"
            v-for="(activity, index) in activities">
            {{activity.logname}}
          </el-timeline-item>
        </el-timeline>
      </el-col>
    </el-row>


  </div>
</template>


<script src="./timeline.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

