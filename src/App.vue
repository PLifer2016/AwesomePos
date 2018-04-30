<template>
  <div id="app">
    <left-nav></left-nav>
    <div class="main">
      <pos></pos>
    </div>
    <!--<router-view></router-view>-->
  </div>
</template>

<script>
  import leftNav from '@/components/common/leftNav'
  import Pos from '@/components/page/Pos'
export default {
  name: 'App',
  components: {
    leftNav,
    Pos
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

  .main {
    float: left;
    width: 95%;
    background-color: #8af7f7;
    height: 100%;
    overflow: hidden;
  }
</style>
