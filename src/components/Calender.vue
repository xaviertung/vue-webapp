<template>
  <div>
    <table>
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
    const year = 2018;
    const month = 11;
    const allDates = this.getDatesByMonth(year, month);
    return {
      allDates,
    };
  },
  methods: {
    getDatesByMonth: (year, month) => {
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
