# 15 扩展 API
扩展可以通过`预处理器`静态检测，也可以通过`vgGetString`函数动态检测。 扩展函数可以用预编译指令的方式静态的直接包含在应用程序中，也可以通过`eglGetProcAddress`返回函数指针或其他平台定义的方式动态访问。

## 15.1 扩展名命名约定 <span id = "扩展名命名约定"></span>

## 15.2 扩展注册 <span id = "扩展注册"></span>

## 15.3 使用扩展 <span id = "使用扩展"></span>

### 15.3.1 静态访问扩展
todo
### 15.3.2 动态访问扩展
`OpenVG`包含了一种机制，用于应用访问`运行平台`的信息，也可以用于应用访问应用编译时未呈现的扩展信息。

 **VGStringID**
 ```
 typedef enum {
    VG_VENDOR     = 0x2300,
    VG_RENDERER   = 0x2301,
    VG_VERSION    = 0x2302,
    VG_EXTENSIONS = 0x2303
} VGStringID;
 ```
 **vgGetString**

`vgGetString`函数返回`OpenVG`的实现信息，包括扩展信息。返回值可能因当前关联的显示上下文的不同而不同，如果当前没有关系的上下文，则`vgGetString`返回`NULL`。
