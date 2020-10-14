<template>
    <div ref="wrap" class="main">
        <!-- 我的应用 -->
        <div class="my-apply">
            <bui-header title="示例1页面" :leftItem="{
                icon: 'ion-chevron-left',
            }" @leftClick="back">
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
            urlParams: {}
        }
    },
    created() {
        globalEvent.addEventListener("androidback", function (e) {
            navigator.close()
        });
        this.urlParams = this.resolveUrlParams(weex.config.bundleUrl)
        this.applyList.push({
            name: '示例weex应用2',
            code: 'signinsTwo'
        }, {
            name: '示例bt应用',
            code: 'signinsThree'
        })
    },
    mounted() {
    },
    methods: {
        myAllpyEvent(item) {
            linkapi.sendBroadcast('weexAppEvent', {
                code: item.code,
                url: item.code == 'signinsThree' ? 'index.html?code=' + item.code : 'app.js?code=' + item.code
            }, (success) => {
            }, (err) => {
            })
        },
        back: function () {
            linkapi.sendBroadcast('weexBackEvent', { code: this.urlParams.code }, (success) => {
            }, (err) => {
            })
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