# 3 函数、变量及数据类型 <span id = "函数、变量及数据类型"></span>
`OpenVG`的类型定义及函数原型均可以在头文件`openvg.h`中找到，该头文件位于特定于平台头文件位置的`VG`子目录中。`OpenVG`使用8/16/32位数据类型，未使用64位数据类型。如果提供了`khronos_types.h`头文件，则基本数据类型将在同一平台上的所有`Khronos API`之间兼容。

## 3.1 版本 <span id = "版本"></span>
`openvg.h`头文件中定义了指示版本的常量，未来版本将继续为向后兼容的所有以前版本定义常量。

**`OPENVG_VERSION_1_1`**
对于当前版本，常量`OPENVG_VERSION_1_1`已经定义， 旧版本`OPENVG_VERSION_1_0`继续为向后兼容性而定义。在程序运行时可以使用`vgGetString`函数(章节15.3.2描述)查询版本。
```
#define OPENVG_VERSION_1_0 1
#define OPENVG_VERSION_1_1 2
```

## 3.2 基本数据类型 <span id = "基本数据类型"></span>
OpenVG通过C语言的`typedef`关键字定义了许多基本数据类型。实际使用的数据类型是平台特定的。

**`VGbyte`**
`VGbyte`定义了一个8bit宽度的2的补码表示的有符号整数，其可以表示的值的范围是`-128~127`。如果定义了头文件`khronos_types.h`,`VGbyte`将被定义为`khronos_int8_t`。

**`VGubyte`**
`VGubyte`定义了一个8bit宽度的无符号整数，其可以表示的值的范围是`0~255`。如果定义了头文件`khronos_types.h`,`VGubyte`将被定义为`khronos_uint8_t`。

**`VGshort`**
`VGshort`定义了一个8bit宽度的2的补码表示的有符号整数，其可以表示的值的范围是`-32768~32767`。如果定义了头文件`khronos_types.h`,`VGshort`将被定义为`khronos_int16_t`。

**`VGint`**
`VGint`定义了一个8bit宽度的2的补码表示的有符号整数。如果定义了头文件`khronos_types.h`,`VGint`将被定义为`khronos_int32_t`。

**`VGuint`**
`VGubyte`定义了一个32bit宽度的无符号整数。如果定义了头文件`khronos_types.h`,`VGubyte`将被定义为`khronos_uint32_t`。

**`VGbitfield`**
`VGbitfield`定义了一个32bit宽度的无符号整数，用于可能组合多个独立的单位值的参数。`VGbitfield`必须至少能容纳32位。如果定义了头文件`khronos_types.h`,`VGubyte`将被定义为`khronos_uint32_t`。

**`VGboolean`**
`VGboolean`是一个枚举变量，其值只可以是`VG_FALSE`(0)或`VG_TRUE`(1)。任何赋值给`VGboolean`的非零值都将被解释为`VG_TRUE`。
```
typedef enum {
    VG_FALSE = 0,
    VG_TRUE  = 1
} VGboolean;
```

**`VGfloat`**
`VGfloat`定义了一个32位`IEEE 754`格式的浮点值。如果定义了头文件`khronos_types.h`,`VGubyte`将被定义为`khronos_float_t`。


## 3.3 浮点和整数表示 <span id = "浮点和整数表示"></span>
todo

## 3.4 颜色 <span id = "颜色"></span>
在`OpenVG`中，除了存储在图像像素中的颜色(比如用于清除、绘制和卷积的边缘扩展的颜色)外，其他颜色都表示为非预乘的`sRGBA`[sRGB99]颜色值。图像像素可以在许多颜色空间中定义，包括sRGB、线性RGB、线性灰度(或亮度)和非线性编码、感知均匀的灰度，以预乘或非预乘形式。除非另有说明，颜色和alpha值的范围为[0,1]。这适用于像素管道中的中间值以及应用程序指定的值。如果存在一个alpha通道，但位深度为0，每个像素的alpha值取为1。

### 3.4.1 线性和非线性颜色表示
### 3.4.2 颜色空间定义
### 3.4.3 预乘透明度
### 3.4.4 颜色格式转换


## 3.5 枚举数据类型 <span id = "枚举数据类型"></span>

## 3.5 句柄数据类型 <span id = "句柄数据类型"></span>



