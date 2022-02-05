<template>
  <div id="app">
    <kalendar :configuration="calendar_settings" :events.sync="events" />
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Vue from "vue";
import PortalVue from "portal-vue";
Vue.use(PortalVue);

import { Kalendar } from 'kalendar-vue';

import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    Kalendar,
    HelloWorld
  },
  data() {
		return {
			events: [
        {
          from: '2020-03-18T18:00:00Z',
          to: '2020-03-18T19:00:00Z',
          data: 'Event 1',
        },
        {
          from: '2020-03-18T19:00:00Z',
          to: '2020-03-18T21:00:00Z',
          data: 'Olive & Friends',
        },
      ],
			calendar_settings: {
				view_type: 'day',
				cell_height: 10,
				scrollToNow: false,
				//start_day: getCurrentDay(),
				military_time: false,
				read_only: false,
				day_starts_at: 0,
				day_ends_at: 24,
				overlap: true,
				hide_dates: ['2019-08-09'],
				hide_days: [],
				past_event_creation: true,
			},
			new_appointment: {},
		};
	},
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
