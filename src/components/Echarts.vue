<template>
 <div id="myEcharts" class="EchartsStyle"></div>
</template>

<script>
export default {
  data () {
    return {

    }
  },
  mounted () {
    this.deawLine()
  },
  methods: {
    deawLine () {
      // 初始化Echarts实例
      let myCharts = this.$echarts.init(document.getElementById('myEcharts'))

      // 绘制图表
      myCharts.setOption({
        title: {text: '在vue中使用Echarts'},
        tooltip: {
          // trigger: 'axis'
        },
        xAxis: {
          data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
        },
        yAxis: {},
        series: [{
          name: '销量',
          type: 'bar',
          data: [5, 20, 36, 10, 10, 20]
        }]

      })
    }
  }
}

</script>
<style>
.EchartsStyle{
  width: 100%;
  height: 600px;
}
</style>
