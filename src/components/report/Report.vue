<template>
  <div>
    <!-- 面包屑导航 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>数据统计</el-breadcrumb-item>
      <el-breadcrumb-item>数据报表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片视图区 -->
    <el-card>
      <div id="main" style="width: 750px;height:400px;"></div>
    </el-card>
  </div>
</template>

<script>
import echarts from "echarts";
import _ from "lodash";
export default {
  created() {},
  //此时页面元素已经被渲染完毕
  async mounted() {
    // 基于准备好的dom，初始化echarts实例
    var myChart = echarts.init(document.getElementById("main"));
    // this.loadData();
    const { data: res } = await this.$http.get("reports/type/1");
    if (res.meta.status !== 200) this.$message.error("获取图表数据失败");
    const r = _.merge(res.data, this.options);
    // 使用刚指定的配置项和数据显示图表。
    myChart.setOption(r);
  },
  data() {
    return {
      // 指定图表的配置项和数据
      options: {
        title: {
          text: "用户来源"
        },
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "cross",
            label: {
              backgroundColor: "#E9EEF3"
            }
          }
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true
        },
        xAxis: [
          {
            boundaryGap: false
          }
        ],
        yAxis: [
          {
            type: "value"
          }
        ]
      }
    };
  },
  methods: {
    async loadData() {
      const { data: res } = await this.$http.get("reports/type/1");
      if (res.meta.status !== 200) this.$message.error("获取图表数据失败");
      this.res = _.merge(res.data, this.option);
      console.log(this.res);
    }
  }
};
</script>

<style lang="less" scoped>
</style>