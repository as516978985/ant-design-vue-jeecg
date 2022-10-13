<template>
  <div class='card'>
    <div class='card_profile'>
      <div class='card_title'>
        <div class='event_card_title'><i class='iconfont icon-neirongfabu'></i>{{ item.title }}</div>
        <div class='card_content'>{{ item.content }}</div>
      </div>
      <!-- <div class="card_img" v-html="'<img src='+'@/assets/img/photo.jpg'+'>'"> -->
      <div class='card_img'><img :src='item.rightImg' /></div>
    </div>
    <div class='card_time'>
      <i class='iconfont icon-Leftbar_feature'></i>&nbsp;{{ item.startTime }}-{{ item.endTime }}
    </div>
    <div class='card_create'>
      <div class='profile_photo'>
        <img :src='item.leftImg'>
        <div>&nbsp; {{ item.name }}</div>
      </div>
      <div class='created'>创建于{{ item.createTime }}</div>
    </div>
    <div class='card_request'>
      <div @click='agree' v-if='item.attentionFlag=="0"'>关注任务</div>
      <div @click='agree'v-else>取消关注</div>
    </div>
  </div>


</template>

<script>
export default {
  props: ['item'],
  data() {
    return {}
  },
  methods: {
    agree() {
      this.$emit("addAttention",this.item)
      console.log(this.item)
    },
    refuse() {
      console.log(this.item)
    }
  }
}
</script>

<style scoped>
.card {
  /* width: 24vw; */
  height: 30vh;
  background-color: #f7f7f7;
  margin: 0 1% 1% 1%;
}

.event_card_title {
  display: flex;
  margin-top: 5%;
  margin-bottom: 2%;
}

.iconfont {
  /* margin: 5% 5% 0 5%; */
  margin: 0 5%;
  padding: 0;
  font-size: 100%;
  color: #b6b6b6;
}

.card_profile {
  display: flex;
  flex-basis: 35%;
  margin-left: 0.3vh;
  align-items: center;
  justify-content: space-between;
}

.card_title {
  flex-basis: 60%;
  color: #b6b6b6;
  font-weight: bolder;
  font-size: 1.1vw;
}

.card_content {
  margin: 0 0 0 5%;
  font-weight: bold;
  color: black;
}

.card_img {
  flex-basis: 18%;
}

.card_img > img {
  margin-top: 0.5vh;
  height: 6vh;
  width: 6vh;
}


.card_time {
  display: flex;
  align-items: center;
  border: 1px solid #b6b6b6;
  /*border-bottom: none;*/
  width: 14vw;
  margin-left: 4%;
  color: #b6b6b6;
  font-size: 0.9vw;
  flex-basis: 15%;
}

.card_create {
  display: flex;
  align-items: center;
  flex-basis: 25%;
  justify-content: space-between;
  border-bottom: 1px solid #b6b6b6;
  border-top: 1px solid #b6b6b6;
  color: #b6b6b6;
  font-size: 1.2vw;
  margin: 1% 5% 0 4%;
}

.profile_photo {
  display: flex;
  align-items: center;
}

.profile_photo > img {
  margin: 0;
  height: 6vh;
  width: 6vh;

}

.card_request {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  flex-basis: 21%;
  font-size: 1.2vw;
  margin: 0 5%;
}

.card_request > div {
  margin-left: 10%;
  font-weight: bolder;
  color: #3cb58f;
}
</style>