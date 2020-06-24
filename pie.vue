<template>
  <div :id="idName" class="myPit"></div>
</template>

<script>
  import echarts from 'echarts'
  export default {
    name: "pie",
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
       //位置
      center: {
        type: Array,
        default () {
          return ['50%', '50%']
        }
      },
      //legend位置
      position: {
        type: Array,
        default () {
          return ['middle', 'middle']
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
          arrData.push(item.name)
        });
        return arrData
      }
    },
    mounted(){
      const vm = this;
      // myChart.showLoading();
      // myChart.hideLoading();
      const option = {
          title: {
            text: vm.title,
            left: 'center',
            textStyle:{
              fontSize: 16,
              fontWeight:"normal",
              color:"#333"
            }
          },
          tooltip: {
              trigger: 'item',
              formatter: '{a} <br/>{b} : {c} ({d}%)'
          },
          legend: {
              orient: 'vertical',
              left: vm.position[0],
              top: vm.position[1],
              data: vm.handleArr(vm.data),
              icon: "rect",   // 这个字段控制形状  类型包括 circle，rect ，roundRect，triangle，diamond，pin，arrow，none
              itemWidth: 6,  // 设置宽度
              itemHeight: 6, // 设置高度
              // itemGap: 40 // 设置间距
          },
          series: [
              {
                  // name: '访问来源',
                  name: vm.title,
                  type: 'pie',
                  radius: '70%',
                  center: vm.center,
                  data: vm.data,
                  label: {
                    normal: {
                      position: 'inner',
                      show : false
                    }
                  },
                  emphasis: {
                      itemStyle: {
                          shadowBlur: 10,
                          shadowOffsetX: 0,
                          shadowColor: 'rgba(0, 0, 0, 0.5)'
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
  .myPit{
    width: 100%;
    height: 100%;
  }
</style>