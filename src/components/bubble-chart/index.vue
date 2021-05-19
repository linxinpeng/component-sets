<!--
 * @Author: linxp
 * @Date: 2021-05-10 14:57:37
 * @LastEditTime: 2021-05-10 17:25:58
 * @LastEditors: Please set LastEditors
 * @Description: 泡泡图
 * @FilePath: \small-app\src\components\bubble-chart\index.vue
-->
<template>
    <div
        class="bubble-chart"
        ref="bubble-chart"
        :style="{
            width: `calc(${width} - ${yLableWidth})`,
            height,
            marginLeft: yLableWidth
        }"
    >
        <!-- X轴数据 -->
        <div class="bubble-chart__x">
            <span v-for="col in xData" :key="col.value">{{ col.label }}</span>
        </div>
        <!-- Y轴数据 -->
        <div class="bubble-chart__y" :style="{ left: '-' + yLableWidth }">
            <span :style="{ width: yLableWidth }" v-for="row in yData" :key="row.value">{{ row.label }}</span>
        </div>
        <div class="bubble-chart__col" v-for="col in xData" :key="col.value">
            <div class="bubble-chart__item" v-for="row in yData" :key="row.value">
                <span
                    class="bubble"
                    :style="{
                        top: item && item[1] + 'px',
                        left: item && item[0] + 'px',
                        width: bubbleSize + 'px',
                        height: bubbleSize + 'px',
                        borderColor: bubbleBorderColor,
                        backgroundColor: bubbleBgColor
                    }"
                    v-for="(item, idx) in getBubble(col.value, row.value, randomArray)"
                    :key="idx"
                ></span>
            </div>
        </div>
    </div>
</template>
<script>
/**
 *  @property {String} 	bubbleBorderColor		泡泡边框颜色
 *  @property {String} 	bubbleBgColor		泡泡背景颜色
 *  @property {Number} 	bubbleSize		泡泡大小
 *  @property {Array} 	data	 圆点数据
 *  @property {String} 	width	 图例宽度
 *  @property {String} 	height	 图例高度
 *  @property {String} 	yLableWidth	 y轴label宽度
 *  @property {Array} 	xData	 x轴数据
 *  @property {Array} 	yData	 y轴数据
 */
export default {
    name: "bubble-chart",
    props: {
        bubbleBorderColor: {
            type: String,
            default: "#4090ff"
        },
        bubbleBgColor: {
            type: String,
            default: "rgba(64, 144, 255, 0.45)"
        },
        // 泡泡大小
        bubbleSize: {
            type: Number,
            default: 18
        },
        // 圆点数据
        data: {
            type: Array,
            default() {
                return [
                    {
                        // 列值  行值
                        area: ["0-9", "h"],
                        number: 2
                    },
                    {
                        area: ["0-9", "c"],
                        number: 1
                    },
                    {
                        area: ["10-50", "l"],
                        number: 10
                    },
                    {
                        area: [">50", "c"],
                        number: 6
                    }
                ];
            }
        },
        // 图表宽度
        width: {
            type: String,
            default: "500px"
        },
        // 图表高度
        height: {
            type: String,
            default: "300px"
        },
        // y轴label宽度
        yLableWidth: {
            type: String,
            default: "56px"
        },
        // x轴label
        xData: {
            type: Array,
            default() {
                return [
                    {
                        label: "0-9",
                        value: "0-9"
                    },
                    {
                        label: "10-50",
                        value: "10-50"
                    },
                    {
                        label: "50以上",
                        value: ">50"
                    }
                ];
            }
        },
        // y轴label
        yData: {
            type: Array,
            default() {
                return [
                    {
                        label: "高级",
                        value: "h"
                    },
                    {
                        label: "中级",
                        value: "c"
                    },
                    {
                        label: "低级",
                        value: "l"
                    }
                ];
            }
        }
    },
    data() {
        return {
            boxWidth: 0
        };
    },
    computed: {
        /**
         * 根据用户提供数量 随机渲染点
         */
        randomArray() {
            const { data, height, yLableWidth, yData, xData, boxWidth } = this;
            // 当外层盒子有宽度时 才往下继续执行
            if (boxWidth === 0) return [];
            const maxWidth = (boxWidth - parseInt(yLableWidth)) / xData.length;
            const maxHeight = parseInt(height) / yData.length;
            let ary = [];
            data.forEach((item) => {
                let points = [];
                for (var i = 0; i < item.number; i++) {
                    points.push([
                        this.randomNum(0, maxWidth - this.bubbleSize),
                        this.randomNum(0, maxHeight - this.bubbleSize)
                    ]);
                }
                ary.push({
                    area: item.area,
                    points
                });
            });
            return ary;
        }
    },
    methods: {
        /**
         * 获取当前为哪块区域下的点
         * @param {string} col 当前列值
         * @param {string} row 当前行值
         * @param {array} randomArray 用户指定数据渲染的点
         */
        getBubble(col, row, randomArray) {
            let bubbles = randomArray.find((item) => item.area[0] === col && item.area[1] === row);
            if (bubbles) {
                return bubbles.points;
            }
            return [];
        },
        /**
         * 生成随机数
         * @param {number} minNum 最小值
         * @param {number} maxNum 最大值
         */
        randomNum(minNum, maxNum) {
            switch (arguments.length) {
                case 1:
                    return parseInt(Math.random() * minNum + 1, 10);
                case 2:
                    return parseInt(Math.random() * (maxNum - minNum + 1) + minNum, 10);
                default:
                    return 0;
            }
        }
    },
    mounted() {
        this.$nextTick(() => {
            this.boxWidth = this.$refs["bubble-chart"].getBoundingClientRect().width;
        });
    }
};
</script>
<style lang="scss" scoped>
.bubble-chart {
    position: relative;
    display: flex;
    border: 1px solid rgba(0, 0, 0, 0.12);
    .bubble {
        position: absolute;
        display: inline-block;
        width: 18px;
        height: 18px;
        border: 1px solid #4090ff;
        background-color: rgba(64, 144, 255, 0.45);
        border-radius: 50%;
        cursor: pointer;
        transition: all 0.3s ease;
        &:hover {
            transform: scale(1.1);
        }
    }
    &__col {
        display: flex;
        flex-direction: column;
        flex-shrink: 0;
        flex: 1;
        border-right: 1px dashed rgba(0, 0, 0, 0.08);
        &:last-child {
            border-right: none;
        }
    }
    &__item {
        position: relative;
        flex-shrink: 0;
        flex: 1;
        border-bottom: 1px dashed rgba(0, 0, 0, 0.08);
        &:last-child {
            border-bottom: none;
        }
        &:hover {
            background-color: rgba(64, 144, 255, 0.05);
        }
    }
    &__x {
        position: absolute;
        bottom: -30px;
        left: 0;
        right: 0;
        display: flex;
        span {
            text-align: center;
            flex-shrink: 0;
            flex: 1;
            font-size: 14px;
            color: rgba(0, 0, 0, 0.65);
        }
    }
    &__y {
        position: absolute;
        top: 0;
        bottom: 0;
        display: flex;
        flex-direction: column;
        span {
            flex: 1;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            font-size: 14px;
            color: rgba(0, 0, 0, 0.65);
            padding-right: 10px;
            box-sizing: border-box;
        }
    }
}
</style>