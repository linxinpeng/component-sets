<!--
 * @Author: your name
 * @Date: 2021-05-19 10:47:13
 * @LastEditTime: 2021-05-19 11:06:04
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \component-sets\src\App.vue
-->
<template>
  <div id="app">
    <el-container>
      <el-container>
        <el-main>
          <el-card>
            <div slot="header" class="clearfix">
              <span>GOJS例子1</span>
            </div>
            <risk-evolution />
          </el-card>
          <el-card style="margin-top: 12px">
            <div slot="header" class="clearfix">
              <span>GOJS例子2</span>
            </div>
            <gojs-test2 />
          </el-card>
          <el-card style="margin-top: 12px">
            <div slot="header" class="clearfix">
              <span>泡泡图1</span>
            </div>
            <div class="ppt">
              <bubble-chart width="100%" />
            </div>
          </el-card>
          <el-card style="margin-top: 12px">
            <div slot="header" class="clearfix">
              <span>泡泡图2</span>
            </div>
            <div class="ppt">
              <bubble-chart-center />
            </div>
          </el-card>
          <el-card style="margin-top: 12px">
            <div slot="header" class="clearfix">
              <span>词云图</span>
            </div>
            <div class="ppt">
              <word-clouds :data="wordCloudsData"/>
            </div>
          </el-card>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import riskEvolution from "@/components/risk-evolution";
import GojsTest2 from "@/components/gojs-test2";
import bubbleChart from "@/components/bubble-chart";
import bubbleChartCenter from "@/components/bubble-chart-center";
import wordClouds from "@/components/word-clouds";
export default {
  name: "app",
  components: {
    riskEvolution,
    GojsTest2,
    bubbleChart,
    bubbleChartCenter,
    wordClouds,
  },
  data() {
    let wordCloudsData = [];
    for (let i = 0; i < 100; i++) {
      wordCloudsData.push({
        name: "关键词" + i,
        value: Math.floor(Math.random() * 100),
      });
    }
    return {
      wordCloudsData
    };
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
body {
  background-color: #f2f2f2;
}
#app {
  width: 1300px;
  margin: 0 auto;
}
.side-nav {
  padding: 20px;
  margin: 0;
  overflow: hidden;
}
.ppt {
  display: flex;
  justify-content: center;
  padding: 30px;
}
.side-nav li {
  list-style: none;
}
.side-nav li a {
  display: block;
  height: 40px;
  color: #444;
  line-height: 40px;
  font-size: 14px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  font-weight: 400;
  cursor: pointer;
}
.side-nav li a:hover {
  color: #409eff;
}
</style>
