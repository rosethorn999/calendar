<template>
  <div class="main">
    <table>
      <tr>
        <th v-for="(week,weekIndex) in DAY" :key="weekIndex">{{week|firstThree}}</th>
      </tr>

      <tr v-for="(week,weekIndex) in DDs" :key="weekIndex">
        <td v-for="(date,dayIndex) in week" :key="dayIndex"
          :class="{nowDate:date.DD===now.DD&&now.MM===viewDate.MM&&now.YYYY===viewDate.YYYY,notThisMontth:date.MM!==viewDate.MM}">
          <span>{{date.DD}}</span>
          <ul>
            <li v-for="(item,index) in getEventByDate(date)" :key="item.guid" :class="{selected:item.selected}"
              @mouseover="showInfo(index)">{{item.name}}</li>
          </ul>
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
    viewDate: {
      type: Object,
      default: function() {
        return {
          YYYY: 2018,
          MM: 0,
          DD: 0
        };
      }
    }
  },
  data: function() {
    return {
      DAY: dateText.DAY,
      events: {
        "20180729": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false }
        ],
        "20180807": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false },
          { name: "task3", guid: "task3", selected: false }
        ],
        "20180808": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false }
        ],
        "20180901": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false }
        ],
        "20180908": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false }
        ]
      }
    };
  },
  computed: {
    DDs() {
      let ret = [];
      const d = new Date(this.viewDate.YYYY, this.viewDate.MM, 1);
      const dayOfMonthStart = d.getDay();
      const d_ofThisMonth = new Date(
        this.viewDate.YYYY,
        this.viewDate.MM + 1, //todo<bug>:遇到12月會炸裂
        0
      );
      const DDDD_ofThisMonth = d_ofThisMonth.getDate();

      ret.push([]);
      let weekCount = 0; //週數
      //add rest date that before start of this month and need to show in this month.
      const d_ofLastMonth = new Date(
        this.viewDate.YYYY,
        this.viewDate.MM, //todo<bug>:遇到1月會炸裂
        0
      );
      const DDDD_ofLastMonth = d_ofLastMonth.getDate();
      for (let i = 0; i < dayOfMonthStart; i++) {
        let YYYY = d_ofLastMonth.getFullYear();
        let MM = d_ofLastMonth.getMonth();
        let DD = DDDD_ofLastMonth - dayOfMonthStart + i + 1;
        ret[weekCount].push({ YYYY: YYYY, MM: MM, DD: DD });
      }

      //start date of this month
      let v = 1;
      do {
        let YYYY = this.viewDate.YYYY;
        let MM = this.viewDate.MM;
        let DD = v;
        ret[weekCount].push({ YYYY: YYYY, MM: MM, DD: DD });
        if (ret[weekCount].length === 7) {
          ret.push([]);
          weekCount += 1;
        }
        v += 1;
      } while (v <= DDDD_ofThisMonth);

      //add rest date after end of this month
      v = 1;
      do {
        let YYYY = this.viewDate.YYYY;
        let MM = this.viewDate.MM + 1;
        let DD = v;
        ret[weekCount].push({ YYYY: YYYY, MM: MM, DD: DD });
        v += 1;
      } while (ret[weekCount].length < 7);

      return ret;
    }
  },
  filters: {
    firstThree(v) {
      return v.substring(0, 3);
    }
  },
  methods: {
    getEventByDate(dateObj) {
      const YYYYMMDD =
        dateObj.YYYY +
        ("0" + (dateObj.MM + 1)).slice(-2) + //need+1; because:"20180808"={YYYY:2018,MM:7,DD:8}
        ("0" + dateObj.DD).slice(-2);
      console.log(YYYYMMDD);
      return this.events[YYYYMMDD];
    },
    showInfo() {}
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
td.notThisMontth {
  opacity: 0.3;
}
.nowDate {
  span {
    background: #23ce7b;
  }
}
</style>
