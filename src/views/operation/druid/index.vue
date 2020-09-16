<template>
  <div class="app-container">
    <div class="block">
      <iframe frameborder="0" height="768px" scrolling="auto" src="http://linjiashop-admin-api.microapp.store/druid/sql.html" width="100%"></iframe>


    </div>

  </div>
</template>

<script>
  export default {
    created: function () {
      this.init()
    },
    methods: {
      init() {
        this.$notify({
          title: '提示',
          message: '用户名密码参考：application.properties',
          duration: 3000
        })
      },
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss" scoped>
  @import "src/styles/common.scss";
</style>

