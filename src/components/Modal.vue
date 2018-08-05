<template>
  <div class="main" v-show="show" >
      <div v-show="show" class="modalMain">
          <div class="modalHeader">
            <!-- {{pkg.header}} -->
          </div>
          <div class="modalBody">
            <div class="modalDate">
              <p>Date:</p>
              <select class="dateYears" v-model.number="YYYY">>
                <option :value="2017">2017</option>
                <option :value="2018">2018</option>
                <option :value="2019">2019</option>
              </select>
              <select class="dateMonth" v-model.number="MM">
                <option v-for="MM in 12" :key="MM" :value="MM">{{MM}}</option>
              </select>
              <select class="dateDay" v-model.number="DD">
                <option v-for="DD in 31" :key="DD" :value="DD">{{DD}}</option>
              </select>
            </div>
            <div class="modalCaption">
              <p>Title:</p>
              <input type="text" v-model="title">
            </div>
            <div class="modalColorPicker">
              <p>Type:</p>
              <ul>
                <li v-for="(c,index) in colors" :style="{background:'#'+c}" :key="c" @click="type=index"></li>              
              </ul>
            </div>
            <div class="modalRemarks">
              <p>Note:</p>
              <input class="modalRemarksBox" type="input" v-model="note">
            </div>
            
            <!-- {{pkg.body}} -->
          </div>
          <div class="modalFooter">
            <!-- {{pkg.footer}} -->
            <div class="modelButtons">
            <input type="button" value="OK" class="buttonConfirm" :disabled="columnError" @click="modalEvent(true)">
            <input type="button" value="Cancel" class="buttonCancel" @click="modalEvent(false)">
            </div>
          </div>
      </div>
      <div class="modalBG"></div>
  </div>  
</template>

<script>
import colors from "../assets/colors.json";

export default {
  name: "Modal",
  props: {
    showModal: { type: Boolean, dafault: true },
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
    return {
      show: false,
      colors: colors,

      YYYY: 2018,
      MM: 1,
      DD: 1,
      title: "",
      type: 0,
      note: ""
    };
  },
  watch: {
    showModal(v) {
      this.show = v;
    }
  },
  methods: {
    modalEvent(isConfirm) {
      this.$emit("send", {
        isConfirm: isConfirm,
        YYYY: this.YYYY,
        MM: this.MM,
        DD: this.DD,
        title: this.title,
        type: this.type,
        note: this.note
      });
    }
  },
  computed: {
    columnError() {
      return this.title === "";
    }
  },
  mounted() {
    this.show = this.showModal;
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
* {
  margin: 0px;
  padding: 0px;
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
  width: 525px;
  top: 20%;
  left: calc(50% - 250px);
}
.modalHeader {
  background: #ffffff;
  height: 20px;
}
.modalBody {
  background: #ffffff;
  height: 300px;
  padding: 15px;
  box-sizing: border-box;
}
@mixin modalItem {
  margin-top: 0px;
  margin-bottom: 15px;
}
@mixin modalItemP {
  text-align: end;
  width: 25%;
  display: inline-block;
}
@mixin modalSelect {
  margin-left: 15px;
}

.modalDate {
  @include modalItem;
  p {
    @include modalItemP;
  }
  select {
    @include modalSelect;
  }
  .dateYears {
    height: 25px;
    width: 20%;
  }
  .dateMonth {
    height: 25px;
    width: 10%;
  }
  .dateDay {
    height: 25px;
    width: 10%;
  }
}
.modalCaption {
  @include modalItem;
  p {
    @include modalItemP;
  }
  input {
    @include modalSelect;
    width: 60%;
    height: 25px;
  }
}
.modalColorPicker {
  @include modalItem;
  p {
    @include modalItemP;
  }
  li {
    list-style: none;
    width: 15px;
    height: 15px;
    border: 1px solid #ccc;
    border-radius: 50%;
    display: inline-block;
    margin: 3.5px;
    margin-top: 15px;
  }
}
.modalRemarks {
  @include modalItem;
  p {
    text-align: start;
  }
  .modalRemarksBox {
    margin-top: 10px;
    width: 100%;
    height: 85px;
    text-align: start;
  }
}
.modalFooter {
  @mixin buttonCSS {
    margin-left: 5px;
    margin-right: 5px;
    font-size: 1em;
    border: 0;
    background: #ffffff00;
    width: 90px;
    height: 30px;
    transition: all 0.2s;
  }
  @mixin buttonHover {
    background: #21ce7c;
    color: white;
  }
  background: #ffffff;
  height: 45px;
  position: relative;
  .modelButtons {
    position: absolute;
    right: 0;
    display: block;
    margin-right: 10px;
    font-weight: bold;
    .buttonConfirm {
      @include buttonCSS;
      color: #21ce7c;
    }
    .buttonConfirm:hover {
      @include buttonHover;
    }
    .buttonConfirm:disabled {
      color: #ccc;
      background: #ffffff00;
    }
    .buttonCancel {
      @include buttonCSS;
    }
    .buttonCancel:hover {
      @include buttonHover;
    }
  }
}
</style>
