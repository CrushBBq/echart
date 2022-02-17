<template>
  <div id="app">
    <el-row :gutter="4" style="margin-bottom: 20px">
      <el-col :span="4">
        <el-input
            placeholder="最小值"
            v-model.number="form.minValue">
        </el-input>
      </el-col>
      <el-col :span="4">
        <el-input
            placeholder="最大值"
            v-model.number="form.maxValue">
        </el-input>
      </el-col>
      <el-col :span="4">
        <el-input
            placeholder="数量"
            v-model.number="form.num">
        </el-input>
      </el-col>
      <el-col :span="4">
        <el-select v-model="form.sort" placeholder="排序">
          <el-option :value="0" label="从小到大"></el-option>
          <el-option :value="1" label="从大到小"></el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-button type="primary" @click="getPage">生成</el-button>
      </el-col>
    </el-row>
    <div ref="echarts" style="width: 500px;height: 500px">
    </div>
  </div>
</template>

<script>
import * as echarts from "echarts"
export default {
  name: 'App',
  data() {
    return {
      form:{
        minValue:"",
        maxValue:"",
        sort:0,
        num:"",
      },
      echartData:[],
      chart: null,
      option: {
        tooltip: {},
        xAxis: {
          data: []
        },
        yAxis: {},
        series: [{
          type: 'bar',
          data:[]
        }],
        color: ['#409EFF']
      }
    }
  },

  methods: {
    getPage() {
      this.echartData = []
      this.option.xAxis.data = []
      this.$nextTick(()=>{
        for(let i = this.form.minValue;i<=this.form.num;i++){
          if(i<=this.form.maxValue){
            this.echartData.push(i)
            this.option.xAxis.data.push(i)
          }
        }
        if(this.form.sort === 1){
          this.option.series[0].data =  this.echartData.sort(function(a, b){return b-a})
        }else{
          this.option.series[0].data =  this.echartData.sort()
        }
        this.chart.setOption(this.option)
      })
    }
  },
  mounted() {
    this.chart = echarts.init(this.$refs.echarts)
    this.echartData =[]
    this.option.xAxis.data = []
  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
}
#app{
  padding: 50px;
}
</style>
