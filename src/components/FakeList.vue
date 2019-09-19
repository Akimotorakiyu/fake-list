<style lang="stylus" scoped>
.container {
  overflow-y: scroll;
}
</style>

<template>
  <div class="container" @scroll="onScroll" style="height:480px;" ref="container">
    <div :style="{height:renderInfo.topDivHeight+'px'}"></div>
    <div v-for="item in renderInfo.data" :key="item[idkey]" style="height:48px;">{{item.name}}</div>
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
      default: []
    },
    idkey: {
      type: String,
      default: "id"
    },
    show: {
      type: Number,
      default: 8
    },
    cache: {
      type: Number,
      default: 8
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
      const scrollHeightShould = this.data.length * this.itemHtight;
      const bottomPart =
        this.container.scrollHeight -
        this.container.scrollTop -
        this.container.clientHeight;

      const topUnVisiableCount =
        (this.container.scrollTop -
          (this.container.scrollTop % this.itemHtight)) /
        this.itemHtight;

      const bottomUnVisiableCount =
        (bottomPart - (bottomPart % this.itemHtight)) / this.itemHtight;

      const showCount =
        this.data.length - topUnVisiableCount - bottomUnVisiableCount;

      const showData = this.data.slice(topUnVisiableCount, showCount);

      const topDivHeight = topUnVisiableCount * this.itemHtight;
      const bottomDivHeight = bottomUnVisiableCount * this.itemHtight;

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
  },
  mounted() {
    // (this.$refs.container as any).scroll(0, 10);
  }
});
</script>