<template>
  <div id="app">
    <c-header></c-header>
    <tab></tab>
    <keep-alive>
      <router-view></router-view>
    </keep-alive> 
  </div>
</template>

<script>
import CHeader from './components/header/c-header'
import Tab from './components/tab/tab'
export default {
  name: 'app',
  components: {
    CHeader,
    Tab
  }
}
</script>

<style>

</style>
