# 5 API参数设置 <span id = "API参数设置"></span>

API参数可以使用`set`和`get`函数来设置和查询，使用泛型函数可以在不添加额外函数的情况下实现API的可扩展性，通过向`Khronos组织`注册，扩展可以接收唯一标识符值作为新参数类型。

参数有两种形式：一种是渲染环境相关的设置，另一种特定的基于`VGHandle`对象的相关的设置。前者使用`vgSet`和`vgGet`函数，后者使用`vgSetParameter`和`vgGetParameter`函数。

## 5.1 环境参数类型 <span id = "环境参数类型"></span>
渲染环境设置的参数类型在枚举变量`VGParamType`中定义，每个参数的数据类型及缺省值如下表所示。

**VGParamType**
枚举变量`VGParamType`定义了使用`vgSet`和`vgGet`函数设置和查询参数类型及对应的值。
```
typedef enum {
    /* 模式设置 */
    VG_MATRIX_MODE                 = 0x1100,
    VG_FILL_RULE                   = 0x1101,
    VG_IMAGE_QUALITY               = 0x1102,
    VG_RENDERING_QUALITY           = 0x1103,
    VG_BLEND_MODE                  = 0x1104,
    VG_IMAGE_MODE                  = 0x1105,
    /* 矩形裁剪 */
    VG_SCISSOR_RECTS               = 0x1106,
    /* 色彩变换 */
    VG_COLOR_TRANSFORM             = 0x1170,
    VG_COLOR_TRANSFORM_VALUES      = 0x1171,
    /* 笔画参数 */
    VG_STROKE_LINE_WIDTH           = 0x1110,
    VG_STROKE_CAP_STYLE            = 0x1111,
    VG_STROKE_JOIN_STYLE           = 0x1112,
    VG_STROKE_MITER_LIMIT          = 0x1113,
    VG_STROKE_DASH_PATTERN         = 0x1114,
    VG_STROKE_DASH_PHASE           = 0x1115,
    VG_STROKE_DASH_PHASE_RESET     = 0x1116,
    /* VG_TILE_FILL平铺模式的边缘填充颜色 */
    VG_TILE_FILL_COLOR             = 0x1120,
    /* vgClear的颜色 */
    VG_CLEAR_COLOR                 = 0x1121,
    /* 字符源 */
    VG_GLYPH_ORIGIN                = 0x1122,
    /* 使能/禁能掩蔽和裁切 */
    VG_MASKING                     = 0x1130,
    VG_SCISSORING                  = 0x1131,
    /* 像素布局信息 */
    VG_PIXEL_LAYOUT                = 0x1140,
    VG_SCREEN_LAYOUT               = 0x1141,
    /* 图像过滤器的源格式选择 */
    VG_FILTER_FORMAT_LINEAR        = 0x1150,
    VG_FILTER_FORMAT_PREMULTIPLIED = 0x1151,
    /* 图像过滤器的使能目标写掩码 */
    VG_FILTER_CHANNEL_MASK         = 0x1152,
    /* 极限值定义(只读) */
    VG_MAX_SCISSOR_RECTS           = 0x1160,
    VG_MAX_DASH_COUNT              = 0x1161,
    VG_MAX_KERNEL_SIZE             = 0x1162,
    VG_MAX_SEPARABLE_KERNEL_SIZE   = 0x1163,
    VG_MAX_COLOR_RAMP_STOPS        = 0x1164,
    VG_MAX_IMAGE_WIDTH             = 0x1165,
    VG_MAX_IMAGE_HEIGHT            = 0x1166,
    VG_MAX_IMAGE_PIXELS            = 0x1167,
    VG_MAX_IMAGE_BYTES             = 0x1168,
    VG_MAX_FLOAT                   = 0x1169,
    VG_MAX_GAUSSIAN_STD_DEVIATION  = 0x116A
} VGParamType;
```
## 5.2 设置查询环境参数 <span id = "设置查询环境参数"></span>
根据设置值的参数类型的不同，每一个`vgGet/vgGetParameter`或` vgSet/vgSetParameter`函数都有四种变体，他们靠后缀名区分：
- **`i `** : 表示整型标量
- **`f `** : 表示浮点型标量
- **`iv`** : 表示整型矢量
- **`fv`** : 表示浮点型矢量

设置矢量数据的函数也可以用来设置标量值，当其用于设置标量时，其参数`count`值应为1，相应的。设置整型数据的函数也可以用于设置浮点型数据，当其用于设置浮点值时，被设置的浮点值将被转换为一个整型值，如果转换结果值超出了整数值的范围，则用接近的有效整数值替代。

参数`count`用于表示要设置的矢量数据的长度。
对于需要固定长度的矢量值(例如，类型为VGfloat[4]的颜色值)，`count`必须具有适当的值。
对于可能接受的值的数量有限制的参数(例如，它是一个特定数字的倍数，对于矩形裁切参数，它被指定为一组4元组)，`count`必须遵守相应的限制。
对于接受任意数量的值的参数(例如，dash模式)，`count`取值小于最大值都可以被使用，超过最大值的值将被忽略。
如果count形参为0，则指针实参不会被解引用。例如，调用`vgSet(VG_STROKE_DASH_PATTERN, 0, (void *)0)`将dash模式设置为零长度数组(具有禁用dash模式的效果)，而不取消对第三个参数的引用。如果由于count的值不合适而发生错误，则调用对参数值没有影响。

部分参数值是只读的，在这些值上调用`vgSet`或`vgSetParameter`不会带来任何效果。

***`vgSet`***

`vgSet`函数用于设置当前环境的参数值。
```
void vgSetf (VGParamType paramType, VGfloat value)
void vgSeti (VGParamType paramType, VGint value)
void vgSetfv(VGParamType paramType, VGint count, const VGfloat * values)
void vgSetiv(VGParamType paramType, VGint count, const VGint * values)
```
> **`ERRORS`**
> 
> `VG_ILLEGAL_ARGUMENT_ERROR`
>
> - 如果参数类型不是`VGParamType`枚举变量已列出的类型
> - 如果在`vgSetf`或`vgSeti`函数中引用了需要矢量数据的参数类型
> - 如果在`vgSetfv`或`vgSetiv`函数中引用了需要标量数据的参数类型且`count`参数取值不是`1`
> - 如果`value`的传入值不是`vgSetf`或`vgSeti`中给定参数类型的合法值，
> - 如果`values[i]`的传入值不是`vgSetfv`或`vgSetiv`中给定参数类型的合法值(`0 ≤ i < count`)
> - 如果`vgSetfv`或`vgSetiv`函数中`value`的传入值是`NULL`但是`count`却是一个大于0 的值
> - 如果`vgSetfv`或`vgSetiv`函数中`value`的传入值没有正确的对齐
> - 如果`vgSetfv`或`vgSetiv`函数中`count`的传入值小于0
> - 如果`count`的传入值不是给定参数类型的有效值

比如，如果要将混合模式设置为整型值`VG_BLEND_SRC_OVER`(参考章节13.6),应用程序应该调用:
```
vgSeti(`VG_BLEND_MODE`, `VG_BLEND_SRC_OVER`);
```

***`vgGet` 和 `vgGetVectorSize`***

`vgGet`函数返回当前环境的参数值。

`vgGetiv`或`vgGetfv`函数获取的相应参数类型的数据有一个`最大长度`，该`最大长度`可通过`vgGetVectorSize`函数获取。对于标量数据，该函数返回`1`。如果`vgGetiv`或`vgGetfv`函数调用时`count`传入了一个小于`最大长度`的值，则函数按照传入的`count`值返回相应的元素个数;如果`count`传入了一个大于`最大长度`的值，将触发一个错误。

不管内部的实现方式怎样，`vgSet`函数(除非特别注明，这里的`vgSet`函数都认为是已经没有错误的完整执行)传入的原始值都可以通过`vgGet`函数返回，该规则确保`OpenVG`状态可以完整的保存及恢复。

如果在调用`vgGetf`,`vgGeti`,或`vgGetVectorSize`函数时发生了错误，那么返回值是无效的；如果在调用`vgGetfv`或`vgGetiv`函数时发生错误，那么传出参数`value`不会被写入任何值。
```
VGfloat vgGetf (VGParamType paramType)
VGint vgGeti (VGParamType paramType)
VGint vgGetVectorSize(VGParamType paramType)
void vgGetfv(VGParamType paramType, VGint count, VGfloat * values)
void vgGetiv(VGParamType paramType, VGint count, VGint * values)
```
> **`ERRORS`**
> 
> `VG_ILLEGAL_ARGUMENT_ERROR`
>
> - 如果参数类型不是`VGParamType`枚举变量已列出的类型
> - 如果在`vgGetf`或`vgGeti`函数中引用了需要矢量数据的参数类型
> - 如果在`vgGetf`或`vgGeti`函数中的`value`参数值传入了`NULL`
> - 如果`vgGetfv`或`vgGetiv`函数中`value`的传入值没有正确的对齐
> - 如果`vgGetfv`或`vgGetiv`函数中`count`的传入值小于0
> - 如果`vgGetfv`或`vgGetiv`函数中`count`的传入值大于`vgGetVectorSize`函数的相应参数类型的返回值

### 5.2.1 环境参数的缺省值
当一个新的`OpenVG`环境被创建完成后，它包含了一些默认值，其列出如下表所示(注:因空间限制,缺省值分为了几块描述)：
 
| 参数  | 数据类型 | 缺省值 |
| -------------------   | ------------------- |------------------- |
|VG_MATRIX_MODE   | VGMatrixMode  |VG_MATRIX_PATH_USER_TO_SURFACE|
|VG_FILL_RULE     |VGFillRule     |VG_EVEN_ODD|
|VG_IMAGE_QUALITY |VGImageQuality |VG_IMAGE_QUALITY_FASTER |
|VG_RENDERING_QUALITY |VGRenderingQuality |VG_RENDERING_QUALITY_BETTER |
|VG_BLEND_MODE| VGBlendMode |VG_BLEND_SRC_OVER|
|VG_IMAGE_MODE|VGImageMode |VG_DRAW_IMAGE_NORMAL|
|VG_SCISSOR_RECTS |VGint * |{ }(array of length 0)|
|VG_COLOR_TRANSFORM |VGboolean |VG_FALSE (disabled)|
|VG_COLOR_TRANSFORM_VALUES |VGfloat[8]| { 1.0f, 1.0f, 1.0f, 1.0f, 0.0f, 0.0f, 0.0f, 0.0f }|
|VG_STROKE_LINE_WIDTH |VGfloat |1.0f|
|VG_STROKE_CAP_STYLE |VGCapStyle| VG_CAP_BUTT|
|VG_STROKE_JOIN_STYLE |VGJoinStyle| VG_JOIN_MITER|
|VG_STROKE_MITER_LIMIT |VGfloat |4.0f|
|VG_STROKE_DASH_PATTERN |VGfloat * |{ } (array of length 0) (disabled)|
|VG_STROKE_DASH_PHASE |VGfloat |0.0f|
|VG_STROKE_DASH_PHASE_RESET |VGboolean |VG_FALSE (disabled)|
|VG_TILE_FILL_COLOR |VGfloat[4] |{ 0.0f, 0.0f, 0.0f, 0.0f }|
|VG_CLEAR_COLOR |VGfloat[4] |{ 0.0f, 0.0f, 0.0f, 0.0f }|
|VG_GLYPH_ORIGIN |VGfloat[2] |{ 0.0f, 0.0f }|
|VG_MASKING |VGboolean |VG_FALSE (disabled)|
|VG_SCISSORING |VGboolean |VG_FALSE (disabled)|
|VG_PIXEL_LAYOUT |VGPixelLayout |VG_PIXEL_LAYOUT_UNKNOWN|
|VG_SCREEN_LAYOUT |VGPixelLayout |Layout of the drawing surface|
|VG_FILTER_FORMAT_LINEAR |VGboolean |VG_FALSE (disabled)
|VG_FILTER_FORMAT_PREMULTIPLIED |VGboolean |VG_FALSE (disabled)|
|VG_FILTER_CHANNEL_MASK |VGbitfield |(VG_RED \|VG_GREEN\| VG_BLUE \|VG_ALPHA)|
||||

只读参数 `VG_MAX_SCISSOR_RECTS`，`VG_MAX_DASH_COUNT`，`VG_MAX_KERNEL_SIZE`，`VG_MAX_SEPARABLE_KERNEL_SIZE`， `VG_MAX_GAUSSIAN_STD_DEVIATION`，
`VG_MAX_COLOR_RAMP_STOPS`，`VG_MAX_IMAGE_WIDTH`，`VG_MAX_IMAGE_HEIGHT`，`VG_MAX_IMAGE_PIXELS`，`VG_MAX_IMAGE_BYTES`和
`VG_MAX_FLOAT`的缺省值初始化为实现定义的值。

参数`VG_SCREEN_LAYOUT`初始化为当前显示设备的当前会图面的布局(如果适用)。

矩阵模式下的矩阵 `VG_MATRIX_PATH_USER_TO_SURFACE`， `VG_MATRIX_IMAGE_USER_TO_SURFACE`，
`VG_MATRIX_GLYPH_USER_TO_SURFACE`，`VG_MATRIX_FILL_PAINT_TO_USER`和 `VG_MATRIX_STROKE_PAINT_TO_USER`初始化为单位矩阵(参考章节6.5)

$$
\left[\begin{matrix}sh & shx & tx \\shy & sy & ty \\w_0 & w_1 & w_2 \end{matrix} \right] 
=
\left[\begin{matrix}1 & 2 & 3 \\4 & 5 & 6 \\7 & 8 & 9 \end{matrix} \right]
$$

默认情况下，没有为`填充模式`或`描边绘制模式`设置绘图对象。而是用默认的paint参数值代替，如章节9.1.3所述。

## 5.3 设置查询对象参数 <span id = "设置查询对象参数"></span>
这里的对象是特指使用`VGHandle`作为其句柄的对象，比如  `VGImage`, `VGPaint`, `VGPath`, `VGFont`和`VGMaskLayer`。这些对象自己的参数可以使用`vgSetParameter`和`vgGetParameter`函数来设置和查询。这些函数的原型(包括对`count`参数无效值的处理)和`vgGet`和`vgSet`函数非常相似。

***`vgSetParameter`***

`vgSetParameter`函数在给定的基于`VGHandle`的对象上设置参数的值。
```
void vgSetParameterf (VGHandle object, VGint paramType, VGfloat value)
void vgSetParameteri (VGHandle object, VGint paramType, VGint value)
void vgSetParameterfv(VGHandle object, VGint paramType, VGint count, const VGfloat * values)
void vgSetParameteriv(VGHandle object, VGint paramType, VGint count, const VGint * values)
```
> **`ERRORS`**
>
> `VG_BAD_HANDLE_ERROR`
>  - 如果`object`不是一个有效的句柄，或者没有与当前环境共享。
> 
> `VG_ILLEGAL_ARGUMENT_ERROR`
> 
> - 如果`paramType`不是来自相应枚举的有效值。
> - 如果在`vgSetParameterf`或`vgSetParameteri`函数中`paramType`参数引用了需要矢量数据的参数类型
> - 如果在`vgSetParameterfv`或`vgSetParameteriv`函数中`paramType`参数引用了需要标量数据的参数类型且`count`参数传入值不等于1
> - 如果`value`的传入值不是`vgSetParameterf`或`vgSetParameteri`中给定参数类型的合法值，
> - 如果`values[i]`的传入值不是`vgSetParameterfv`或`vgSetParameteriv`中给定参数类型的合法值(`0 ≤ i < count`)
> - 如果`vgSetParameterfv`或`vgSetParameteriv`函数中`value`的传入值是`NULL`但是`count`却是一个大于0 的值
> - 如果`vgSetParameterfv`或`vgSetParameteriv`函数中`value`的传入值没有正确的对齐
> - 如果`vgSetParameterfv`或`vgSetParameteriv`函数中`count`的传入值小于0
> - 如果`count`的传入值不是给定参数类型的有效值


***`vgGetParameter` 和 `vgGetParameterVectorSize`***

`vgGetParameter`函数返回基于`VGHandle`的对象上对应`paramType`的值。

`vgGetParameteriv`或`vgGetParameterfv`函数获取的相应参数类型的数据有一个`最大长度`，该`最大长度`可通过`vgGetParameterVectorSize`函数获取。对于标量数据，该函数返回`1`。如果`vgGetParameteriv`或`vgGetParameterfv`函数调用时`count`传入了一个小于`最大长度`的值，则函数按照传入的`count`值返回相应的元素个数;如果`count`传入了一个大于`最大长度`的值，将触发一个错误。

不管内部的实现方式怎样，`vgSetParameter`函数(除非特别注明，这里的`vgSetParameter`函数都认为是已经没有错误的完整执行)传入的原始值都可以通过`vgGetParameter`函数返回，该规则确保`OpenVG`状态可以完整的保存及恢复。

如果在调用`vgGetParameterf`,`vgGetParameteri`,或`vgGetParameterVectorSize`函数时发生了错误，那么返回值是无效的；如果在调用`vgGetParameterfv`或`vgGetParameteriv]`函数时发生错误，那么传出参数`value`不会被写入任何值。
```
VGfloat vgGetParameterf (VGHandle object, VGint paramType)
VGint vgGetParameteri (VGHandle object, VGint paramType)
VGint vgGetParameterVectorSize (VGHandle object, VGint paramType)
void vgGetParameterfv(VGHandle object, VGint paramType, VGint count, VGfloat * values)
void vgGetParameteriv(VGHandle object, VGint paramType, VGint count, VGint * values)
```
> **`ERRORS`**
>
> `VG_BAD_HANDLE_ERROR`
>
> - 如果`object`不是一个有效的句柄，或者没有与当前环境共享。
> 
> `VG_ILLEGAL_ARGUMENT_ERROR`
>
> - 如果参数类型不是`VGParamType`枚举变量已列出的类型
> - 如果在`vgGettParameterf`或`vgGettParameteri`函数中引用了需要矢量数据的参数类型
> - 如果在`vgGettParameterf`或`vgGettParameteri`函数中的`value`参数值传入了`NULL`
> - 如果`vgGettParameterfv`或`vgGettParameteriv`函数中`value`的传入值没有正确的对齐
> - 如果`vgGettParameterfv`或`vgGettParameteriv`函数中`count`的传入值小于0
> - 如果`vgGettParameterfv`或`vgGettParameteriv`函数中`count`的传入值大于`vgGettParameterVectorSize`函数的相应参数类型的返回值
