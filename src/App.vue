<script>
import { CalendarView, CalendarViewHeader } from "vue-simple-calendar";
// The next two lines are processed by webpack. If you're using the component without webpack compilation,
// you should just create <link> elements for these. Both are optional, you can create your own theme if you prefer.
require("vue-simple-calendar/static/css/default.css");
require("vue-simple-calendar/static/css/holidays-us.css");

import axios from "axios";

export default {
  components: {
    CalendarView,
    CalendarViewHeader,
  },

  data() {
    return {
      showDate: new Date(),
			message: "",
			startingDayOfWeek: 0,
			disablePast: false,
			disableFuture: false,
			displayPeriodUom: "month",
			displayPeriodCount: 1,
			displayWeekNumbers: false,
			showTimes: true,
			selectionStart: null,
			selectionEnd: null,
			newItemTitle: "",
			newItemStartDate: "",
			newItemEndDate: "",
			useDefaultTheme: true,
			useHolidayTheme: true,
			useTodayIcons: false,

      items: [
              ],
    };
  },
  mounted() {
    this.loadItems();
  },
  methods: {
    setShowDate(d) {
      this.showDate = d;
    },
    loadItems() {
      axios
        .get(`https://api.airtable.com/v0/appo69KYP3rx6Hod9/baslerin`, {
          headers: { Authorization: "Bearer " + "keyq7dDVXJSdLoCKX" },
        })
        .then((response) => {
          // load the API response into items for datatable
          this.items = response.data.records.map((item) => {
            return {
              id: item.id,
              style: "top: 0px; bottom: 0px; background-repeat: no-repeat !important;   background-size: cover !important; border: 4px solid #feddd2;  border-radius: 8%;  height: auto;  outline: none; background-image: url('" + item.fields.Bild + ")",
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

</script>
<template>
  <div style="height:800px"> 
    <calendar-view
      :show-date="showDate"
      class="theme-default holiday-us-traditional holiday-us-official"
			:items="items"
    >
      <calendar-view-header
        slot="header"
        slot-scope="t"
        :header-props="t.headerProps"
        @input="setShowDate"
      />
    </calendar-view>
  </div>
</template>
<style>
.fc-daygrid-day-bg {
  text-align: center;

  text-decoration: none;
}
.fc-bg-event {
  background-repeat: no-repeat !important;   background-size: cover !important; border: 10px solid #feddd2;  border-radius: 8%;  height: auto;  outline: none;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  color: #2c3e50;
  height: 67vh;
  width: 90vw;
  margin-left: auto;
  margin-right: auto;
}
</style>
