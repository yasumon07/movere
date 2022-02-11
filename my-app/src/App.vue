<template>
  <div id="app">
    <a-spin :spinning="spinning" size="large" class="spinning">
      <div class="spin-content">
        <transition name="fade">

          <kalendar :configuration="calendar_settings" :events.sync="events" :key="key">
            <!-- CREATED CARD SLOT -->

            <div slot="created-card" slot-scope="{ event_information }" :class="`details-card ${event_information.data.isStab ? 'stab' : ''}`" @click="showModal(event_information)">
              <!-- <span style="float: left">{{`${moment(event_information.start_time).format("HH:SS")} - ${moment(event_information.end_time).format("HH:SS")}`}}</span> -->
              <div style="text-align: left">
                <img v-if="event_information.data.isWork" src="./work.png" alt="Logo" class="header__logo" width="15">
                <img v-if="event_information.data.isTrain" src="./train.png" alt="Logo" class="header__logo" width="15">
                <img v-if="event_information.data.isBus" src="./train.png" alt="Logo" class="header__logo" width="15">
                <img v-if="event_information.data.isTime" src="./time.png" alt="Logo" class="header__logo" width="15">
                <img v-if="event_information.data.isPlain" src="./plain.png" alt="Logo" class="header__logo" width="15">&nbsp;
                <a-space size="middle" text-align="left">
                  <!-- <span>({{`${moment(event_information.start_time).format("HH:SS")} - ${moment(event_information.end_time).format("HH:SS")}`}})</span> -->
                  <span>{{event_information.data.title}}</span>
                  <span v-if="event_information.data.isWait"><a-icon type="smile" style="font-size: 1rem; cursor: pointer; color: green" @click="showModalType('rest')" /></span>
                  <span v-if="event_information.data.isWait"><a-icon type="wifi" style="font-size: 1rem; cursor: pointer; color: blue" @click="showModalType('wifi')" /></span>
                </a-space>
              </div>

              <!-- <span class="appointment-title">{{event_information.data}}</span> -->
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
        </transition>
        <a-button type="primary" shape="circle" icon="plus" class="Page-Btn" @click="showModalCreate()" />
      </div>
    </a-spin>
    <a-modal v-model="visibleCreate" title="予定の登録" @ok="handleOk" :z-index="110">
      <div>
        <a-space>
          <a-icon type="car" style="font-size: 1.2rem" />
          <a-input placeholder="出発地" />
          <span>~</span>
          <a-input placeholder="到着地" />
        </a-space>
        <br><br>
        <a-space>
          <a-icon type="calendar" style="font-size: 1.2rem" />
          <a-date-picker @change="onChange" />
          <a-time-picker :default-open-value="moment('00:00:00', 'HH:mm:ss')" @change="onChange" />
        </a-space>
        <br><br>
        <a-space>
          <span style="margin-left: 1.2rem"></span>
          <a-date-picker @change="onChange" />
          <a-time-picker :default-open-value="moment('00:00:00', 'HH:mm:ss')" @change="onChange" />
        </a-space>
        <br />
      </div>
    </a-modal>
    <a-modal v-model="visibleDetail" title="詳細情報" :footer="false" @ok="false" :z-index="110">
      <a-timeline>
        <a-timeline-item>2022-02-06 12:22 ○○経由</a-timeline-item>
        <a-timeline-item>2022-02-06 12:52 △△経由</a-timeline-item>
        <a-timeline-item color="red">
          <a-icon slot="dot" type="clock-circle-o" style="font-size: 16px;" />
          2022-02-06 12:52 □□経由
        </a-timeline-item>
        <a-timeline-item>2022-02-06 17:52 △△経由</a-timeline-item>
      </a-timeline>
      <!-- <a-timeline mode="alternate">
        <a-timeline-item>Create a services site 2015-09-01</a-timeline-item>
        <a-timeline-item color="green">
          Solve initial network problems 2015-09-01
        </a-timeline-item>
        <a-timeline-item>
          <a-icon slot="dot" type="clock-circle-o" style="font-size: 16px;" />
          Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque
          laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto
          beatae vitae dicta sunt explicabo.
        </a-timeline-item>
        <a-timeline-item color="red">
          Network problems being solved 2015-09-01
        </a-timeline-item>
        <a-timeline-item>Create a services site 2015-09-01</a-timeline-item>
        <a-timeline-item>
          <a-icon slot="dot" type="clock-circle-o" style="font-size: 16px;" />
          Technical testing 2015-09-01
        </a-timeline-item>
      </a-timeline> -->
    </a-modal>
    <a-modal v-model="visibleRest" title="周辺情報" :footer="false" @ok="false" :z-index="110">
      <center>
        <img src="./rest.jpg" alt="Logo" class="header__logo" width="300">
      </center>
    </a-modal>
    <a-modal v-model="visibleWifi" title="周辺情報" :footer="false" @ok="false" :z-index="110">
      <center>
        <img src="./wifi.jpg" alt="Logo" class="header__logo" width="300">
      </center>
    </a-modal>
  </div>
</template>

<script>
import Vue from "vue";
import PortalVue from "portal-vue";
import Antd from 'ant-design-vue';
import 'ant-design-vue/dist/antd.css';
Vue.config.productionTip = false;
import moment from "moment";
Vue.use(PortalVue);
Vue.use(Antd);

import { Kalendar } from 'kalendar-vue';

const events = [
        {
          from: '2022-02-06T09:55+09:00',
          to: '2022-02-06T10:10+09:00',
          data: {
            title: '徒歩 15分',
            description: '',
            isWork: true,
            recommend: 1
          },
        },
        {
          from: '2022-02-06T09:55+09:00',
          to: '2022-02-06T10:15+09:00',
          data: {
            title: '徒歩 20分',
            description: '',
            isWork: true,
            recommend: 2
          },
        },
        {
          from: '2022-02-06T09:55+09:00',
          to: '2022-02-06T10:40+09:00',
          data: {
            title: '徒歩 45分',
            isWork: true,
            description: '',
            recommend: 3
          },
        },
        {
          from: '2022-02-06T10:17+09:00',
          to: '2022-02-06T10:59+09:00',
          data: {
            title: 'JR琵琶湖線　快速・大阪行き',
            description: '出発予定時刻；22:17',
            isTrain: true,
            recommend: 1
          },
        },
        {
          from: '2022-02-06T10:10+09:00',
          to: '2022-02-06T10:10+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T10:17+09:00',
          to: '2022-02-06T10:17+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T10:05+09:00',
          to: '2022-02-06T10:05+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T10:05+09:00',
          to: '2022-02-06T11:30+09:00',
          data: {
            title: 'JR琵琶湖線　快速・大阪行き',
            isTrain: true,
            description: '',
            recommend: 2
          },
        },
        {
          from: '2022-02-06T10:05+09:00',
          to: '2022-02-06T10:05+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T10:45+09:00',
          to: '2022-02-06T10:45+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T10:45+09:00',
          to: '2022-02-06T10:45+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T10:45+09:00',
          to: '2022-02-06T22:40+09:00',
          data: {
            title: 'WILLER EXPRESS B655便',
            isBus: true,
            description: '',
            recommend: 3
          },
        },
        {
          from: '2022-02-06T11:00+09:00',
          to: '2022-02-06T11:20+09:00',
          data: {
            title: '待ち時間',
            isTime: true,
            description: '',
            recommend: 1,
            isWait: true,
          },
        },
        {
          from: '2022-02-06T11:40+09:00',
          to: '2022-02-06T11:40+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T11:40+09:00',
          to: '2022-02-06T13:20+09:00',
          data: {
            title: '待ち時間',
            isTime: true,
            description: '',
            recommend: 2,
            isWait: true,
          },
        },
        {
          from: '2022-02-06T11:40+09:00',
          to: '2022-02-06T11:40+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T13:30+09:00',
          to: '2022-02-06T11:40+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T13:30+09:00',
          to: '2022-02-06T20:20+09:00',
          data: {
            title: 'ブルーライナーB便102',
            isPlain: true,
            description: '',
            recommend: 2,
            isWait: true,
          },
        },
        {
          from: '2022-02-06T13:30+09:00',
          to: '2022-02-06T11:40+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T11:00+09:00',
          to: '2022-02-06T11:11+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T11:00+09:00',
          to: '2022-02-06T11:11+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T11:20+09:00',
          to: '2022-02-06T11:59+09:00',
          data: {
            title: 'ブルーライナーA便102',
            isPlain: true,
            description: '',
            recommend: 1
          },
        },
        {
          from: '2022-02-06T11:20+09:00',
          to: '2022-02-06T11:21+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T11:20+09:00',
          to: '2022-02-06T11:21+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T12:00+09:00',
          to: '2022-02-06T18:20+09:00',
          data: {
            title: 'ブルーライナーA便102',
            isPlain: true,
            isDetail: true,
            description: '',
            recommend: 1
          },
        },
        {
          from: '2022-02-06T12:00+09:00',
          to: '2022-02-06T12:01+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T12:00+09:00',
          to: '2022-02-06T12:01+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T18:20+09:00',
          to: '2022-02-06T19:30+09:00',
          data: {
            title: '徒歩 1分',
            description: '',
            recommend: 1
          },
        },
        {
          from: '2022-02-06T18:20+09:00',
          to: '2022-02-06T18:21+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T18:20+09:00',
          to: '2022-02-06T18:21+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T19:30+09:00',
          to: '2022-02-06T23:45+09:00',
          data: {
            title: 'WILLER EXPRESS B655便',
            isBus: true,
            description: '',
            recommend: 1
          },
        },
        {
          from: '2022-02-06T19:30+09:00',
          to: '2022-02-06T19:31+09:00',
          data: {
            isStab: true
          },
        },
        {
          from: '2022-02-06T19:30+09:00',
          to: '2022-02-06T19:31+09:00',
          data: {
            isStab: true
          },
        },
      ];

export default {
  name: 'App',
  components: {
    Kalendar
  },
  data() {
		return {
      isRecommend: false,
      key: 0,
      spinning: false,
      eventsItems: [],
      // eventsItems: this.events,
      moment,
      visibleCreate: false,
      visibleRest: false,
      visibleWifi: false,
      visibleDetail: false,
			events: [],
			calendar_settings: {
				view_type: 'day',
				// cell_height: 10,
				cell_height: 20,
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
    toggle() {
      this.key = this.key ? 0 : 1;
    },
    onChange(date, dateString) {
      console.log(date, dateString);
    },
    showModal(type) {
      console.log(type)
      console.log(type.data ? type.data.recommend : '')
      if (!this.isRecommend) {
        this.events = events.filter(x => !x.data.isStab && x.data.recommend == type.data.recommend);
        this.toggle();
        this.isRecommend = true;
        this.$message.success('経路が確定しました！');
        setTimeout(() => this.visibleDetail = false, 1000)
        return;
      }

      if (type == 'rest') {
        this.visibleRest = true;
        return;
      }
      if (type == 'wifi') {
        this.visibleWifi = true;
        return;
      }
      if (this.visibleRest || this.visibleWifi) {
        return;
      }
      this.visibleDetail = true;
    },
    showModalType(type) {
      console.log(type)
      console.log(type.data ? type.data.recommend : '')
      if (!this.isRecommend) {
        this.events = events.filter(x => !x.data.isStab && x.data.recommend == type.data.recommend);
        this.toggle();
        this.isRecommend = true;
        this.$message.success('経路が確定しました！');
        return;
      }

      if (type == 'rest') {
        this.visibleRest = true;
        return;
      }
      if (type == 'wifi') {
        this.visibleWifi = true;
        return;
      }
      this.visibleDetail = true;
    },
    // showModalDetail() {
    //   this.visibleDetail = true;
    //   // if (event_information.data.isDetail) this.visibleDetail = true;
    // },
    showModalCreate() {
      this.visibleCreate = true;
    },
    handleOk(e) {
      console.log(e);
      this.spinning = true;
      this.visibleCreate = false;

      this.events = events;
      this.toggle();
      setTimeout(() => {
        this.spinning = false;
        this.$message.success('経路の候補が見つかりました！');
      }, 1500)
    },
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

.ant-spin-nested-loading > div > .ant-spin {
  position:fixed;
  width:600px;
  height:400px;
  top:50%;
  left:50%;
  margin-left:-300px;
  margin-top:-200px;
}

.Page-Btn{
  position: fixed;
  right: 24px;
  bottom: 24px;
  width: 46px;
  height: 46px;
  line-height: 46px;
  text-align: center;
  border-radius: 50%;
  /* background: #5bc8ac; */
  z-index: 100;
}

.created-event {
  color: black !important;
  font-weight: bold !important;
  background-color: #e2e2e2 !important;
  margin-bottom: 3px !important;
  /* border: solid 1px #7a7979!important; */
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
