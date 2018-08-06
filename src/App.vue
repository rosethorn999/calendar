<template>
  <div id="app">    
    <SideBar :now="nowDate" :events="events" @addEvent="openModal"/>
    <MenuBar :now="nowDate" @MMModify="MMModify" :viewMonth="viewMonth"/>
    <Calendar :now="nowDate" :viewMonth="viewMonth"/>
    <Modal :showModal="showModal" :pkg="modalPKG" @send="modalEvent"/>
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
      viewMonth: {
        YYYY: null,
        MM: null,
        DD: null,
        DAY: null
      },
      events: [
        { name: "task1", guid: "task1", selected: false },
        { name: "task2", guid: "task2", selected: false },
        { name: "task3", guid: "task3", selected: false }
      ],
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
        this.events.push(newEvent);
      }
      this.showModal = false;
    },
    MMModify(goNext) {
      let v = goNext ? 1 : -1;
      this.viewMonth.MM += v;
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
    this.viewMonth = {
      YYYY: d.getFullYear(),
      MM: d.getMonth()
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
