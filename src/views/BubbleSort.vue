<template>
  <Layout>
    <div>
      <el-row>
        <el-button @click="beginSort">开始排序</el-button>
        <el-button @click="reset">重置</el-button>
      </el-row>
      <div id="myCharts" style="width: 100%;height: 680px;margin-top: 20px;">

      </div>
    </div>

  </Layout>
</template>
<script>
import Layout from "@/components/Layout";
import * as echarts from "echarts";
export default {
  name: 'BubbleSort',
  components: {Layout},
  mounted(){
    this.fetchData()
    this.fetchChart()
  },
  data(){
    return {
      someData: [],
      colorList: [],
      list:[],
      flag: false,
      isColor: false,
      timer: '',
      i: 0,
      j: 1,
      initColor: '#409EFF',
      selectColor: 'hotpink',
      needColor: 'orange',
      successColor: '#5CB87A',
      arrayLength: 200,
      myCharts : '',
      myOptions: {
          title: {
            text: '冒泡排序'
          },
          animation:true,
          tooltip: {},
          xAxis: {
            data: [],
          },
          yAxis: {},
          series: [{
            type: 'bar',
            data: [],
            itemStyle: {
              normal: {
                color: ''
              }
            }
          }]
        }
    }
  },
  methods:{
    getChart() {
      this.myCharts = echarts.init(document.getElementById('myCharts'))
    },
    fetchData(){
      this.someData = Array.from({length:this.arrayLength},() => {return Math.ceil(Math.random()*this.arrayLength)})
      this.list = this.someData.map((item,index)=>{
        return {value: item,color: this.initColor,id:index,sort:false}
      })
      this.colorList = this.list.map(item => {return item.color})
      this.setColor()
      this.setChartData()
      this.getChart()
    },
    fetchChart(){
      this.myCharts.setOption(this.myOptions)
    },
    setChartData(){
      this.myOptions.series[0].data = this.list.map(item => {return item.value})
    },
    setColor(){
      this.myOptions.series[0].itemStyle.normal.color = (params) => {
        return this.colorList[params.dataIndex]
      }
    },
    reset() {
      this.fetchData()
      this.fetchChart()
      this.flag = false
      if (this.timer){
        clearInterval(this.timer)
      }
      this.i = 0
      this.j = 1
    },
    beginSort() {
      let begin = Date.parse( new Date());
       this.timer = setInterval(() => {
        if (this.flag){
          clearInterval(this.timer)
          let end = Date.parse( new Date());
          this.$message({
            message: '排序完成,耗时' + (end - begin) + '毫秒',
            type: 'success'
          });
        }else{
          this.bubbleSort()
        }
      }, 30)
    },
    viewColor(){
      this.colorList = this.list.map((item,index) => {
        var color = this.initColor
        if ((index === this.i) || (index === this.j)){
          color = this.selectColor
        }
        if (item.sort){
          color = this.successColor
        }
        return color
      })
      this.myOptions.animation = false
      this.isColor = true
      this.setColor()
      this.setChartData()
      this.fetchChart()
    },
    bubbleSort() {
      if (!this.flag){
        if (!this.isColor){
          this.viewColor()
        }else{
          this.isColor = false
          const lastLength = this.list.length - this.list.filter((item) => {
            return item.sort === true
          }).length

          const needLength = this.list.filter((item) => {
            return item.sort === false
          }).length

          if (needLength === 0){
            this.flag = true
          }

          if (!this.list[this.i].sort){
            if (this.list[this.i].value > this.list[this.j].value){
              let temp = this.list[this.j]
              this.list[this.j] = this.list[this.i]
              this.list[this.i] = temp
            }
            this.i = this.j

            if (needLength <= 1){
              this.list[this.i - 1].sort = true
            }else{
              if (this.i + 1 >= lastLength){
                this.list[this.i].sort = true
                this.i = 0
                this.j = 1
              }else{
                this.j++
              }
            }
          }
          this.setChartData()
          this.fetchChart()
        }
      }else{
        console.log('done')
      }
    }
  }
}
</script>
<style>

</style>