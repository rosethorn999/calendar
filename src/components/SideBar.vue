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
        <span class="buttonCheck">
          <i class="fas fa-check"></i>
        </span>
        <span @click="removeEvent" class="buttonLess" :class="{disabled:!anyOneSelected}">
          <i class="fas fa-minus-circle"></i>
        </span>
        <span @click="addEvent" class="buttonAdd">
          <i class="fas fa-plus-circle"></i>
        </span>
      </div>
      <ul class="eventItemArea">
        <li v-for="(item,index) in events[YYYYMMDD]" :key="item.guid" :class="{selected:item.selected}" :style="{'background':'#'+colors[item.type]}"
          @click="setAsSelected(index)">
          <i class="fas fa-check"></i>
            <abbr :title="item.note">{{item.title}}</abbr>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import colors from "../assets/colors.json";
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
    return {
      colors: colors,
      clicks: 0,
      clickTimer: null
    };
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
      this.clicks++;
      if (this.clicks === 1) {
        this.clickTimer = setTimeout(() => {
          this.events[this.YYYYMMDD][i].selected = !this.events[this.YYYYMMDD][
            i
          ].selected;
          console.log("single");
          this.clicks = 0;
        }, 200);
      } else {
        clearTimeout(this.clickTimer);
        console.log("db");
        this.$emit("editEvent", this.events[this.YYYYMMDD][i]);
        this.clicks = 0;
      }
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
.main {
  color: #fff;
  float: left;
  height: 100vh;
  width: 22.5%;
  background: #23ce7b;
}
.dateDisplay {
  margin-top: 30px;
  height: calc(30vh - 30px);
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
  height: 70vh;
  h3 {
    display: inline-block;
  }
  .eventButtonArea {
    float: right;
    @mixin buttonCSS {
      cursor: pointer;
      border-radius: 50%;
      margin-left: 10px;
      font-size: 26px;
      font-weight: bold;
    }
    .buttonCheck {
      color: #f9ee00;
      @include buttonCSS;
    }
    .buttonLess {
      color: red;
      @include buttonCSS;
    }
    .buttonAdd {
      color: white;
      @include buttonCSS;
    }
    .disabled {
      opacity: 0.3;
      cursor: no-drop;
    }
  }
  .eventItemArea {
    margin-top: 10%;
    height: 90%;
    overflow: auto;
    li {
      margin-top: 3%;
      border-radius: 5px;
      padding: 3px 15px;
      text-align: end;
      background: #000000;
      i{
        float: left;
      }
    }
  }
}

.selected {
  opacity: 0.3;
}
</style>
