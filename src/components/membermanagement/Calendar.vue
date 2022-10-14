<template>
  <div >
    <a-calendar :fullscreen='false' @panelChange='onPanelChange' @select='onSelect' />
  </div>
</template>
<script>
export default {
  methods: {
    onPanelChange(value, mode) {
      console.log(value, mode)
    },
    onSelect(date) {
      this.$emit("clickDay",date)
    }
  }
}
</script>
<style scoped>
* >>> .ant-fullcalendar table {
  height: 30vh !important;
}

* >>> .ant-fullcalendar-header {
  padding: 11px 0 11px 0;
  text-align: center;
}
</style>
  