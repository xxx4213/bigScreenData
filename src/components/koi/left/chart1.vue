<!-- left1 动态刷新饼形图数据 -->
<template>      
    <div ref="refChart" :style="{ height: kHOne + 'px'}"></div>
</template>

<script>
// import { getModuleData } from "../../../api/ems/index";
require("../../../assets/theme/shine")
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
    allData: [],
    kHOne: null,
    koiTime: null,
    // 局部刷新定时器    
    koiTimer: null,
  }
  },
  created () {},
  beforeMount(){
    this.kHOne = Math.round(this.screenHeight * 0.44)
  },
  mounted(){
    // 页面大小改变时触发
    window.addEventListener('resize',this.getScreenHeight, false);
    // 页面大小改变时触发
    window.addEventListener('resize',this.getScreenWidth, false);
    // 鼠标移动时触发
    //window.addEventListener('mousemove',this.getHeight, false);
    this.resizeScreen();
    // 图表初始化
    this.initChart();
    this.getData();
    window.addEventListener("resize", this.screenAdapter);
    this.screenAdapter();
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
    // 页面大小改变时触发
    window.removeEventListener('resize',this.getScreenHeight, false);
    // 页面大小改变时触发
    window.removeEventListener('resize',this.getScreenWidth, false);
    window.removeEventListener("resize", this.screenAdapter);
  },
  methods: { 
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
        this.kHOne = Math.round(this.screenHeight * 0.44);
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
          text: "良品占比",
          left: 'center'
        },      
        tooltip: {
          trigger: 'item'
        },
        legend: {
            orient: 'vertical',
            left: 'left',
        },
        series: [
            {
              name: '材料',
              type: 'pie',
              // 环形图大小
              radius: ['45%', '70%'],
              // 环形图位置
              center: ["56%", "60%"], 
              color: ['#13864f', '#fc011a'],
              avoidLabelOverlap: false,
              itemStyle: {
                borderRadius: 10,
                borderColor: '#fff',
                borderWidth: 1,
                // colorList: [
                //           '#45C2E0', '#C1EBDD', '#FFC851','#5A5476','#1869A0','#FF9393'
                //           ]
              },
              label: {
                show: false,
                position: 'center',
                formatter: '{d}%' // 当前百分比
              },
              emphasis: {
                label: {
                  show: true,
                  fontSize: '16',
                  fontWeight: 'bold'
                }
              },
              labelLine: {
                show: false
              },
            }
          ]        
      };
      // 图表初始化配置
      this.chartInstance.setOption(initOption);

      // 鼠标移入停止定时器
      this.chartInstance.on("mouseover", () => {
        clearInterval(this.koiTimer);
      });

      // 鼠标移入启动定时器
      this.chartInstance.on("mouseout", () => {
        this.getDataTimer();
      });      
    },
    getData() {
      // getModuleData().then(res => {
      //     this.allData = res.data;
      //     this.updateChart(); 
      //     //console.log("数据left1->"+JSON.stringify(this.allData.pcsSum));
      //     // echarts查不到数据，将初始化echarts的方法全部放置到接口方法中即可。  
      // })         
      // 获取服务器的数据, 对this.allData进行赋值之后, 调用updateChart方法更新图表
      //console.log("ALLDATA->",JSON.stringify(res.data))
      //console.log("ALLDATA->",JSON.stringify(res.allData))
      this.updateChart();
    },
    updateChart() {
      // let num1 = this.randomNum(100,201);
      let num1 = 995;
      // let num2 = this.randomNum(50,201);
      let num2 = 5;
      let num3 = this.randomNum(60,201);
      let num4 = this.randomNum(70,201);
      let num5 = this.randomNum(85,201);
      this.allData = [];
      this.allData.push({ value: num1, name: '良品' })
      this.allData.push({ value: num2, name: '不良品' })
      // this.allData.push({ value: num3, name: 'TTZZ' })
      // this.allData.push({ value: num4, name: 'GGHH' })
      // this.allData.push({ value: num5, name: 'YYXX' })
      
      // 处理图表需要的数据
      const dataOption = {
        series: [
            {
              data: this.allData
            }
          ]          
      };
      // 图表数据变化配置
      this.chartInstance.setOption(dataOption);
    },
    screenAdapter() {
      this.titleFontSize = (this.$refs.refChart.offsetWidth / 100) * 2;
      const adapterOption = {
        title: {
          textStyle: {
            fontSize: Math.round(this.titleFontSize * 2),
          },
        },
        // 圆点分类标题
        legend: {
          textStyle: {
            fontSize: Math.round(this.titleFontSize * 1.2),
          },
        }
      };
      // 图标自适应变化配置
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
<style scoped='scss' scoped>

</style>