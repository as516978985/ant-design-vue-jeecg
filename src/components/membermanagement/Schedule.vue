<template>
  <div>
    <a-tabs default-active-key='1' @change='callback'>
      <a-tab-pane key='1' tab='我的日程'>
        <div class='content test-1'>

          <a-card v-for='item in scheduleList' :key='item.id'>
            <div class='item'>
              <div class='item_title'>
                <i v-if='item.icon' :class='`iconfont size ${item.icon}`'></i>
                <div v-if='item.checkFlag==1' style='color: #bec1c0'>{{ item.title }}</div>
                <div v-else>{{ item.title }}</div>
                <input v-if='item.checkFlag==1' type='checkbox' @change='check(item)' checked>
                <input v-else type='checkbox' @change='check(item)'>
              </div>
              <div class='item_content'>
                <div class='item_one' v-if='item.startTime && item.endTime'>
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
        <div class='content test-1'>
          <textarea class='addNote test-1' type='text' v-model='addNoteContext' @keyup.enter='addNote'
                    placeholder='添加笔记 回车确认' />

          <a-card v-for='item in noteList' :key='item.id'>
            <div class='item_note'>
              <div class='note_title'>
                <div>
                  <input v-if='item.checkFlag==1' type='checkbox' @change='checkNote(item)' checked>
                  <input v-else type='checkbox' @change='checkNote(item)'>
                </div>
                <div class='note_create_time' v-if='item.checkFlag==1' style='color: #bec1c0'>笔记创建时间：
                  {{ item.createTime }}
                </div>
                <div class='note_create_time' v-else>笔记创建时间：{{ item.createTime }}</div>
              </div>
              <div class='note_content' v-if='item.checkFlag==1' style='color: #bec1c0'>
                {{ item.content }}
              </div>
              <div class='note_content' v-else>
                {{ item.content }}
              </div>

              <div class='note_content'>

              </div>
            </div>

          </a-card>
        </div>
      </a-tab-pane>
    </a-tabs>
  </div>
</template>
<script>
import { getAction, postAction } from '../../api/manage'

export default {
  // created() {
  //   console.log("S区",this.checkedDay)
  // },
  data() {
    return {
      scheduleList: [],
      noteList: [{
        userId: 1,
        checkFlag: 1,
        createTime: '2022-10-14',
        content: '读取到的笔记内容读取到的笔记内容读取到的笔记内容读取到的笔记内容读取到的笔记内容读取到的笔记内容'
      }],
      addNoteContext: '',
      url: {
        changeScheduleFlag: 'http://localhost:8080/jeecg-boot/user/changeScheduleFlag'
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
      let params = new URLSearchParams()
      params.append('scheduleId', `${item.id}`)
      params.append('userId', `${item.userId}`)
      params.append('date', `${item.recDate}`)
      let { data } = await postAction(this.url.changeScheduleFlag, params)
      console.log('获得的数据为：', data)
      this.scheduleList =
        [...data.filter(item => item.checkFlag === 0).sort(this.dataCompare('modifyTime', false)),
          ...data.filter(item => item.checkFlag === 1).sort(this.dataCompare('modifyTime', true))]
    },

    /**
     * 排序
     * @param field 字段，此处用[]取字段，因为.用不了
     * @param method 第二个参数没有传递，默认升序排序
     * @returns {function(*, *): number}
     */
    dataCompare(field, method) {

      if (method === undefined) {
        method = 1
      } else {
        method = method ? 1 : -1
      }
      return function(obj1, obj2) {
        var val1 = obj1[field],
          val2 = obj2[field]
        return val1 < val2 ? method * (-1) : method * 1
      }
    },
    checkNote(item) {
      this.$emit('changeNoteFlag', item)
    },


    /**
     * 添加笔记
     * @returns {Promise<void>}
     */
    async addNote() {
      console.log('checkDay', this.checkedDay)
      console.log('看一下输入的内容', this.addNoteContext)

      console.log('子数据', this.addNoteContext)
      this.$emit('addNote', this.addNoteContext)
      this.addNoteContext = ''
    }
  }
}
</script>
<style scoped>
* >>> .ant-tabs-nav .ant-tabs-tab {
  width: 83% !important;
  /*width: inherit !important;*/
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
  border: 0;
  width: fit-content;
  margin-right: 3%;
}

.item_two {
  border: 0;
  width: fit-content;

}

.item_content i {
  font-size: 60%;
  color: #bfc1c0;
  font-weight: bolder;
  margin: 0;
  padding: 0;
}

.addNote {
  border: 1px solid gray;
  border-radius: 10px;
  width: 100%;
  height: 25%;
}

.item_note {
  display: flex;
  flex-basis: 20%;
  flex-direction: column;
  background-color: #f6f9f8;
  /*color: black;*/
  padding: 2%;
}

.note_title {
  display: flex;
  justify-content: space-between;
  padding: 0 2%;
}

.note_content {
  margin-top: 2%;
  text-indent: 2em;
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
  