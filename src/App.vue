<template>
  <div id="app">    
    <SideBar :now="nowDate" :events="events" :viewDate="selectedViewDate"
      @addEvent="addModal" @editEvent="editModal"/>
    <MenuBar :now="nowDate" :viewDate="viewDate"
      @MMModify="MMModify" @gotoThisMonth="gotoThisMonth"/>
    <Calendar :now="nowDate" :viewDate="viewDate" :events="events"
      @changeSelectedDate="changeSelectedDate"/>
    <Modal :showModal="showModal" :pkg="modalPKG" :viewDate="selectedViewDate" :viewEvent="selectedViewEvent"
      @send="modalEvent"/>
  </div>
</template>

<script>
import SideBar from "./components/SideBar.vue";
import MenuBar from "./components/MenuBar.vue";
import Calendar from "./components/Calendar.vue";
import Modal from "./components/Modal.vue";
import factoryEvent from "./assets/factoryEvent.json";

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
      selectedViewEvent: {
        title: null,
        type: null,
        note: null
      },
      events: factoryEvent,
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
    addModal() {
      this.selectedViewEvent = { title: null, type: null, note: null };
      this.showModal = true;
    },
    editModal(eventInfo) {
      this.selectedViewEvent = eventInfo;
      this.showModal = true;
    },
    modalEvent(modalDetail) {
      if (modalDetail.isConfirm) {
        const _title = modalDetail.title;
        const _type = modalDetail.type;
        const _guid = modalDetail.guid;
        const _note = modalDetail.note;
        const _done = modalDetail.done;
        const eventInfo = {
          guid: _guid,
          title: _title,
          type: _type,
          note: _note,
          done: _done,
          selected: false
        };
        const YYYYMMDD =
          modalDetail.YYYY +
          ("0" + (modalDetail.MM + 1)).slice(-2) + //need+1; because:"20180808"={YYYY:2018,MM:7,DD:8}
          ("0" + modalDetail.DD).slice(-2);

        if (modalDetail.eventType === "add") {
          if (!this.events.hasOwnProperty(YYYYMMDD)) {
            this.$set(this.events, YYYYMMDD, []);
          }
          this.events[YYYYMMDD].push(eventInfo);
        } else if (modalDetail.eventType === "edit") {
          for (let i = 0; i < this.events[YYYYMMDD].length; i++) {
            if (this.events[YYYYMMDD][i].guid === eventInfo.guid) {
              this.$set(this.events[YYYYMMDD], i, eventInfo);
              break;
            }
          }
        }
      }
      this.showModal = false;
    },
    gotoThisMonth() {
      this.viewDate.YYYY = this.nowDate.YYYY;
      this.viewDate.MM = this.nowDate.MM;
      this.viewDate.DD = this.nowDate.DD;
      this.viewDate.DAY = this.nowDate.DAY;

      this.selectedViewDate.YYYY = this.nowDate.YYYY;
      this.selectedViewDate.MM = this.nowDate.MM;
      this.selectedViewDate.DD = this.nowDate.DD;
      this.selectedViewDate.DAY = this.nowDate.DD;
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
      const YYYYMMDD =
        this.selectedViewDate.YYYY +
        ("0" + (this.selectedViewDate.MM + 1)).slice(-2) + //need+1; because:"20180808"={YYYY:2018,MM:7,DD:8}
        ("0" + this.selectedViewDate.DD).slice(-2);
      if (this.events[YYYYMMDD]) {
        for (let i = 0; i < this.events[YYYYMMDD].length; i++) {
          if (this.events[YYYYMMDD][i].selected === true) {
            this.events[YYYYMMDD][i].selected = false;
          }
        }
      }
      this.selectedViewDate = dataObj;
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
* {
  margin: 0px;
  list-style: none;
  padding: 0px;
}
body {
  margin: 0px;
  padding: 0px;
}
#app {
  font-family: "微軟正黑體", "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
abbr {
  text-decoration: none;
}
</style>
