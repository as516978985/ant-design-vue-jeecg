<template>
  <div :style="{ width: '300px', border: '1px solid #d9d9d9', borderRadius: '4px' }">
    <a-calendar :fullscreen="false" @panelChange="onPanelChange" />
  </div>
</template>
<script>
export default {
  methods: {
    onPanelChange(value, mode) {
      console.log(value, mode);
    },
  },
};
</script>
<style scoped>
*>>>.ant-fullcalendar table {
  height: 30vh !important;
}

*>>>.ant-fullcalendar-header {
  padding: 11px 0 11px 0;
  text-align: center;
}
</style>
  