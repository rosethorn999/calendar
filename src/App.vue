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
          { name: "task1", guid: "task1", type: 0, selected: false },
          { name: "task2", guid: "task2", type: 1, selected: false }
        ],
        "20180807": [
          { name: "task0", guid: "task0", type: 0, selected: false },
          { name: "task1", guid: "task1", type: 1, selected: false },
          { name: "task2", guid: "task2", type: 2, selected: false },
          { name: "task3", guid: "task3", type: 3, selected: false },
          { name: "task4", guid: "task4", type: 4, selected: false },
          { name: "task5", guid: "task5", type: 5, selected: false },
          { name: "task6", guid: "task6", type: 6, selected: false },
          { name: "task7", guid: "task7", type: 7, selected: false },
          { name: "task8", guid: "task8", type: 8, selected: false },
          { name: "task9", guid: "task9", type: 9, selected: false },
          { name: "task10", guid: "task10", type: 10, selected: false },
          { name: "task11", guid: "task11", type: 11, selected: false },
          { name: "task12", guid: "task12", type: 12, selected: false },
          { name: "task13", guid: "task13", type: 13, selected: false },
          { name: "task14", guid: "task14", type: 14, selected: false },
          { name: "task15", guid: "task15", type: 15, selected: false },
          { name: "task16", guid: "task16", type: 16, selected: false },
          { name: "task17", guid: "task17", type: 17, selected: false },
          { name: "task18", guid: "task18", type: 18, selected: false },
          { name: "task19", guid: "task19", type: 19, selected: false }
        ],
        "20180808": [
          { name: "task1", guid: "task1", type: 0, selected: false },
          { name: "task2", guid: "task2", type: 1, selected: false }
        ],
        "20180901": [
          { name: "task1", guid: "task1", type: 0, selected: false },
          { name: "task2", guid: "task2", type: 1, selected: false }
        ],
        "20180908": [
          { name: "task1", guid: "task1", type: 0, selected: false },
          { name: "task2", guid: "task2", type: 1, selected: false }
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
        const _type = modalDetail.type;
        const _guid = this.getGuid();
        const newEvent = {
          guid: _guid,
          name: _name,
          type: _type,
          selected: false
        };
        const YYYYMMDD =
          modalDetail.YYYY +
          ("0" + (modalDetail.MM + 1)).slice(-2) + //need+1; because:"20180808"={YYYY:2018,MM:7,DD:8}
          ("0" + modalDetail.DD).slice(-2);
        if (!this.events.hasOwnProperty(YYYYMMDD)) {
          this.$set(this.events, YYYYMMDD, []);
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
