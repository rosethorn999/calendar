<template>
  <div class="main">
  <button @click="MMModify(false)">LeftIcon</button>
  <button @click="MMModify(true)">RightIcon</button>
  <select>
    <option>Month</option>
  </select>
  <span>{{now.YYYY}}</span>
  <div class="monthSwitcher">
    <div v-for="(item,index) in MMs" :key="index" :class="{selected:index===viewMonth.MM}">{{item | firstThree}}</div>
  </div>
  </div>
</template>

<script>
import dateText from "../assets/dateText.json";
export default {
  name: "MenuBar",
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
      MMs: dateText.MM
    };
  },
  filters: {
    firstThree: function(v) {
      return v.substring(0, 3);
    }
  },
  methods: {
    MMModify(goNext) {
      this.$emit("MMModify", goNext);
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
  box-sizing: border-box;
  float: left;
  height: 15vh;
  width: 75%;
  border-bottom: 1px solid #23ce7b;
}
.monthSwitcher {
  text-align: center;
  display: flex;
  width: 100%;
}
.monthSwitcher > div {
  font-weight: bold;
  flex: 1;
}
.selected {
  color: #23ce7b;
}
</style>
