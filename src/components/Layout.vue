<template>
  <div class="container">
      <el-row type="flex" justify="center" align="middle" class="main">
          <el-col :span="20">
            <slot></slot>
          </el-col>
      </el-row>
  </div>
</template>
<script>
export default {
  name: 'Layout'
}
</script>
<style>
.container {
  height: 100%;
}
.main {
  height: 80%;
}
</style>