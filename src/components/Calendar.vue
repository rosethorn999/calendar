<template>
  <div class="main">
    <table class="days">
      <tr>
        <th v-for="(week,weekIndex) in DAY" :key="weekIndex">{{week|firstThree}}</th>
      </tr>
    </table>
    <table class="dates">
      <tr v-for="(week,weekIndex) in DDs" :key="weekIndex" class="dateTR">
        <td v-for="(date,dayIndex) in week" :key="dayIndex"
          :class="{nowDate:date.DD===now.DD&&now.MM===viewDate.MM&&now.YYYY===viewDate.YYYY , notThisMontth:date.MM!==viewDate.MM}"
          @click="changeSelectedDate(date)">
          <div>
            <span>{{date.DD}}</span>
          </div>
          <div class="eventContainer">
            <ul class="eventItemArea">
              <li v-for="(item,index) in getEventByDate(date)"
                v-if="index<=maxContainIndex" :key="item.guid" :class="{doneEvnet:item.done}" :style="{'background':'#'+colors[item.type]}">
                <abbr :title="item.note">{{item.title}}</abbr>
              </li>
            </ul>
          </div>
          <div>
            <span v-if="getEventByDate(date).length>maxContainIndex">More</span>
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import colors from "../assets/colors.json";
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
    },
    events: {
      type: Object,
      default: function() {
        return {};
      }
    }
  },
  data: function() {
    return {
      interval: null,
      maxContainIndex: 99,
      colors: colors,
      DAY: dateText.DAY
    };
  },
  computed: {
    DDs() {
      let ret = [];
      if (this.viewDate.YYYY !== null && this.viewDate.MM !== null) {
        const d = new Date(this.viewDate.YYYY, this.viewDate.MM, 1);
        const dayOfMonthStart = d.getDay();
        const d_ofThisMonth = new Date(
          this.viewDate.YYYY,
          this.viewDate.MM + 1,
          0
        );
        const DDDD_ofThisMonth = d_ofThisMonth.getDate();

        ret.push([]);
        let weekCount = 0; //週數
        //add rest date that before start of this month and need to show in this month.
        const d_ofLastMonth = new Date(this.viewDate.YYYY, this.viewDate.MM, 0);
        const DDDD_ofLastMonth = d_ofLastMonth.getDate();
        for (let i = 0; i < dayOfMonthStart; i++) {
          let YYYY = d_ofLastMonth.getFullYear();
          let MM = d_ofLastMonth.getMonth();
          let DD = DDDD_ofLastMonth - dayOfMonthStart + i + 1;
          let DAY = ret[weekCount].length; //DAY same as index of this array's position
          ret[weekCount].push({ YYYY: YYYY, MM: MM, DD: DD, DAY: DAY });
        }

        //start date of this month
        let v = 1;
        do {
          let YYYY = this.viewDate.YYYY;
          let MM = this.viewDate.MM;
          let DD = v;
          let DAY = ret[weekCount].length; //DAY same as index of this array's position
          ret[weekCount].push({ YYYY: YYYY, MM: MM, DD: DD, DAY: DAY });
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
          let DAY = ret[weekCount].length; //DAY same as index of this array's position
          ret[weekCount].push({ YYYY: YYYY, MM: MM, DD: DD, DAY: DAY });
          v += 1;
        } while (ret[weekCount].length < 7);
      }

      return ret;
    }
  },
  watch: {
    viewDate() {
      this.calcEventHeight();
    }
  },
  filters: {
    firstThree(v) {
      return v.substring(0, 3);
    }
  },
  methods: {
    getEventByDate(dateObj) {
      let ret = [];
      const YYYYMMDD =
        dateObj.YYYY +
        ("0" + (dateObj.MM + 1)).slice(-2) + //need+1; because:"20180808"={YYYY:2018,MM:7,DD:8}
        ("0" + dateObj.DD).slice(-2);
      ret = this.events[YYYYMMDD] || [];
      return ret;
    },
    changeSelectedDate(dateObj) {
      this.$emit("changeSelectedDate", dateObj);
    },
    calcEventHeight() {
      if (this.interval) {
        clearInterval(this.interval);
      }
      this.interval = setInterval(() => {
        let container = document.querySelector(".eventContainer");
        let event = document.querySelector(".eventItemArea>li");
        if (container && event) {
          this.maxContainIndex =
            container.offsetHeight / event.offsetHeight - 1;
          clearInterval(this.interval);
        }
      }, 250);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.main {
  float: left;
  height: 85vh;
  width: 77.5%;
}
table {
  table-layout: fixed;
  border-collapse: collapse;
  width: 100%;
  &.days {
    height: 5%;
  }
  &.dates {
    height: 95%;
  }
  th {
    border-top: 0px;
    border-right: 1px solid #e0e0e0;
    border-left: 0px;
  }
  tr {
    vertical-align: top;
    max-height: 20%;
  }
  td {
    border-top: 0px;
    border-right: 1px solid #e0e0e0;
    border-bottom: 1px solid #e0e0e0;
    border-left: 0px;
  }
  .dateTR {
    td span {
      margin: 5px;
    }
  }
  .nowDate {
    span {
      background: #23ce7b;
      padding: 2px;
      border-radius: 50%;
      color: white;
    }
  }
  td.notThisMontth {
    opacity: 0.3;
  }
  td > div:first {
    height: 20px;
  }
  td > div:nth-child(2) {
    overflow: hidden;
    height: calc(100% - 42px);
  }
  td > div:last-child {
    height: 20px;
    text-align: center;
    color: #a4a4a4;
    font-size: 0.8em;
  }

  .eventItemArea {
    display: block;
    margin-top: 2%;
    height: 40px;
    li {
      color: white;
      font-size: 0.8em;
      border-radius: 5px;
      padding: 0px 15px;
      text-align: end;
      &.doneEvnet {
        text-decoration: line-through;
      }
    }
  }
}
</style>
