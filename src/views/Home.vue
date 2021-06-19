<template>
  <Layout>
    <el-button type="primary">冒泡排序</el-button>
    <el-button type="success">插入排序</el-button>
    <el-button type="info">选择排序</el-button>
    <el-button type="warning">归并排序</el-button>
    <el-button type="danger">快速排序</el-button>
  </Layout>
</template>
<script>
import Layout from "@/components/Layout";
export default {
  name: 'Home',
  components: {Layout}
}
</script>