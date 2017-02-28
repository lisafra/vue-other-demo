<template lang="html">
  <table class="common-table compute-sum" border="1" width="1000">
    <tbody v-if="dataCompute">
      <tr>
        <td width="13%" rowspan="3" style="">共[<span>{{dataCompute.companyTotal}}</span>]家公司</td>
        <td v-for="item in dataCompute.avg" width="6%">{{item}}</td>
      </tr>
      <tr>
        <td v-for="item in dataCompute.max" width="6%">{{item}}</td>
      </tr>
      <tr>
        <td v-for="item in dataCompute.min" width="6%">{{item}}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    dataCompute: {
    }
  }
}
</script>

<style lang="css">
  .compute-sum{
    position: fixed;
    bottom:0;
    background-color: #e8e8e8;
  }
  .compute-sum td{
    text-align: center;
    vertical-align: middle;
  }

</style>
