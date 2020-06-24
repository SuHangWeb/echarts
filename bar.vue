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
        color: ['#3398DB'],
        grid: {
            left: "15%",
            top: "28%",
            right: "12%",
            bottom: "22%"
          },
        xAxis: {
            type: 'category',
             axisLabel:{
              interval: 0,
              color: '#1E1E1E',
              rotate: 25,
              fontSize: 10,
              margin: 10
            },
            triggerEvent: true,
            data: vm.handleArr(vm.data,'name'),
            axisTick:{
              show:false,
              alignWithLabel: true
            },
            axisLine:{
              lineStyle: {
                color: '#78787A'
              }
            }, 
        },
         yAxis: [
            {
              type: 'value',
              axisTick: {show: false},
              axisLine: {show: false},
              name: '次',
              nameLocation: 'end',
              nameTextStyle: {
                color: '#969696'
              },
              axisLabel: {
                color: '#1E1E1E',
                fontSize: 10
              }
            }
          ],
        series: [
            {
                type: 'bar',
                barWidth: '38%',
                data:  vm.handleArr(vm.data,'value'),
                itemStyle: {
                    color: {
                      type: 'linear',
                      x: 0,
                      x1: 0,
                      y: 1,
                      y1: 0,
                      colorStops: [
                        {
                          offset: 0,
                          color: '#3051f1'
                        },
                        {
                          offset: 1,
                          color: '#4c90ff'
                        }
                      ]
                    }
                },
                label: {
                  normal: {
                    show: true,
                    color: '#4071FF',
                    // textStyle: {color: '#fff'},
                    position: "top",
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