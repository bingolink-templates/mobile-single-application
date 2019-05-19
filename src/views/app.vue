<template>
    <div style="flex:1">
        <singleApplication></singleApplication>
    </div>
</template>

<!--引入bui-weex样式文件-->
<style lang="scss" src="bui-weex/src/css/buiweex.scss"></style>

<script>
    import singleApplication from "./singleApplication.vue";
    module.exports = {
        data () {
            return {}
        },
        components: {
          singleApplication
        }
    }
</script>