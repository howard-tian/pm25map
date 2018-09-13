<template>
  <div class="home">
    <div class="map">
        <div id="china-map"></div>
    </div>
  </div>
</template>

<script>

let echarts = require('echarts/lib/echarts')
// 散点图
require('echarts/lib/chart/scatter')
// 散点图放大
require('echarts/lib/chart/effectScatter')
// 地图
require('echarts/lib/chart/map')
// 图例
require('echarts/lib/component/legend')
// 提示框
require('echarts/lib/component/tooltip')
// 地图geo
require('echarts/lib/component/geo')
// 中国地图
require('echarts/map/js/china')


export default {
  data: function() {
      return {
          timer: '',
          chinaMap: null
      }
  },
  methods: {
    timerFunc: function() {
        this.displayMap();
    },
    displayMap() {
      if(!this.chinaMap) {
        this.chinaMap = echarts.init(document.getElementById('china-map'));
      }
      console.log(this.chinaMap);

      let showLoadingDefault = {
        text: '加载中...',
        color: '#23531',
        textColor: '#fff',
        // 地图背景色
        maskColor: '#272D3A',
        zlevel: 2
      }
      this.chinaMap.showLoading(showLoadingDefault);
      
      console.log('start to set option');

      let options = this.chinaMap.getOption();
      console.log(options);

      this.chinaMap.hideLoading();
      this.chinaMap.setOption({
        backgroundColor: '#272D3A',
        // 标题
        title: {
          text: 'PM2.5',
          left: 'center',
          textStyle: {
            color: '#fff'
          }
        },
        // 地图上圆点的提示
        tooltip: {
          trigger: 'item',
          formatter: function (params) {
            return params.name + ' : ' + params.value[2]
          }
        },
        // 图例按钮 点击可选择哪些不显示
        legend: {
          orient: 'vertical',
          left: 'left',
          top: 'bottom',
          data: ['地区热度', 'top5'],
          textStyle: {
            color: '#fff'
          }
        },
        // 地理坐标系组件
        geo: {
          map: 'china',
          label: {
            // true会显示城市名
            emphasis: {
              show: true
            }
          },
          itemStyle: {
            // 地图背景色
            normal: {
              areaColor: '#465471',
              borderColor: '#282F3C'
            },
            // 悬浮时
            emphasis: {
              areaColor: '#8796B4'
            }
          }
        },
        // 系列列表
        series: [
          {
            name: '地区热度',
            // 表的类型 这里是散点
            type: 'scatter',
            // 使用地理坐标系，通过 geoIndex 指定相应的地理坐标系组件
            coordinateSystem: 'geo',
            data: [],
            // 标记的大小
            symbolSize: 18,
            // 鼠标悬浮的时候在圆点上显示数值
            label: {
              normal: {
                show: false
              },
              emphasis: {
                show: false
              }
            },
            itemStyle: {
              normal: {
                color: '#ddb926'
              },
              // 鼠标悬浮的时候圆点样式变化
              emphasis: {
                borderColor: '#fff',
                borderWidth: 1
              }
            },
            zlevel: 10,
            markPoint : {
                symbol:'emptyCircle',
                symbolSize : 18,
                effect : {
                    show: true,
                    shadowBlur : 0
                },
                itemStyle:{
                    normal:{
                        label:{show:false}
                    }
                },
                data: [
                  {name: "上海",coord: [121.48, 31.22]},
                  {name: "合肥", coord: [117.27,31.86]},
                  {name: "武汉", coord: [114.31,30.52]},
                  {name: "大庆", coord: [125.03,46.58]},
                  {name: '海门', coord: [121.15, 31.89, 90]},
                  {name: '鄂尔多斯', coord: [109.781327, 39.608266, 120]},
                  {name: '招远', coord: [120.38, 37.35, 142]},
                  {name: '舟山', coord: [122.207216, 29.985295, 123]}
                ]
              }
          },
          {
            name: 'top5',
            // 表的类型 这里是散点
            type: 'effectScatter',
            // 使用地理坐标系，通过 geoIndex 指定相应的地理坐标系组件
            coordinateSystem: 'geo',
            data: [],
            // 标记的大小
            symbolSize: 18,
            showEffectOn: 'render',
            rippleEffect: {
              brushType: 'stroke'
            },
            hoverAnimation: true,
            label: {
              normal: {
                show: false
              }
            },
            itemStyle: {
              normal: {
                color: '#f4e925',
                shadowBlur: 10,
                shadowColor: '#333'
              }
            },
            zlevel: 20,
            markPoint : {
              symbolSize : 32,
                data: [
                  {name: "大庆", coord: [125.03,46.58]},
                  {name: '海门', coord: [121.15, 31.89, 90]},
                  {name: '鄂尔多斯', coord: [109.781327, 39.608266, 120]},
                  {name: '招远', coord: [120.38, 37.35, 142]},
                  {name: '舟山', coord: [122.207216, 29.985295, 123]}
                ]
            }
          }
        ]
      });

      console.log('set option finish');
    }
  },
  mounted () {
    this.displayMap();
    this.timer = setInterval(this.timerFunc, 3000);
  },
  beforeDestroy() {
    clearInterval(this.timer)
  },
}
</script>


<style lang="less" scoped>
.home {
  text-align: center;
}
.map {
  height: 720px;
  width: 100%;

  > div {
    height: 100%;
  }
}
</style>
