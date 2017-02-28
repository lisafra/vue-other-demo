<template lang="html">
  <div>
    <table class="common-table" width="1000">
      <thead>
        <tr>
            <th width='6%'>筛选</th>
            <th width='16%'>百家欧指</th>
            <th colspan="4" width='28%'>最新指数</th>
            <th colspan="3" width='18%'>最新概率</th>
            <th colspan="3" width='18%'>最新凯利指数</th>
            <th colspan="2" width='12%'>赔付率</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td v-for="item in tableTitle" width="item.width">{{item.text}}</td>
        </tr>
        <tr v-for="items in selectData">
          <td width="tableTitle[0].width">{{items.index}}</td>
          <td width="tableTitle[1].width">{{items.ProviderName}}</td>
          <td width="tableTitle[2].width">{{items.End.home}}</td>
          <td width="tableTitle[3].width">{{items.End.draw}}</td>
          <td width="tableTitle[4].width">{{items.End.away}}</td>
          <td width="tableTitle[5].width">{{items.Updatetime}}</td>
          <td width="tableTitle[6].width">{{items.Radio.home}}</td>
          <td width="tableTitle[7].width">{{items.Radio.draw}}</td>
          <td width="tableTitle[8].width">{{items.Radio.away}}</td>
          <td width="tableTitle[9].width">{{items.Kelly.home}}</td>
          <td width="tableTitle[10].width">{{items.Kelly.draw}}</td>
          <td width="tableTitle[11].width">{{items.Kelly.away}}</td>
          <td width="tableTitle[12].width">{{items.Payoff}}</td>
          <td width="tableTitle[13].width">详情</td>
        </tr>
      </tbody>
    </table>
    <table-sum :dataCompute="dataCompute"></table-sum>
  </div>
</template>

<script>
import dataSum from './sum.vue'

// 求数组最大值的方法
var fnArrMax = function (arr) {
  return Math.max.apply(null, arr)
}

// 求数组最小值的方法
var fnArrMin = function (arr) {
  return Math.min.apply(null, arr)
}

// 求数组平均值的方法
var fnArrAvg = function (arr) {
  let sum = 0
  arr.map((v, k) => {
    sum += parseInt(arr[k])
  })
  return parseFloat(sum / arr.length).toFixed(2)
}

const ERR_OK = 0

export default {
  data () {
    return {
      tableTitle: [{
        width: '6%',
        text: '序'
      }, {
        width: '16%',
        text: '公司名'
      }, {
        width: '7%',
        text: '主'
      }, {
        width: '7%',
        text: '平'
      }, {
        width: '7%',
        text: '客'
      }, {
        width: '7%',
        text: '更新'
      }, {
        width: '6%',
        text: '主'
      }, {
        width: '6%',
        text: '平'
      }, {
        width: '6%',
        text: '客'
      }, {
        width: '6%',
        text: '主'
      }, {
        width: '6%',
        text: '平'
      }, {
        width: '6%',
        text: '客'
      }, {
        width: '6%',
        text: '值'
      }, {
        width: '6%',
        text: '查看'
      }],
      resourceData: {},
      selectData: {},
      dataCompute: {}
    }
  },
  created () {
    this.$http.get('/api/records').then(response => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.resourceData = response.data

        // 数据筛选与重构
        this.resourceData.map((v, k) => {
          v.index = k + 1
          v.isShow = true
          this.selectData = this.resourceData
        })

        // 数据计算
        var companyTotal = 0
        var endHomeArr = []
        var endDrawArr = []
        var endAwayArr = []
        var UpdatetimeArr = []
        var RadioHomeArr = []
        var RadioDrawArr = []
        var RadioAwayArr = []
        var KellyHomeArr = []
        var KellyDrawArr = []
        var KellyAwayArr = []
        var PayoffArr = []

        this.selectData.map((v, k) => {
          companyTotal++
          endHomeArr.push(v.End.home)
          endDrawArr.push(v.End.draw)
          endAwayArr.push(v.End.away)
          UpdatetimeArr.push(v.Updatetime)
          RadioHomeArr.push(v.Radio.home)
          RadioDrawArr.push(v.Radio.draw)
          RadioAwayArr.push(v.Radio.away)
          KellyHomeArr.push(v.Kelly.home)
          KellyDrawArr.push(v.Kelly.draw)
          KellyAwayArr.push(v.Kelly.away)
          PayoffArr.push(v.Payoff)
        })

        this.dataCompute.companyTotal = companyTotal
        this.dataCompute.max = ['最大值']
        this.dataCompute.min = ['最小值']
        this.dataCompute.avg = ['平均值']
        this.dataCompute.width = ['6%']

        this.tableTitle.map((v, k) => {
          if (k > 1) this.dataCompute.width.push(v.width)
        })

        let sumTotal = [
          endHomeArr, endDrawArr, endAwayArr, UpdatetimeArr, RadioHomeArr, RadioDrawArr, RadioAwayArr, KellyHomeArr, KellyDrawArr, KellyAwayArr, PayoffArr
        ]

        sumTotal.map((v, k) => {
          this.dataCompute.max.push(fnArrMax(v))
          this.dataCompute.min.push(fnArrMin(v))
          this.dataCompute.avg.push(fnArrAvg(v))
        })

        console.log(this.dataCompute)
      }
    })
  },
  components: {
    'table-sum': dataSum
  }
}
</script>
<style lang="css">
.common-table{
  border: 2px solid #6D9BE0;
}
.common-table thead tr {
  height:40px;
  text-align: center;
  font-size: 14px;
  background-color: #6D9BE0;
  color:#fff;
}
.common-table th , .common-table td{
  vertical-align: middle;
}
.common-table td{
  height:30px;
  text-align: center;
  font-size: 12px;
  border-left: 1px solid #6D9BE0;
  border-bottom: 1px solid #6D9BE0;
}

</style>
