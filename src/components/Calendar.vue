<template>
  <div class="main">
    <table>
      <tr>
        <th v-for="(week,weekIndex) in DAY" :key="weekIndex">{{week|firstThree}}</th>
      </tr>
      <tr v-for="(week,weekIndex) in DDs" :key="weekIndex">
        <td v-for="(date,dayIndex) in week" :key="dayIndex" :class="{nowDate:date===now.DD&&now.MM===viewMonth.MM&&now.YYYY===viewMonth.YYYY}">
          <span>{{date}}</span>
        </td>        
      </tr>
    </table>
  </div>
</template>

<script>
import dateText from "../assets/dateText.json";

export default {
  name: "Calendar",
  props: {
    now: {
      type: Object,
      default: function() {
        return {
          YYYY: 2018,
          MM: 0,
          DD: 1,
          DAY: 0
        };
      }
    },
    viewMonth: {
      type: Object,
      default: function() {
        return {
          YYYY: 2018,
          MM: 0
        };
      }
    }
  },
  data: function() {
    return {
      DAY: dateText.DAY
    };
  },
  computed: {
    DDs() {
      let ret = [];
      const d = new Date(this.viewMonth.YYYY, this.viewMonth.MM, 1);
      const dayOfMonthStart = d.getDay();
      const dateEndOfThisMonth = new Date(
        this.now.YYYY,
        this.now.MM + 1, //todo<bug>:遇到12月會炸裂
        0
      ).getDate();

      ret.push([]);
      let weekCount = 0; //週數
      //add rest date that before start of this month and need to show in this month.
      const dateEndOfLastMonth = new Date(
        this.now.YYYY,
        this.now.MM - 1, //todo<bug>:遇到1月會炸裂
        0
      ).getDate();
      for (let i = 0; i < dayOfMonthStart; i++) {
        let v = dateEndOfLastMonth - dayOfMonthStart + i + 1;
        ret[weekCount].push(v);
      }

      //start date of this month
      let v = 1;
      do {
        ret[weekCount].push(v);
        if (ret[weekCount].length === 7) {
          ret.push([]);
          weekCount += 1;
        }
        v += 1;
      } while (v <= dateEndOfThisMonth);

      //add rest date after end of this month
      v = 1;
      do {
        ret[weekCount].push(v);
        v += 1;
      } while (ret[weekCount].length < 7);

      return ret;
    }
  },
  filters: {
    firstThree(v) {
      return v.substring(0, 3);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
* {
  margin: 0px;
}
.main {
  float: left;
  height: 85vh;
  width: 75%;
}
table {
  table-layout: fixed;
  border-collapse: collapse;
  width: 100%;
  height: 100%;
}
th {
  height: 20px;
  border-top: 0px;
  border-right: 1px solid #e0e0e0;
  border-left: 0px;
}
tr {
  padding: 0px;
}
td {
  padding: 0px;
  border-top: 0px;
  border-right: 1px solid #e0e0e0;
  border-bottom: 1px solid #e0e0e0;
  border-left: 0px;
  width: 14%;
}
.nowDate {
  span {
    background: #23ce7b;
  }
}
</style>
