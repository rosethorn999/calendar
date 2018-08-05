<template>
  <div class="main" v-show="show" >
      <div v-show="show" class="modalMain">
          <div class="modalHeader">
            <!-- {{pkg.header}} -->
          </div>
          <div class="modalBody">
            <div class="modalDate">
              <p>日期:</p>
            </div>
            <div class="modalCaption">
              <p>標題名稱:</p>
            </div>
            <div class="modalColorPicker">
              <p>Color Picker:</p>
              <ul>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
              </ul>
            </div>
            <div class="modalRemarks">
              <p>備註:</p>
              <input class="modalRemarksBox" type="input">
            </div>
            
            <!-- {{pkg.body}} -->
          </div>
          <div class="modalFooter">
            <!-- {{pkg.footer}} -->
            <div class="modelButtons">
            <input type="button" value="確定" class="buttonConfirm">
            <input type="button" value="取消" class="buttonCancel">
            </div>
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
}
.modalDate {
  @include modalItem;
  p {
    @include modalItemP;
  }
}
.modalCaption {
  @include modalItem;
  p {
    @include modalItemP;
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
    border: 1px solid #666;
    width: 100%;
    height: 90px;
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
    .buttonCancel {
      @include buttonCSS;
    }
    .buttonCancel:hover {
      @include buttonHover;
    }
  }
}
</style>
