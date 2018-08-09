<template>
  <div class="main">
    <div class="dateDisplay">
      <h2>{{viewDate.MM|monthText}}</h2>
      <h2>{{viewDate.DAY|dayText}}</h2>
      <h2>{{viewDate.DD}}</h2>
    </div>
    <div class="eventArea">
      <h3>Event</h3>
      <div class="eventButtonArea">
        <input type="button" value="-" :disabled="!anyOneSelected" @click="removeEvent" class="buttonLess">
        <input type="button" value="+" @click="addEvent" class="buttonAdd">
      </div>
      <ul class="eventItemArea">
        <li v-for="(item,index) in events[YYYYMMDD]" :key="item.guid" :class="{selected:item.selected}"
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
      type: Object,
      default: function() {
        return {};
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
          DD: 0,
          DAY: 0
        };
      }
    }
  },
  data: function() {
    return {};
  },
  computed: {
    YYYYMMDD() {
      return (
        this.viewDate.YYYY +
        ("0" + (this.viewDate.MM + 1)).slice(-2) +
        ("0" + this.viewDate.DD).slice(-2)
      );
    },
    anyOneSelected: function() {
      let ret = false;

      let eList = this.events[this.YYYYMMDD];
      if (eList) {
        //avoid this.YYYYMMDD has not been calculated
        for (let i = 0; i < eList.length; i++) {
          if (eList[i].selected === true) {
            ret = true;
            break;
          }
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
      let eList = this.events[this.YYYYMMDD];
      for (let i = eList.length - 1; i >= 0; i--) {
        const shouldBeKilled = eList[i].selected === true;
        if (shouldBeKilled) {
          this.events[this.YYYYMMDD].splice(i, 1);
        }
      }
    },
    setAsSelected: function(i) {
      this.events[this.YYYYMMDD][i].selected = !this.events[this.YYYYMMDD][i]
        .selected;
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
  list-style: none;
}
.main {
  color: #fff;
  float: left;
  height: 100vh;
  width: 22.5%;
  background: #23ce7b;
}
.dateDisplay {
  margin-top: 30px;
  h2 {
    text-align: center;
    margin-top: 10px;
    font-size: 2em;
  }
  h2:last-child {
    font-size: 4em;
  }
}
.eventArea {
  box-sizing: border-box;
  padding: 10% 15%;
  h3 {
    display: inline-block;
  }
  .eventButtonArea {
    float: right;
    @mixin buttonCSS {
      border: none;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      margin-left: 10px;
      font-size: 26px;
      line-height: 26px;
      color: #23ce7b;
      font-weight: bold;
    }
    .buttonLess {
      background: red;
      @include buttonCSS;
    }
    .buttonAdd {
      background: white;
      @include buttonCSS;
    }
  }
  .eventItemArea {
    margin-top: 10%;
    li {
      margin-top: 3%;
      border-radius: 5px;
      padding: 3px 15px;
      text-align: end;
      background: #000000;
    }
  }
}

.selected {
  opacity: 0.3;
}
</style>
