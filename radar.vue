<template>
  <div :id="idName" class="myRadar"></div>
</template>

<script>
  import echarts from 'echarts'
  export default {
    name: "radar",
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
       * return 处理过后的数据
       */
      handleArr(arr){
        let arrData = []
        arr.map((item) => {
          arrData.push(item.value)
        });
        return arrData
      }
    },
    mounted(){
      const vm = this;
      // myChart.showLoading();
      // myChart.hideLoading();
      const option = {
          color: ['#0C44E5'],
          title: {
            left:"center",
            text: vm.title,
            textStyle:{
              color:"#1E1E1E",
                fontSize:16
            }
          },
          tooltip: {
            confine: true,
            trigger: 'item',
            formatter: function (params) {
              // console.log(params)
              return params.name;
            }
          },
          legend: {},
          radar: {
              // shape: 'circle',
              center:["50%","60%"],
              name: {
              textStyle: {
                  color: '#1E1E1E',
                  fontSize: 11
                }
              },
              triggerEvent: true,
              radius: '60%',
              indicator: vm.data,
              splitArea:{
                  show : true,   
                  areaStyle : {
                      color: ["#fff"]  // 图表背景网格的颜色
                  }
              }
          },
          series: [{
              type: 'radar',
              symbol: 'circle',
               areaStyle: {normal: {
                color: 'rgba(64, 113, 255, .6)'
                }
              },
              // areaStyle: {normal: {}},
              data: [
                vm.handleArr(vm.data)
              ]
          }]
      };
      const chartObj = echarts.init(document.getElementById(vm.idName));
      chartObj.setOption(option)
    }
  }
</script>
<style lang="scss" scoped>
  .myRadar{
    width: 100%;
    height: 100%;
  }
</style>