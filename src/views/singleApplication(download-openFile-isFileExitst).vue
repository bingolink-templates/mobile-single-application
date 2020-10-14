<template>
    <div ref="wrap">
        <bui-header title="download-openFile-isFileExitst" :leftItem="leftItem" @leftClick="back">
        </bui-header>
        <div class='guarantee'>
            <p class="a" @click='aa'>下载文件</p>
            <p class="a" @click='bb'>打开文件</p>
            <p class="a" @click='cc'>判断文件是否存在</p>
            <!-- <bui-image src="/image/c.png" width="750px" :height="height" @click='aa'></bui-image>
            <bui-image src="/image/c.png" width="750px" :height="height" @click='bb'></bui-image>
            <bui-image src="/image/c.png" width="750px" :height="height" @click='cc'></bui-image> -->
        </div>
    </div>
</template>

<script>
const dom = weex.requireModule("dom");
const link = weex.requireModule("LinkModule");
const StepCounter = weex.requireModule("StepCounter");
const linkapi = require("linkapi");
const fileTransfer = weex.requireModule("FileTransferModule");
const file = weex.requireModule("FileModule");
export default {
    data() {
        return {
            height: '317px',
            channel: new BroadcastChannel("WidgetsMessage"),
            urlParams: {},
            eCode: '',
            leftItem: {
                icon: 'ion-chevron-left',
            },
            path: ''
        }
    },
    created() {
        this.height = this.$isIPad ? '159wx' : '317px'
    },
    mounted() {
        this.broadcastWidgetHeight()
        // this.urlParams = this.resolveUrlParams(weex.config.bundleUrl)
    },
    methods: {
        back: function () {
            // 返回上一个页面
            this.$pop();
        },
        aa() {
            // linkapi.getTodayStepCount((res) => {
            //     this.$alert(res);
            // }, (err) => {
            //     this.$alert(err);
            // })
            // link.startGestureLockValidation([], () => {
            //     let runApp = {
            //         appCode: 'bingo_newsList',
            //         data: {
            //             title: '新闻列表'
            //         }
            //     }
            //     // 打开应用的方式
            //     linkapi.runApp(runApp)
            // }, () => {
            //     this.$alert('调用手势失败');
            // })
            fileTransfer.download('https://storebeta.bingolink.biz/store/store/getFile?fileId=e6444b995ab54e1694c$d08daa02d10d3&access_token=ZGR6QWRZOjk1NjIwMzYyM2RjMDp1akxueWxFMEU', {
                // saveDir: '保存的目录，默认保存到应用目录下',
                // filename: '保存的文件名，默认使用系统建议的文件名',
                // headers: 'headers',
                // method: 'method',
            }, (progess) => {
                this.$alert(progess);
                // console.log('下载过程：', progess);
            }, (res) => {
                this.path = res.savePath
                this.$alert(res.savePath);
                // console.log('成功回调', res);
            }, (error) => {
                this.$alert(error);
                // console.log('失败回调', error);
            });
        },
        bb() {
            // linkapi.onStepCountChange((res) => {
            //     this.$alert(2);
            // }, (err) => {
            //     this.$alert(1);
            // })
            file.openFile(this.path, {
                // saveDir: '保存的目录，默认保存到应用目录下',
                // filename: '保存的文件名，默认使用系统建议的文件名',
                // headers: 'headers',
                // method: 'method',
            }, (res) => {
                this.$alert(res);
            }, (error) => {
                this.$alert(error);
            })
        },
        cc() {
            fileTransfer.isFileExist('https://storebeta.bingolink.biz/store/store/getFile?fileId=e6444b995ab54e1694c$d08daa02d10d3&access_token=ZGR6QWRZOjk1NjIwMzYyM2RjMDp1akxueWxFMEU', {
                // saveDir: '保存的目录，默认保存到应用目录下',
                // filename: '保存的文件名，默认使用系统建议的文件名',
                // headers: 'headers',
                // method: 'method',
            }, (res) => {
                this.$alert(res);
            }, (error) => {
                this.$alert(error);
            })
        },
        getComponentRect(_params) {
            var that = this
            dom.getComponentRect(this.$refs.wrap, (ret) => {
                this.channel.postMessage({
                    widgetHeight: ret.size.height,
                    id: _params.id
                });
            });
        },
        resolveUrlParams(url) {
            // let url = weex.config.bundleUrl;
            if (!url) return {};
            url = url + "";
            var index = url.indexOf("?");
            if (index > -1) {
                url = url.substring(index + 1, url.length);
            }
            var pairs = url.split("&"),
                params = {};
            for (var i = 0; i < pairs.length; i++) {
                var pair = pairs[i];
                var indexEq = pair.indexOf("="),
                    key = pair,
                    value = null;
                if (indexEq > 0) {
                    key = pair.substring(0, indexEq);
                    value = pair.substring(indexEq + 1, pair.length);
                }
                params[key] = value;
            }
            return params;
        },
        broadcastWidgetHeight() {
            let _params = this.$getPageParams();
            // 防止高度通知失败
            setTimeout(() => {
                this.getComponentRect(_params)
            }, 200)
            setTimeout(() => {
                this.getComponentRect(_params)
            }, 1200)
        }
    }
}
</script>

<style lang="css" src="../css/common.css"></style>
<style>
.guarantee {
    background: #fff;
}
.a {
    /* width: 750wx; */
    height: 100wx;
    display: flex;
    justify-content: center;
    align-content: center;
    align-items: center;
    background-color: #ccc;
    margin-bottom: 100px;
}
</style>