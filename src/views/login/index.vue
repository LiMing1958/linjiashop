<template>
  <div class="login-container"  style="background-image: url(img/bg.jpg);background-size: 100% 100%;">

    <el-form :model="loginForm" :rules="loginRules" auto-complete="on" class="login-form" label-position="left" ref="loginForm">

      <div class="title-container">
        <h3 class="title">{{ $t('login.title') }}</h3>
        <lang-select class="set-language"/>
      </div>

      <el-form-item prop="username">
        <span class="svg-container">
          <svg-icon icon-class="user" />
        </span>
        <el-input
          :placeholder="$t('login.username')"
          auto-complete="on"
          name="username"
          type="text"
          v-model="loginForm.username"
        />
      </el-form-item>

      <el-form-item prop="password">
        <span class="svg-container">
          <svg-icon icon-class="password" />
        </span>
        <el-input
          :placeholder="$t('login.password')"
          :type="pwdType"
          @keyup.enter.native="handleLogin"
          auto-complete="on"
          name="password"
          v-model="loginForm.password" />
        <span @click="showPwd" class="show-pwd">
          <svg-icon icon-class="eye" />
        </span>
      </el-form-item>

      <el-button :loading="loading" @click.native.prevent="handleLogin" style="width:100%;margin-bottom:30px;" type="info">{{ $t('login.logIn') }}</el-button>

      <div style="position:relative">
        <div class="tips">
          <span>{{ $t('login.username') }} : admin</span>
          <span>{{ $t('login.password') }} : admin</span>
        </div>
        <div class="tips">
          <span style="margin-right:18px;">{{ $t('login.username') }} : developer</span>
          <span>{{ $t('login.password') }} : developer</span>
        </div>
      </div>
    </el-form>


  </div>
</template>

<script src="./login.js"></script>
<style lang="scss" rel="stylesheet/scss" src="./login.scss"></style>
