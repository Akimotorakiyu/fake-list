<template>
  <div id="app">
    <h1>长列表滚动优化</h1>
    <FakeList/>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import FakeList from "./components/FakeList.vue";

export default Vue.extend({
  name: "app",
  components: {
    FakeList
  },
  data() {
    return {
      data: Array.from(Array(100)).map((ele, index) => {
        return { id: index, name: index };
      })
    };
  }
});
</script>
<style lang="stylus">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
