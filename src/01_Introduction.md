# 1 简介 <span id = "简介"></span>
OPENVG 是一个二维矢量图形的API，支持硬件加速，由 Khronos 开发。它为复杂的2D图形应用程序提供了硬件无关的独立接口，以允许设备制造商在适当的地方提供硬件加速。

本文档定义了基于`C语言`的接口，其他语言的接口也会在以后提供。

[术语解释]：
  - **实现**: 指基于硬件或软件的OPENVG实现。 
  - **应用程序**：特指使用OPENVG的应用程序。
  
## 1.1 特性集 <span id = "特性集"></span>

`OPENVG`提供了一个绘图模型，该模型与现有的2D绘图API及格式类似，比如`Adobe PostScript`, `PDF`, `Adobe Flash`, `Sun Microsystems Java2D`, `W3C SVG `等。版本v1.1支持`SVG Tiny 1.2 渲染器`和`Adobe Flash Lite渲染器`的所有绘图功能，并为未来可能的`SVG基础渲染器`实现提供了函数支持。

## 1.2 目标应用 <span id = "目标应用"></span>

在设计`OPENVG`的API时，考虑了以下几类的目标应用：
- **SV与Adobe Flash查看器**
  ddfdf
- **便携式地图应用**
  xxx
- **游戏**
  xxx
- **灵活的用户接口**
  xx
- **底层图形设备接口**
  xx

## 1.3 目标设备 <span id = "目标设备"></span>

## 1.4 设计哲学 <span id = "设计哲学"></span>

## 1.5 命名及排版约定 <span id = "命名及排版约定"></span>

## 1.6 库命名 <span id = "库命名"></span>
