<template>
    <div ref="wrap">
        <bui-header title="weex与bui交互" :leftItem="leftItem" @leftClick="back">
        </bui-header>
        <div class="run" @click="run">演示跳转到bt应用，weex可以接收bt返回的参数</div>
        <div class="run" @click="send">演示weex发送到bt通知事件</div>
        <div class="run">weex内嵌编码为test_params1的bt应用，需要自行上架到应用管理</div>
        <LinkApp code='test_params1' style="height: 400px; width: 750px;"></LinkApp>
    </div>
</template>

<script>
const dom = weex.requireModule("dom");
const link = weex.requireModule("LinkModule");
const linkapi = require("linkapi");
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
        linkapi.registerReceiverEvent('weexSendEvent', (res) => {
            this.$alert(res);
        }, (success) => {
        }, (err) => {
        })
    },
    methods: {
        back: function () {
            // 返回上一个页面
            this.$pop();
        },
        run: function () {
            // 打开应用的方式， test_params1需要上架到应用
            linkapi.runApp({
                appCode: 'test_params1',
                data: {
                    title: 'weex与bui交互'
                }
            }, (res) => {
                this.$alert(res);
            }, (err) => {
                this.$alert(err);
            })
        },
        send: function () {
            linkapi.sendBroadcast('buiSendEvent', { data: [3, 4, 5] }, (success) => {
            }, (err) => {
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
    height: 100wx;
    display: flex;
    justify-content: center;
    align-content: center;
    align-items: center;
    background-color: #ccc;
    margin-bottom: 100px;
}
</style>