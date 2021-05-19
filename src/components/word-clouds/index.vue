<!-- 
    词云图
    linxp
    2021.05.13
-->
<template>
    <div class="word-clouds">
        <word-cloud
            :data="data"
            nameKey="name"
            valueKey="value"
            :color="colors"
            :showTooltip="false"
            :rotate="rotate"
            :fontSize="[12, 18]"
            :wordClick="wordClickHandler"
        >
        </word-cloud>
    </div>
</template>
<script>
import wordCloud from "vue-wordcloud";
export default {
    name: "word-clouds",
    // 接收父组件传过来的属性
    props: {
        // 数据
        data: {
            type: Array,
            default() {
                return [{
                    name: "",
                    value: ""
                }];
            }
        },
        colors: {
            type: Array,
            default() {
                return ["#FC6464", "#FAAD14 ", "#13B8B8", "rgba(0,0,0,0.35)"];
            }
        },
        rotate: {
            type: Object,
            default() {
                return {
                    from: -60,
                    to: 60,
                    numOfOrientation: 5
                };
            }
        }
    },
    data() {
        return {};
    },
    methods: {
        wordClickHandler(name, value, vm) {
            console.log("wordClickHandler", name, value, vm);
        }
    },
    components: {
        wordCloud
    }
};
</script>
<style lang="scss" scoped>
.word-clouds {
    width: 100%;
    height: 300px;
    border: 1px solid rgba(0, 0, 0, 0.12);
    // overflow: hidden;
    
}

 .wordCloud {
        height: 100%;
    }
</style>