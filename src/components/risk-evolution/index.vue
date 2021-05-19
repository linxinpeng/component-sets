<!-- 
    风险演进
    linxp
    2021.05.06
-->
<template>
  <div class="risk-evolution" id="risk-evolution"></div>
</template>
<script>
import go from "gojs";
export default {
  name: "risk-evolution",
  // 接收父页面传过来的属性
  props: {
    nodes: {
      type: Array,
      default: () => {
        return [
          {
            key: "01",
            text: "平静时期",
            activeColor: "#00C08C",
            bg: require("@/assets/images/gojs/bg1-nor.png"),
            bgActive: require("@/assets/images/gojs/bg1-current.png"),
            loc: "-450 100",
            active: false,
          },
          {
            key: "02",
            text: "串联时期",
            activeColor: "#4090FF",
            bg: require("@/assets/images/gojs/bg2-nor.png"),
            bgActive: require("@/assets/images/gojs/bg2-current.png"),
            loc: "-150 100",
            active: true,
          },
          {
            key: "03",
            text: "形成时期",
            activeColor: "#FAAD14",
            bg: require("@/assets/images/gojs/bg3-nor.png"),
            bgActive: require("@/assets/images/gojs/bg3-current.png"),
            loc: "150 100",
            active: false,
          },
          {
            key: "04",
            text: "产生时期",
            activeColor: "#FF5667",
            bg: require("@/assets/images/gojs/bg4-nor.png"),
            bgActive: require("@/assets/images/gojs/bg4-current.png"),
            loc: "450 100",
            active: false,
          },
        ];
      },
    },
    links: {
      type: Array,
      default: () => {
        return [
          {
            from: "01",
            to: "02",
            source: require("@/assets/images/gojs/jt.png"),
            textBorder: false,
          },
          {
            from: "02",
            to: "03",
            source: require("@/assets/images/gojs/jt.png"),
            textBorder: false,
          },
          {
            from: "03",
            to: "04",
            source: require("@/assets/images/gojs/jt.png"),
            textBorder: false,
          },
          {
            from: "02",
            to: "03",
            text: "转移概览：20%",
            fromSpot: "TopCenter", // 出发线位置
            toSpot: "TopCenter", // 进入线位置
            dashed: [5, 5], // 虚线配置
            arrow: "Standard", // 箭头
            h: 20, // 出发线高度
            textBorder: true,
          },
          {
            from: "02",
            to: "04",
            text: "转移概览：23%",
            toSpot: "TopCenter",
            fromSpot: "TopCenter",
            dashed: [5, 5],
            arrow: "Standard",
            h: 50,
            textBorder: true,
          },
        ];
      },
    },
  },
  data() {
    return {
      diagram: null,
    };
  },
  mounted() {
    const vm = this;
    const g = go.GraphObject.make;
    this.diagram = g(go.Diagram, "risk-evolution", {
      "animationManager.isInitial": false,
      isReadOnly: true,
      allowSelect: false,
    });

    this.diagram.nodeTemplate = g(
      go.Node,
      "Auto",
      new go.Binding("location", "loc", go.Point.parse),
      g(
        go.Panel,
        "Auto",
        {
          cursor: "pointer",
        },
        g(
          go.Picture,
          {
            width: 196,
            height: 72,
          },
          new go.Binding("source", "active", (item, obj) => {
            return item ? obj.part.data.bgActive : obj.part.data.bg;
          })
        ),
        g(
          go.TextBlock,
          {
            font: "16px MicrosoftYaHei",
            stroke: "rgba(0,0,0,0.65)",
          },
          new go.Binding("text", "text")
        )
      )
    );

    this.diagram.linkTemplate = g(
      go.Link,
      {
        routing: go.Link.Orthogonal,
        corner: 10,
      },
      new go.Binding("fromEndSegmentLength", "h"),
      new go.Binding("toSpot", "toSpot", go.Spot.parse),
      new go.Binding("fromSpot", "fromSpot", go.Spot.parse),
      g(
        go.Shape,
        {
          strokeWidth: 2,
          stroke: "rgba(0,0,0,0.15)",
        },
        new go.Binding("strokeDashArray", "dashed"),
        new go.Binding("stroke", "dashed", (item) => {
          return item ? "#4090FF" : "";
        })
      ),
      g(
        go.Shape,
        {
          width: 0,
          height: 0,
          stroke: "#4090FF",
          fill: "#4090FF",
        },
        new go.Binding("toArrow", "arrow")
      ),
      g(
        go.Picture, // this is a Link label
        {
          width: 14,
          height: 14,
        },
        new go.Binding("source", "source")
      ),
      g(
        go.Panel,
        "Auto",
        g(
          go.Shape,
          "RoundedRectangle",
          {
            margin: 0,
            fill: "#fff",
            strokeWidth: 2,
            stroke: "#4090FF",
          },
          new go.Binding("visible", "textBorder")
        ),
        g(
          go.TextBlock,
          {
            margin: new go.Margin(1, 8, 1, 8),
            font: "14px PingFangSC-Regular",
            stroke: "#4090FF",
          },
          new go.Binding("text", "text")
        )
      )
    );

    const myModel = g(go.GraphLinksModel);
    myModel.nodeDataArray = vm.nodes;
    myModel.linkDataArray = vm.links;
    this.diagram.model = myModel;
  },
};
</script>
<style lang="scss" scoped>
.risk-evolution {
  width: 100%;
  height: 216px;
}
</style>