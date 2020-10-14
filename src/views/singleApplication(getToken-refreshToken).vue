<template>
    <div ref="wrap">
        <bui-header title="getToken-refreshToken" :leftItem="leftItem" @leftClick="back">
        </bui-header>
        <div class='guarantee'>
            <bui-image src="/image/c.png" width="750px" :height="height" @click='aa'></bui-image>
            <bui-image src="/image/c.png" width="750px" :height="height" @click='bb'></bui-image>
            <bui-image src="/image/c.png" width="750px" :height="height" @click='cc'></bui-image>
        </div>
    </div>
</template>

<script>
const dom = weex.requireModule("dom");
const link = weex.requireModule("LinkModule");
const linkapi = require("linkapi");
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
        }
    },
    created() {
        this.height = this.$isIPad ? '159wx' : '317px'
    },
    mounted() {
        this.broadcastWidgetHeight()
        this.urlParams = this.resolveUrlParams(weex.config.bundleUrl)
    },
    methods: {
        back: function () {
            // 返回上一个页面
            this.$pop();
        },
        aa() {
            var that = this
            link.getToken(
                [{ "grantType": "token_tenant_credentials", "eCode": that.urlParams.eCode }],
                res => {
                    linkapi.get({
                        url: "http://link-dev.bingosoft.net/appapi/v2/escategory/with/escount?terminalType=1",
                        method: "GET",
                        headers: {
                            Authorization: 'Bearer ' + res.accessToken
                        },
                        data: {},
                        timeout: 5000,
                        success: function (result) {
                            that.$alert(result);
                        },
                        fail: function (error) {
                            that.$alert(error);
                        }
                    });
                },
                err => {
                    that.$alert(err);
                }
            );
        },
        bb() {
            var that = this
            link.refreshToken([{ "grantType": "token_tenant_credentials", "eCode": that.urlParams.eCode }], function (res) {
                linkapi.get({
                    url: "http://link-dev.bingosoft.net/appapi/v2/escategory/with/escount?terminalType=1",
                    method: "GET",
                    headers: {
                        Authorization: 'Bearer ' + res.accessToken
                    },
                    data: {},
                    timeout: 5000,
                    success: function (result) {
                        that.$alert(result);
                    },
                    fail: function (error) {
                        that.$alert(error);
                    }
                });
            }, (err) => {
                that.$alert(err);
            })
        },
        cc() {
            this.$alert(this.urlParams);
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
</style>