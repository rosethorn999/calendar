<template>
  <div class="main">
  <button @click="MMModify(false)" class="buttonLeft"><i class="fas fa-chevron-left"></i></button>
  <button @click="MMModify(true)" class="buttonRight"><i class="fas fa-chevron-right"></i></button>
  <select class="optionDate">
    <option>Month</option>
  </select>
  <span class="displayYear">{{viewDate.YYYY}}</span>
  <div class="monthSwitcher">
    <div v-for="(item,index) in MMs" :key="index" :class="{selected:index===viewDate.MM}">{{item | firstThree}}</div>
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
    viewDate: {
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
  width: 77.5%;
  border-bottom: 1px solid #23ce7b;
  position: relative;
  .buttonLeft,
  .buttonRight {
    border: none;
    background: transparent;
    font-size: 2.2em;
    margin: 1% 2%;
    cursor: pointer;
  }
  .optionDate {
    position: absolute;
    right: 0;
    margin: 2.5% 150px;
  }
  .displayYear {
    font-size: 2.5em;
    position: absolute;
    right: 0;
    margin: 1% 30px;
  }
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
