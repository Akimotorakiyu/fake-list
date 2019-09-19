# fake-list

长列表滚动优化

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

## 思路

不缓存，直接计算出显示区域的item。

直接根据数据量和scrollTop、clientHeight计算出需要显示的item，无缓存
