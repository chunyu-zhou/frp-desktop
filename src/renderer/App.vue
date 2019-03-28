<template>
  <div id="app">
    <Header></Header>
    <router-view></router-view>
  </div>
</template>

<script>
  import Header from './components/public/header'
  export default {
    components: {
      Header
    },
    name: 'frp-desktop'
  }
</script>

<style>
  /* CSS */
  * {
    padding: 0;
    margin: 0;
  }
  body {
    font-family: Chinese Quote,-apple-system,BlinkMacSystemFont,Segoe UI,PingFang SC,Hiragino Sans GB,Microsoft YaHei,Helvetica Neue,Helvetica,Arial,sans-serif,Apple Color Emoji,Segoe UI Emoji,Segoe UI Symbol;
    line-height: 1.5;
    -webkit-app-region: no-drag;
    -webkit-user-select: none;
    background-color: #212735;
  }
  #app {
    -moz-box-shadow:0px 1px 6px #E7EAED;
    -webkit-box-shadow:0px 1px 6px #E7EAED;
    box-shadow:0px 1px 6px #E7EAED;

    background-color: #212735;
    overflow: hidden;
    height: 560px;
    width: 400px;
  }
</style>
