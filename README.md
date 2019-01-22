title: cytoscape.js学习
author: cacacai
date: 2019-01-21 18:01:43
tags:
---
Cytoscape.js是一个用于分析和可视化的图论库。这涵盖了从网络生物学到社交网络分析的各种用途。
主要是描述点和线之间的关系
官方网站http://js.cytoscape.org/
官方教程http://js.cytoscape.org/#core/initialisation
GitHub托管项目https://github.com/cytoscape/cytoscape.js/
<!--more-->


# 获取cytoscape.js
- cytoscape在github https://github.com/cytoscape/cytoscape.js/ 上下载最新的 release 版本，解压出来的文件夹里的 dist 目录里可以找到最新版本的 echarts 库。
cytoscape.min.js：一个缩小的UMD版本，其中包含所有依赖项。该文件适用于小页面，例如学术论文的补充材料。
cytoscape.umd.js：包含所有依赖项的非缩小UMD构建。此文件对于小页面上的调试很有用，例如学术论文的补充材料。
cytoscape.cjs.js：一个没有任何捆绑依赖关系的非缩小CJS（Node.js）构建。这可以由Node.js或像Webpack这样的捆绑器通过require（'cytoscape'）自动使用。
cytoscape.esm.js：没有任何捆绑依赖项的非缩小ESM（导入/导出）构建。这可以由Node.js或像Webpack这样的捆绑器通过'cytoscape'导入cytoscape自动使用。

- cdn引入 [cdnjs](https://cdnjs.com/libraries/cytoscape)
- 通过 npm 获取 cytoscape，npm install cytoscape

[更多的引入方式](http://js.cytoscape.org/#getting-started/including-cytoscape.js)

# 引入cytoscape
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <!-- 引入 cytoscape 文件 -->
    <script src="cytoscape.min.js"></script>
</head>
</html>
```

>建议使用cytoscape.min.js版本其他版本有点问题

# 绘制一个简单的图形

在绘图前我们需要为 cytoscape 准备一个具备高宽的 DOM 容器。
```
<body>
    <!-- 为 cytoscape 准备一个具备大小（宽高）的 DOM -->
    <div id="cy" ></div>
</body>

```

> 请注意，Cytoscape.js在初始化时使用HTML DOM元素容器的尺寸进行布局和渲染。因此，在任何Cytoscape.js相关代码之前将CSS样式表放在<head>中是非常重要的。否则，可能偶尔会错误地报告尺寸，从而导致不希望的行为。
  
这是官方提示，DOM的样式定义需要在head标签内避免发生问题

然后就可以通过cytoscape 方法创建一个 图像实例，下面是完整代码。
```
<!doctype html>
<html>
<head>
    <title>Tutorial 1: Getting Started</title>
    <script src='cytoscape.min.js'></script>
    <style>
        #cy {
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0px;
            left: 0px;
        }
    </style>
</head>

<body>
    <div id="cy"></div>
    <script>
        var cy = cytoscape({
            container: document.getElementById('cy'), // container to render in

            elements: [ // list of graph elements to start with
                { // node a
                    data: { id: 'a' }
                },
                { // node b
                    data: { id: 'b' }
                },
                { // edge ab
                    data: { id: 'ab', source: 'a', target: 'b' }
                }
            ],
            style: [ // the stylesheet for the graph
                {
                    selector: 'node',
                    style: {
                        'background-color': '#0f0',
                        'label': 'data(id)'
                    }
                },

                {
                    selector: 'edge',
                    style: {
                        'width': 3,
                        'line-color': '#f00',
                        'target-arrow-color': '#00f',
                        'target-arrow-shape': 'triangle'
                    }
                }
            ],
            layout: {
                name: 'grid',
                rows: 1
            }
        });
    </script>
</body>
</html>
```
 
这样第一个图形就ok了
# 参数列表

Cytoscape.js的一个实例有许多可以在初始化时设置的选项。
下面概述了它们的默认值。
> 请注意，一切都是可选的。默认情况下，您将获得带有默认样式表的空图。为方便起见，浏览器外部的环境（例如Node.js）会自动设置为无头。(机器翻译)

```
var cy = cytoscape({
  // very commonly used options
  container: undefined,
  elements: [ /* ... */ ],
  style: [ /* ... */ ],
  layout: { name: 'grid' /* , ... */ },

  // initial viewport state:
  zoom: 1,
  pan: { x: 0, y: 0 },

  // interaction options:
  minZoom: 1e-50,
  maxZoom: 1e50,
  zoomingEnabled: true,
  userZoomingEnabled: true,
  panningEnabled: true,
  userPanningEnabled: true,
  boxSelectionEnabled: false,
  selectionType: 'single',
  touchTapThreshold: 8,
  desktopTapThreshold: 4,
  autolock: false,
  autoungrabify: false,
  autounselectify: false,

  // rendering options:
  headless: false,
  styleEnabled: true,
  hideEdgesOnViewport: false,
  hideLabelsOnViewport: false,
  textureOnViewport: false,
  motionBlur: false,
  motionBlurOpacity: 0.2,
  wheelSensitivity: 1,
  pixelRatio: 'auto'
});
```
## 基本参数
- container：应在其中呈现图形的HTML DOM元素。如果Cytoscape.js无头地运行，则未指定。容器预计是一个空的div;可视化拥有div。
- elements：指定为普通对象的元素数组。为方便起见，也可以将此选项指定为解析为JSON元素的promise。
- style：用于设置图形样式的样式表。为方便起见，也可以将此选项指定为解析为样式表的promise。
- layout：指定布局选项的普通对象。最初运行的布局由name字段指定。有关其支持的选项，请参阅布局文档。如果要在元素JSON中指定自己的节点位置，则可以使用预设布局 - 默认情况下，它不会设置任何位置，使节点处于当前位置（即在初始化时的options.elements中指定）。

## 初始化视图

- zoom：图形的初始缩放级别。确保在布局中禁用视口操作选项（如fit），以便在应用布局时不会覆盖它。您可以设置options.minZoom和options.maxZoom以设置缩放级别的限制。
- pan：图形的初始平移位置。确保在布局中禁用视口操作选项（如fit），以便在应用布局时不会覆盖它。


## 交互参数
- minZoom：图表缩放级别的最小界限。视口的缩放比例不能小于此缩放级别。
- maxZoom：图表缩放级别的最大界限。视口的缩放比例不能大于此缩放级别。
- zoomingEnabled：是否通过用户事件和编程方式启用缩放图形。
- userZoomingEnabled：是否允许用户事件（例如鼠标滚轮，捏合缩放）缩放图形。对此缩放的编程更改不受此选项的影响。
- panningEnabled：是否通过用户事件和编程方式启用平移图形。
- userPanningEnabled：是否允许用户事件（例如拖动图形背景）平移图形。平移的程序化更改不受此选项的影响。
- boxSelectionEnabled：是否启用了框选择（即，拖动框叠加，并将其释放为选择）。如果启用，则用户必须点击以平移图表。
- selectionType：一个字符串，指示用户输入的选择行为。对于“添加”，用户进行的新选择会添加到当前所选元素的集合中。对于“单个”，用户做出的新选择成为当前所选元素的整个集合（即，未选择先前的元素）。
- touchTapThreshold＆desktopTapThreshold：一个非负整数，分别表示用户在点击手势，触摸设备和桌面设备上可以移动的最大允许距离。这使得用户更容易点击。这些值具有合理的默认值，因此建议不要更改这些选项，除非您有充分的理由这样做。大的价值几乎肯定会产生不良后果。
- autoungrabify：默认情况下节点是否应该不被授权（用户无法抓取）（如果为true，则覆盖单个节点状态）。
- autolock：默认情况下是否应锁定节点（根本不可拖动）（如果为true，则覆盖单个节点状态）。
- autounselectify：默认情况下节点是否应该是不可分割的（不可变选择状态）（如果为true，则覆盖单个元素状态）。
## 渲染参数

- headless：一种便利选项，可以将实例初始化为无头运行。您不需要在隐式无头的环境中设置它（例如Node.js）。但是，设置无头是很方便的：如果你想在浏览器中使用无头实例，则为true。
- styleEnabled：一个布尔值，指示是否应使用样式。对于无头（即在浏览器之外）环境，不需要显示，因此也不需要样式化 - 从而加速代码。如果您需要特殊情况，可以在无头环境中手动启用样式。请注意，如果您计划渲染图形，则禁用样式没有意义。另请注意，必须调用cy.destroy（）来清理启用样式的无头实例。
- hideEdgesOnViewport：一个渲染提示，当设置为true时，渲染器在操作视口时不会渲染边。这使得平移，缩放，拖动等对大图更具响应性。由于性能增强，此选项现在基本上没有实际意义。
- textureOnViewport：一种渲染提示，当设置为true时，渲染器在平移和缩放过程中使用纹理而不是绘制元素，从而使大图更具响应性。由于性能增强，此选项现在基本上没有实际意义。
- motionBlur：渲染提示，当设置为true时，渲染器使用运动模糊效果使帧之间的过渡看起来更平滑。这可以增加大图的感知性能。由于性能增强，此选项现在基本上没有实际意义。
- motionBlurOpacity：当motionBlur：true时，此值控制运动模糊帧的不透明度。值越高，运动模糊效果越明显。由于性能增强，此选项现在基本上没有实际意义。
- wheelSensitivity：缩放时更改滚轮灵敏度。这是一个乘法修饰符。因此，0到1之间的值会降低灵敏度（变焦较慢），而大于1的值会增加灵敏度（变焦更快）。此选项设置为合理的值，适用于Linux，Mac和Windows上的主流鼠标（Apple，Logitech，Microsoft）。如果您的特定系统上的默认值似乎太快或太慢，您可能在操作系统或小众鼠标中有非默认的鼠标设置。除非您的应用程序仅适用于特定硬件，否则不应更改此值。否则，您可能会对大多数用户进行缩放太慢或太快的风险。
- pixelRatio：使用手动设置的值覆盖屏幕像素比（建议使用1.0，如果已设置）。这可以通过减少需要渲染的有效区域来提高高密度显示器的性能，尽管在最近的浏览器版本中这是不太必要的。如果要使用硬件的实际像素比，可以设置pixelRatio：'auto'（默认值）。
# 更多
详细的[API](http://js.cytoscape.org/#core/graph-manipulation) http://js.cytoscape.org/#core/graph-manipulation