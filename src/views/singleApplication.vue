<template>
    <div ref="wrap" class="main">
        <!-- 我的应用 -->
        <div class="my-apply">
            <bui-header title="首页">
            </bui-header>
            <div>
                <div class="list-item flex-ww flex-dr" v-if='applyList.length!=0'>
                    <div class="flex-ac item-content" v-for="(item, index) in applyList" :key='index' @click='myAllpyEvent(item)'>
                        <bui-image placeholder='/image/ic_service_default.png' src="/image/ic_service_default.png" width="30wx" height="30wx" @click='myAllpyEvent(item)'>
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
            appItems: []
        }
    },
    created() {
        globalEvent.addEventListener("androidback", function (e) {
            navigator.close()
        });
        this.applyList.push({
            name: '示例weex应用',
            code: 'signins'
        })
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
    },
    methods: {
        myAllpyEvent(item) {
            this.appItems.push({
                code: item.code,
                url: 'app.js?code=signins'
            })
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