<template>
<div id="app">
  <header>欧赔数据页面</header>
  <v-table></v-table>
</div>
</template>

<script>
import Table from './components/europdata/Table.vue'

export default {
  name: 'app',
  components: {
    'v-table': Table
  }
}
</script>

<style>
#app{
  width:1000px;
  margin:100px auto 0px;
}
header{
  margin-bottom: 50px;
  height:60px;
  line-height: 60px;
  text-align: center;
  font:  bold 42px '微软雅黑';
  color: rgba(150,80,60,0.8);
}
</style>
