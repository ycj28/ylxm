<template>
   <div class="data-view">
      <el-card>
         <div id="main1"></div>

      </el-card><el-card>
         <div id="main2"></div>

      </el-card>
   </div>
</template>
<script>
import { dataview } from '../../api/api.js'
export default {
   data () {
      return {

      }
   },
   created () {
      // 第二个图表的绘制
      dataview().then(res => {
         console.log(res)
         if (res.data.status === 200) {
            let { legend, xAxis, series } = res.data.data
            this.draw(legend, xAxis, series)
         }
      }).catch(err => {
         throw err
      })
   },
   mounted () {
      let myChart = this.$echarts.init(document.getElementById('main1'))
      myChart.setOption({
         title: {
            text: '各宿舍区域人数',
         },
         tooltip: {},
         xAxis: {
            data: ['北区', '南区', '中区', '公寓', '大家乐']
         },
         yAxis: {

         },
         series: [{
            name: '人数',
            type: 'bar',
            data: [45, 42, 46, 48, 52]
         }]
      })
   },
   methods: {
      draw (legend, xAxis, series) {
         let myChart1 = this.$echarts.init(document.getElementById('main2'))
         let option = {
            title: {
               text: '会话量'
            },
            tooltip: {
               trigger: 'axis'
            },
            legend: {
               data: legend
            },
            xAxis: {
               type: 'category',
               data: xAxis
            },
            yAxis: {
               type: 'value'
            },
            series: series
         }
         myChart1.setOption(option)
      }
   }
}
</script>
<style lang="scss" scoped>
.data-view {
   width: 100%;
   display: flex;
   justify-content: space-between;

   .el-card {
      width: 50%;

      #main1,
      #main2 {
         height: 500px;
      }
   }
}
</style>