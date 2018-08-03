<template>
  <div class="main">    
    <table>
      <tr>
        <th v-for="(week,weekIndex) in WW" :key="weekIndex">{{week|firstThree}}</th>
      </tr>
      <tr v-for="(week,weekIndex) in DDs" :key="weekIndex">
        <td v-for="(day,dayIndex) in week" :key="dayIndex">{{day}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import MMWW from "../assets/MMWW.json";

export default {
  name: "Calendar",
  props: {},
  data: function() {
    return {
      WW: MMWW.WW
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
</style>
