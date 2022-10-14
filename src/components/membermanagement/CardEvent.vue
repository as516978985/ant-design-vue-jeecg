<template>
  <div>
    <a-tabs default-active-key='1' @change='callback'>
      <a-tab-pane key='1' tab='待办事项' class='event'>
        <div class='event_content test-1'>
          <event-card v-for='item in itemList' :key='item.id' :item='item' @addAttention='addAttention' />
        </div>
      </a-tab-pane>

      <a-tab-pane key='2' tab='关注任务'>
        <div class='event_content test-1'>
          <event-card v-for='item in attentions' :key='item.id' :item='item' @addAttention='addAttention' />
        </div>
      </a-tab-pane>

      <a-tab-pane key='3' tab='里程碑任务'>

      </a-tab-pane>
    </a-tabs>
  </div>
</template>
<script>
import eventCard from './eventCard.vue'
import { getAction, postAction } from '../../api/manage'

export default {
  components: {
    eventCard
  },
  created() {

  },
  data() {
    return {
      itemList: [],
      attentions: [],

      url: {
        changeAttention: 'http://localhost:8080/jeecg-boot/users/changeEventCardAttention/'
      }

    }
  },
  methods: {
    /**
     * 标签控制
     * @param key
     */
    callback(key) {
      console.log(key)
    },
    /**
     * 关注控制
     * @param val
     */
    async addAttention(val) {
      let params = new URLSearchParams()
      params.append('id', `${val.id}`)
      let { data } = await postAction(this.url.changeAttention, params)
      this.itemList = [...data.filter(item => {
        return item.attentionFlag === '0'
      })]
      this.attentions = [...data.filter(item => {
        return item.attentionFlag === '1'
      })]

      // this.itemList = [...val.filter(item => {
      //   return item.attentionFlag === '0'
      // })]
      // this.attentions = [...val.filter(item => {
      //   return item.attentionFlag === '1'
      // })]
    }
  }
}
</script>

<style scoped>
* >>> .ant-tabs-bar {
  margin: 0 0 2vh 0;
}

* >>> .ant-tabs-nav {
  height: 6.7vh;
}


.event_content {
  display: flex;
  height: 30vh;
  justify-content: flex-start;
  flex-wrap: wrap;
  overflow: auto;
  /* margin-top: 2%; */
  /*flex-shrink: 0;*/
  /*border: 1px solid black;*/
}

.event_content > div {
  display: flex;
  flex-direction: column;
  flex-basis: 31%;
  height: 30vh;
  margin: 0 1.5vh 4vh 1.5vh;
  /*border: 1px solid black;*/
  /*align-items: center;*/
  background-color: #f7f7f7;
}

/*滚动条*/
.test-1::-webkit-scrollbar {
  /*滚动条整体样式*/
  width: 5px;
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