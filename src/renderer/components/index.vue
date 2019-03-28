<template>
    <div class="index">
        <div class="main">
            <div class="top">
                <a-tooltip placement="bottom">
                    <template slot='title'>
                        设置规则
                    </template>
                    <div class="setting">
                        <a-icon type="setting" />
                    </div>
                </a-tooltip>
                <div class="title">没有运行</div>
            </div>

            <div class="or">
                <div class="circular1">
                    <div class="circular2">
                        <!--<img src="../assets/image/app.png">-->
                        <span>正在加载配置</span>
                    </div>
                    <a-tooltip>
                        <template slot='title'>
                            运行日志
                        </template>
                        <div class="circular3">
                            <a-icon type="question" />
                        </div>
                    </a-tooltip>
                </div>
            </div>
            <div class="btns">
                <div class="btn" :class="btn_class" @click="setStatus">{{btn_title}}</div>
            </div>
        </div>

        <!--<div class="add-page">
            <div class="top">
                <a-tooltip placement="bottom">
                    <template slot='title'>
                        自定义 frpc.ini
                    </template>
                    <div class="setting">
                        <a-icon type="highlight" />
                    </div>
                </a-tooltip>
                <div class="title">没有运行</div>
            </div>

            <div class="frpc-list">
                <div class="frpc-title">
                    当前规则 （3）
                </div>
                <div class="list">
                    <ul>
                        <li>1</li>
                        <li>2</li>
                        <li>3</li>
                        <li>4</li>
                        <li>5</li>
                        <li>5</li>
                    </ul>
                </div>
            </div>
        </div>-->
    </div>
</template>

<script>
    import path from "path";
    import { execFile } from "child_process";
    import fs from "fs";
    const ini = require("ini");
    const fixPath = require("fix-path");
    export default {
        name: "index",
        data() {
            return {
                iniLength: 0,
                frpid: "10188",
                iniObject: "",
                btn_class: "loding",
                btn_title: "正在加载配置",
                frpObject: null,
            }
        },
        created() {
            let frpPath = path.join(__dirname, "../../../frp/");
            var config = ini.parse(fs.readFileSync(`${frpPath}frpc.ini`, "utf-8"));
            this.iniObject = config;
            this.iniLength = Object.keys(this.iniObject).length - 1;
            this.init();
        },
        mounted() {
            fixPath();
        },
        methods: {
            setStatus() {
                console.log(this.frpid);
                if(this.frpid != '') {

                    try {
                        process.kill(this.frpid);
                    } catch (e) {
                        console.log('pid not found');
                    }

                    console.log(this.frpObject);


                    return false;
                    try {

                        this.btn_class = 'ready';
                        this.btn_title= '启动穿透';
                        console.log("停止成功");
                    } catch (e) {
                        console.log("停止出错");
                    }
                    console.log('frpid111: '+ this.frpid);
                } else {
                    let frpPath = path.join(__dirname, "../../../frp/");
                    let shell = execFile(
                        "nohup",
                        [`${frpPath}frpc-2.4.1.exe`, "-c", `${frpPath}frpc.ini`],
                        { env: { PATH: process.env.PATH } }
                    );
                    this.frpid = shell.pid;
                    console.log(shell);
                    console.log('frpid: '+ this.frpid);
                    this.frpObject = shell;

                    shell.stdout.on('data', (data) => {
                        console.log(`stdout: ${data}`);
                    });

                    shell.stderr.on('data', (data) => {
                        console.log(`stderr: ${data}`);
                    });

                    this.btn_class = 'run';
                    this.btn_title= '停止穿透';
                    shell.on('close', (code, signal) => {
                        console.log(`子进程收到信号 ${signal} 而终止`);
                    });


                    this.$electron.ipcRenderer.send("frpid", this.frpid);
                    this.$electron.ipcRenderer.send("restart",this.frpid);

                    // this.$electron.ipcRenderer.send("frpid", this.frpid);
                }
            },
            init() {
                if (this.iniLength) {
                    /*let frpPath = path.join(__dirname, "../../../frp/");
                    let shell = execFile(
                        "nohup",
                        [`${frpPath}frpc`, "-c", `${frpPath}frpc.ini`],
                        { env: { PATH: process.env.PATH } }
                    );
                    this.frpid = shell.pid;
                    console.log(shell);
                    console.log('frpid'+ this.frpid);
                    shell.on("exit", code => {
                        console.log(`子进程退出码：${code}`);
                    });

                    this.$electron.ipcRenderer.send("frpid", this.frpid);*/
                    this.btn_class = 'ready';
                    this.btn_title= '启动穿透';
                }
            },
            run() {
                let frpPath = path.join(__dirname, "../../../frp/");
                let shell = execFile(
                    "nohup",
                    [`${frpPath}frpc`, "-c", `${frpPath}frpc.ini`],
                    { env: { PATH: process.env.PATH } }
                );
                this.frpid = shell.pid;
                console.log(shell);
                console.log('frpid'+ this.frpid);
                shell.on("exit", code => {
                    console.log(`子进程退出码：${code}`);
                });

                this.$electron.ipcRenderer.send("frpid", this.frpid);
            }
        }
    }
</script>

<style scoped>
    .index {
        padding: 20px 30px;
    }
    .top .setting, .top .title {
        display: inline-block;
        vertical-align: middle;
    }
    .top {
        height: 35px;
        line-height: 35px;
        color: #fff;
        margin-bottom: 30px;
    }
    .top .setting{
        font-size: 20px;
        cursor: pointer;
    }
    .top .title {
        width: calc(100% - 60px);
        text-align: center;
    }
    .or .circular1{
        border-radius: 100%;
        border: 3px solid #293041;
        width: 265px;
        height: 265px;
        text-align: center;
        margin: 0 auto;
        position:relative;
    }
    .circular1 .circular2 {
        border-radius: 100%;
        position: absolute;
        border: 15px solid #293041;
        width: 180px;
        height: 180px;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
    }
    .circular2 img {
        position: absolute;
        width: 50px;
        height: 50px;
        text-align: center;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
    }
    .circular2 span {
        width: 100%;
        text-align: center;
        color: #fff;
        line-height: 140px;
    }
    .circular1 .circular3 {
        position: absolute;
        border-radius: 100%;
        border: 2px solid #293041;
        width: 42px;
        height: 42px;
        right: 21px;
        top: 0;
        background-color: #212735;
        cursor: pointer;
        color: #94979F;
        font-size: 20px;
    }
    .circular3 i{
        vertical-align: middle;
    }
    .circular3:hover {
        border: 2px solid #CE3E5D;
        color: #CE3E5D;
        box-shadow: 0 5px 25px 0 rgba(206,62,93,.6);
    }
    .btns {
        width: 100%;
        text-align: center;
        margin: 0 auto;
        margin-top: 60px;
    }
    .btns .btn {
        margin: 0 auto;
        width: 180px;
        height: 46px;
        border-radius: 100px;
        font-size: 18px;
        line-height: 46px;
        color: #fff;
        background: #31394C;
        cursor: default;
    }
    .btn.ready {
        cursor: pointer;
    }
    .btn.run,.btn.ready {
        cursor: pointer;
    }
    .btn.ready {
        background: linear-gradient(90deg,#51e098 0,#00b374 100%);
        box-shadow: 0 5px 25px 0 rgba(0,179,116,.4);
    }
    .btn.run {
        background: linear-gradient(90deg,#F14466 0,#B53A57 100%);
        box-shadow: 0 5px 25px 0 rgba(241,68,102,.4);
    }
    .btns .btn.ready:hover {
        background: #51e098;
    }
    .btns .btn.run:hover {
        background: #F14466;
    }

    .frpc-list {
        width: 100%;
        color:#fff;
    }
    .frpc-title {
        padding-left: 15px;
        border-left: 3px solid #94979F;
    }
    .list {
        width: 100%;
        height: 410px;
        margin-top: 15px;
    }
    .list li{
        list-style: none;
        height: 50px;
        line-height: 50px;
    }
    .list li:nth-child(even){ /*偶数行样式*/
        background-color: #293041;
        border-bottom: 1px solid #293041;
    }
    .list li:nth-child(odd){ /*奇数行样式*/
        background-color: #31394C;
        border-bottom: 1px solid #31394C;
    }
    .list li:hover {
        background-color: #1C222F;
        border-bottom: 1px solid #00b374;
    }
</style>
