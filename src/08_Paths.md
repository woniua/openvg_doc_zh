# 8 路径

## 8.6 路径操作 <span id = "路径操作"></span>
### 8.6.1 路径存储
### 8.6.2 路径的销毁及创建
函数`vgCreatePath`和`vgDestroyPath`分别用于路径的创建和销毁。在路径的整个生命周期内，应用程序可以通过定义枚举变量`VGPathCapabilities`指定哪条路径应该被执行。

**VGPathCapabilities** 
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

**vgCreatePath**

todo

**vgClearPath**

todo

**vgDestroyPath**

todo

### 8.6.3 路径查询
### 8.6.4 路径性能的查询及修改
### 8.6.5 路径间的数据复制
### 8.6.6 向路径追加数据
### 8.6.7 修改路径数据
### 8.6.8 转化路径
### 8.6.9 在路径之间做插值
### 8.6.10 路径长度
### 8.6.11 位置及路径切线
### 8.6.12 查询路径的包围框

