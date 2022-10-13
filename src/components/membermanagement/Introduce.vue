<template>
  <div class='introduce'>
    <div class='img'><img :src='userInfo.img'/></div>
    <div class='name'>
      {{ userInfo.name }}
    </div>
    <div class='department'>
      {{ userInfo.department }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      userInfo: {}
    }
  }
}

</script>

<style scope>
.introduce {
  flex-basis: 5%;
  flex-direction: column;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: white;
  flex-shrink: 0;
  font-size: 1.2vw;
}

.img {
  flex-basis: 50%;
}

img {
  margin-top: 30%;
  height: 9vh;
  width: 5vw;
}

.name {
  flex-basis: 15%;
  margin: 1% 0;
}

.department {
  flex-basis: 15%;
  margin-bottom: 2%;
}
</style>