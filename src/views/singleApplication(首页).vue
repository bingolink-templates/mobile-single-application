<template>
    <div ref="wrap" class="main">
        <!-- 我的应用 -->
        <div class="my-apply">
            <bui-header title="首页">
            </bui-header>
            <div v-if="isShow">
                <div class="list-item flex-ww flex-dr" v-if='applyList.length!=0'>
                    <div class="flex-ac item-content" v-for="(item, index) in applyList" :key='index' @click='myAllpyEvent(item)'>
                        <bui-image v-if='item.image' placeholder='/image/ic_service_default.png' :src="item.image" width="30wx" height="30wx" @click='myAllpyEvent(item)'>
                        </bui-image>
                        <text class="c0 f24 h37 mt15 lines1" v-if='item.name'>{{item.name}}</text>
                    </div>
                </div>
            </div>
        </div>
        <LinkApp v-for="(app, i) in appItems" :key="i" ref='app.js?isTabApp=1' v-if="app.code" :code='app.code' :url='app.url' class="linkapp"></LinkApp>
    </div>
</template>

<script>
const dom = weex.requireModule('dom');
const link = weex.requireModule("LinkModule");
const linkapi = require('linkapi');
const storage = weex.requireModule('storage');
const globalEvent = weex.requireModule('globalEvent');
const navigator = weex.requireModule('navigator');
export default {
    data() {
        return {
            applyList: [],
            isShow: false,
            isError: true,
            number: 0,
            timeFlag: false,
            code: '',
            appItems: []
        }
    },
    created() {
        globalEvent.addEventListener("androidback", function (e) {
            navigator.close()
        });
        if (WXEnvironment && (WXEnvironment.deviceModel.indexOf('iPad') === 0)) {
            link.getSystemSize(
                [],
                res => {
                    this.number = Math.floor((res.LeftScreenSize.Width) / 90)
                }
            );
        } else {
            this.number = Math.floor((750 / 2) / 90)
        }
        linkapi.registerReceiverEvent('weexBackEvent', (res) => {
            if (res.code) {
                for (let index = 0; index < this.appItems.length; index++) {
                    if (this.appItems[index].code == res.code) {
                        this.appItems.splice(index, 1)
                    }
                }
            } else {
                navigator.close()
            }
        }, (success) => {
        }, (err) => {
        })
        linkapi.registerReceiverEvent('weexAppEvent', (res) => {
            this.appItems.push({
                code: res.code,
                url: res.url
            })
        }, (success) => {
        }, (err) => {
        })
    },
    mounted() {
        var that = this
        this.getStorage(function () {
            that.getApply()
        })
    },
    methods: {
        // 我的应用
        myAllpyEvent(item, more) {
            this.appItems.push({
                code: 'signins',
                url: 'app.js?code=signins'
            })
        },
        getApply() {
            link.getServerConfigs([], (params) => {
                var type;
                if (this.$isIPad()) {
                    type = 6
                } else if (this.$isAndroid()) {
                    type = 1
                } else if (this.$isIPhone()) {
                    type = 2
                }
                linkapi.get({
                    url: params.appApiUri + '/es/favorited',
                    data: {
                        terminalType: type
                    }
                }).then((res) => {
                    this.isError = true
                    this.isShow = true
                    if (res.code == 200) {
                        let applyArr = []
                        this.applyList = []
                        for (let index = 0; index < res.data.length; index++) {
                            const element = res.data[index];
                            let applyObj = {}
                            let image = element.icon ? element.icon.split('||')[0] : '/image/ic_service_default.png'
                            applyObj['name'] = element.name
                            applyObj['actionParams'] = element.actionParams
                            applyObj['image'] = image != '' ? params.uamUri + '/ui/upload?action=download&filepath=' +
                                image : ''
                            applyObj['id'] = element.id
                            applyArr.push(applyObj)
                        }
                        // 为了图标均匀布局
                        var remainder = applyArr.length % this.number
                        if (applyArr.length != 0 && remainder != 0) {
                            var remainderIn = this.number - remainder
                            for (let j = 0; j < remainderIn; j++) {
                                applyArr.push({})
                            }
                        }
                        this.applyList = applyArr
                        storage.setItem('applyLocalData', JSON.stringify(applyArr))
                    }
                }, (err) => {
                    this.error()
                })
            }, (err) => {
                this.error()
            });
        },
        // 数据缓存
        getStorage(callback) {
            var that = this
            storage.getItem('applyLocalData', res => {
                if (res.result == 'success') {
                    var data = JSON.parse(res.data)
                    that.isShow = true
                    that.applyList = data
                } else {
                    callback()
                }
            })
        },
        error() {
            this.isError = false
            this.isShow = true
        }
    }
}
</script>

<style lang="css" src="../css/common.css"></style>
<style>
.linkapp {
    position: fixed;
    left: 0px;
    right: 0px;
    top: 0px;
    bottom: 0px;
}
.my-apply {
    background-color: #fff;
}
.apply-title {
    height: 44wx;
    padding: 0 12wx;
    border-bottom: 1px solid #f2f2f2;
}
.title {
    justify-content: flex-start;
    flex-direction: row;
    align-content: center;
    align-items: center;
}

.line {
    width: 5px;
    height: 36px;
    margin-right: 12px;
}
.list-item {
    justify-content: space-between;
    padding-bottom: 15wx;
}
.item-content {
    width: 90wx;
    margin-top: 15wx;
}
.no-content {
    height: 83wx;
}
.center-height {
    line-height: 20wx;
}
</style>