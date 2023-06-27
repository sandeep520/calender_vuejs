<template>
  <div class="demo-app">
    <div class="demo-app-main">
      <FullCalendar class="demo-app-calendar" :options="celender">
        <template v-slot:eventContent="arg">
          <b>{{ arg.timeText }}</b>
          <i>{{ arg.event.title }}</i>
        </template>
      </FullCalendar>
    </div>
  </div>
</template>

<script>
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";
import timeGridPlugin from "@fullcalendar/timegrid";
import interactionPlugin from "@fullcalendar/interaction";
import { createEventId } from "./event_utils";

export default {
  components: {
    FullCalendar,
  },
  data() {
    return {
      id: 0,
      celender: {
        plugins: [dayGridPlugin, timeGridPlugin, interactionPlugin],
        headerToolbar: {
          left: "prev,next today",
          center: "title",
          right: "dayGridMonth,timeGridWeek,timeGridDay",
        },
        initialView: "dayGridMonth",
        editable: true,
        selectable: true,
        // selectMirror: true,
        dayMaxEvents: true,
        // weekends: true,
        select: this.handleDate,
        eventClick: this.handleEventClick,
        // eventsSet: this.handleEvents,

        events: [],
      },
    };
  },
  methods: {
    handleDate(selectEvent) {
      console.log(selectEvent);
      let title = prompt("Please enter a new title for your event");
      // let celenderData = selectEvent.view.calendar;
      // console.log(selectEvent.view.calendar);
      // celenderData.unselect();

      if (title) {
        this.id++;
        this.celender.events.push({
          id: this.id,
          title,
          start: selectEvent.startStr,
          end: selectEvent.endStr,
          allDay: selectEvent.allDay,
        });

        localStorage.setItem("event", JSON.stringify(this.celender.events));
      }
    },
    handleEventClick(clickInfo) {
      console.log(clickInfo);
      if (
        confirm(
          `Are you sure you want to delete the event '${clickInfo.event.title}'`
        )
      ) {
        clickInfo.event.remove();
        let data = JSON.parse(localStorage.getItem("event"));
        data.forEach((item, index) => {
          if (clickInfo.event.id == item.id) {
            this.celender.events.splice(index, 1);
            localStorage.setItem("event", JSON.stringify(this.celender.events));
          }
        });
        // let data = JSON.parse(localStorage.getItem("event"));
        // data.forEach((item, index) => {
        //   if ( this.celender.events.id !== item.id) {
        //     data.splice(index, 1, clickInfo);
        //   }
        // });
      }
      console.log(this.celender.events);
    },
    // handleEvents(events) {
    //   // console.log(events);
    //   // this.currentEvents = events;
    //   // console.log(this.currentEvents);
    //   localStorage.setItem("event", JSON.stringify(this.celender.events));
    // },
  },
  mounted() {
    const arr = JSON.parse(localStorage.getItem("event"));
    arr.forEach((element) => {
      this.celender.events.push(element);
    });
  },
};
</script>

<style lang="css">
.demo-app {
  display: flex;
  min-height: 100%;
  font-family: Arial, Helvetica Neue, Helvetica, sans-serif;
  font-size: 14px;
}
.demo-app-sidebar {
  width: 300px;
  line-height: 1.5;
  background: #eaf9ff;
  border-right: 1px solid #d3e2e8;
}
.demo-app-sidebar-section {
  padding: 2em;
}
.demo-app-main {
  flex-grow: 1;
  padding: 3em;
}
.fc {
  /* the calendar root */
  max-width: 1100px;
  margin: 0 auto;
}
</style>
