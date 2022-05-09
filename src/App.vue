<template>
  <div id="app">
    <button @click="component = 'LoginVue'">Login</button>
    <button @click="component = 'EventCalendar'">Calendar</button>
    <KeepAlive>
      <Component :is="component" />
    </KeepAlive>
  </div>
</template>

<script>


import EventCalendar from './components/EventCalendar.vue';
import LoginVue from './components/login_comp.vue';

export default {
  name: 'App',
  data() {
    return {
      component: 'LoginVue'
    }


  },

  components: {
    EventCalendar,
    LoginVue,
  }

};





</script>

<style>
</style>

