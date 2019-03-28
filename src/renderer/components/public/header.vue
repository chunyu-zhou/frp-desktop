<template>
    <div class="header" style="-webkit-app-region: drag">
        <div class="logo">
            <img src="data:image/png;base64,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">
            <span class="title">
                Frp桌面客户端
                <span class="version">
                    <a-badge dot>
                        <span class="version">V5.1.11</span>
                    </a-badge>
                </span>
            </span>
        </div>
        <div class="window-menu">
            <a-dropdown :trigger="['click']" placement="bottomCenter">
                <div class="icon-item">
                    <i class="iconfont">&#xe66d;</i>
                </div>
                <a-menu slot="overlay">
                    <a-menu-item>
                        <a href="javascript:;">
                            <a-icon type="setting" /> 设置中心
                        </a>
                    </a-menu-item>
                    <a-menu-item>
                        <a href="javascript:;">
                            <a-icon type="sync" /> 检查更新
                        </a>
                    </a-menu-item>
                    <a-menu-item>
                        <a href="javascript:;">
                            <a-icon type="message" /> 关 于
                        </a>
                    </a-menu-item>
                    <a-menu-item>
                        <a href="javascript:;">
                            <a-icon type="message" /> BUG反馈
                        </a>
                    </a-menu-item>
                    <a-menu-item>
                        <a href="javascript:;">
                            <a-icon type="poweroff" class="logout" /> 退 出
                        </a>
                    </a-menu-item>
                </a-menu>
            </a-dropdown>

            <div class="icon-item">
                <i class="iconfont">&#xe602;</i>
            </div>
            <div class="icon-item">
                <i class="iconfont">&#xe601;</i>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Header"
    }
</script>

<style>
    .window-menu {
        -webkit-app-region: no-drag;
    }
</style>
<style scoped>
    .header {
        width: 100%;
        height: 45px;
        padding:0 10px;
        background-color: #293041;
    }
    .logo, .window-menu {
        display: inline-block;
        vertical-align: middle;
    }
    .logo {
        width: 275px;
    }
    .window-menu {
        width: 100px;
    }
    .logo img{
        width: 24px;
        height: 24px;
        vertical-align: middle;
    }
    .logo .title{
        height: 45px;
        line-height: 45px;
        vertical-align: top;
        font-size: 13px;
        color: #94979F;
    }
    .logo .version{
        margin-left: 5px;
        display: inline-block;
    }
    .window-menu .icon-item {
        color: #94979F;
        width: 30px;
        height: 100%;
        text-align: center;
        display: inline-block;
    }
    .icon-item:hover{
        color: #fff;
        cursor: pointer;
    }
    .icon-item .iconfont {
        font-size: 14px;
    }
</style>
