# 8 路径 <span id = "路径"></span>
`路径`是`OpenVG` `API`的核心。所有要绘制的几何图形都必须用一条或多条路径来定义。路径由一系列段命令(或段)定义。标准格式的每一个分段命令都可以指定移动、直线段、二次或三次贝塞尔曲线段或椭圆弧。扩展可以定义其他段类型。

## 8.1 移动 <span id = "移动"></span>
路径段可能由`移动`段命令组成，该命令使路径直接跳转到给定点，无需绘图就开始新的子路径。

## 8.2 直线段 <span id = "直线段"></span>
路径可以包含`水平`、`垂直`或`任意线段`命令。一个特殊的`关闭路径`段命令可以用来生成一条直线段，将路径的当前顶点连接到路径当前部分开始的顶点。

## 8.3 贝塞尔曲线 <span id = "贝塞尔曲线"></span>
贝塞尔曲线是由参数定义的多项式曲线。也就是说，它们被定义为`(x(t), y(t))`形式的点的集合，其中`x(t)`和`y(t)`是`t`的多项式，`t`从0到1连续变化。路径可以包含二次或三次贝塞尔曲线段命令。

### 8.3.1 二次贝塞尔曲线
二次贝塞尔曲线由三个点定义，`(x0, y0)`, `(x1, y1)`和 `(x2, y2)`。曲线从`(x0, y0)`开始到`(x2, y2)`结束。曲线的形状受内部控制点`(x1, y1)`位置的影响，但曲线通常不经过该点。假设三个点不重合，曲线在初始点`x0`处的切线与向量`x1 - x0`方向一致，在终点`x2`处的切线与向量`x2 - x1`方向一致。组成曲线的点的集合公式如下：
$$x(t) = x_0(1-t)^2 + 2*x_1*(1-t)*t + x_2*t^2 \tag{0 < t < 1}$$
$$y(t) = y_0(1-t)^2 + 2*y_1*(1-t)*t + y_2*t^2 \tag{0 < t < 1}$$

### 8.3.2 三次贝塞尔曲线
二次贝塞尔曲线由四个点定义，`(x0, y0)`, `(x1, y1)`, `(x2, y2)`和 `(x3, y3)`。曲线从`(x0, y0)`开始到`(x3, y3)`结束。曲线的形状受内部控制点`(x1, y1)`和`(x2, y2)`位置的影响，但曲线通常不经过该点。假设这些点不重合，曲线在初始点`x0`处的切线与向量`x1 - x0`方向一致，在终点`x3`处的切线与向量`x3 - x2`方向一致。组成曲线的点的集合公式如下：
$$x(t) = x_0(1-t)^3 + 3*x_1*(1-t)^2*t + 3*x_2*(1-t)*t^2 + x_3*t^3 \tag{0 < t < 1}$$
$$y(t) = y_0(1-t)^3 + 3*y_1*(1-t)^2*t + 3*y_2*(1-t)*t^2 + y_3*t^3 \tag{0 < t < 1}$$

### 8.3.3 G1平滑段
G1平滑的二次段或三次段隐式地定义了它们的第一个内控点，从而保证当它们与前面的二次段或三次段连接时，连接点处的切线方向是连续的。从几何学的角度讲，这确保了两个段没有锐角，，但是异常的切向量的长度可能在连接点处出现不连续。
G1平滑的二次段或三次段的初始点的平滑度可以通过放置合适的第一个控制点`(x1, y1)`来保证。通过先前给出的二次或三次段的内部控制点`(px,py)`和终点`(ox,oy)`，可以通过公式`(2*(ox-px),2*(ox-px))`计算点`(x1,y1)`((即点`(px, py)`关于点`(ox, oy)`的对称点)。对于相同类型的段，这将提供`C1平滑`(见8.3.4章节)。
[图像](@todo)

### 8.3.4 C1平滑段(仅供参考)
@todo

### 8.3.5 C2平滑段(仅供参考)
@todo

### 8.3.6 将段从二次型转换为三次型(仅供参考)
@todo

## 8.4 椭圆弧 <span id = "椭圆弧"></span>
椭圆弧段将一对控制点与旋转角度(以度为单位)的椭圆部分连接起来。有了这些参数，根据环绕椭圆的方向(顺时针/逆时针)和环绕路径的大小不同，有四种可能的弧线。

下图显示了两个可能的椭圆，其水平轴为`rh`，垂直轴为`rv`，逆时针旋转角度`rot`(图中显示为标有`rot`的垂直线与标有`rv`的直线之间的夹角)通过点`(x0,y0)`和`(x1, y1)`。连接点的四条弧通过`L`和`S`标记大和小，`CW`和`CCW`标记顺时针和逆时针。
[图片](@todo)
负数的`rh`和`rv`将会由他们的绝对值代替。如果`rh`和`rv`有一个等于0，并且弧线的端点不重合，那么弧线就像投影到包含端点的直线上一样。如果`rh`和`rv`都等于0，或者弧线的端点重合，那么这条弧被画成两端点之间的线段。参数`rot`的值为对`360°`的模。

如果由于端点之间的距离太远(将在下一节中详细介绍)，使得给定参数下不存在椭圆弧，则画出的弧就好像半径被‘允许解的最小因子’均匀地放大一样。

关于椭圆的数学注释见`附录A`。

## 8.5 路径标准格式 <span id = "路径标准格式"></span>
复杂路径可以在应用程序的内存中构造，然后传递给`OpenVG`去定义`VGPath`对象。这样的路径数据是由一系列段命令组成，每个段命令有着独立的几何坐标和参数。

在本章节，我们为路径定义`标准数据格式`，可用于定义各种类型的路径段的序列。其他路径数据格式将由扩展定义。

**`VG_PATH_FORMAT_STANDARD`**

宏`VG_PATH_FORMAT_STANDARD`定义了一个常量，该常量作为`vgCreatePath`函数的一个参数来指示路径数据使用标准格式存储。随着`API`版本的修订，版本号的低16位可能会增加。每个`OpenVG`的版本都兼容先前版本定义的数据格式，即后向兼容。

希望定义额外路径格式的扩展可以注册高16位不同的格式标识符；扩展供应商可以将低16位用于版本控制。
```
#define VG_PATH_FORMAT_STANDARD 0;
```

### 8.5.1 路径段命令的副作用
为了定义每个段命令类型的语义，我们定义了上参考点(初值均为`(0,0)`):
 - `(sx,sy)` : 当前子路径的开始,即最后一个`MOVE_TO段`的位置。
 - `(ox,oy)` : 上一个线段的最后一个点
 - `(px,py)` : 如果上一个线段是一个(规则的或光滑的)二次或三次贝塞尔曲线，表示最后一个内部控制点，否则表示前一线段的最后一个点。

图6说明了这些点在段命令序列末尾的位置`{MOVE_TO, LINE_TO,CUBIC_TO}`。
[图片](@todo)

在接下来的讨论中，我们将点`(x0, y0)`，`(x1, y1)`和`(x2, y2)`定义为绝对坐标，对于使用相对坐标定义的段，`(x0, y0`)等定义为添加到`(ox, oy)`的输入坐标值。椭圆`rh`、`rv`和`rot`参数不受使用相对坐标的影响。每一个段(`MOVE_TO段`除外)开始于前一个段定义的点`(ox, oy)`。

路径包含了一系列的子路径。当遇到路径段命令时，每一个段被追加到当前子路径。当前子路径以`MOVE_TO`或`CLOSE_PATH`段结束，并开始一个新的当前子路径。路径数据的结束也是当前子路径的结束。

### 8.5.2 段命令
下表描述了每个分段命令类型及其前缀、所需的指定坐标和参数的数量、分段命令的数值、任何隐式坐标的公式，以及分段命令对点`(ox, oy)`、`(sx, sy)`和`(px, py)`和`当前子路径终止`的副作用。
| 段描述  |段名称 | 坐标 | 值 | 隐式点 | 副作用 |
| ------- | ------- |------- |------- | ------- |------- |
| 路径关闭| CLOSE_PATH|无 | 0| | (px,py)=(ox,oy)=(sx,sy)时结束当前路径|
| 移动| MOVE_TO| x0,y0| 2| | (px,py)=(ox,oy)=(sx,sy)时结束当前路径|
| 线段|LINE_TO |x0,y0 |4 | | (px,py)=(ox,oy)=(x0,y0)|
| 水平线|HLINE_TO |x0 |6 |y0=oy | (px,py)=(x0,oy) ox=x0|
| 垂直线|VLINE_TO |y0 | 8| x0=ox|(px,py)=(x0,oy) ox=x0 |
| 二次方程|QUAD_TO |x0,y0,x1,y1 |10 | | (px,py)=(x0,y0),(ox,oy)=(x1,y1)|
| 三次方程| CUBIC_TO|x0,y0,x1,y1,x2,y2 |12 | |(px,py)=(x1,y1),(ox,oy)=(x2,y2) |
| G1二次曲线平滑|SQUAD_TO | x1,y1| 14|(x0,y0)=(2ox­-px,2oy-py) |(px,py)= (2ox-­px, 2oy-­py),(ox,oy)=(x1,y1) |
| G1三次曲线平滑|SCUBIC_TO | x1,y1,x2,y2|16 |(x0,y0)=(2ox­-px,2oy-py) |(px,py)=(x1,y1) (ox,oy)=(x2,y2)|
| 小路径逆时针弧|SCCWARC_TO |rh,rv,rot,x0,y0 |18 | | (px,py)=(ox,oy)=(x0,y0)|
| 小路径顺时针弧|SCWARC_TO| rh,rv,rot,x0,y0 | 20||(px,py)=(ox,oy)=(x0,y0) |
| 大路径逆时针弧|LCCWARC_TO |rh,rv,rot,x0,y0 | 22| | (px,py)=(ox,oy)=(x0,y0)|
| 大路径顺时针弧| LCWARC_TO| rh,rv,rot,x0,y0| 24| | (px,py)=(ox,oy)=(x0,y0)|
| 保留| Reserved| 26,28,30| | | |
| | | | | | |

每个段类型都可以使用绝对坐标或相对坐标来定义。将相对坐标`(x,y)`添加到`(ox,oy)`可以得到绝对坐标`(ox+x,oy+y)`。在每个段渲染的过程中相对坐标立即转换为绝对坐标。

提供`HLINE_TO`和`VLINE_TO`段类型是为了避免`SVG查看器`之类的应用程序在解析路径数据时执行自己的相对与绝对转换。

在SVG中，平滑二次段和三次段的行为与上面定义的行为略有不同。如果一个光滑二次段没有跟在一个二次段后面，或者一个光滑三次段没有跟在一个三次段后面，则初始控制点`(x0, y0)`位于`(ox, oy)`，而不是计算为`(px, py)`的对称点。当与前一段的类型不一致时，其行为可以通过将SVG平滑段转换为指定其所有控制点的常规段来模拟。

请注意，由于坐标是基于`(ox, oy)`， `(sx, sy)`和初始值为`(0,0)`的`(px, py)`，即使路径不是以`MOVE_TO`段(也包括`HLINE_TO`,` VLINE_TO`,或相对段类型)开始，路径的坐标也会被定义。

### 8.5.3 坐标数据格式
坐标和参数数据(后面简称为坐标数据)可以用一组格式来表示,如下表所示。多字节的坐标数据(比如坐标数据类型为`S_16`,`S_32`及`F`)在应用程序内存中使用平台的本机字节顺序表示。具体实现时可以将`S_32`和`F`格式的传入数据量化为更少的比特数，但是要保证至少16位的精度。

正确的使用`平滑曲线段`和8位、16位的数据类型可以为常见路径数据(如字体符号)节省大量内存。使用更小的数据类型还可以在将路径数据从应用程序内存传输到`OpenVG`时节省总线带宽。
| 数据类型  |`VG_PATH_DATATYPE_`后缀 | 字节数 | 值
| ------- | ------- |------- |------- |
|8位有符号整型|S_8|1|0|
|16位有符号整型|S_16|2|1|
|32位有符号整型|S_32|3|2|
|IEEE 754格式浮点型|F|4|3|
|||||

**`VGPathDatatype`**
`VGPathDatatype`枚举变量定义的坐标类型及其值描述如下
```
ypedef enum {
    VG_PATH_DATATYPE_S_8  = 0,
    VG_PATH_DATATYPE_S_16 = 1,
    VG_PATH_DATATYPE_S_32 = 2,
    VG_PATH_DATATYPE_F    = 3
} VGPathDatatype;
```

### 8.5.4 分段类型标记定义
分段类型标记被定义为一个8位整型数据，其中高三位保留做以后使用，接下来的四位表示段命令类型，最低位表示坐标值的相对/绝对属性(`0`表示绝对坐标值，`1`表示相对坐标值)。保留位必须置为0。

对于`CLOSE_PATH`段命令，表示相对/绝对坐标属性的`bit[0]`将被忽略。
[图片](@todo)

**`VGPathAbsRel`**

`VGPathAbsRel`枚举变量描述了坐标的相对/绝对属性，其定义描述如下
```
typedef enum {
    VG_ABSOLUTE = 0,
    VG_RELATIVE = 1
} VGPathAbsRel;
```
**`VGPathSegment`**
`VGPathSegment`枚举变量定义了每一个段命令类型，其值被定义为`1`的移位,这样可以轻松的与`VGPathAbsRel`进行组合。其定义描述如下
```
typedef enum {
    VG_CLOSE_PATH = ( 0 << 1),
    VG_MOVE_TO    = ( 1 << 1),
    VG_LINE_TO    = ( 2 << 1),
    VG_HLINE_TO   = ( 3 << 1),
    VG_VLINE_TO   = ( 4 << 1),
    VG_QUAD_TO    = ( 5 << 1),
    VG_CUBIC_TO   = ( 6 << 1),
    VG_SQUAD_TO   = ( 7 << 1),
    VG_SCUBIC_TO  = ( 8 << 1),
    VG_SCCWARC_TO = ( 9 << 1),
    VG_SCWARC_TO  = (10 << 1),
    VG_LCCWARC_TO = (11 << 1),
    VG_LCWARC_TO  = (12 << 1)
} VGPathSegment;
```

**`VGPathCommand`**
`VGPathCommand`枚举变量为每个`分段命令类型`和`坐标的绝对/相对值`定义了组合值。这些值与`VGPathAbsRel`中的适当值，以`按位或`（`|`)的方式获得完整的段命令值。其定义描述如下
```
typedef enum {
    VG_MOVE_TO_ABS    = VG_MOVE_TO    | VG_ABSOLUTE,
    VG_MOVE_TO_REL    = VG_MOVE_TO    | VG_RELATIVE,
    VG_LINE_TO_ABS    = VG_LINE_TO    | VG_ABSOLUTE,
    VG_LINE_TO_REL    = VG_LINE_TO    | VG_RELATIVE,
    VG_HLINE_TO_ABS   = VG_HLINE_TO   | VG_ABSOLUTE,
    VG_HLINE_TO_REL   = VG_HLINE_TO   | VG_RELATIVE,
    VG_VLINE_TO_ABS   = VG_VLINE_TO   | VG_ABSOLUTE,
    VG_VLINE_TO_REL   = VG_VLINE_TO   | VG_RELATIVE,
    VG_QUAD_TO_ABS    = VG_QUAD_TO    | VG_ABSOLUTE,
    VG_QUAD_TO_REL    = VG_QUAD_TO    | VG_RELATIVE,
    VG_CUBIC_TO_ABS   = VG_CUBIC_TO   | VG_ABSOLUTE,
    VG_CUBIC_TO_REL   = VG_CUBIC_TO   | VG_RELATIVE,
    VG_SQUAD_TO_ABS   = VG_SQUAD_TO   | VG_ABSOLUTE,
    VG_SQUAD_TO_REL   = VG_SQUAD_TO   | VG_RELATIVE,
    VG_SCUBIC_TO_ABS  = VG_SCUBIC_TO  | VG_ABSOLUTE,
    VG_SCUBIC_TO_REL  = VG_SCUBIC_TO  | VG_RELATIVE,
    VG_SCCWARC_TO_ABS = VG_SCCWARC_TO | VG_ABSOLUTE,
    VG_SCCWARC_TO_REL = VG_SCCWARC_TO | VG_RELATIVE,
    VG_SCWARC_TO_ABS  = VG_SCWARC_TO  | VG_ABSOLUTE,
    VG_SCWARC_TO_REL  = VG_SCWARC_TO  | VG_RELATIVE,
    VG_LCCWARC_TO_ABS = VG_LCCWARC_TO | VG_ABSOLUTE,
    VG_LCCWARC_TO_REL = VG_LCCWARC_TO | VG_RELATIVE,
    VG_LCWARC_TO_ABS  = VG_LCWARC_TO  | VG_ABSOLUTE,
    VG_LCWARC_TO_REL  = VG_LCWARC_TO  | VG_RELATIVE
} VGPathCommand;
```

### 8.5.5 路径例子
下面的代码示例展示如何使用标准表示遍历存储在应用程序内存中的路径数据。一个字节包含了`段命令`，`段命令类型`，`相对/绝对标志`，由应用程序中定义的`SEGMENT_COMMAND`和`SEGMENT_ABS_REL`宏来提取。坐标的数量和每个坐标的字节数(对于给定的数据格式)也是通过查找表确定的。最后，与表示当前段的路径数据相关的部分被复制到一个临时缓冲区中，并作为用户定义的`processSegment`函数的参数，该函数可以执行进一步的处理。
```
#define PATH_MAX_COORDS 6 /* Maximum number of coordinates/command */
#define PATH_MAX_BYTES  4 /* Bytes in largest data type */
#define SEGMENT_COMMAND(command) ((command) & 0x1e)/* Extract segment type */ \
#define SEGMENT_ABS_REL(command) ((command) & 0x1) /* Extract absolute/relative bit */ \

/* Number of coordinates for each command */
static const VGint numCoords[] = {0,2,2,1,1,4,6,2,4,5,5,5,5};
/* Number of bytes for each datatype */
static const VGint numBytes[] = {1,2,4,4};
/* User‐defined function to process a single segment */
extern void processSegment(VGPathSegment command, VGPathAbsRel absRel,
                           VGPathDatatype datatype, void * segmentData);
/* Process a path in the standard format, one segment at a time. */
void processPath(const VGubyte * pathSegments, const void * pathData,
                 int numSegments, VGPathDatatype datatype)
{
    VGubyte segmentType, segmentData[PATH_MAX_COORDS*PATH_MAX_BYTES];
    VGint segIdx = 0, dataIdx = 0;
    VGint command, absRel, numBytes;
    while (segIdx < numSegments)
    {
        segmentType = pathSegments[segIdx++];
        command     = SEGMENT_COMMAND(segmentType);
        absRel      = SEGMENT_ABS_REL(segmentType);
        numBytes    = numCoords[command]*numBytes[datatype];
        /* Copy segment data for further processing */
        memcpy(segmentData, &pathData[dataIdx], numBytes);
        /* Process command */
        processSegment(command, absRel, datatype, (void *) segmentData);
        dataIdx    += numBytes;
    }
}
```
## 8.6 路径操作 <span id = "路径操作"></span>
除了填充或描边路径外，`API`还允许对路径进行以下基本操作:
 - 创建具有给定功能集的路径(`vgCreatePath`)
 - 从路径中移除数据(`vgClearPath`)
 - 销毁路径(`vgDestroyPath`)
 - 查询路径信息(`vgGetParameter`)
 - 查询路径的功能集(`vgGetPathCapabilities`)
 - 减少路径的功能集(`vgRemovePathCapabilities`)
 - 将数据从一个路径追加到另一个路径(`vgAppendPath`)
 - 在路径上追加数据(`vgAppendPathData`)
 - 修改路径中的坐标(`vgModifyPathCoords`)
 - 路径转换(`vgModifyPathCoords`)
 - 在两条路径之间进行插值(`vgInterpolatePath`)
 - 确定路径的几何长度(`vgPathLength`)
 - 获取路径上给定几何距离的点的位置和切线信息(`vgPointAlongPath`)
 - 为路径获取一个轴对齐的包围框(`vgPathBounds`,`vgTransformedPathBounds`)

高级几何原语在可选的VGU实用程序库中定义(参考章节17)：
 - 在路径上追加一条线(`vguLine`)
 - 将折线(线段连接序列)或多边形附加到路径(`vguRoundRect`)
 - 将椭圆附加到路径上(`vguEllipse`)
 - 将圆弧附加到路径上(`vguArc`)

### 8.6.1 路径存储
`OpenVG`内部实现了路径数据的存储。路径可以通过`VGPath`句柄引用。应用程序可以使用上面定义的内存表示来初始化路径，也可以用扩展定义的其他形式定义路径。在硬件加速存储器中实现路径数据的存储是可能的，实现也可以使用它们自己内部的路径段。其目的是使应用程序能够定义一组路径，例如，当前字体中的每个字形都有一个路径，并且能够以最大效率重新呈现以前定义的每个路径。

**`VGPath`**
`VGPath`是可以用来引用路径的一个句柄。
```
typedef VGHandle VGPath;
```

### 8.6.2 路径的销毁及创建
函数`vgCreatePath`和`vgDestroyPath`分别用于路径的创建和销毁。在路径的整个生命周期内，应用程序可以通过定义枚举变量`VGPathCapabilities`指定哪条路径应该被执行。

**`VGPathCapabilities`** 
`VGPathCapabilities`定义了一组常量，该组常量描述了在一个给定的`路径对象`上可以执行哪些操作。在路径被定义的同时，应用程序同时指定在该路径上可以执行哪些操作。在随后的运行过程中，应用程序可以禁能先前使能的权限,但是一旦被禁能就不可以在重新使能。该特性允许OpenVG实现使用可能不支持所有路径操作的内部路径表示，这可能会在不执行这些操作的路径上获得更高的性能。
位及相应的功能描述列出如下：

| 位名  | 功能描述 |
| ------------- | ------------- |
| VG_PATH_CAPABILITY_APPEND_FROM  | 使用路径作为 **`vgAppendPath`** 的`srcPath`参数  |
| VG_PATH_CAPABILITY_APPEND_TO  | 使用路径作为 **`vgAppendPath`**和**`vgAppendPathData`** 的`dstPath`参数  |
| VG_PATH_CAPABILITY_MODIFY  | 使用路径作为 **`vgModifyPathCoords`** 的`dstPath`参数  |
| VG_PATH_CAPABILITY_TRANSFORM_FROM  | 使用路径作为 **`vgTransformPath`** 的`srcPath`参数  |
| VG_PATH_CAPABILITY_TRANSFORM_TO  | 使用路径作为 **`vgTransformPath`** 的`dstPath`参数  |
| VG_PATH_CAPABILITY_INTERPOLATE_FROM  | -使用路径作为 **`vgInterpolatePath`** 的`startPath`或`endPath`参数  |
| VG_PATH_CAPABILITY_INTERPOLATE_TO  | 使用路径作为 **`vgInterpolatePath`** 的`dstPath`参数  |
| VG_PATH_CAPABILITY_PATH_LENGTH  | 使用路径作为 **`vgPathLength`** 的`path`参数  |
| VG_PATH_CAPABILITY_POINT_ALONG_PATH  | 使用路径作为 **`vgPointAlongPath`** 的`path`参数  |
| VG_PATH_CAPABILITY_TANGENT_ALONG_PATH  | todo  |
| VG_PATH_CAPABILITY_PATH_BOUNDS  | 使用路径作为 **`vgPathBounds`** 的`path`参数  |
| VG_PATH_CAPABILITY_PATH_TRANSFORMED_BOUNDS  | 使用路径作为 **`vgPathTransformedBounds`** 的`path`参数  |
| VG_PATH_CAPABILITY_ALL  | 以上所有定义的集合  |
|||

```
typedef enum {
    VG_PATH_CAPABILITY_APPEND_FROM             = (1 << 0),
    VG_PATH_CAPABILITY_APPEND_TO               = (1 << 1),
    VG_PATH_CAPABILITY_MODIFY                  = (1 << 2),
    VG_PATH_CAPABILITY_TRANSFORM_FROM          = (1 << 3),
    VG_PATH_CAPABILITY_TRANSFORM_TO            = (1 << 4),
    VG_PATH_CAPABILITY_INTERPOLATE_FROM        = (1 << 5),
    VG_PATH_CAPABILITY_INTERPOLATE_TO          = (1 << 6),
    VG_PATH_CAPABILITY_PATH_LENGTH             = (1 << 7),
    VG_PATH_CAPABILITY_POINT_ALONG_PATH        = (1 << 8),
    VG_PATH_CAPABILITY_TANGENT_ALONG_PATH      = (1 << 9),
    VG_PATH_CAPABILITY_PATH_BOUNDS             = (1 << 10),
    VG_PATH_CAPABILITY_PATH_TRANSFORMED_BOUNDS = (1 << 11),
    VG_PATH_CAPABILITY_ALL                     = (1 << 12) ‐ 1
} VGPathCapabilities;
```
忽略当前路径能力的设置去调用`vgCreatePath`,`vgClearPath`,`vgDestroyPath`函数是被允许的，因为这些函数丢弃了现有的路径定义。

**`vgCreatePath`**

`vgCreatePath`创建一个准备接受段数据的新路径，并返回一个`VGPath`句柄，路径数据根据给出的参数`pathFormat`做格式化，该参数多数情况下填入`VG_PATH_FORMAT_STANDARD`。`datatype`参数包含来自枚举变量`VGPathDatatype`的值，该值表示坐标数据的类型。`capabilities`参数是所需`VGPathCapabilities`的按位或。未与`VGPathCapabilities`中的值对应的功能位没有影响。如果发生错误，则返回`VG_INVALID_HANDLE`。

`scale`和`bias`参数用于解释每个坐标的路径数据；输入的坐标值`v`将会被解释为`(scale * v + bias)`。`scale`参数必须不为零。`datatype`，`scale`，`bias`一起定义了一个路径有效坐标数据范围，在该范围之外的路径中放置坐标的分段命令将会溢出，导致出现未定义的坐标值，使用`vgPathLength`和`vgPointAlongPath`之类的查询函数查询该类值也将返回未定义的结果。

`segmentCapacityHint`参数提供了最终可以存储在路径中的段的个数。`coordCapacityHint`参数提供了最终可以存储在路径中的坐标(见8.5.2章节中标的坐标列)的个数。小于等于零的值表示其能力未知。不管提示值为多少，路径存储空间在任何情况下都会根据需要增长，但是提供提示可以通过减少随着路径增长而分配额外空间的需要进而来提高性能。实现应该允许应用程序少量地追加段和协调指定的容量，而不会由于过度的内存重新分配使得性能降低。
```
VGPath vgCreatePath(VGint pathFormat,
                    VGPathDatatype datatype,
                    VGfloat scale, VGfloat bias,
                    VGint segmentCapacityHint,
                    VGint coordCapacityHint,
                    VGbitfield capabilities)
```
> **`ERRORS`**
> 
> `VG_UNSUPPORTED_PATH_FORMAT_ERROR`
>  - 如果`pathFormat`传入了不支持的格式
> 
> `VG_ILLEGAL_ARGUMENT_ERROR`
>  - 如果`datatype`的传入值不是`VGPathDatatype`枚举标量中列出的值。
> 如果`scale`等于0

**`vgClearPath`**

`vgClearPath`函数将绑定在路径上的段命令机器坐标移除。其句柄在执行该函数后将继续有效，其路径格式和数据类型保留现有的值。`capabilities`参数是所需`VGPathCapabilities`的按位或。未与`VGPathCapabilities`中的值对应的功能位没有影响。对于生命周期比较短的路径，使用`vgClearPath`可能比销毁和重新创建路径更有效。
```
void vgClearPath(VGPath path, VGbitfield capabilities)
```
> **`ERRORS`**
> 
> `VG_BAD_HANDLE_ERROR`
>  - 如果`path`不是一个有效的路径句柄，或者没有在当前环境共享。

**`vgDestroyPath`**

`vgDestroyPath`释放与`path`关联的所有资源，并使句柄在共享它的所有上下文中无效。
```
void vgDestroyPath(VGPath path)
```
> **`ERRORS`**
> 
> `VG_BAD_HANDLE_ERROR`
>  - 如果`path`不是一个有效的路径句柄，或者没有在当前环境共享。

### 8.6.3 路径查询
### 8.6.4 路径性能的查询及修改
### 8.6.5 路径间的数据复制
### 8.6.6 向路径追加数据
### 8.6.7 修改路径数据
### 8.6.8 转化路径
### 8.6.9 在路径之间做插值
### 8.6.10 路径长度
### 8.6.11 位置及路径切线
### 8.6.12 查询路径的包围框、

## 8.7 路径处理 <span id = "路径操作"></span>
### 8.7.1 路径填充
### 8.7.2 路径笔画
### 8.7.3 stroke参数
### 8.7.4 stroke生成
### 8.7.5 设置stroke参数
### 8.7.6 无缩放stroke

## 8.8 路径填充或描边 <span id = "路径填充或描边"></span>