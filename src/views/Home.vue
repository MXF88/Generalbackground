<template>
<div>
  <el-container>
    <el-header>后台管理系统</el-header>
    <el-container>
      <el-aside width="200px">
        <el-menu router v-for="(item,index) in this.$router.options.routes" :key="index">
          <el-submenu index="1" v-if="item.children">
            <template slot="title"><i class="el-icon-s-order"></i>{{item.name}}</template>
              <el-menu-item :index="children.path" v-for="(children,indexj) in item.children" :key="indexj">{{children.name}}</el-menu-item>
            </el-submenu>
        </el-menu>
      </el-aside>
      <el-container>
        <el-main>
          <router-view/>
        </el-main>
      </el-container>
    </el-container>
  </el-container>
</div>
</template>

<script>
export default {
  name: 'Home'
}
</script>

<style scoped>

</style>
