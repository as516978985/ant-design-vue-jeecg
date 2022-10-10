<template>
  <div class="task" :style="borderColor">
    <i class="iconfont icon-Leftbar_feature" :style="fontColor"></i>
    <div class="title">{{ taskList.title }}</div>
    <div class="block"></div>
    <div class="count" :style="borderChangeColor">{{ taskList.count }}</div>
    <span>&nbsp个</span>
    <!-- <div :class="[PercentColor ? 'percentup' : 'percentdown']">↑{{Math.abs(taskList.percent)}}%</div> -->

    <div v-if="PercentColor" class="percentup">↑{{ Percent }}%</div>
    <div v-else class="percentdown">↓{{ Percent }}%</div>
    <div class="data">数据截止于昨天</div>
  </div>

</template>

<script>
export default {
  props: ["taskList"],
  computed: {
    PercentColor() {
      return this.taskList.percent >= 0
    },

    Percent() {
      return Math.abs(this.taskList.percent)
    }
    ,
    borderColor() {
      return `border-right: 4px solid ${this.taskList.color}`
    },
    fontColor() {
      return `background-color: ${this.taskList.color}`
    },
    borderChangeColor() {
      return `border-image: linear-gradient(to bottom, ${this.taskList.color}, white) 1;`
    },

  },

  data() {
    return {};
  },
}

</script>

<style scoped>
.task {
  display: flex;
  background-color: white;
  flex-wrap: wrap;
  align-content: center;
  width: 24.5%;
  padding-bottom: 1%;
  border-right: 4px solid black;
}


.task > * {
  display: flex;
  align-items: center;
  justify-content: center;
  /* margin-bottom: 5%; */
}

.iconfont {
  /*margin-left: 7%;*/
  /*margin-right: 3%;*/
  margin: 5% 3% 2% 7%;
  color: white;
  /* padding: 1.5%; */
  padding: 0 2%;
  font-size: 1.5vw;
  background-color: black;
  /*margin-top: 5%;*/

}

.title {
  font-size: 1.1vw;
  font-weight: bold;
  margin-top: 5%;
  margin-bottom: 2%;
}

.block {
  background-color: #e3e3e3;
  width: 91%;
  height: 1%;
  /* margin-left: 5%; */
  margin: 2% 0 4% 5%
}

.count {
  font-size: 1.3vw;
  font-weight: bold;
  margin-left: 7%;
  border-left: 3px solid;
  border-image: linear-gradient(to bottom, black, white) 1;
}

span {
  color: #a4a4a4;
  font-size: 1.2vw;
}

.percentup {
  color: #9bf622;
  margin: 1% 1% 0 3%;
  font-size: 1vw;
  /*margin-top: 1%;*/
}

.percentdown {
  color: red;
  /*margin-top: 1%;*/
  margin: 1% 1% 0 3%;
  font-size: 1vw;
}

.data {
  color: #a4a4a4;
  font-size: 1vw;
  /* margin-top: 1%; */
}
</style>