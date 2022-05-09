<script >
// import { reactive } from 'vue'
// import '@fullcalendar/core/vdom'
import FullCalendar from '@fullcalendar/vue'
import DayGridPlugin from '@fullcalendar/daygrid'
import TimeGridPlugin from '@fullcalendar/timegrid'
import InteractionPlugin from '@fullcalendar/interaction'
import ListPlugin from '@fullcalendar/list'
import EventModal from './EventModal'

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
                dateClick: this.handleSelect,
                eventClick:this.handleEventClick,
                headerToolbar: {
                    left: 'prev,next today',
                    center: 'title',
                    right: 'dayGridMonth,timeGridWeek,timeGridDay'
                },
                editable: true,
                selectable: true,
                weekends: true,
                events:[
          { title: 'event 1', date: '2019-04-01' },
          { title: 'event 2', date: '2019-04-02' }
        ]


                // select: (arg) => {
                //     console.log(arg.start, arg.end)
                // },

            }
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

    methods: {
        // newEvent() {
        //     this.$modal.show(Modal, {
        //         text: "TExt", 
        //     })

        // },
        handleSelect(arg) {
            this.$store.commit("ADD_EVENT", {
                id: (new Date()).getTime(),
                title: "Something",
                start: arg.start,
                end: arg.end,
                allDay: arg.allDay
            }),
            console.log(arg.start)
        },
        handleEventClick (arg) {
            console.log(arg)
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
        <button @click="newEvent">New event</button>
        <FullCalendar
            :options="calendarOptions"

        />
    </div>
</template>


