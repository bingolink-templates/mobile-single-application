<template>
    <div ref="wrap" class="main">
        <bui-header title="Android-replace" :leftItem="leftItem" @leftClick="back">
        </bui-header>
        <div class="run" @click="replace">replace到百度地址</div>
        <div class="run" @click="startContactMulitSelector">startContactMulitSelector选人</div>
        <div class="run" @click="openCamera">打开camera</div>
        <div class="came" v-if="camera">
            <camera ref='cameraComp' class="cameraComp" facing="front" />
            <div class="scan">
                <text class="scan-font">对准摄像头进行拍摄</text>
            </div>
            <div class="take">
                <text class="scan-take" @click="take()">take picture</text>
            </div>
        </div>
    </div>
</template>

<script>
const dom = weex.requireModule("dom");
const link = weex.requireModule("LinkModule");
const linkapi = require("linkapi");
const navigator = weex.requireModule('navigator');
const navigatorLink = weex.requireModule('NavigatorExModule');
const globalEvent = weex.requireModule('globalEvent');
export default {
    data() {
        return {
            leftItem: {
                icon: 'ion-chevron-left',
            },
            camera: false
        }
    },
    created() {
    },
    mounted() {
        globalEvent.addEventListener("androidback", function (e) {
            navigator.close()
        });
    },
    methods: {
        back: function () {
            // 返回上一个页面
            this.$pop();
        },
        take: function () {
            this.$refs.cameraComp.execute('takePicture', { "savePath": '/storage/self/primary/pic.jpg' }, () => {
                 this.camera = false
            }, () => {
                this.camera = false
            });
        },
        openCamera() {
            this.camera = true
        },
        replace: function () {
            navigatorLink.replace('http://10.7.1.61:8081/dist/app.js?_wx_tpl=http://10.7.1.61:8081/dist/app.js', {}, ()=>{});
        },
        startContactMulitSelector: function () {
            linkapi.startContactMulitSelector('选人', 1, {}, (res) => {
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
.main {
    flex: 1;
}
.came {
    position: absolute;
    top: 0;
    left: 0;
    flex: 1;
}
.cameraComp {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    flex: 1;
}
.scan {
    position: absolute;
    top: 200wx;
    left: 100wx;
    width: 200wx;
    height: 200wx;
    border: 2px solid #ccc;
}
.scan-font {
    width: 200wx;
    height: 200wx;
    line-height: 200wx;
    font-size: 20wx;
    color: #fff;
    text-align: center;
}
.take {
    position: absolute;
    top: 420wx;
    left: 150wx;
    width: 100wx;
    height: 30wx;
    line-height: 30px;
    background-color: rgb(64, 64, 190);
    border-radius: 3px;
    justify-content: center;
    align-items: center;
}
.scan-take {
    text-align: center;
    color: #fff;
    font-size: 16px;
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