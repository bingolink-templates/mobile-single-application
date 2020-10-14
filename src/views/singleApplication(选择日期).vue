<template>
    <div ref="wrap">
        <bui-header title="picker" :leftItem="leftItem" @leftClick="back">
        </bui-header>
        <div class="run" @click="send2">picker-年月日时分不限制</div>
        <div class="run" @click="send3">picker-年月日不限制</div>
        <div class="run" @click="send4">picker-时分</div>
        <div class="run" @click="send5">picker-指定年月日</div>
        <div class="run" @click="send6">picker-指定年月日时分</div>
        <div class="run" @click="send">picker-年月日限制最小天数2020-8-23/最大天数为2020-9-28</div>
        <div class="run" @click="send1">picker-年月日时分限制最小天数2020-8-23/最大天数为2020-9-28</div>
    </div>
</template>

<script>
const dom = weex.requireModule("dom");
const link = weex.requireModule("LinkModule");
const linkapi = require("linkapi");
const picker = weex.requireModule('picker');
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
        send: function () {
            picker.pickDate({
                min: '2020-8-23',
                max: '2020-9-28',
                format: 'yyyy-MM-dd'
            }, event => {
                this.$alert(JSON.stringify(event));
                if (event.result === 'success') {
                }
            })
        },
        send1: function () {
            picker.pickDate({
                min: '2020-8-23 10:10',
                max: '2020-9-28 10:10',
                format: 'yyyy-MM-dd HH:mm'
            }, event => {
                this.$alert(JSON.stringify(event));
                if (event.result === 'success') {
                }
            })
        },
        send2: function () {
            picker.pickDate({
                format: 'yyyy-MM-dd HH:mm'
            }, event => {
                this.$alert(JSON.stringify(event));
                if (event.result === 'success') {
                }
            })
        },
        send3: function () {
            picker.pickDate({
                format: 'yyyy-MM-dd'
            }, event => {
                this.$alert(JSON.stringify(event));
                if (event.result === 'success') {
                }
            })
        },
        send4: function () {
            picker.pickDate({
                format: 'HH:mm'
            }, event => {
                this.$alert(JSON.stringify(event));
                if (event.result === 'success') {
                }
            })
        },
        send5: function () {
            picker.pickDate({
                value: '2020-8-25',
                min: '2020-8-23',
                max: '2020-9-28',
                format: 'yyyy-MM-dd'
            }, event => {
                this.$alert(JSON.stringify(event));
                if (event.result === 'success') {
                }
            })
        },
        send6: function () {
            picker.pickDate({
                value: '2020-8-25 10:10',
                min: '2020-8-23 10:10',
                max: '2020-9-28 10:10',
                format: 'yyyy-MM-dd HH:mm'
            }, event => {
                this.$alert(JSON.stringify(event));
                if (event.result === 'success') {
                }
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
    height: 20wx;
    display: flex;
    justify-content: center;
    align-content: center;
    align-items: center;
    background-color: #ccc;
    margin-bottom: 100px;
}
</style>