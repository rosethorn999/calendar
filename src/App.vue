<template>
  <div id="app">    
    <SideBar :now="nowDate" :events="events" :viewDate="selectedViewDate"
      @addEvent="openModal"/>
    <MenuBar :now="nowDate" :viewDate="viewDate"
      @MMModify="MMModify" />
    <Calendar :now="nowDate" :viewDate="viewDate" :events="events"
      @changeSelectedDate="changeSelectedDate"/>
    <Modal :showModal="showModal" :pkg="modalPKG" :viewDate="selectedViewDate"
      @send="modalEvent"/>
  </div>
</template>

<script>
import SideBar from "./components/SideBar.vue";
import MenuBar from "./components/MenuBar.vue";
import Calendar from "./components/Calendar.vue";
import Modal from "./components/Modal.vue";

export default {
  name: "app",
  components: {
    SideBar,
    MenuBar,
    Calendar,
    Modal
  },
  data: function() {
    return {
      selectedViewDate: {
        YYYY: null,
        MM: null,
        DD: null,
        DAY: null
      },
      viewDate: {
        YYYY: null,
        MM: null,
        DD: null,
        DAY: null
      },
      events: {
        "20180729": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false }
        ],
        "20180807": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false },
          { name: "task3", guid: "task3", selected: false }
        ],
        "20180808": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false }
        ],
        "20180901": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false }
        ],
        "20180908": [
          { name: "task1", guid: "task1", selected: false },
          { name: "task2", guid: "task2", selected: false }
        ]
      },
      showModal: false,
      modalPKG: {
        header: "HEADER TEXT",
        body: "BODY TEXT",
        footer: "FOOTER TEXT"
      }
    };
  },
  computed: {
    nowDate() {
      const d = new Date();
      return {
        YYYY: d.getFullYear(),
        MM: d.getMonth(),
        DD: d.getDate(),
        DAY: d.getDay()
      };
    }
  },
  methods: {
    openModal() {
      this.showModal = true;
    },
    modalEvent(modalDetail) {
      if (modalDetail.isConfirm) {
        const _name = modalDetail.title;
        const _guid = this.getGuid();
        const newEvent = {
          guid: _guid,
          name: _name,
          selected: false
        };
        const YYYYMMDD =
          modalDetail.YYYY +
          ("0" + (modalDetail.MM + 1)).slice(-2) + //need+1; because:"20180808"={YYYY:2018,MM:7,DD:8}
          ("0" + modalDetail.DD).slice(-2);
        if (!this.events.hasOwnProperty(YYYYMMDD)) {
          this.events[YYYYMMDD] = [];
        }
        this.events[YYYYMMDD].push(newEvent);
      }
      this.showModal = false;
    },
    MMModify(goNext) {
      if (goNext) {
        if (this.viewDate.MM === 11) {
          this.viewDate.MM = 0;
          this.viewDate.YYYY = this.viewDate.YYYY + 1;
        } else {
          this.viewDate.MM += 1;
        }
      } else {
        if (this.viewDate.MM === 0) {
          this.viewDate.MM = 11;
          this.viewDate.YYYY = this.viewDate.YYYY - 1;
        } else {
          this.viewDate.MM -= 1;
        }
      }
    },
    changeSelectedDate(dataObj) {
      this.selectedViewDate = dataObj;
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
  },
  mounted() {
    const d = new Date();
    this.viewDate = {
      YYYY: d.getFullYear(),
      MM: d.getMonth(),
      DD: d.getDate(),
      DAY: d.getDay()
    };
    this.selectedViewDate = {
      YYYY: d.getFullYear(),
      MM: d.getMonth(),
      DD: d.getDate(),
      DAY: d.getDay()
    };
  }
};
</script>

<style lang="scss">
body {
  margin: 0px;
  padding: 0px;
}
#app {
  font-family: "微軟正黑體", "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
