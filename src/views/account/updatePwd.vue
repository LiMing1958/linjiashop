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
        <el-form :model="form" label-width="150px"   ref="form">

        <el-row>
          <el-col :span="24">
            <el-form-item label="原密码">
              <el-input type="password" v-model="form.oldPassword"  ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="新密码" prop="password">
              <el-input minlength=5 type="password"  v-model="form.password"  ></el-input>
            </el-form-item>
          </el-col>


          <el-col :span="24">
            <el-form-item label="重复密码">
              <el-input type="password" v-model="form.rePassword"  ></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item>
          <el-button @click="updatePwd" type="primary">{{ $t('button.submit') }}</el-button>
        </el-form-item>
        </el-form>
      </el-col>
    </el-row>


  </div>
</template>

<script src="./updatePwd.js"></script>


<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

