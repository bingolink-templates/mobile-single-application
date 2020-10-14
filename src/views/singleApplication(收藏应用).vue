<template>
    <div ref="wrap">
        <bui-header title="picker" :leftItem="leftItem" @leftClick="back">
        </bui-header>
        <div class="run" @click="send1">getAllServices</div>
        <div class="run" @click="send2">getFavoriteService</div>
        <div class="run" @click="send3">startApp</div>
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
        send1: function () {
            linkapi.getAllServices((res) => {
                this.$alert(res);
            }, (err) => {
                this.$alert(err);
            })
        },
        send2: function () {
            linkapi.getFavoriteApp((res) => {
                this.$alert(res);
            }, (err) => {
                this.$alert(err);
            })
        },
        send3: function () {
             link.startApp(['', {}], () => {
                 this.$alert('success');
             }, () => {
                 this.$alert('error');
             });
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