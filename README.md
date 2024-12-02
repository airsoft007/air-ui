# air-ui  冻结表格组件 for Vue3

本例程演示如何采用单table标签实现表格顶部标题行、左侧列、右侧列的冻结。因为element ui/element plus的
表格组件使用了多table实现冻结表格，拖动滚动条时需要控制多table的联动，当数据量略大时严重卡顿，性能极差。
采用单table实现的表格，由原生div容器产生滚动条，不存在多表联动问题，性能高。

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
