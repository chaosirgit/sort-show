<template>
  <Layout>
    <div>
      <el-row>
        <el-button @click="beginSort">开始排序</el-button>
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
      initColor: '#409EFF',
      selectColor: 'hotpink',
      successColor: '#5CB87A',
      arrayLength: 10,
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
        return {value: item,color: this.initColor,id:index}
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
    beginSort(){
      for (var i = 0; i < this.list.length ; i++){
          for (var j = i + 1; j < this.list.length; j++){
            this.colorList = this.list.map(item => {
              var color = this.initColor
              if ((item.id === i) || (item.id === j)){
                color = this.selectColor
              }
              return color
            })
            if (this.list[i].value > this.list[j].value){
              var temp = this.list[j]
              this.list[j] = this.list[i]
              this.list[i] = temp
            }
            this.myOptions.animation = false
            this.setColor()
            this.setChartData()
            this.fetchChart()
          }
      }
    }
  }
}
</script>
<style>

</style>