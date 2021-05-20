<template>
    <div class="side" :style="{
        top,
        right
    }">
        <div class="side-nav">
            <span
                v-for="nav in navs"
                :class="{ 'is-active': active === nav.id }"
                :key="nav.id"
                @click="handleScroll(nav.id)"
                :title="nav.label"
                >{{ nav.label }}</span
            >
        </div>
    </div>
</template>
<script>
export default {
    name: "side-nav",
    props: {
        navs: {
            type: Array,
            default() {
                return [
                    {
                        id: "nav1",
                        label: "Radio单选"
                    },
                    {
                        id: "nav2",
                        label: "排名"
                    },
                    {
                        id: "nav3",
                        label: "列表信息"
                    }
                ];
            }
        },
        top: {
            type: String,
            default: '100px'
        },
        right: {
            type: String,
            default: '50px'
        }
    },
    data() {
        return {
            active: "nav1",
            offsetAry: []
        };
    },
    computed: {
        pDom() {
            return this.$parent.$el;
        }
    },
    methods: {
        handleScroll(id) {
            document.getElementById(id).scrollIntoView({ behavior: "smooth" });
            this.active = id;
        },
        /**
         * 页面滚动事件
         */
        onScroll() {
            const vm = this;
            // 获取滚动条top
            let { isScrollTop } = vm.vmScrollTop;
            // 设置偏移量
            isScrollTop = isScrollTop + 100;
            let { offsetAry } = vm;
            let total = offsetAry.length;
            let item = offsetAry.find(
                (v, idx) => v.top <= isScrollTop && offsetAry[idx + 1] && offsetAry[idx + 1].top > isScrollTop
            );

            this.active = (item && item.id) || offsetAry[total - 1].id;
        },
        // 获取用户锚点位置集合
        queryDomTop() {
            const vm = this;
            const { navs } = vm;
            navs.forEach((v) => {
                let dom = document.getElementById(v.id);
                vm.offsetAry.push({
                    top: dom.offsetTop,
                    id: v.id
                });
            });
        }
    },
    mounted() {
        this.queryDomTop();
    }
};
</script>
<style lang="scss" scoped>
.side {
    position: fixed;
    top: 100px;
    right: 50px;
    z-index: 99999;
    padding: 16px 25px;
    background-color: #fff;
    box-shadow: 0 2px 12px 0 rgba(0,0,0,.1);
    max-width: 200px;
}
.side-nav {
    padding: 0;
    display: flex;
    flex-direction: column;
    border-left: 1px solid rgba(0, 0, 0, 0.09);
    span {
        font-size: 14px;
        color: rgba(0, 0, 0, 0.65);
        margin-bottom: 14px;
        padding-left: 15px;
        line-height: 1;
        cursor: pointer;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        &:hover{
            color: #1890ff;
        }
        &.is-active {
            position: relative;
            color: #1890ff;
            &::before {
                content: "";
                position: absolute;
                top: 0;
                left: -1.5px;
                width: 3px;
                height: 100%;
                background: #1890ff;
                border-radius: 2px;
            }
        }
        &:last-child {
            margin-bottom: 0;
        }
    }
}
</style>