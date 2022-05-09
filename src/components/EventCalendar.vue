<script >
// import { reactive } from 'vue'
// import '@fullcalendar/core/vdom'
import FullCalendar from '@fullcalendar/vue'
import DayGridPlugin from '@fullcalendar/daygrid'
import TimeGridPlugin from '@fullcalendar/timegrid'
import InteractionPlugin from '@fullcalendar/interaction'
import ListPlugin from '@fullcalendar/list'
import EventModal from './EventModal'
import { createEventId } from '../store/event-utils'
import axios from 'axios';

import { mapGetters } from 'vuex'
// const options = reactive({
//     plugins: [dayGridPlugin, timeGridPlugin, listPlugin, interactionPlugin],
//     initalView: 'dayGridMonth'
// }),
export default {
    data() {
        return {
            calendarOptions: {
                plugins: [DayGridPlugin, TimeGridPlugin, InteractionPlugin, ListPlugin],
                initialView: 'dayGridMonth',
                // dateClick: this.handleSelect,
                eventClick: this.handleEventClick,
                eventsSet: this.handleEvents,
                // initialEvents: INITIAL_EVENTS,
                headerToolbar: {
                    left: 'prev,next today',
                    center: 'title',
                    right: 'dayGridMonth,timeGridWeek,timeGridDay'
                },
                editable: true,
                selectable: true,
                weekends: true,
                // events: "EVENTS",
                // events: [
                //     { title: 'event 1', date: '2022-05-09' },
                //     { title: 'event 2', date: '2022-05-10' }
                // ],
                events: [],



                select: this.handleDateSelect,

            },
            eventSources: [
                
                console.log("tesssss"),
                this.getEvents(),
                //    {
                //       url: 'http://localhost:3000/getAppointments',
                //       type: 'GET',
                //       data: {
                //          id: this.$store.getters.user
                //       },
                //       error: function() {
                //          alert('there was an error while fetching events!');
                //       },
                //    }
            ],
        }
    },
    // data: () => ({
    //     calendarPlugins: {
    //         DayGridPlugin,
    //         TimeGridPlugin,
    //         InteractionPlugin,
    //         ListPlugin
    //     }
    // }),
    components: { FullCalendar },
    computed: {
        ...mapGetters(["EVENTS"])
    },

    methods: {
        // newEvent() {
        //     this.$modal.show(Modal, {
        //         text: "TExt", 
        //     })

        // },
        renameKey(obj, oldKey, newKey) {
            obj[newKey] = obj[oldKey];
            delete obj[oldKey];
        },
        handleDateSelect(selectInfo) {
            // let title = "new event"
            let title = prompt('Please enter a new title for your event')
            let calendarApi = selectInfo.view.calendar
            calendarApi.unselect() // clear date selection
            if (title) {
                calendarApi.addEvent({
                    id: createEventId(),
                    title,
                    start: selectInfo.startStr,
                    end: selectInfo.endStr,
                    allDay: selectInfo.allDay
                }),
                    axios.post("http://127.0.0.1:8000/events/create-event/", { name: title, date: selectInfo.startStr },
                        {
                            'headers': {
                                'Authorization': 'Token ' + JSON.parse(localStorage.getItem('auth-token')).token,
                                'Accept': 'application/json',
                                'Content-Type': 'application/json;charset=UTF-8'
                            }
                        })
                        .then((response => {

                            console.log(response.data);

                        }))
                        .catch((error) => {
                            console.log(error);
                        });
            }
        }, getEvents() {
            
            console.log("get_event"),
                axios.get('http://127.0.0.1:8000/events/list-event/', {
                    'headers': {
                        'Authorization': 'Token ' + JSON.parse(localStorage.getItem('auth-token')).token,
                        'Accept': 'application/json',
                        'Content-Type': 'application/json;charset=UTF-8'
                    }
                })
                    // .then(resp => (this.events = resp.data.data),console.log( resp.data.data))
                    // .catch(err => console.log(err.response.data));
                    .then((response => {

                        // console.log(response.data[0].name);


                        // for (var prop in response.data[0]) {
                        //     console.log(prop);

                        //     var obj = {
                        //         title: response.data[prop].name,
                        //         start: response.data[prop].date,
                        //     };
                        //     console.log(prop); // U can use this obj object to applu your further logic
                        // }
                        var obj = [];
                        for (var i = 0; i < response.data.length; i++) {
                            
                            obj.push({
                                id: response.data[i].id,
                                title: response.data[i].name,
                                start: response.data[i].date,
                            });

                            // console.log(response.data[i].name);
                        }


                       

                        // console.log(obj, "obj obj", typeof(obj), typeof(obj.data));
                        this.events = obj;
                        this.calendarOptions.events = obj;

                    }))
                    .catch((error) => {
                        console.log(error);
                        // console.log("errorerrorerrorerrorerrorerrorerrorerror");
                    });
        },

        // handleSelect(arg) {
        //     this.$store.commit("ADD_EVENT", {
        //         id: (new Date()).getTime(),
        //         title: "Something",
        //         start: arg.start,
        //         end: arg.end,
        //         allDay: arg.allDay
        //     }),
        //         console.log(arg.start)
        // },

        handleEvents(events) {
            this.currentEvents = events
        },
        created() {
            this.getEvents();
        },
        handleEventClick(arg) {
            console.log(arg, "test", arg.event.id)

            if (arg.event) {
                if (confirm("Do you really want to delete?")) {

                    
                    axios.delete(`http://127.0.0.1:8000/events/delete-event/${arg.event.id}`,
                        {
                            'headers': {
                                'Authorization': 'Token ' + JSON.parse(localStorage.getItem('auth-token')).token,
                                'Accept': 'application/json',
                                'Content-Type': 'application/json;charset=UTF-8'
                            }
                        })
                        .then((response => {

                            console.log(response.data);

                        }))
                        .catch((error) => {
                            console.log(error);
                        });
                        arg.event.remove();
                }
            }
            this.$modal.show(EventModal, {
                text: "This is from the component",
                event: arg.event
            })
        },

    }



}
</script>
<template>
    <div>
        <!-- <button @click="newEvent">New event</button> -->

        <FullCalendar :options="calendarOptions" />
    </div>
</template>


