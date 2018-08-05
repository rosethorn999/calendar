<template>
  <div class="main">    
    <table>
      <tr>
        <th v-for="(week,weekIndex) in DAY" :key="weekIndex">{{week|firstThree}}</th>
      </tr>
      <tr v-for="(week,weekIndex) in DDs" :key="weekIndex">
        <td v-for="(date,dayIndex) in week" :key="dayIndex" :class="{nowDate:date===now.DD}"><span>{{date}}</span></td>
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
    }
  },
  data: function() {
    return {
      DAY: dateText.DAY
    };
  },
  computed: {
    DDs: () => {
      let ret = [];
      for (let i = 0; i < 5; i++) {
        ret.push([]);
        for (let j = 0; j < 7; j++) {
          ret[i].push(i * 7 + j);
        }
      }
      return ret;
    }
  },
  filters: {
    firstThree: function(v) {
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
