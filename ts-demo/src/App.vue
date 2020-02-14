<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="less">
#app {
  width: 300px;
  margin: 0 auto;
}
#nav{
  width: 200px;
  height: 30px;
  font-size: 20px;
  font-weight: 600
}
</style>
