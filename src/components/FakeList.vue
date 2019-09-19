<style lang="stylus" scoped>
.container {
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch;
}
</style>

<template>
  <div class="container" @scroll="onScroll" style="height:480px;" ref="container">
    <div :style="{height:renderInfo.topDivHeight+'px'}"></div>
    <div
      v-for="item in renderInfo.data"
      :key="item[idkey]"
      :style="{height:itemHtight+'px'}"
    >{{item.name}}</div>
    <div :style="{height:renderInfo.bottomDivHeight+'px'}"></div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "FakeList",
  props: {
    itemHtight: {
      type: Number,
      default: 48
    },
    data: {
      type: Array,
      default: () =>
        Array.from(Array(100)).map((ele, index) => {
          return { id: index, name: index };
        })
    },
    idkey: {
      type: String,
      default: "id"
    }
  },
  data() {
    return {
      container: {
        scrollHeight: 4800, // 垂直可滚动高度
        clientHeight: 480, // 视口区域高度
        scrollTopMax: 4320, // 垂直实际最大滚动高度,scrollHeight-clientHeight
        scrollTop: 0 // 垂直滚动距离
      }
    };
  },
  computed: {
    renderInfo() {
      // 滚动区域理论高度
      const scrollHeightShould = this.data.length * this.itemHtight;
      // 滚动区域距顶部高度
      // this.container.scrollTop
      // 滚动区域距离底部高度
      const bottomPart =
        scrollHeightShould -
        this.container.scrollTop -
        this.container.clientHeight;
      // 顶部不可见元素数量
      const topUnVisiableCount =
        (this.container.scrollTop -
          (this.container.scrollTop % this.itemHtight)) /
        this.itemHtight;
      // 底部不可见元素数量
      const bottomUnVisiableCount =
        (bottomPart - (bottomPart % this.itemHtight)) / this.itemHtight;
      // 显示的元素数量
      const showCount =
        this.data.length - topUnVisiableCount - bottomUnVisiableCount;
      // 显示的元素
      const showData = this.data.slice(
        topUnVisiableCount,
        topUnVisiableCount + showCount
      );
      // 顶部与底部用来占位的div的高度
      const topDivHeight = topUnVisiableCount * this.itemHtight;
      const bottomDivHeight = bottomUnVisiableCount * this.itemHtight;
      // 渲染信息
      const info = {
        data: showData,
        showCount: 0,
        topUnVisiableCount: 0,
        topDivHeight: 0,
        bottomDivHeight: 0
      };
      info.topUnVisiableCount = topUnVisiableCount;
      info.showCount = showCount;
      info.topDivHeight = topDivHeight;
      info.bottomDivHeight = bottomDivHeight;

      return info;
    }
  },
  methods: {
    onScroll(event: Event) {
      const {
        scrollTop,
        scrollTopMax,
        clientHeight,
        scrollHeight
      } = event.srcElement as any;
      this.container = { scrollTop, scrollTopMax, clientHeight, scrollHeight };
    }
  }
});
</script>