<template>
    <div ref="wrap">
        <bui-header title="picker" :leftItem="leftItem" @leftClick="back">
        </bui-header>
        <div class="run" @click="send1">getTodayStepCount</div>
        <div class="run" @click="send2">getNativeTodayStepCount</div>
        <div class="run" @click="send3">getStepCountFrom</div>
        <div class="run" @click="send4">savedFrom</div>
        <div class="run" @click="send5">getStepCountSavedFrom</div>
    </div>
</template>

<script>
const dom = weex.requireModule("dom");
const link = weex.requireModule("LinkModule");
const linkapi = require("linkapi");
const StepCounter = weex.requireModule("StepCounter");
export default {
    data() {
        return {
            leftItem: {
                icon: 'ion-chevron-left',
            }
        }
    },
    created() {
    },
    mounted() {
    },
    methods: {
        back: function () {
            // 返回上一个页面
            this.$pop();
        },
        send1: function () {
            StepCounter.getTodayStepCount({}, (res) => {
                this.$alert(res);
            }, (err) => {
                this.$alert(err);
            })
        },
        send2: function () {
            StepCounter.getTodayStepCount({ "from": "native" }, (res) => {
                this.$alert(res);
            }, (err) => {
                this.$alert(err);
            })
        },
        send3: function () {
            StepCounter.getStepCountFrom({}, (res) => {
                this.$alert(res);
            })
        },
        send4: function () {
            StepCounter.savedFrom({ "from": "native" }, '', (res) => {
                this.$alert(res);
            })
        },
        send5: function () {
            StepCounter.getStepCountSavedFrom({}, (res) => {
                this.$alert(res);
            }, (err) => {
                this.$alert(err);
            })
        }
    }
}
</script>

<style lang="css" src="../css/common.css"></style>
<style>
.guarantee {
    background: #fff;
}
.run {
    height: 50wx;
    display: flex;
    justify-content: center;
    align-content: center;
    align-items: center;
    background-color: #ccc;
    margin-bottom: 20px;
}
</style>