<script>
import { onMount } from "svelte";
import { get } from 'svelte/store';
import FullCalendar, { CalendarApi } from 'svelte-fullcalendar';
import dayGridPlugin from '@fullcalendar/daygrid';
import iCalendarPlugin from '@fullcalendar/icalendar';
// import googleCalendarPlugin from '@fullcalendar/google-calendar';
// import listPlugin from '@fullcalendar/list';
// import timeGridPlugin from '@fullcalendar/timegrid';
// import ical from 'ical';

let data_theme = "light";

let data = [];
let parsed_data = [];
let string_data;

let geo_id;

let svi_cre_data;

let local_data;

let location_buffer;
let location_coordinates;
let event_area;
let point;
let coordinates;
let cal_coordinates;

let add_to_calendar_url;

let calendar;
let calendarAPI;

let display_ej_table = false;

let eventSourcesArray = [];
let eventsArray = [];
let eventFeed;
let address;
let address_display;
let timezone;

let new_event;

let date_display_options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };


let first_event = true;

let options;

let calendar_popup_show = false;

let calendar_popup = {
        title: '',
        details: '',
        x: '',
        y: '',
    }


let create_menu = "";

let discussion;

var offset = new Date().getTimezoneOffset();
console.log(offset);

let now = new Date();
now.setMinutes(now.getMinutes() - offset)
now = now.toISOString();
now = now.slice(0, -8);
console.log(now);

let event = {
        when: now,
}


    onMount(async () => {

        const common = (await import('@fullcalendar/common')).default

        initializeCalendarOptions();
        
    });


async function initializeCalendarOptions() {
    calendarAPI = calendar.getAPI();

options = {
initialView: 'dayGrid',
plugins: [
        (await import('@fullcalendar/daygrid')).default,
        // (await import('@fullcalendar/list')).default,
        // (await import('@fullcalendar/timegrid')).default,
        // (await import('$lib/icalendar')).default,
        (await import('@fullcalendar/icalendar')).default
        // (await import('@fullcalendar/google-calendar')).default,
    ],
weekends: true,
googleCalendarApiKey: variables.googleCalendar,
eventSources: JSON.parse(eventSourcesArray),
events: JSON.parse(eventsArray),
eventClick: function(info) {
info.jsEvent.preventDefault(); // don't let the browser navigate

console.log(info);
add_to_calendar_url = info.event.url;

console.log(info.event.extendedProps);
calendar_popup.title = info.event.title;
calendar_popup.details = info.event.extendedProps.description;
calendar_popup.location = info.event.extendedProps.location;
calendar_popup.x = info.jsEvent.pageX;
calendar_popup.y = info.jsEvent.pageY;

calendar_popup_show = true;
console.log(calendar_popup_show);
},
//   eventMouseLeave: function(info) {
//       calendar_popup_show = false;
//   }
};
}


</script>

<FullCalendar bind:this="{calendar}" {options} />