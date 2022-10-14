<template>
  <!-- 页面主体 -->
  <div class='page'>
    <!-- 主体左侧内容 -->
    <div class='left'>
      <!-- 任务量 -->
      <div class='cardTask'>
        <task-card v-for='item in cardList' :key='item.id' :taskList='item' />
      </div>

      <!-- 待办事项 -->
      <div class='eventArea'>
        <card-event ref='eventAreaRef' />
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
          <introduce ref='introduceRef' />
        </div>
        <!-- 日历 -->
        <div class='calendar'>
          <calendar @clickDay='getScheduleByClickDay' />

        </div>
        <!-- 日程 -->
        <div class='schedule'>
          <schedule ref='scheduleRef' />
        </div>
      </div>

    </div>


  </div>
</template>

<script>
import TaskCard from '../components/membermanagement/TaskCard.vue'
import ChartTitle from '../components/membermanagement/ChartTitle.vue'
import CardEvent from '../components/membermanagement/CardEvent'
import Introduce from '../components/membermanagement/Introduce'
import Calendar from '../components/membermanagement/Calendar.vue'
import Schedule from '../components/membermanagement/Schedule'
import Chart from '../components/membermanagement/Chart'
import { getAction, postAction } from '../api/manage'

export default {
  name: 'Index',
  components: {
    TaskCard, ChartTitle, CardEvent, Introduce, Calendar, Schedule, Chart
  },
  created() {
    this.getTaskCard()
    this.getEventCard()
    this.getUserInfo()
    this.getSchedule()
  },

  data() {
    return {
      //用户id，需要登录后获取到
      userId: 1,
      cardList: [],
      url: {
        taskCard: 'http://localhost:8080/jeecg-boot/users/getTaskCard',
        scheduleCard: 'http://localhost:8080/jeecg-boot/users/getScheduleCard',
        userCard: 'http://localhost:8080/jeecg-boot/users/getUserInfo',
        eventCard: 'http://localhost:8080/jeecg-boot/users/getEventCard',
        getNote: 'http://localhost:8080/jeecg-boot/users/getNote'
      }
    }
  },

  methods: {
    /**
     * 获取当前时间 yyyy-MM-dd hh:mm:ss
     * @returns {string}
     */
    getCurrentTime() {
      var date = new Date()
      var year = date.getFullYear() //月份从0~11，所以加一
      var dateArr = [date.getMonth() + 1, date.getDate(), date.getHours(), date.getMinutes(), date.getSeconds()]
      for (var i = 0; i < dateArr.length; i++) {
        if (dateArr[i] >= 1 && dateArr[i] <= 9) {
          dateArr[i] = '0' + dateArr[i]
        }
      }
      var strDate = year + '-' + dateArr[0] + '-' + dateArr[1] + ' ' + dateArr[2] + ':' + dateArr[3] + ':' + dateArr[4]

      return strDate
    },
    /**
     * 获取当前日期 yyyy-MM-dd
     */
    getCurrentDateOfYMd(formData) {
      let time
      if (formData === undefined) {
        time = new Date()
      } else {
        time = new Date(formData)
      }
      return time.toLocaleDateString().replaceAll('/', '-')
    },

    /**
     * 根据收到的数据中的字段进行排序
     * @param field 字段
     * @param method 排序方式 true升序（默认） false降序
     * @returns {function(*, *): number}
     */
    dataCompare(field, method) {
      // 第二个参数没有传递，默认升序排序
      if (method === undefined) {
        method = 1
      } else {
        method = method ? 1 : -1
      }
      return function(obj1, obj2) {
        // 方括号也是访问对象属性的一种方式，优点是可以通过变量访问。
        // 常规写法是 var val1 = obj1.prop;var val2 = obj2.prop;,但是这种不支持变量写法，所有这里不适用
        var val1 = obj1[field],
          val2 = obj2[field]

        // 若是升序排序，此时rev=1,rev*-1=-1,等价于return val1 < val2 ? -1 : 1,，即val1<val2时，val1放在val2前面，否则放后面
        // 若是降序排序，下面句子等价于return val1 < val2 ? 1 : -1，即val1<val2时，val1放在val2后面，否则放在val2前面
        return val1 < val2 ? method * (-1) : method * 1
      }
    },
    /**
     * 获取卡片信息数据
     * @returns {Promise<void>}
     */
    async getTaskCard() {
      const { data } = await getAction(this.url.taskCard, null)
      this.cardList = data
    },

    /**
     * 获取用户信息数据
     * @returns {Promise<void>}
     */
    async getUserInfo() {
      const { data } = await getAction(this.url.userCard + `/${this.userId}`)
      this.$nextTick(() => {
        this.$refs.introduceRef.userInfo = data
      })
    },
    /**
     * 获取代办事项卡片数据
     * @returns {Promise<void>}
     */
    async getEventCard() {
      const { data } = await getAction(this.url.eventCard + `/${this.userId}`)
      this.$refs.eventAreaRef.itemList = [...data.filter(item => {
        return item.attentionFlag === '0'
      })]
      this.$refs.eventAreaRef.attentions = [...data.filter(item => {
        return item.attentionFlag === '1'
      })]
    },
    /**
     * 获取日程数据
     * @param day
     * @returns {Promise<void>}
     */
    async getSchedule(day) {
      let params = new URLSearchParams()
      params.append('userId', `${this.userId}`)

      if (day === undefined) {
        params.append('date', `${this.getCurrentDateOfYMd()}`)
      } else {
        params.append('date', `${day}`)
      }
      const { data } = await postAction(this.url.scheduleCard, params)
      this.$nextTick(() => {
        this.$refs.scheduleRef.scheduleList =
          [...data.filter(item => item.checkFlag === '0').sort(this.dataCompare('modifyTime', false)),
            ...data.filter(item => item.checkFlag === '1').sort(this.dataCompare('modifyTime', true))]
      })
    },

    /**
     * 点击获取日程数据
     * @param date
     */
    getScheduleByClickDay(date) {
      let clickDay = this.getCurrentDateOfYMd(date._d)
      this.getSchedule(clickDay)
    },

    async getNote(day) {
      let params = new URLSearchParams()
      params.append('userId', `${this.userId}`)

      if (day === undefined) {
        params.append('date', `${this.getCurrentDateOfYMd()}`)
      } else {
        params.append('date', `${day}`)
      }
      // const { data } = await postAction(this.url.getNote, params)
      // this.$nextTick(() => {
      //   this.$refs.scheduleRef.noteList =
      //     [...data.filter(item => item.checkFlag === '0'),
      //       ...data.filter(item => item.checkFlag === '1')]
      // })
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
