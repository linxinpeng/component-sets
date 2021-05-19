<template>
    <div class="core-circle">
        <!-- 状态提示 -->
        <div class="core-circle__tips">
            <div class="core-circle__tip" style="border-color: #fa4040">发起人</div>
            <div class="core-circle__tip" style="border-color: #faad14">积极响应人</div>
            <div class="core-circle__tip" style="border-color: #4090ff">积极现场参与人</div>
        </div>
        <!-- 关系图gojs -->
         <dg-scrollbar>
        <div id="flow" style="width: 100%; height: 800px"></div>
         </dg-scrollbar>
        <!-- 统计弹窗 -->
        <transition name="fade">
            <div
                v-if="show"
                :style="{
                    top: y,
                    left: x
                }"
                class="popup__content"
            >
                <p>通话（{{ numbers[0] }}）</p>
                <p>同线索（{{ numbers[1] }}）</p>
            </div>
        </transition>
        <!-- 人员详情弹窗 -->
        <transition name="fade">
            <div class="pop" :style="style" v-show="visible">
                <i @click="visible = false" class="el-icon-close pop__close"></i>
                <div class="pop__info">
                    <div class="pop__img">
                        <img src="@/assets/images/default/test.png" />
                    </div>
                    <div class="pop__current-info">
                        <div>
                            <img src="@/assets/images/info.png" />
                            <span>350987198802231080</span>
                        </div>
                        <div>梁玲宁（梁子）</div>
                        <div>
                            <span>男</span>
                            <span>26岁</span>
                            <span>汉族</span>
                        </div>
                    </div>
                </div>

                <div class="pop__other">
                    <div>
                        <label>户籍地：</label>
                        <span>天津市西芹区</span>
                    </div>
                    <div>
                        <label>现住地址：</label>
                        <span>合肥市瑶海区某某路</span>
                    </div>
                    <div>
                        <label>群体身份：</label>
                        <span>骨干</span>
                    </div>
                </div>
                <div class="pop__tag">
                    <span class="tag--green">国家重点人</span>
                    <span class="tag--red">省重点人员</span>
                    <span class="tag--blue">出租车司机</span>
                    <span class="tag--green">出租车司</span>
                    <span class="tag--green">本省重点人员</span>
                    <span class="tag--red">出租车司机</span>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
import go from "gojs";
export default {
    name: "core-circle",
    data() {
        return {
            nodes: [
                {
                    key: "1",
                    img: require("@/assets/images/default/test.png"),
                    text: "黄嘉欣",
                    role: "initiator"
                },
                {
                    key: "2",
                    img: require("@/assets/images/default/tx-1.png"),
                    text: "李二狗",
                    role: "responder"
                },
                {
                    key: "3",
                    img: require("@/assets/images/default/tx-2.png"),
                    text: "李三狗",
                    role: "responder"
                },
                {
                    key: "4",
                    img: require("@/assets/images/default/tx-1.png"),
                    text: "杜月笙",
                    role: "responder"
                },
                {
                    key: "5",
                    img: require("@/assets/images/default/tx-2.png"),
                    text: "周润发",
                    role: "responder"
                },
                {
                    key: "6",
                    img: require("@/assets/images/default/tx-2.png"),
                    text: "李三狗",
                    role: "join"
                },
                {
                    key: "7",
                    img: require("@/assets/images/default/tx-1.png"),
                    text: "杜月笙",
                    role: "join"
                },
                {
                    key: "8",
                    img: require("@/assets/images/default/tx-2.png"),
                    text: "周润发",
                    role: "join"
                }
            ],
            links: [
                {
                    from: "1",
                    to: "2",
                    total: 6,
                    th: 3,
                    txs: 3,
                    active: false
                },
                {
                    from: "1",
                    to: "3",
                    total: 14,
                    th: 6,
                    txs: 8,
                    active: false
                },
                {
                    from: "1",
                    to: "4",
                    total: 6,
                    th: 3,
                    txs: 3,
                    active: false
                },
                {
                    from: "1",
                    to: "5",
                    total: 6,
                    th: 3,
                    txs: 3,
                    active: false
                },
                {
                    from: "1",
                    to: "6",
                    total: 6,
                    th: 3,
                    txs: 3,
                    active: false
                },
                {
                    from: "1",
                    to: "7",
                    total: 6,
                    th: 3,
                    txs: 3,
                    active: false
                },
                {
                    from: "7",
                    to: "8",
                    total: 0,
                    active: false
                },
                {
                    from: "3",
                    to: "7",
                    total: 6,
                    th: 3,
                    txs: 3,
                    active: false
                }
            ],
            myGo: null,

            x: 0, // 小弹窗位置
            y: 0, // 小弹窗位置
            show: false,
            visible: false,
            prevLabel: null,

            numbers: [], // 弹窗数量值

            style: {
                //弹窗位置
                top: 0,
                left: 0
            },

            // 过滤条件
            form: {
                relation: "txs",
                type: "qb",
                date: ""
            },

            options: [
                {
                    label: "同线索",
                    value: "txs"
                },
                {
                    label: "全部",
                    value: "qb"
                }
            ]
        };
    },
    methods: {
        /**
         * 重新渲染数据
         */
        refresh() {
            const g = go.GraphObject.make;
            const model = g(go.GraphLinksModel);
            model.nodeDataArray = this.nodes;
            model.linkDataArray = this.links;
            this.myGo.model = model;
        },
        /**
         * 关闭弹窗
         */
        handleClose() {
            this.show = false;
        },
        /**
         * link label 点击事件 设置弹窗出现位置
         * @param {object} node 当前节点数据
         */
        onSelectionChanged(e, node) {
            this.changeLabelStatus(e, node);

            let pos = this.myGo.transformDocToView(node.getDocumentPoint(go.Spot.Center));
            this.x = pos.x + 10 + "px";
            this.y = pos.y - 20 + "px";
            this.numbers = [node.part.data.th, node.part.data.txs];
            this.show = true;
        },
        /**
         * 改变link label 状态
         * @param {object} node 当前节点数据
         */
        changeLabelStatus(e, node) {
            const vm = this;
            e.diagram.model.commit(function (m) {
                if (vm.prevLabel || vm.prevLabel !== node.part.data) {
                    m.set(vm.prevLabel, "active", false);
                    m.set(node.part.data, "active", true);
                    vm.prevLabel = node.part.data;
                }
            }, "clicked");
        }
    },
    mounted() {
        const vm = this;
        const g = go.GraphObject.make;
        this.myGo = g(go.Diagram, "flow", {
            isReadOnly: true,
            layout: g(go.ForceDirectedLayout)
        });
        // 节点配置
        this.myGo.nodeTemplate = g(
            go.Node,
            "Vertical",
            {
                selectionAdorned: false,
                click: function (e, node) {
                    const position = vm.myGo.transformDocToView(node.getDocumentPoint(go.Spot.Center));
                    // 设置弹窗位置
                    vm.$set(vm.style, "top", position.y - 20 + "px");
                    vm.$set(vm.style, "left", position.x + 60 + "px");
                    vm.visible = true;
                }
            },
            g(
                go.Panel,
                "Spot",
                {
                    cursor: "pointer"
                },
                g(
                    go.Shape,
                    {
                        figure: "Circle",
                        width: 64,
                        height: 64,
                        strokeWidth: 2,
                        fill: "rgba(0,0,0,0)"
                    },
                    new go.Binding("stroke", "role", (role) => {
                        if (role === "initiator") {
                            return "#FA4040";
                        } else if (role === "responder") {
                            return "#FAAD14";
                        } else {
                            return "#4090FF";
                        }
                    })
                ),
                g(
                    go.Panel,
                    "Spot",
                    { isClipping: true },
                    g(go.Shape, {
                        figure: "Circle",
                        width: 60,
                        height: 60
                    }),
                    g(
                        go.Picture,
                        {
                            width: 60,
                            height: 60
                        },
                        new go.Binding("source", "img")
                    )
                )
            ),
            g(
                go.Panel,
                "Auto",
                {
                    margin: 6
                },
                g(go.Shape, {
                    figure: "RoundedRectangle",
                    // parameter1: 1000111,
                    // width: 100,
                    height: 20,
                    stroke: "rgba(0,0,0,0.15)",
                    fill: "rgba(0,0,0,0)"
                }),
                g(
                    go.TextBlock,
                    {
                        font: "small-caps 14px MicrosoftYaHei-Bold",
                        margin: 16,
                        stroke: "rgba(0,0,0,0.85)"
                    },
                    new go.Binding("text", "text")
                )
            )
        );
        // 连线配置
        this.myGo.linkTemplate = g(
            go.Link,
            {
                curve: go.Link.Bezier,
                selectionAdorned: false
            },
            g(go.Shape, {
                strokeWidth: 1,
                stroke: "#ccc",
                strokeMiterLimit: 10
            }),

            g(
                go.Panel,
                "Auto",
                {
                    cursor: "pointer",
                    mouseEnter: vm.onSelectionChanged,
                    mouseLeave: function () {
                        vm.show = false;
                    }
                },
                g(
                    go.Shape,
                    "Circle",
                    {
                        width: 28,
                        height: 28,
                        stroke: "rgba(0,0,0,0)"
                    },
                    new go.Binding("visible", "total", (total) => {
                        return total !== 0;
                    }),
                    new go.Binding("fill", "active", (active) => {
                        return active ? "#4090FF" : "#D9E8FC";
                    })
                ),
                g(
                    go.TextBlock,
                    {
                        font: "small-caps 14px MicrosoftYaHei-Bold",
                        stroke: "#0063EA"
                    },
                    new go.Binding("text", "total"),
                    new go.Binding("visible", "total", (total) => {
                        return total !== 0;
                    }),
                    new go.Binding("stroke", "active", (active) => {
                        return active ? "#ffffff" : "#0063EA";
                    })
                )
            )
        );
        const model = g(go.GraphLinksModel);
        model.nodeDataArray = this.nodes;
        model.linkDataArray = this.links;
        this.myGo.model = model;
    }
};
</script>
<style lang="scss" scoped>
.core-circle {
    position: relative;
    &__filters {
        display: flex;
        align-items: center;
    }
    &__tips {
        display: flex;
        align-items: center;
        padding: 16px 0;
    }
    &__tip {
        display: flex;
        align-items: center;
        font-size: 14px;
        color: #595959;
        margin-right: 12px;
        border-width: 0;
        &::before {
            content: "";
            height: 14px;
            width: 14px;
            border-width: 2px;
            border-style: solid;
            border-color: inherit;
            border-radius: 50%;
            box-sizing: border-box;
            margin-right: 8px;
        }
    }
}
.popup__content {
    position: absolute;
    width: 190px;
    background: #ffffff;
    box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.1);
    position: absolute;
    padding: 12px 24px;
    box-sizing: border-box;
    z-index: 9999;
    p {
        font-size: 14px;
        color: rgba(0, 0, 0, 0.85);
        line-height: 30px;
    }
    i {
        position: absolute;
        top: 16px;
        right: 14px;
        cursor: pointer;
        &:hover {
            color: #4090ff;
        }
    }
}
.pop {
    position: absolute;
    width: 442px;
    background: #ffffff;
    box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.1);
    // position: fixed;
    z-index: 999;
    padding: 16px 24px;
    box-sizing: border-box;
    &__close {
        position: absolute;
        top: 16px;
        right: 16px;
        cursor: pointer;
    }
    &__info {
        display: flex;
    }
    &__img {
        display: flex;
        align-items: center;
        width: 76px;
        height: 76px;
        border-radius: 2px;
        overflow: hidden;
        margin-right: 14px;
        img {
            width: 100%;
        }
    }
    &__current-info {
        font-size: 14px;
        div:first-child {
            display: flex;
            align-items: center;
            color: #0063ea;
            img {
                width: 15px;
                margin-right: 6px;
            }
        }
        div:nth-child(2) {
            color: rgba(0, 0, 0, 0.85);
            line-height: 2;
        }
        div:last-child {
            color: rgba(0, 0, 0, 0.65);
        }
    }
    &__tag {
        span {
            display: inline-block;
            padding: 4px 12px;
            line-height: 1;
            border-radius: 11px;
            white-space: nowrap;
            margin-right: 4px;
            margin-top: 8px;
            font-size: 14px;
            &.tag--green {
                color: #00c08c;
                background: rgba($color: #00c08c, $alpha: 0.08);
            }
            &.tag--blue {
                color: #0063ea;
                background: rgba($color: #0063ea, $alpha: 0.08);
            }
            &.tag--red {
                color: #fe8637;
                background: rgba($color: #fe8637, $alpha: 0.08);
            }
        }
    }
    &__other {
        div {
            margin-top: 12px;
            font-size: 14px;
            label {
                display: inline-block;
                color: rgba(0, 0, 0, 0.65);
                width: 80px;
                text-align: right;
            }
            span {
                color: rgba(0, 0, 0, 0.85);
            }
        }
    }
}
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
}
</style>