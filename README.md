# fake-list

长列表滚动优化


## 思路

不缓存，直接计算出显示区域的item。

直接根据数据量和scrollTop、clientHeight计算出需要显示的item，无缓存。

### 白屏问题

移动端快速滚动出现div半屏白屏问题。
过快的时候，少量的item缓存并不够填补白屏部分。

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```