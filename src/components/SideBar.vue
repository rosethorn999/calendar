<template>
  <div class="main">
    <h2>{{MMText}}</h2>
    <h2>{{WWText}}</h2>
    <h2>{{DD}}</h2>
    <div>
      Event
      <input type="button" value="-">
      <input type="button" value="+">
      <ul>
        <li v-for="(item,index) in events" :key="index">{{item.name}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import MMWW from "../assets/MMWW.json";

export default {
  name: "SideBar",
  props: {},
  data: function() {
    return {
      MM: 8,
      WW: 4,
      DD: 3,
      events: [{ name: "task1" }, { name: "task2" }, { name: "task3" }]
    };
  },
  computed: {
    MMText: function() {
      let ret;
      const MMList = MMWW.MM;
      ret = MMList[this.MM];
      return ret;
    },
    WWText: function() {
      let ret;
      const WWList = MMWW.WW;
      ret = WWList[this.WW];
      return ret;
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
</style>
