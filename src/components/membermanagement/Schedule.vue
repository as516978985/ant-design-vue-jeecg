<template>
  <div>
    <a-tabs default-active-key='1' @change='callback'>
      <a-tab-pane key='1' tab='我的日程'>
        <div class='content test-1'>

          <a-card v-for='item in itemList' :key='itemList.id'>
            <div class='item'>
              <div class='item_title'>
                <i v-if='item.icon' :class='`iconfont size ${item.icon}`'></i>
                <div>{{ item.title }}</div>
                <input v-if='item.checkFlag==1' type='checkbox' @change='check(item)' checked>
                <input v-else type='checkbox' @change='check(item)'>
              </div>
              <div class='item_content'>
                <div class='item_one' v-if='item.startTime&&item.endTime'>
                  <i class='iconfont icon-clock'>{{ item.startTime }}-{{ item.endTime }}</i>
                </div>
                <div class='item_two' v-if='item.advanceTime'>
                  <i class='iconfont icon-notification'> 提前{{ item.advanceTime }}分钟提醒</i>
                </div>
              </div>
            </div>
          </a-card>

        </div>

      </a-tab-pane>
      <a-tab-pane key='2' tab='笔记' force-render>

      </a-tab-pane>
    </a-tabs>
  </div>
</template>
<script>
export default {
  props: ['itemList'],
  created() {
    console.log("scheduleList的值为",this.scheduleList)
  },
  mounted() {
    // console.log("子组件收到scheduleList数据",this.itemList)
    // console.log('将父组件接到的数据转存到scheduleList中')
    // console.log("从父组件收到的值",this.itemList)
    // console.log("排序开始")
    // console.log('将父组件接到的数据转存到scheduleList中')
    // this.scheduleList=[...this.flagSort(this.itemList)]
    // console.log("排序结束")
    // console.log("现在scheduleList的值为",this.scheduleList)
    // console.log("准备将值交给for循环")
  },
  data() {
    return {
      scheduleList:[]
    }
  },
  // computed: {
  //   order() {
  //     return this.itemList.checkFlag == 1 ? 'order: 1' : 'order: 0'
  //   }
  // },
  methods: {
    callback(key) {
      console.log(key)
    },
    /**
     * 勾选日程框
     * @param item
     */
    check(item) {
      console.log("被改变的item",item)
      if (item.checkFlag === 1) {
        item.checkFlag = 0
      } else {
        item.checkFlag = 1
      }
    },
    /**
     * 根据flagCheck排序
     * @param list
     * @returns {*}
     */
    flagSort(list) {
      list.sort((s, e) => {
        return Number(s.checkFlag) - Number(e.checkFlag)
      })
      return list
    }
  }
}
</script>
<style scoped>
* >>> .ant-tabs-nav .ant-tabs-tab {
  width: 22.3vh !important;
  margin: 0;
  text-align: center !important;
}

* >>> .ant-card-body {
  padding: 0;
  margin-bottom: 2vh;
}

* >>> .ant-card-bordered {
  border: none;
}

* >>> .ant-tabs-bar {
  margin: 0 0 2% 0;
}

.size {
  margin-right: -8%;
  color: white;
  padding: 0 2%;
  font-size: 1.5vw;
  background-color: #3988fd;
}

.content {
  height: 31.1vh;
  width: inherit;
  overflow: auto;
}

.item {
  display: flex;
  flex-basis: 20%;
  flex-direction: column;
  background-color: #f7f9f8;
  padding-bottom: 2%;
}

.item_title {
  display: flex;
  width: 90%;
  flex-basis: 50%;
  font-weight: bold;
  justify-content: space-between;
  align-items: center;
  margin: 1.2vh 1.5vw 0 1.5vw;
}

.item_title > div {
  flex-basis: 70%;
  font-size: 1.1vw;
}

.item_title > input {
  flex-basis: 5%;
  height: 2vh;
}

.item_content {
  display: flex;
  flex-basis: 50%;
  justify-content: flex-start;
  color: #f7f9f8;
  align-items: center;
  font-size: 1.5vw;
  margin-left: 6%;
}

.item_one {
  flex-basis: 30%;
  border: 0;
}

.item_two {
  border: 0;
}

.item_content i {
  font-size: 60%;
  color: #bfc1c0;
  font-weight: bolder;
  margin: 0;
  padding: 0;
}

.test-1::-webkit-scrollbar {
  /*滚动条整体样式*/
  width: 0px;
  /*高宽分别对应横竖滚动条的尺寸*/
  height: 1px;
}

.test-1::-webkit-scrollbar-thumb {
  /*滚动条*/
  border-radius: 50px;
  /*-webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);*/
  background: #e8e8e8;
}

.test-1::-webkit-scrollbar-track {
  /*滚动轨道*/
  /*-webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);*/
  background: white;
  border-radius: 50px;
  width: 10%;
}
</style>
  