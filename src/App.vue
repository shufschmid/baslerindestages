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
        events2: [
          {
            title: "Sibel Arslan",
            date: "2021-12-29",
            display: "background",
            classNames: ["class20211229"],
            description:
              '... die Basler Nationalrätin nicht nur Politik kann, sondern auch Comedy. Vor etwa zwei Monaten sass Sibel Arslan in der Kaserne, zusammen mit der Musikerin La Nefera, den Moderator*innen Ugur Gültekin und Fatima Moumouni und rockte die Late Night Show "Diasboah". Schon dort dachte ich: Arslan ist die geborene Entertainerin. Kürzlich trat die Basta-Politikerin in der Satiresendung "Deville" auf. Ihr Endjahresroast liess die Komiker*innen es bizli alt aussehen. Luegsch am beschte sälber.',
          },
          {
            title: "Weihnachtsstern am Tellplatz",
            date: "2021-12-24",

            display: "background",
            classNames: ["class20211224"],
            description: `... er der «letzte Mohikaner der einstigen Weihnachts-Herrlichkeit» ist («Gundeldinger Zeitung»). Dieser Stern ist das tapfere Überbleibsel einer einst grosszügigen Weihnachtsbeleuchtung im Gundeli. Bis 2009 zierten viele metallene Weihnachtsbäumchen das Quartier – ein Werk der Interessengemeinschaft Gundeldingen, kurz IGG. Doch davon übrig geblieben ist nur noch der Stern. Das findet Katja Müggler schade. Die Gundeli-Bewohnerin wollte auf eigene Faust das Quartier weihnächtlich schmücken. Von den Behörden wurde ihr aber ein Riegel geschoben. Die Geschichte der Weihnachts-Kämpferin hat Bajour-Kollege Alex hier aufgeschrieben:🎄bajour.ch/gundeliweihnacht`,
          },
          {
            title: "Patrick Böhler",
            date: "2021-12-27",

            display: "background",
            classNames: ["class20211227"],
            description: `... er im Namen der Rumpel-Clique, den Rumpelsuuri und der Clique Nummere 1 ein Ersatzdatum für den Morgestraich 2022 fordert. Aktuell sind zwar erst 43 Unterschriften zusammengekommen, aber der Titel der Online-Petition klingt gut: MOORGESTRAICH 20 JUNI 2022 - E SUMMERNACHTSTRAUM! Die Argumente der Initiant*innen findest du auf openpetition.de - dort kannst du auch virtuell unterschreiben, falls dir die Idee gefällt. `,
          },
        ],
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
      this.events = [];
      axios
        .get(`https://api.airtable.com/v0/appo69KYP3rx6Hod9/baslerin`, {
          headers: { Authorization: "Bearer " + "keyq7dDVXJSdLoCKX" },
        })
        .then((response) => {
          // load the API response into items for datatable
          this.calendarOptions.events = response.data.records.map((item) => {
            return {
              id: item.id,
              display: "background",
            classNames: ["class"+item.fields.date],
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
.class2021-12-29 {
  background-image: url("https://ci5.googleusercontent.com/proxy/8wvXqM-Tk4F7gHK1kySaupRrl0QNPjU-69YM04jtDeMJkFvU2iLmUs6SHaCUEjJ4WofWS3cCxUOmGsBcLuflfg5xIGdUp7EDQ7jDtJX5RwmiDH3FGH_DNwksOpvD3JgJL1ldAEyad_zMi3ZWHwvknhOFRcbn7g=s0-d-e1-ft#https://mcusercontent.com/c30add995be4a0a845d9e933a/images/94f1618c-e0d3-c5b2-6fa3-95f51768ee23.jpg") !important;

  background-repeat: no-repeat !important; /* Do not repeat the image */
  background-size: cover !important;
  background: none;
  opacity: 100 !important;
  border: 10px solid #feddd2;
  border-radius: 8%;
  height: auto;
  outline: none;
}
.class2021-12-24 {
  background-image: url("https://ci3.googleusercontent.com/proxy/nJHGKT73hLyhWq1ZduPDK596B0gk8yi5HKBpdleig4ZY-NeUKdlL1YzIVgonIDzqjibpOVbTvWidGl-3tMC78o4fji7bUC0eZoY5-EuY7iMOGpMmZLD3NnxlkWwYhFXRq3hjnxd6dXlAyu94bkYwTrSerw-c2A=s0-d-e1-ft#https://mcusercontent.com/c30add995be4a0a845d9e933a/images/ebae8fcb-a65f-5e1d-598c-4a92dc76e804.jpg") !important;

  background-repeat: no-repeat !important; /* Do not repeat the image */
  background-size: cover !important;
  background: none;
  opacity: 100 !important;
  border: 10px solid #feddd2;
  border-radius: 8%;
  height: auto;
  outline: none;
}
.class2021-12-27 {
  background-image: url("https://ci5.googleusercontent.com/proxy/hYRVHoufYI1Qy_hMVH2WCwsFPvrnE-j6OS15st5iEUzIoyKAhjKsbyMl1-ivqEFwr3GgQLXNC2MCmT8DHBO8OSOQwJDRg62nJoivH--FJnK_TlCbo2heWf1GOXQvCDu64LOrQccVtF3F9w5FlknF_hUV_60hUA=s0-d-e1-ft#https://mcusercontent.com/c30add995be4a0a845d9e933a/images/ea69706c-230e-cf12-e8cc-bd6b143d5827.png") !important;

  background-repeat: no-repeat !important; /* Do not repeat the image */
  background-size: cover !important;
  background: none;
  opacity: 100 !important;
  border: 10px solid #feddd2;
  border-radius: 8%;
  height: auto;
  outline: none;
}
</style>
