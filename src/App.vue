<template>
  <div id="app">
    <button @click="component = 'LoginVue'">Login</button>
    <button @click="component = 'EventCalendar'" v-if="isHidden">Calendar</button>
    <KeepAlive>
      <Component :is="component" />
    </KeepAlive>
  </div>
</template>

<script>


// import EventCalendar from './components/EventCalendar.vue';
import LoginVue from './components/login_comp.vue';

export default {
  name: 'App',
  data() {
    return {
      component: 'LoginVue',
      isHidden: true,
    
      
    }


  },

  components: {
    EventCalendar:()=>import('./components/EventCalendar.vue'),
    LoginVue,
  }

};





</script>

<style>
</style>

