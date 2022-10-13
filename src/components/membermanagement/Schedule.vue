<template>
  <div>
    <a-tabs default-active-key='1' @change='callback'>
      <a-tab-pane key='1' tab='我的日程'>
        <div class='content test-1'>

          <a-card v-for='item in scheduleList' :key='item.id'>
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
import { getAction, postAction } from '../../api/manage'

export default {
  // created() {
  //   console.log('scheduleList的值为', this.scheduleList)
  // },
  updated() {
    console.log('现在scheduleList的值为', this.scheduleList)
  },
  data() {
    return {
      scheduleList: [],
      url: {
        changeScheduleFlag: 'http://localhost:8080/jeecg-boot/users/changeScheduleFlag'
      }
    }
  },
  methods: {
    /**
     * tab控制
     * @param key
     */
    callback(key) {
      console.log(key)
    },
    /**
     * 勾选日程框
     * @param item
     */
    async check(item) {
      console.log('被改变的item', item)
      let { data } = await getAction(this.url.changeScheduleFlag + `/${item.id}`)
      this.scheduleList =
        [...data.filter(item => item.checkFlag === '0').sort(this.dataCompare('modifyTime', false)),
          ...data.filter(item => item.checkFlag === '1').sort(this.dataCompare('modifyTime',true))]
    },

    /**
     * 排序
     * @param field
     * @param method
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
  