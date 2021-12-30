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
        }).then(()=>{
          this.calendarOptions.events.forEach(createCssClasses)
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
  document.getElementsByClassName(item.classNames[0])[0].style.backgroundImage = "url("+item.Bild+")"; 
  document.getElementsByClassName(item.classNames[0])[0].style.backgroundRepeat= "no-repeat !important"; /* Do not repeat the image */
  document.getElementsByClassName(item.classNames[0])[0].style.backgroundSize= "cover !important";
  document.getElementsByClassName(item.classNames[0])[0].style.background= "none";
  document.getElementsByClassName(item.classNames[0])[0].style.opacity="100 !important";
  document.getElementsByClassName(item.classNames[0])[0].style.border=" 10px solid #feddd2";
  document.getElementsByClassName(item.classNames[0])[0].style.borderRadius="8%";
  document.getElementsByClassName(item.classNames[0])[0].style.height="auto";
  document.getElementsByClassName(item.classNames[0])[0].style.outline= "none";
  console.log(item.Bild)
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

</style>
