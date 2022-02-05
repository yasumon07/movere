<template>
  <div id="app">
    <kalendar :configuration="calendar_settings" :events.sync="events">
      <!-- CREATED CARD SLOT -->
      <div slot="created-card" slot-scope="{ event_information }" class="details-card" @click="hoge">
        <p style="float: left">{{`${moment(event_information.start_time).format("HH:SS")} - ${moment(event_information.end_time).format("HH:SS")}`}}</p>
        <h4 class="appointment-title">{{event_information.data}}</h4>
        <!-- <h4 class="appointment-title">{{event_information.data.title}}</h4> -->
        <!-- <small>
          {{event_information.data.description}}
        </small> -->
        <!-- <span class="time">{{event_information.start_time | formatToHours}} - {{event_information.end_time |
          formatToHours}}</span>
        <button @click="removeEvent(event_information)" class="remove">
          X
        </button> -->
      </div>
    </kalendar>
  </div>
</template>

<script>
import Vue from "vue";
import PortalVue from "portal-vue";
import moment from "moment";
Vue.use(PortalVue);

import { Kalendar } from 'kalendar-vue';

export default {
  name: 'App',
  components: {
    Kalendar
  },
  data() {
		return {
      moment,
			events: [
        // {
        //   from: '2022-02-06T21:55+09:00',
        //   to: '2022-02-06T22:10+09:00',
        //   data: {
        //     title: '徒歩 15分',
        //     description: ''
        //   },
        // },
        {
          from: '2022-02-06T21:55+09:00',
          to: '2022-02-06T22:10+09:00',
          data: '徒歩 15分',
        },
        // {
        //   from: '2022-02-06T22:11+09:00',
        //   to: '2022-02-06T22:17+09:00',
        //   data: '待ち時間',
        // },
        {
          from: '2022-02-06T22:17+09:00',
          to: '2022-02-06T22:59+09:00',
          data: 'JR琵琶湖線　快速・大阪行き',
        },
        {
          from: '2022-02-06T23:00+09:00',
          to: '2022-02-06T23:20+09:00',
          data: '待ち時間',
        },
        // {
        //   from: '2022-02-06T23:20+09:00',
        //   to: '2022-02-06T23:20+09:00',
        //   data: '徒歩 1分',
        // },
        {
          from: '2022-02-06T23:20+09:00',
          to: '2022-02-07T06:20+09:00',
          data: 'ブルーライナーA便102',
        },
        {
          from: '2022-02-07T06:20+09:00',
          to: '2022-02-07T07:30+09:00',
          data: '徒歩　1分',
        },
        {
          from: '2022-02-07T07:30+09:00',
          to: '2022-02-07T13:45+09:00',
          data: 'WILLER EXPRESS B655便',
        },
      ],
			calendar_settings: {
				view_type: 'day',
				// cell_height: 10,
				cell_height: 25,
				scrollToNow: false,
				//start_day: getCurrentDay(),
				military_time: false,
				read_only: false,
				day_starts_at: 0,
				day_ends_at: 24,
				overlap: true,
				hide_dates: ['2019-08-09'],
				hide_days: [],
				past_event_creation: false,
        // formatDayNavigator:(date) => { return new Date(date).toUTCString().slice(5, 11) }
			},
			new_appointment: {},
		};
	},
  methods: {
    hoge() {
      alert(111111)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 5px;
}

.created-event {
  color: black !important;
  font-weight: bold !important;
  background-color: #d3d3d3 !important;
  margin-bottom: 3px !important;
}

.kalendar-wrapper.gstyle .created-event {
  border-radius: 16px !important;
}

.details-card {
	display: flex;
	flex-direction: column;
	width: 100px;
	height: 100%;
  color: black !important;
  /* font-weight: bold !important; */
  /* background-color: #ffffff !important; */
  padding: 3px;
  /* margin-bottom: 2px !important; */
}
.details-card	button {
		margin: 0;
		border: none;
		color: #4c4b4b;
		position: absolute;
		padding-right: 0px;
		top: 5px;
		right: 5px;
		cursor: pointer;
		background: transparent;
}
.details-card	button svg {
	width: 18px;
	height: 18px;
	fill: white;
}

.details-card .remove {
	opacity: 0;
	transition: opacity 0.15s;
}

/* .details-card &:hover .remove {
	opacity: 1;
} */

.popup-event .buttons {
	display: flex;
	justify-content: space-between;
}

.popup-event .buttons button {
	border: none;
	color: #29771c;
	background-color: rgba(#00f0b5, 0.04);
	padding: 5px 10px;

	/* &.cancel {
		background-color: rgba(#f61067, 0.04);
		color: #f61067;
	} */
}
</style>
