<template>
  <div class="main">
    <h2>{{viewDate.MM|monthText}}</h2>
    <h2>{{viewDate.DAY|dayText}}</h2>
    <h2>{{viewDate.DD}}</h2>
    <div>
      Event
      <input type="button" value="-" :disabled="!anyOneSelected" @click="removeEvent">
      <input type="button" value="+" @click="addEvent">
      <ul>
        <li v-for="(item,index) in events" :key="item.guid" :class="{selected:item.selected}"
        @click="setAsSelected(index)">{{item.name}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import dateText from "../assets/dateText.json";

export default {
  name: "SideBar",
  props: {
    events: {
      type: Array,
      default: function() {
        return [];
      }
    },
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
    return {};
  },
  computed: {
    anyOneSelected: function() {
      let ret = false;
      let eList = this.events;
      for (let i = 0; i < eList.length; i++) {
        if (eList[i].selected === true) {
          ret = true;
          break;
        }
      }
      return ret;
    }
  },
  filters: {
    monthText: function(v) {
      let ret;
      const MMList = dateText.MM;
      ret = MMList[v];
      return ret;
    },
    dayText: function(v) {
      let ret;
      const MMList = dateText.DAY;
      ret = MMList[v];
      return ret;
    }
  },
  methods: {
    addEvent: function() {
      this.$emit("addEvent");
    },
    removeEvent: function() {
      let eList = this.events;
      for (let i = eList.length - 1; i >= 0; i--) {
        const shouldBeKilled = eList[i].selected === true;
        if (shouldBeKilled) {
          this.events.splice(i, 1);
        }
      }
    },
    setAsSelected: function(i) {
      this.events[i].selected = !this.events[i].selected;
    },
    getGuid: function() {
      return "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(/[xy]/g, function(
        c
      ) {
        var r = (Math.random() * 16) | 0,
          v = c == "x" ? r : (r & 0x3) | 0x8;
        return v.toString(16);
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
* {
  margin: 0px;
}
h2 {
  text-align: center;
}
.main {
  color: #fff;
  float: left;
  height: 100vh;
  width: 25%;
  background: #23ce7b;
}
.selected {
  opacity: 0.3;
}
</style>
