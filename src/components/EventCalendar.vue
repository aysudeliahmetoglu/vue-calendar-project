<script >
// import { reactive } from 'vue'
// import '@fullcalendar/core/vdom'
import FullCalendar from '@fullcalendar/vue'
import DayGridPlugin from '@fullcalendar/daygrid'
import TimeGridPlugin from '@fullcalendar/timegrid'
import InteractionPlugin from '@fullcalendar/interaction'
import ListPlugin from '@fullcalendar/list'

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
                headerToolbar: {
                    left: 'prev,next today',
                    center: 'title',
                    right: 'dayGridMonth,timeGridWeek,timeGridDay'
                },
                editable: true,
                selectable: true,
                weekends: true,
                select: (arg) =>  {
                    console.log(arg.start, arg.end)
                }
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
    components: { FullCalendar }

}
</script>
<template>
    <div>
        <FullCalendar :options="calendarOptions" />
    </div>
</template>


