<script>
import "@fullcalendar/core/vdom"; // solves problem with Vite
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";
import interactionPlugin from "@fullcalendar/interaction";
import axios from "axios";

export default {
  components: {
    FullCalendar, // make the <FullCalendar> tag available
  },
  data() {
    return {
      calendarOptions: {
        plugins: [dayGridPlugin, interactionPlugin],
        initialView: "dayGridMonth",
        eventClick: this.handleClick,
        events: [],
        eventContent: function (eventInfo) {
          return { html: eventInfo.event.extendedProps.customHtml };
        },
      },

      events: [],
    };
  },
  mounted() {
    this.loadItems();
  },
  methods: {
    loadItems() {
      this.items = [];
      axios
        .get(`https://api.airtable.com/v0/appo69KYP3rx6Hod9/baslerin`, {
          headers: { Authorization: "Bearer " + "keyq7dDVXJSdLoCKX" },
        })
        .then((response) => {
          // load the API response into items for datatable
          this.items = response.data.records.map((item) => {
            return {
              id: item.id,
              ...item.fields,
            };
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    handleClick: function (arg) {
      alert(
        "Basler*in des Tages ist heute " +
          arg.event.title +
          ", weil " +
          arg.event.extendedProps.description
      );
    },
  },
};

function createCssClasses(item) {
  document.getElementsByClassName(item.classNames[0])[0].style.background =
    "none";
  document.getElementsByClassName(item.classNames[0])[0].style.opacity = "100";
  document.getElementsByClassName(item.classNames[0])[0].style.backgroundImage =
    "url(" + item.Bild + ")";
}
</script>
<template>
  <div>
    {{ events }}
    <FullCalendar :options="calendarOptions" />
  </div>
</template>
<style>
.fc-daygrid-day-bg {
  text-align: center;

  text-decoration: none;
}
.fc-bg-event {
  background-repeat: no-repeat !important; /* Do not repeat the image */
  background-size: cover !important;
  border: 10px solid #feddd2;
  border-radius:8%;
  height:auto;
  outline:none;

}
</style>
