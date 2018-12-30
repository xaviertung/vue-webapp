<template>
  <div class="container">
    <header>
      <div class="prevous" v-on:click="decrease">上个月</div>
      <div class="year">{{year}}</div>
      <div class="month">{{month}}</div>
      <div class="next" v-on:click="increase">下个月</div>
    </header>
    <table style="width:90%;">
      <thead>
        <tr>
          <th>日</th>
          <th>一</th>
          <th>二</th>
          <th>三</th>
          <th>四</th>
          <th>五</th>
          <th>六</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowIndex) in allDates" :key="rowIndex">
          <td
            v-for="(value, key, colIndex) in row"
            :key="colIndex"
            :style="{
              color: value.type=== 'current' &&
              year === thisYear &&
              month === thisMonth &&
              value.date === today ? 'red' : 'black'}"
            :class="value.type === 'current' ? 'current' : value.type === 'prev' ? 'prev' : 'post'"
          >
            {{value.date}}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<style scoped>
.container {
  font-size: 20px;
}
.prevous {
  font-size: 1.2em;
  display: inline-block;
}
.year {
  font-size: 1.2em;
  display: inline-block;
}
.month {
  font-size: 1.2em;
  display: inline-block;
}
.next {
  font-size: 1.2em;
  display: inline-block;
}
.prev {
  background: #8f8f8f;
}
.current {
  background: #fff;
}
.post {
  background: lightgreen;
}
</style>

<script>
export default {
  name: 'Calender',
  data() {
    const now = new Date();
    const today = now.getDate();
    const year = now.getFullYear();
    const month = now.getMonth() + 1;
    return {
      today,
      year,
      month,
      thisYear: year,
      thisMonth: month,
    };
  },
  computed: {
    allDates() {
      return this.getDatesByMonth(this.year, this.month);
    },
  },
  methods: {
    increase() {
      if (this.month === 12) {
        this.year += 1;
        this.month = 1;
      } else {
        this.month += 1;
      }
    },
    decrease() {
      if (this.month === 1) {
        this.year -= 1;
        this.month = 12;
      } else {
        this.month -= 1;
      }
    },
    getDatesByMonth: (year, originMonth) => {
      const month = originMonth - 1;
      const prevousMonth = {
        lastDate: new Date(year, month, 0).getDate(),
      };

      const currentLastDate = new Date(
        month === 11 ? year + 1 : year, month === 11 ? 0 : month + 1, 0,
      ).getDate();
      const currentMonth = {
        lastDate: currentLastDate,
        firstDatePosition: new Date(year, month, 1).getDay(),
        lastDatePosition: new Date(year, month, currentLastDate).getDay(),
      };


      const dateCount = currentMonth.firstDatePosition
                      + currentMonth.lastDate
                      + (7 - currentMonth.lastDatePosition - 1);

      const allDates = [];

      for (let i = 0; i < dateCount / 7; i += 1) {
        allDates[i] = [];
      }

      let rowNum = 0;
      let colNum = 0;

      const pushDate = (currentDate) => {
        allDates[rowNum][colNum] = currentDate;
        colNum += 1;
        if (colNum === 7) {
          colNum = 0;
          rowNum += 1;
        }
      };

      // 0 1 2 3 4 5 6
      // 30-5-0    30-5+5
      // 252627282930

      for (let i = 0; i < currentMonth.firstDatePosition; i += 1) {
        const currentDate = {
          type: 'prev',
          date: prevousMonth.lastDate - (currentMonth.firstDatePosition - 1) + i,
        };

        pushDate(currentDate);
      }

      for (let i = 1; i <= currentMonth.lastDate; i += 1) {
        const currentDate = {
          type: 'current',
          date: i,
        };
        pushDate(currentDate);
      }

      // 0 1 2 3 4 5 6
      //     2-1     6-1
      // 30310102030405

      for (let i = currentMonth.lastDatePosition + 1; i <= 6; i += 1) {
        const currentDate = {
          type: 'post',
          date: i - currentMonth.lastDatePosition,
        };
        pushDate(currentDate);
      }

      return allDates;
    },
  },
};
</script>
