<template>
  <div class='chart'>
    <div id='main'></div>

  </div>
</template>

<script>

export default {
  name: 'Chart',
  mounted() {
    this.showChart()
  },
  data() {
    return {
      // 指定图表的配置项和数据
      option: {
        //上方标题
        // title: {
        //     text: '数据统计',
        //     textStyle: {
        //         fontSize: 18,
        //         lineHeight: 50,
        //     }
        // },
        axisPointer: {
          lineStyle: {
            color: 'rgb(148 122 230)',
            width: 3,
            type: 'dashed',
            cap: 'butt',
            shadowBlur: 0,
            shadowOffsetX: 0,
            shadowOffsetY: 0
          }
        },

        //与div的距离
        grid: {
          left: '3%',
          right: '3%',
          top: '5%',
          bottom: '15%'
        },

        tooltip: {
          trigger: 'axis'
        },

        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: ['', '05-01', '05-02', '05-03', '05-04', '05-05', '05-06', '05-07', '05-08', '05-09', '05-10', '05-11', '05-12', '05-13'],
          splitLine:
            {
              show: true,
              lineStyle:
                {
                  type: 'dashed'
                }
            }
        },

        yAxis: {
          type: 'value',
          splitLine:
            {
              show: true,
              lineStyle:
                {
                  type: 'dashed'
                }
            }
        },

        series: [
          {
            data: [, 4, 5.8, 5.2, 5.9, 5.5, 6.5, 6, 9, 7.8, 6.1, 7, 6.2, 8.3, 5],
            type: 'line',
            smooth: true,
            colorBy: 'series',
            lineStyle: {
              width: 3,
              type: 'solid',
              shadowColor: 'black', //默认透明
              shadowBlur: 10,
              shadowOffsetX: 2,
              shadowOffsetY: 6,
              color: 'rgb(148 122 230)'

            },
            itemStyle: {
              color: 'rgb(148 122 230)'
            }


          }
        ]
      }
    }
  },
  methods: {
    showChart() {
      // 基于准备好的dom，初始化echarts实例
      var myChart = this.$echarts.init(document.getElementById('main'))

      // 使用刚指定的配置项和数据显示图表。
      myChart.setOption(this.option)
    }

  }

}
</script>

<style scoped>
.chart {
  display: flex;
  flex-direction: column;
  justify-content: center;
  /*flex-basis: 40%;*/
  margin-bottom: 2%;
  flex-shrink: 0;
  /*添加下面代码！！！*/
  /*width: 70.8vw;*/
  /*height: 30vh;*/
  /*flex-basis: 85%;*/
  background-color: white;
}

#main {
  /*width: 100%;*/
  /*height: 100%;*/
  /*修改为下面代码*/
  width: 74.5vw;
  height: 34vh;
}

/*增加下面代码*/
canvas {
  width: 92% !important;
  height: 34.1% !important;
  /*width: 69.5vw !important;*/
  /*height: 34.1vh !important;*/
}
</style>