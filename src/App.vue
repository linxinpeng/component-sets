<!--
 * @Author: your name
 * @Date: 2021-05-19 10:47:13
 * @LastEditTime: 2021-05-20 09:20:15
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \component-sets\src\App.vue
-->
<template>
  <div id="app">
    <side-nav :navs="navs" />
    <el-container>
      <el-container>
        <el-main>
          <div class="start-line">Start（before 05.20）</div>
          <el-card id="nav1">
            <div slot="header" class="clearfix">
              <span>GOJS例子1</span>
              <el-popover placement="right" width="400" trigger="hover">
                <div class="box">
                  <div class="item">
                    <label>难点一：</label>
                    <span>要固定该布局，需指定节点位置。 (节点位置：loc: "150 100"，位置绑定： new go.Binding("location", "loc", go.Point.parse))</span>
                  </div>
                  <div class="item">
                    <label>难点二：</label>
                    <span>需要改变线的出发位置跟进入位置 new go.Binding("toSpot", "toSpot", go.Spot.parse),
      new go.Binding("fromSpot", "fromSpot", go.Spot.parse)</span>
                  </div>
                   <div class="item">
                    <label>难点三：</label>
                    <span>改变线的高度使之横向不重叠，需指定出发段长度，new go.Binding("fromEndSegmentLength", "h")</span>
                  </div>
                </div>
                <i slot="reference" class="el-icon-question"></i>
              </el-popover>
            </div>
            <risk-evolution />
          </el-card>
          <el-card id="nav2" style="margin-top: 12px">
            <div slot="header" class="clearfix">
              <span>GOJS例子2</span>
              <el-popover placement="right" width="400" trigger="hover">
                <div class="box">
                  <div class="item">
                    <label>难点一：</label>
                    <span>该布局需指定layout: g(go.ForceDirectedLayout)</span>
                  </div>
                  <div class="item">
                    <label>难点二：</label>
                    <span>获取节点位置，以便生成弹窗位置 const position = vm.myGo.transformDocToView(node.getDocumentPoint(go.Spot.Center))</span>
                  </div>
                  <div class="item">
                    <label>难点三：</label>
                    <span>设置图片超出隐藏 isClipping: true</span>
                  </div>
                </div>
                <i slot="reference" class="el-icon-question"></i>
              </el-popover>
            </div>
            <gojs-test2 />
          </el-card>
          <el-card id="nav3" style="margin-top: 12px">
            <div slot="header" class="clearfix">
              <span>泡泡图1</span>
              <el-popover placement="right" width="400" trigger="hover">
                <div class="box">
                  <div class="item">
                    <label>技术点一：</label>
                    <span>根据用户数据平均切割图表</span>
                  </div>
                   <div class="item">
                    <label>技术点二：</label>
                    <span>根据用户数据在指定格子内生成点</span>
                  </div>
                </div>
                <i slot="reference" class="el-icon-question"></i>
              </el-popover>
            </div>
            <div class="ppt">
              <bubble-chart width="100%" />
            </div>
          </el-card>
          <el-card id="nav4" style="margin-top: 12px">
            <div slot="header" class="clearfix">
              <span>泡泡图2</span>
              <el-popover placement="right" width="400" trigger="hover">
                  <div class="box">
                  <div class="item">
                    <label>技术点一：</label>
                    <span>涉及标线使用markLine，symbol: ["arrow", "arrow"]可指定两端箭头</span>
                  </div>
                   <div class="item">
                    <label>技术点二：</label>
                    <span>根据用户数据在指定格子内生成点</span>
                  </div>
                </div>
                <i slot="reference" class="el-icon-question"></i>
              </el-popover>
            </div>
            <div class="ppt">
              <bubble-chart-center />
            </div>
          </el-card>
          <el-card id="nav5" style="margin-top: 12px">
            <div slot="header" class="clearfix">
              <span>词云图</span>
              <el-popover placement="right" width="300" trigger="hover">
                <div class="box">
                  <div class="item">
                    <label>插件：</label>
                    <span>word-cloud</span>
                  </div>
                </div>
                <i slot="reference" class="el-icon-question"></i>
              </el-popover>
            </div>
            <div class="ppt">
              <word-clouds :data="wordCloudsData" />
            </div>
          </el-card>
          <div class="start-line end-line">Start（before 05.20）</div>
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
import SideNav from "@/components/side-nav";
export default {
  name: "app",
  components: {
    riskEvolution,
    GojsTest2,
    bubbleChart,
    bubbleChartCenter,
    wordClouds,
    SideNav,
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
      wordCloudsData,
      navs: [
        {
          id: "nav1",
          label: "GOJS例子1",
        },
        {
          id: "nav2",
          label: "GOJS例子2",
        },
        {
          id: "nav3",
          label: "泡泡图1",
        },
        {
          id: "nav4",
          label: "泡泡图2",
        },
        {
          id: "nav5",
          label: "词云图",
        },
      ],
    };
  },
};
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
}
body {
  /* background-color: #f2f2f2; */
}
#app {
  width: 1300px;
  margin: 0 auto;
}
.box .item{
  margin-bottom: 12px;
  font-size: 14px;
  label{
    color: rgba($color: #000000, $alpha: 0.45);
  }
  span{
    color: rgba($color: #000000, $alpha: 0.65);
  }
  &:last-child{
    margin-bottom: 0;
  }
}
.start-line {
  padding-bottom: 20px;
  border-bottom: 1px dashed #409eff;
  margin-bottom: 20px;
  font-size: 16px;
  font-weight: bold;
  color: rgba(0, 0, 0, 0.85);
}
.end-line {
  margin-top: 20px;
}
.start-line::before {
  display: inline-block;
  content: "";
  width: 4px;
  height: 12px;
  background-color: #409eff;
  border-radius: 4px;
  margin-right: 12px;
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
.clearfix i {
  cursor: pointer;
  color: #409eff;
  margin-left: 12px;
}
</style>
