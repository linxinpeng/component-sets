<template>
  <div class="calendar">
    <div class="calendar__action">
      <el-button size="mini" @click="currentYear--">上一年</el-button>
      <el-button size="mini" @click="currentYear++">下一年</el-button>
      <el-button size="mini" @click="monthAdd(-1)">上一月</el-button>
      <el-button size="mini" @click="monthAdd(1)">下一月</el-button>
      <div class="calendar__date">{{ currentYear }}-{{ formatNum(currentMonth) }}</div>
    </div>
    <div class="calendar__header">
      <span
        class="calendar__header-item"
        v-for="(week, index) in weeks"
        :key="index"
      >
        {{ week }}
      </span>
    </div>
    <div class="calendar__body">
      <span class="calendar__item" v-for="item in beforeNum" :key="item"></span>
      <span :class="['calendar__item', today === (currentYear + '-' + currentMonth + '-' + d) && 'is-today']" v-for="d in days" :key="d + 7">{{ d }}</span>
    </div>
  </div>
</template>
<script>
const date = new Date();
export default {
  name: "calendar",
  data() {
    return {
      weeks: ["日", "一", "二", "三", "四", "五", "六"],
      // 当月总计多少天
      dayTotal: 0,
      days: [],
      // 补齐空位
      beforeNum: 0,
      
      today: date.getFullYear() + '-' + Number(date.getMonth() + 1) + '-' + date.getDate(),

      currentWeek: 0,
      currentYear: date.getFullYear(),
      currentMonth: Number(date.getMonth()) + 1,
    };
  },
  computed: {
    yearAndMonth() {
      return this.currentYear + "-" + this.currentMonth;
    },
  },
  watch: {
    yearAndMonth: {
      handler(val) {
        this.dayTotal = this.getDayTotal(this.currentYear, this.currentMonth);
        this.days = this.getArray(1, this.dayTotal);
        this.currentWeek = this.getWeek(val);
        this.beforeNum = this.currentWeek;
      },
      immediate: true,
    },
  },
  methods: {
    monthAdd(num) {
      this.currentMonth += num;
      if (num > 0 && this.currentMonth === 13) {
        this.currentMonth = 1;
        this.currentYear++;
      } else {
        if (this.currentMonth === 0) {
          this.currentMonth = 12;
          this.currentYear--;
        }
      }
    },
    /**
     * 不足10补0
     */
    formatNum(num) {
        return num < 10 ? '0' + num : num + '';
    },
    /**
     * 获取某个数值到某个数值之间的数组
     */
    getArray(start, end) {
      return Array.from(new Array(end + 1).keys()).slice(start);
    },
    /**
     * 获取当月1号为周几
     */
    getWeek(val) {
      const dt = new Date(val);
      return dt.getDay();
    },
    /**
     * 获取当月总计多少天
     */
    getDayTotal(y, m) {
      return new Date(y, m, 0).getDate();
    },
  },
};
</script>
<style lang="scss" scoped>
.calendar {
  width: 100%;
  &__header {
    display: flex;
    width: 100%;
    border-bottom: 1px solid rgba($color: #000000, $alpha: 0.09);
    &-item {
      flex: 1;
      text-align: center;
      height: 50px;
      line-height: 50px;
    }
  }
  &__body {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
  }
  &__item {
    width: calc(100% / 7);
    text-align: center;
    height: 50px;
    line-height: 50px;
  }
  &__action {
    display: flex;
    align-items: center;
    span {
      text-align: center;
      height: 50px;
      line-height: 50px;
    }
  }
  &__date{
      margin-left: auto;
  }
}
.is-today{
    color: #1989fa;
}
</style>