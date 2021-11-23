<template>
  <div id="app">
    <div>
      最小值：
      <el-input-number
        v-model="min"
        :min="0"
        :max="100"
        label="最小值"
        :controls= false
        placeholder='请输入'
        :precision = 0
      />
      最大值：
      <el-input-number
        v-model="max"
        :min="0"
        :max="100"
        label="最大值"
        :controls= false
        placeholder='请输入'
        :precision = 0
      />
      数量：
      <el-input-number
        v-model="count"
        :min="1"
        :max="10"
        label="数量"
        :controls= false
        placeholder='请输入'
        :precision = 0
      />
      排序：
      <el-select v-model="sort" placeholder="请选择">
        <el-option value="从小到大">
        </el-option>
        <el-option value="从大到小">
        </el-option>
      </el-select>
      <el-button
        type="primary"
        @click="creatCharts()"
      >
        生成
      </el-button>
    </div>
    <div id="chartDom"></div>
  </div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  name: 'App',
  data(){
    return {
      min:undefined,
      max:undefined,
      count:undefined,
      sort:'从小到大',
      cdata:[],
      myChart:null
    }
  },
  methods:{
    creatCharts(){
      if(this.min===undefined || !this.max || !this.count || this.min>this.max){
        alert('请输入正确数据信息');
        
        return null;
      }
      if(!this.myChart){
        let chartDom = document.getElementById('chartDom');
        this.myChart = echarts.init(chartDom);
      }
      this.getCdata()
      let option = {
        xAxis: {
          type: 'category',
          data: this.cdata.map((item,index)=>{return index}),
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            data: this.cdata.map((item)=>{return item}),
            type: 'bar',
            showBackground: true,
            backgroundStyle: {
              color: 'rgba(180, 180, 180, 0.2)'
            },
            label:{
              show:true,
              position:'top',
              textStyle:{
                color:'black',
                fontSize:16            
              }
            }
          }
        ]
      };
      this.myChart.setOption(option);
    },
    getCdata(){
      this.cdata=[]
       this.cdata[0]=this.min
      for(let i=1;i<this.count;i++){
        let num = this.min+this.diff*i
        this.cdata[i]=Number(num.toFixed(3))
      }
      if(this.sort === '从大到小'){
        this.cdata.reverse()
      }
      console.log(this.cdata)
    }
  },
  computed:{
    diff(){
      return (this.max-this.min)/(this.count-1)
    }
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#chartDom{
  width: 800px;
  height: 420px;
}
</style>
