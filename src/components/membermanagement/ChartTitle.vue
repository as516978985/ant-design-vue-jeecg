<template>
    <div class="chart_title">
        <div class="title_left">
            <i class="iconfont icon-zoushi" />
            <span>数据统计</span>
            <div class="task" v-for="item in taskList" :key="item.id">
                <div class="square" :style="`background-color:${item.color}`"></div>
                <span class="title">{{item.title}}</span>
            </div>
        </div>
        <div class="title_right">
            <select class="task_type" name="task_type">
                <option name="task_type">延期任务量</option>
            </select>
            <select class="task_type" name="task_type">
                <option name="task_type">本月</option>
            </select>
        </div>
        <div class="block"></div>

    </div>
</template>

<script>
export default {
    comments: {

    },
    props: ['taskList'],
    data() {
        return {
            item: {
                title: "本月新增任务量",
                count: 10,
                percent: 10,
                color: '#9c77f6',
            },
        };
    },
}

</script>

<style scoped>
.chart_title {
    display: flex;
    height: 14%;
    width: 100%;
    flex-wrap: wrap;
    background-color: white;
    /* border-bottom: 1px solid white; */

}

.title_left {
    width: auto;
    height: 98%;
    display: flex;
    flex-grow: 1;
    justify-content: flex-start;
    flex-direction: row;
    overflow: hidden;
    /* align-items: center; */
}

.title_left>* {
    display: flex;
    align-items: center;
    justify-content: center;
}

span {
    font-size: 1.2vw;
}

.iconfont {
    color: yellowgreen;
    padding: 1%;
    font-size: 1.4vw;
}

.task {
    display: flex;
    align-items: center;
    width: auto;
    justify-content: space-between;
}

.square {
    width: 0.5vw;
    height: 1vh;
    margin: 0 0.5vw;
    background-color: black;
}

.title {
    font-size: 1.1vw;
}

.title_right {
    display: flex;
    flex-shrink: 0;
    height: 98%;
    flex-direction: row;
    align-items: center;
    justify-content: flex-end;

}

select {
    background-color: #f7f8f8;
    border: 0;
    font-size: 1vw;
    height: 80%;
    width: 8vw;
    text-align: center;
    margin-right: 10%;
}


.block {
    width: 98%;
    height: 1%;
    background-color: #e4e4e4;
    margin-left: 1%;
}
</style>