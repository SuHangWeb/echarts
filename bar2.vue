<template>
  <div :id="idName" class="myBar"></div>
</template>

<script>
  import echarts from 'echarts'
  export default {
    name: "bar",
    props: {
      //节点id
      idName: {
        type: String,
        default () {
          return ""
        }
      },
      //title
      title: {
        type: String,
        default () {
          return ""
        }
      },
      //数据
      data: {
        type: Array,
        default () {
          return []
        }
      },
    },
    data () {
      return {
      }
    },
    created(){
      
    },
    methods:{
      /**
       * 处理数据
       * arr 原始数据
       * str 字段
       * return 处理过后的数据
       */
      handleArr(arr,str){
        let arrData = []
        arr.map((item) => {
          arrData.push(item[str])
        });
        return arrData
      }
    },
    mounted(){
      const vm = this;
      // myChart.showLoading();
      // myChart.hideLoading();
      const option =  {
          title: {
            left:"center",
            text: vm.title,
            textStyle:{
              color:"#1E1E1E",
                fontSize:16
            }
          },
          grid: {
            left: "15%",
            top: "28%",
            right: "12%",
            bottom: "22%"
          },
          // tooltip: {
          //   trigger: 'axis',
          //   axisPointer: {            // 坐标轴指示器，坐标轴触发有效
          //     type: 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
          //   },
          //   confine: true
          // },
          xAxis: [{
            type: 'value',
            axisLabel: {show:false},
            axisTick:{show:false},
            splitLine: {show: false},
            axisLine: {show:false}
          }],
          yAxis: [{
            type: 'category',
            inverse:true,
            // name: '各级风险数 (次)',
            nameLocation: 'start',
            axisLabel: {
              color: '#1E1E1E',
              fontSize: 12
            },
            axisTick:{show:false},
            splitLine: {show: false},
            axisLine: {show: false},
            data: vm.handleArr(vm.data,'name')
          }],
          series: [{
            name: '',
            type: 'bar',
            data:  vm.handleArr(vm.data,'value'),
            barWidth: 26,
            itemStyle: {
              color: function (params) {
                let colorList = [
                  {
                    type: 'linear',
                    x: 0,
                    x1: 1,
                    colorStops: [
                      {
                        offset: 0,
                        color: '#C93D3D'
                      },
                      {
                        offset: 1,
                        color: 'rgba(201,61,61,.8)'
                      }
                    ]
                  },
                  {
                    type: 'linear',
                    x: 0,
                    x1: 1,
                    colorStops: [
                      {
                        offset: 0,
                        color: '#F0A25F'
                      },
                      {
                        offset: 1,
                        color: 'rgba(240,142,95,.8)'
                      }
                    ]
                  },
                  {
                    type: 'linear',
                    x: 0,
                    x1: 1,
                    colorStops: [
                      {
                        offset: 0,
                        color: '#4071FF'
                      },
                      {
                        offset: 1,
                        color: 'rgba(64, 113, 255, .8)'
                      }
                    ]
                  }]
                return colorList[params.dataIndex]
              },
            },
            label: {
              normal: {
                show: true,
                color: function (params) {
                  let colorList = ['#C93D3D', '#F0A25F', '#4071FF']
                  return colorList[params.dataIndex]
                },
                // textStyle: {color: '#fff'},
                position: "right",
                fontSize: 13,
                fontWeight: 'bold',
                // fontFamily: 'DINPro-Medium'
              }
            }
          }
          ]
        };
      const chartObj = echarts.init(document.getElementById(vm.idName));
      chartObj.setOption(option)
    }
  }
</script>
<style lang="scss" scoped>
  .myBar{
    width: 100%;
    height: 100%;
  }
</style>