<template>
  <div id="show">
    <!-- panel -->
    <div class="show-container">
      <el-card class="show-panel bgcolor1">
        <div class="show-content">
          <span class="show-text">管理者</span>
          <div class="show-number">4</div>
        </div>
      </el-card>
      <el-card class="show-panel bgcolor2">
        <div class="show-content">
          <span class="show-text">角色</span>
          <div class="show-number">2</div>
        </div>
      </el-card>
      <el-card class="show-panel bgcolor3">
        <div class="show-content">
          <span class="show-text">岗位</span>
          <div class="show-number">4</div>
        </div>
      </el-card>
      <el-card class="show-panel bgcolor4">
        <div class="show-content">
          <span class="show-text">其他</span>
          <div class="show-number">0</div>
        </div>
      </el-card>
    </div>
    <!-- card -->
    <div class="card-content">
      <el-card class="card-echart">
        <!-- eCharts -->
        <div id="chart" :style="{ 'width': width, 'height': height }" />
      </el-card>
      <el-card class="card-list">
        <span>{{ nowDate }}1°C 阴</span>
      </el-card>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import { formatTime } from '@/utils/index.js'

export default {
  name: 'Dashboard',
  data() {
    return {
      width: '750px',
      height: '400px'
    }
  },
  computed: {
    ...mapGetters([
      'name'
    ]),
    nowDate() {
      return formatTime(new Date())
    }
  },
  mounted() {
    this.getChartsData()
  },
  methods: {
    getChartsData() {
      const charts = document.getElementById('chart')
      if (charts) {
        const myChart = this.$echarts.init(charts)
        const option = {
          backgroundColor: '#0E204A',
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              lineStyle: {
                color: {
                  type: 'linear',
                  x: 0,
                  y: 0,
                  x2: 0,
                  y2: 1,
                  colorStops: [
                    {
                      offset: 0,
                      color: 'rgba(255,255,255,0)' // 0% 处的颜色
                    },
                    {
                      offset: 0.5,
                      color: 'rgba(255,255,255,1)' // 100% 处的颜色
                    },
                    {
                      offset: 1,
                      color: 'rgba(255,255,255,0)' // 100% 处的颜色
                    }
                  ],
                  global: false // 缺省为 false
                }
              }
            }
          },
          grid: {
            top: '18%',
            left: '1%',
            right: '1%',
            bottom: '25%'
            // containLabel: true
          },
          xAxis: [
            {
              type: 'category',
              boundaryGap: true,
              axisLine: {
                // 坐标轴轴线相关设置。数学上的x轴
                show: true,
                lineStyle: {
                  color: 'rgba(255,255,255,0.4)'
                }
              },
              axisLabel: {
                // 坐标轴刻度标签的相关设置
                textStyle: {
                  color: '#d1e6eb',
                  margin: 15
                }
              },
              axisTick: {
                show: false
              },
              data: ['2018', '2019', '2020', '2021', '2022', '2023']
            }
          ],
          yAxis: [
            {
              type: 'value',
              min: 0,
              // max: 140,
              splitNumber: 4,
              splitLine: {
                show: true,
                lineStyle: {
                  color: 'rgba(255,255,255,0.1)'
                }
              },
              axisLine: {
                show: false
              },
              axisLabel: {
                show: false,
                margin: 20,
                textStyle: {
                  color: '#d1e6eb'
                }
              },
              axisTick: {
                show: false
              }
            }
          ],
          series: [
            {
              name: '注册总量',
              type: 'line',
              // smooth: true, //是否平滑曲线显示
              // 			symbol:'circle',  // 默认是空心圆（中间是白色的），改成实心圆
              showAllSymbol: true,
              // symbol: 'image://./static/images/guang-circle.png',
              symbolSize: 8,
              lineStyle: {
                normal: {
                  color: '#53fdfe' // 线条颜色
                },
                borderColor: '#f0f'
              },
              label: {
                show: true,
                position: 'top',
                textStyle: {
                  color: '#fff'
                }
              },
              itemStyle: {
                normal: {
                  color: 'rgba(255,255,255,1)'
                }
              },
              tooltip: {
                show: false
              },
              areaStyle: {
                // 区域填充样式
                normal: {
                  // 线性渐变，前4个参数分别是x0,y0,x2,y2(范围0~1);相当于图形包围盒中的百分比。如果最后一个参数是‘true’，则该四个值是绝对像素位置。
                  color: this.$echarts.graphic.LinearGradient(
                    0,
                    0,
                    0,
                    1,
                    [
                      {
                        offset: 0,
                        color: 'rgba(0,150,239,0.3)'
                      },
                      {
                        offset: 1,
                        color: 'rgba(0,253,252,0)'
                      }
                    ],
                    false
                  ),
                  shadowColor: 'rgba(53,142,215, 0.9)', // 阴影颜色
                  shadowBlur: 20 // shadowBlur设图形阴影的模糊大小。配合shadowColor,shadowOffsetX/Y, 设置图形的阴影效果。
                }
              },
              data: [150, 200, 259, 360, 378, 450, 450]
            }
          ]
        }
        myChart.setOption(option)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.show {
  &-container {
    margin: 30px;
    display: flex;
    justify-content: space-evenly;
  }
  &-content {
    margin: 0 0 0 80px;
  }
  &-panel {
    width: 300px;
    height: 120px;
    cursor: default;
    border-radius: 10px;
  }
  &-text {
    font-weight: 700;
    font-size: 14px;
    color: #fff;
  }
  &-number {
    color: #fff;
    font-size: 36px;
    line-height: 30px;
    font-family: Bahnschrift;
    margin-top: 15px;
  }
}
.card {
  &-content {
    display: flex;
  }
  &-echart {
    width: 800px;
    margin: 0 30px;
    display: flex;
    justify-content: center;
  }
  &-list {
      width: 500px;
  }
}

.bgcolor1 {
  background: #a8edea;
}
.bgcolor2 {
  background: #a1c4fd;
}
.bgcolor3 {
  background: #4facfe;
}
.bgcolor4 {
  background: #66a6ff;
}
</style>
