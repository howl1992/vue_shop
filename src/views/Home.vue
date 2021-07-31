<template>
<div> <el-button  @click="exit" type="info">退出</el-button></div>

</template>

<script>
export default {
  name: 'home',
  methods: {
    exit () {
      window.sessionStorage.clear()
      this.$router.push('/login')
      this.$message.success('退出成功！')
    }
  }
}
</script>

<style lang="less" scoped>

</style>
