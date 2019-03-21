<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
 *{margin: 0px;padding: 0px;}
 body{
     background-image: url(./assets/bg.jpg);
    background-size:100% 100%; 
 }
 *{
-webkit-tap-highlight-color: rgba(0,0,0,0);
-moz-tap-highlight-color: rgba(0,0,0,0);
-o-tap-highlight-color: rgba(0,0,0,0);
-ms-tap-highlight-color: rgba(0,0,0,0);
tap-highlight-color: rgba(0,0,0,0);
}
 *{ -webkit-tap-highlight-color: rgba(0,0,0,0);-webkit-tap-highlight-color: transparent;}
.el-message-box{width: 350px}
</style>
