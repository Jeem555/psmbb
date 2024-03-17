<template>
  <div>
    <el-row :gutter="20">
      <el-col :span="6">
        <el-card>权限项目</el-card>
      </el-col>
      <el-col :span="6">
        <el-card>毕设项目</el-card>
      </el-col>
      <el-col :span="6">
        <el-card>前后端分离</el-card>
      </el-col>
      <el-col :span="6">
        <el-card>脚手架系统</el-card>
      </el-col>
    </el-row>

    <el-row :gutter="20">
      <el-col :span="8">
        <div style="width:400px;height:400px" id="main">

        </div>
      </el-col>
      <el-col :span="8">
        <div style="width:400px;height:400px" id="main1">

        </div>
      </el-col>
      <el-col :span="8">
        <div style="width:400px;height:400px" id="main2">

        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  name: "Home",
  mounted() {

    var chartDom = document.getElementById('main');
    var myChart = echarts.init(chartDom);
    var option;

    option = {
      xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
      },
      yAxis: {
        type: 'value'
      },
      series: [
        {
          data: [820, 932, 901, 934, 1290, 1330, 1320],
          type: 'line',
          smooth: true
        }
      ]
    };



    var chartDom1 = document.getElementById('main1');
    var myChart1 = echarts.init(chartDom1);
    var option1;

    option1 = {
      xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
      },
      yAxis: {
        type: 'value'
      },
      series: [
        {
          data: [120, 200, 150, 80, 70, 110, 130],
          type: 'bar',
          showBackground: true,
          backgroundStyle: {
            color: 'rgba(180, 180, 180, 0.2)'
          }
        }
      ]
    };



    var chartDom2 = document.getElementById('main2');
    var myChart2 = echarts.init(chartDom2);
    var option2;

    option2 = {
      tooltip: {
        trigger: 'item'
      },
      legend: {
        top: '5%',
        left: 'center'
      },
      series: [
        {
          name: 'Access From',
          type: 'pie',
          radius: ['40%', '70%'],
          avoidLabelOverlap: false,
          itemStyle: {
            borderRadius: 10,
            borderColor: '#fff',
            borderWidth: 2
          },
          label: {
            show: false,
            position: 'center'
          },
          emphasis: {
            label: {
              show: true,
              fontSize: 40,
              fontWeight: 'bold'
            }
          },
          labelLine: {
            show: false
          },
          data: [
            { value: 1048, name: 'Search Engine' },
            { value: 735, name: 'Direct' },
            { value: 580, name: 'Email' },
            { value: 484, name: 'Union Ads' },
            { value: 300, name: 'Video Ads' }
          ]
        }
      ]
    };



    this.request.get('/user/echart').then(res => {
      if(res.code === '200'){
        option.xAxis.data = res.data.list1;      //名称的集合
        option.series[0].data = res.data.list2;  //数值的集合
        option && myChart.setOption(option);

        option1.xAxis.data = res.data.list1;      //名称的集合
        option1.series[0].data = res.data.list2;  //数值的集合
        option1 && myChart1.setOption(option1);

        option2.series[0].data = res.data.list3;
        option2 && myChart2.setOption(option2);
      }
    })

  }
}
</script>

<style scoped>

</style>