<!-- echarts模板，有局部刷新需要在mounted自行开启 -->
<template>      
  <div ref="refChart" :style="{ height: kHOne + 'px'}"></div>
</template>

<script>
require("../../../assets/theme/shine")
// import { get24HoursData1 } from "../../../api/ems/index";
export default {
name: '',
components: {},
data () {
return {
  // 获取浏览器可视区域高度（包含滚动条）、 window.innerHeight
  // 获取浏览器可视区域高度（不包含工具栏高度）、document.documentElement.clientHeight
  // 获取body的实际高度  (三个都是相同，兼容性不同的浏览器而设置的) document.body.clientHeight
  screenHeight: window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight,
  screenWidth: window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth,
  chartInstance: null,
  allData1: [],
  allData2: [],
  allData3: [],
  allData4: [],
  kHOne: null,
  // 自适应浏览器获取宽高大小定时器
  koiTime: null,
  // 局部刷新定时器    
  koiTimer: null,    
}
},
created () {},
beforeMount(){
  this.kHOne = Math.round(this.screenHeight * 0.31)
},
mounted(){
  // 页面大小改变时触发
  window.addEventListener('resize',this.getScreenHeight, false);
  // 页面大小改变时触发
  window.addEventListener('resize',this.getScreenWidth, false);
  // 鼠标移动时触发
  //window.addEventListener('mousemove',this.getHeight, false);
  // 自适应浏览器获取宽高大小定时器
  this.resizeScreen();
  // 图表初始化
  this.initChart();
  // 获取接口数据
  this.getData();
  // 调用Echarts图表自适应方法
  this.screenAdapter();    
  // Echarts图表自适应
  window.addEventListener("resize", this.screenAdapter);
  // 局部刷新定时器
  this.getDataTimer();    
},
beforeDestroy () {
  // 销毁Echarts图表
  this.chartInstance.dispose();
  this.chartInstance = null;
},
destroyed() {
  // 清除自适应屏幕定时器
  clearInterval(this.koiTime);
  this.koiTime = null;
  // 清除局部刷新定时器
  clearInterval(this.koiTimer);
  this.koiTimer = null;
  // 页面大小改变时触发销毁
  window.removeEventListener('resize',this.getScreenHeight, false);
  // 页面大小改变时触发销毁
  window.removeEventListener('resize',this.getScreenWidth, false);
  // Echarts图表自适应销毁
  window.removeEventListener("resize", this.screenAdapter);
},
methods: {
  // 自适应浏览器获取宽高大小定时器
  resizeScreen(){
    this.koiTime = setInterval(() => {
      this.getScreenHeight();
      this.getScreenWidth();
    }, 200)
  },
  // 获取浏览器高度进行自适应
  getScreenHeight() {
      this.screenHeight = window.innerHeight || document.documentElement.innerHeight || document.body.clientHeight;
      // 四舍五入取整数
      this.kHOne = Math.round(this.screenHeight * 0.31);
      //console.log("高度->"+this.screenHeight +"-"+ this.kHOne);
  },
  // 字体大小根据宽度自适应
  getScreenWidth(){
    this.screenWidth = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;
    //console.log("宽度->"+this.screenWidth);
  },
  initChart() {
    this.chartInstance = this.$echarts.init(this.$refs.refChart,'shine');
    const initOption = {
      title: {
        text: "生产线电耗",
        left: 'left'
      },      
      legend: {
          left: 'right',
      },        
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'cross',
          label: {
            backgroundColor: '#6a7985'
          }
        }
      },
      // toolbox: {
      //   feature: {
      //     saveAsImage: {}
      //   }
      // },
      grid: {
        top: '12%',
        left: '1%',
        right: '3%',
        bottom: '6%',
        containLabel: true
      },
      xAxis: [
        {
          type: 'category',
          boundaryGap: false,
        }
      ],
      yAxis: [
        {
          type: 'value'
        }
      ],
      series: [
        {
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
        },
        {
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
        },
        {
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
        },
        {
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
        }
      ]        
    };
    // 图表初始化配置
    this.chartInstance.setOption(initOption);
  },
  getData() {
    // 先进行置空
    this.allData1 = [];
    this.allData2 = [];
    this.allData3 = [];
    this.allData4 = [];
    let num1 = 102;
    let num2 = 126;
    let num3 = 121;
    let num4 = 134;
    let num5 = 111;
    let num6 = 110;
    let num7 = 112;
    let num8 = 124;
    let num9 = 120;
    let num10 = 135;
    let num11 = 130;
    let num12 = 108;
    // let num1 = this.randomNum(50,150);
    // let num2 = this.randomNum(50,150);
    // let num3 = this.randomNum(50,150);
    // let num4 = this.randomNum(50,150);
    // let num5 = this.randomNum(50,150);
    // let num6 = this.randomNum(50,150);
    // let num7 = this.randomNum(50,150);
    // let num8 = this.randomNum(50,150);
    // let num9 = this.randomNum(50,150);
    // let num10 = this.randomNum(50,150);
    // let num11 = this.randomNum(50,150);
    // let num12 = this.randomNum(50,150);
    this.allData1.push(num1,num2,num3,num4,num5,num6,num7,num8,num1,num9,num10,num11,num12);
    this.allData2.push(num1,num2,num3,num4,num5,num6,num7,num8,num1,num9,num10,num11,num12);
    this.allData3.push(num1,num2,num3,num4,num5,num6,num7,num8,num1,num9,num10,num11,num12);
    this.allData4.push(num1,num2,num3,num4,num5,num6,num7,num8,num1,num9,num10,num11,num12);

    // 调用接口方法
    // get24HoursData1().then(res => {
    //       this.allData = res.data;
    //       // 局部刷新数据
    //       this.updateChart();
    //       //console.log("ALLDATA->"+JSON.stringify(this.allData));
    //       // echarts查不到数据，将初始化echarts的方法全部放置到接口方法中即可。  
    // })  
    // 获取服务器的数据, 对this.allData进行赋值之后, 调用updateChart方法更新图表
    //console.log("ALLDATA->",JSON.stringify(res.data))
    //console.log("ALLDATA->",JSON.stringify(this.allData))
    this.updateChart();
  },
  updateChart() {
    // 处理图表需要的数据
    const dataOption = {
      legend: {
        data: ['生产线1', '生产线2', '生产线3', '生产线4']
      },
      xAxis: [
        {
          data: ['12:00', '13:00', '14:00', '15:00', '16:00', '17:00', '18:00', '19:00', '20:00', '21:00', '22:00', '23:00', '24:00']
        }
      ], 
      series: [
        {
          name: '生产线1',
          data: this.allData1
        },
        {
          name: '生产线2',
          data: this.allData2
        },
        {
          name: '生产线3',
          data: this.allData3
        },
        {
          name: '生产线4',
          data: this.allData3
        }
      ]               
    };
    // 图表数据变化配置
    this.chartInstance.setOption(dataOption);
  },
  screenAdapter() {
    this.titleFontSize = Math.round(this.$refs.refChart.offsetWidth / 40);
    const adapterOption = {
      title: {
        textStyle: {
          fontSize: Math.round(this.titleFontSize * 0.8),
        },
      },
      // 圆点分类标题
      legend: {
        textStyle: {
          fontSize: Math.round(this.titleFontSize * 0.65),
        },
      },
      xAxis: {
        //  改变x轴字体颜色和大小
        axisLabel: {
          textStyle: {
            fontSize: Math.round(this.titleFontSize * 0.7),
          },
        },
      },
      yAxis: {
        //  改变y轴字体颜色和大小
        axisLabel: {
          textStyle: {
            fontSize: Math.round(this.titleFontSize * 0.7),
          },
        },
      },
    };
    // 图表自适应变化配置
    this.chartInstance.setOption(adapterOption);
    this.chartInstance.resize();
  },
  // 定时器
  getDataTimer(){
    this.koiTimer = setInterval(() => {
      // 执行刷新数据的方法
      this.getData();
      //console.log("Hello World")
    }, 3000)
  },   
}
}
</script>
<style lang='less' scoped>

</style>