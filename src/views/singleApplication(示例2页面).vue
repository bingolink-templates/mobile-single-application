<template>
    <div ref="wrap" class="main">
        <!-- 我的应用 -->
        <div class="my-apply">
            <bui-header title="示例2页面" :leftItem="{
                icon: 'ion-chevron-left',
            }" @leftClick="back">
            </bui-header>
            <div>
                我是示例2页面
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
    },
    mounted() {
    },
    methods: {
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