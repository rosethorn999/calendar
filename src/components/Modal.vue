<template>
  <div class="main" v-show="show" >
      <div v-show="show" class="modalMain">
          <div class="modalHeader">
            {{pkg.header}}
          </div>
          <div class="modalBody">
            {{pkg.body}}
          </div>
          <div class="modalFooter">
            {{pkg.footer}}
          </div>
      </div>
      <div class="modalBG"></div>
  </div>  
</template>

<script>
export default {
  name: "Modal",
  props: {
    show: { type: Boolean, dafault: true },
    pkg: {
      type: Object,
      default: function() {
        return {
          header: "hhh",
          body: "bbb",
          footer: "fff"
        };
      }
    }
  },
  data: function() {
    return {};
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
* {
  margin: 0px;
}
.main {
  width: 100%;
  height: 100%;
}
.modalBG {
  z-index: 1400;
  width: 100%;
  height: 100%;
  background: #00000038;
  position: fixed;
}

.modalMain {
  cursor: pointer;
  z-index: 1401;
  position: absolute;
  margin: 0px auto;
  width: 500px;
  top: 20%;
  left: calc(50% - 250px);
}
.modalHeader {
  background: #74b3ce;
  height: 20px;
}
.modalBody {
  background: #508991;
  height: 300px;
}
.modalFooter {
  background: #09bc8a;
  height: 20px;
}
</style>
