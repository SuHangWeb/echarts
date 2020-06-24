<template>
  <div id="myGraph" class="myGraph"></div>
</template>

<script>
  import echarts from 'echarts'
  export default {
    name: "Graph",
    props: {
      //节点数据
      nodes: {
        type: Array,
        default () {
          return []
        }
      },
      //节点关系
      links: {
        type: Array,
        default () {
          return []
        }
      },
      //节点分类属性
      sort: {
        type: Array,
        default () {
          return []
        }
      }
    },
    data () {
      return {
         color: ['#c23531','#2f4554', '#61a0a8', '#d48265', '#91c7ae','#749f83',  '#ca8622', '#bda29a','#6e7074', '#546570', '#c4ccd3']
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
                  text: '关系图谱'
               },
               toolbox: {
                  feature: {
                     saveAsImage: {
                        title:" ",
                        excludeComponents:['toolbox','legend']
                     },
                  }
               },
               color: vm.color,
               legend:{
                  data: vm.handleArr(vm.sort)
               },
               tooltip: {
                 trigger: 'item',
                 formatter: function (params) {
                  return params.data.role?`<div><span style="margin-right:5px;display:inline-block;width:10px;height:10px;border-radius:5px;background-color:${params.color};"></span>角色：${params.data.role}</div>`:''; 
                }
               },
               animationDurationUpdate: 1500,
               animationEasingUpdate: 'quinticInOut',
               symbolSize: function (value, params) {//改变节点大小
                  var SizeList = [40, 45, 50, 55, 60];
                  return SizeList[params.dataIndex]
               },
               label: {
                  normal: {
                  show: true,
                  position: 'bottom',
                  rich: {
                     bg: {
                        backgroundColor: '#f5f5f5'
                        }
                     }
                  }
               },
               //https://www.cnblogs.com/koala2016/archive/2016/12/01/6123003.html
               series: [
                  {
                    legendHoverLink:true,//hover 时的联动高亮。
                    zoom:1,//缩放比
                    draggable: true,//单个节点拖拽
                    type: 'graph',
                    layout: 'none',//none 不使用任何布局  circular 环形布局   circular引力布局
                    symbolSize: 50,
                    roam: true,
                    label: {
                        show: true
                    },
                    edgeSymbol: ['circle', 'arrow'], //箭头
                    edgeSymbolSize: [4, 10],//箭头大小
                    edgeLabel: {
                        fontSize: 14,
                        normal:{
                          show:true,
                          formatter:function(params){
                            return params.data.relation?params.data.relation:''
                          }
                        }
                    },
                    categories: vm.sort,
                    data: vm.nodes,
                    links: vm.links,
                    itemStyle: {
                      //  normal: {
                      //     color: function (params) {
                      //        return vm.color[params.dataIndex]
                      //     },
                      //  },
                      shadowColor: 'rgba(0, 0, 0, 0.5)',
                      shadowBlur: 10
                    },
                     xAxisIndex : 0, //x轴坐标 有多种坐标系轴坐标选项
                    yAxisIndex : 0, //y轴坐标 
                    force : { //力引导图基本配置
                        //initLayout: ,//力引导的初始化布局，默认使用xy轴的标点
                        repulsion : 120,//节点之间的斥力因子。支持数组表达斥力范围，值越大斥力越大。
                        gravity : 0.02,//节点受到的向中心的引力因子。该值越大节点越往中心点靠拢。
                        edgeLength :120,//边的两个节点之间的距离，这个距离也会受 repulsion。[10, 50] 。值越小则长度越长
                        layoutAnimation : true
                    //因为力引导布局会在多次迭代后才会稳定，这个参数决定是否显示布局的迭代动画，在浏览器端节点数据较多（>100）的时候不建议关闭，布局过程会造成浏览器假死。                        
                    },
                    lineStyle: {
                        opacity: 0.9,
                        width: 2,
                        curveness: 0.2
                    }
                  }
               ]
          }
      const chartObj = echarts.init(document.getElementById('myGraph'));
      chartObj.setOption(option)
    }
  }
</script>
<style lang="scss" scoped>
  .myGraph{
    width: 100%;
    height: 100%;
  }
</style>