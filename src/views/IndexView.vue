<template>
  <!-- 页面主体 -->
  <div class='page'>
    <!-- 主体左侧内容 -->
    <div class='left'>
      <!-- 任务量 -->
      <div class='cardTask'>
        <task-card v-for='item in cardList' :key='item.title' :taskList='item' />
      </div>

      <!-- 待办事项 -->
      <div class='eventArea'>
        <card-event />
      </div>

      <!-- 图表 -->
      <div class='cardChart'>
        <!-- 表头 -->
        <chart-title :taskList='cardList' />
        <!-- 表主体 -->
        <div class='chart'>
          <chart />
        </div>
      </div>

    </div>
    <!-- 主体右侧内容 -->
    <div class='right'>
      <div class='right_content'>
        <!-- 个人介绍 -->
        <div class='introduce_out'>
          <introduce :userMsg='userMsg' />
        </div>
        <!-- 日历 -->
        <div class='calendar'>
          <calendar />

        </div>
        <!-- 日程 -->
        <div class='schedule'>
          <schedule :itemList='scheduleList' />
        </div>
      </div>

    </div>


  </div>
</template>

<script>
import TaskCard from '../components/membermanagement/TaskCard.vue'
import ChartTitle from '../components/membermanagement/ChartTitle.vue'
import CardEvent from '@/components/membermanagement/CardEvent'
import Introduce from '../components/membermanagement/Introduce'
import Calendar from '../components/membermanagement/Calendar.vue'
import Schedule from '../components/membermanagement/Schedule'
import Chart from '@/components/membermanagement/Chart'

export default {
  name: 'Index',
  components: {
    TaskCard, ChartTitle, CardEvent, Introduce, Calendar, Schedule, Chart
  },
  created() {

  },
  mounted() {
  },
  data() {
    return {
      // 前后端需要对个数和百分数做限制，否则可能会导致卡片样式改变
      cardList: [{
        title: '本月新增任务量',
        count: 10,
        percent: 10,
        color: '#9c77f6'
      }, {
        title: '本月进行中任务量',
        count: 20,
        percent: 20,
        color: '#5686fe'
      }, {
        title: '本月完成任务量',
        count: 100,
        percent: 300,
        color: '#00b691'
      }, {
        title: '本月延期任务量',
        count: 15,
        percent: -10,
        color: '#ff4b43'
      }],

      userMsg: {
        img: require('../assets/img/people.jpg'),
        name: '刘美丽',
        department: '上海部门-产品-UI设计'
      },

      scheduleList: [{
        title: '例会：汇报本周工作内容',
        time: '9:30-10:30',
        advance: '6',
        icon: 'icon-Leftbar_feature',
        checkFlag: 1
      }, {
        title: '例会：汇报本周工作内容',
        // time: "9:30-10:30",
        advance: '7',
        checkFlag: 0
      }, {
        title: '例会：汇报本周工作内容',
        // time: "9:30-10:30",
        // advance: "8",
        icon: 'icon-Leftbar_feature',
        checkFlag: 0
      }]
    }
  },
  methods: {
    getUserMsg() {

    },
    getSchedule() {

    }

  }

}
</script>


<style>
body {
  margin: 0 !important;
  background-color: #f4f5f8 !important;
}

.page {
  display: flex;
  justify-content: space-between;
  width: 100%;
  height: 100%;
  background-color: inherit;
  /* 需要删除 */
  /* margin: 1vh 0 0 0.5vw; */
}

.left {
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: transparent;
  width: 74%;
  height: 100%;
}

.cardTask {
  display: flex;
  justify-content: space-between;
  height: 18%;
  flex-shrink: 0;
  margin-bottom: 2%;
  /*flex-shrink: 0;*/
}

.cardChart {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  height: 40%;
  flex-shrink: 0;
}

.eventArea {
  display: flex;
  flex-direction: column;
   height: 39.5vh;
  flex-grow: 1;
  padding: 0 2% 0 2%;
  max-height: calc(100% - 20% - 42%);
  background-color: white;
  margin-bottom: 2%;
}

.right {
  display: flex;
  flex-direction: column;
  width: 25%;
  height: inherit;
  background-color: white;
}

.right_content {
  display: flex;
  flex-direction: column;
  margin: 0 1.27vw;
  height: inherit;
}

.introduce_out {
  height: 20.5vh;
  flex-direction: column;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  font-size: 1.2vw;
  flex-shrink: 0;
}

.calendar {
  display: flex;
  flex-direction: column;
  height: 40vh;
  align-items: center;
  background-color: white;
  flex-shrink: 0;
}

.calendar > div {
  border: none !important;
  width: 21.9vw !important;
}

.schedule {
  flex-direction: column;
  min-height: 20vh;
  display: flex;
  flex-grow: 1;
}
</style>
