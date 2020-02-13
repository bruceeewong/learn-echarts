# ECharts 学习

> [ECharts 官网](https://www.echartsjs.com/zh/tutorial.html#5%20%E5%88%86%E9%92%9F%E4%B8%8A%E6%89%8B%20ECharts)

## 基础概念

### 实例

一个网页中可以创建多个 echarts 实例。每个 echarts 实例 中可以创建多个图表和坐标系等等（用 option 来描述）。准备一个 DOM 节点（作为 echarts 的渲染容器），就可以在上面创建一个 echarts 实例。每个 echarts 实例独占一个 DOM 节点。

### 系列 series

在 echarts 里，系列（series）是指：一组数值以及他们映射成的图。“系列”这个词原本可能来源于“一系列的数据”，而在 echarts 中取其扩展的概念，不仅表示数据，也表示数据映射成为的图。所以，一个 系列 包含的要素至少有：一组数值、图表类型（series.type）、以及其他的关于这些数据如何映射成图的参数。

### 组件

xAxis（直角坐标系 X 轴）、yAxis（直角坐标系 Y 轴）、grid（直角坐标系底板）、angleAxis（极坐标系角度轴）、radiusAxis（极坐标系半径轴）、polar（极坐标系底板）、geo（地理坐标系）、dataZoom（数据区缩放组件）、visualMap（视觉映射组件）、tooltip（提示框组件）、toolbox（工具栏组件）、series（系列）

### 坐标系

一个坐标系，可能由多个组件协作而成。

直角坐标系:

- xAxis - 直角坐标系 X 轴
- yAxis - 直角坐标系 Y 轴
- grid - 直角坐标系底板

只声明了 xAxis、yAxis 和一个 scatter（散点图系列），echarts 暗自为他们创建了 grid 并关联起他们：

```js
var option = {
  xAxis: {},
  yAxis: {},
  series: {
    type: "scatter",
    data: [
      [13, 44],
      [51, 51],
      [67, 19],
      [19, 33]
    ]
  }
};
```

## 异步加载

获取到数据后再 setOption 对应的属性即可.

ECharts 中在更新数据的时候需要通过 name 属性对应到相应的系列

### loading 动画

自带 API `chart.showLoading()`, `chart.hideLoading()`

### 动态更新

只需要定时获取数据，setOption 填入数据，而不用考虑数据到底产生了那些变化，ECharts 会找到两组数据之间的差异然后通过合适的动画去表现数据的变化。

## 使用 dataset 管理数据

ECharts 4 以前，数据只能声明在各个“系列（series）”中

ECharts 4 开始支持了 dataset 组件用于单独的数据集声明，从而数据可以单独管理，被多个组件复用，并且可以基于数据指定数据到视觉的映射。

```js
option = {
  legend: {},
  tooltip: {},
  dataset: {
    // 提供一份数据。
    source: [
      ["product", "2015", "2016", "2017"],
      ["Matcha Latte", 43.3, 85.8, 93.7],
      ["Milk Tea", 83.1, 73.4, 55.1],
      ["Cheese Cocoa", 86.4, 65.2, 82.5],
      ["Walnut Brownie", 72.4, 53.9, 39.1]
    ]
  },
  // 声明一个 X 轴，类目轴（category）。默认情况下，类目轴对应到 dataset 第一列。
  xAxis: { type: "category" },
  // 声明一个 Y 轴，数值轴。
  yAxis: {},
  // 声明多个 bar 系列，默认情况下，每个系列会自动对应到 dataset 的每一列。
  series: [{ type: "bar" }, { type: "bar" }, { type: "bar" }]
};
```

也可通过 dimensions 指定维度顺序

```js
dataset: {
  // 这里指定了维度名的顺序，从而可以利用默认的维度到坐标轴的映射。
  // 如果不指定 dimensions，也可以通过指定 series.encode 完成映射，参见后文。
  dimensions: ['product', '2015', '2016', '2017'],
  source: [
    {product: 'Matcha Latte', '2015': 43.3, '2016': 85.8, '2017': 93.7},
    {product: 'Milk Tea', '2015': 83.1, '2016': 73.4, '2017': 55.1},
    {product: 'Cheese Cocoa', '2015': 86.4, '2016': 65.2, '2017': 82.5},
    {product: 'Walnut Brownie', '2015': 72.4, '2016': 53.9, '2017': 39.1}
  ]
},
```

按行还是按列做映射

用户可以使用 seriesLayoutBy 配置项，改变图表对于行列的理解。seriesLayoutBy 可取值：

'column': 默认值。系列被安放到 dataset 的列上面。
'row': 系列被安放到 dataset 的行上面。

## 交互组件

- 图例组件 legend
- 标题组件 title
- 视觉映射组件 visualMap
- 数据区域缩放组件 dataZoom
- 时间线组件 timeline

### dataZoom

dataZoom 组件是对 数轴（axis） 进行『数据窗口缩放』『数据窗口平移』操作。

```
dataZoom: [
  {   // 这个dataZoom组件，默认控制x轴。
    type: 'slider', // 这个 dataZoom 组件是 slider 型 dataZoom 组件
    start: 10,      // 左边在 10% 的位置。
    end: 60         // 右边在 60% 的位置。
  }
],
```

## ECharts 中的事件和行为

在 ECharts 中事件分为两种类型，一种是用户鼠标操作点击，或者 hover 图表的图形时触发的事件，还有一种是用户在使用可以交互的组件后触发的行为事件，例如在切换图例开关时触发的

### 鼠标事件

ECharts 支持常规的鼠标事件类型，包括 'click'、'dblclick'、'mousedown'、'mousemove'、'mouseup'、'mouseover'、'mouseout'、'globalout'、'contextmenu' 事件。

```js
// 处理点击事件并且跳转到相应的百度搜索页面
myChart.on('click', function (params) {
    window.open('https://www.baidu.com/s?wd=' + encodeURIComponent(params.name));
}
```
