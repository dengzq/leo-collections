
# CMake / make / ndk

### 1.CMake / make / ndk 是什么

在理解构建工具前，首先需要理解什么是动态库与静态库。动态库与静态库都是一种能将原生代码（C/C++代码）集成到目标 app 的一种方式。对于 Android 平台，动态库以 `.so` 的方式呈现，静态库以 `.a` 的方式呈现。它们的主要区别在于：

* 静态库需要预先编译到应用内部，动态可以以动态下载的方式加载使用
* 静态库 `.a` 文件体积相较于动态库 `.so` 而言更大


对于 `C/C++`源代码，可以使用 `gcc`编译器将原生代码编译成动态库或静态库。但当程序包含多个源文件时，直接使用 gcc 进行产物编译就会特别麻烦。因此我们选择使用构建工具对 C/C++ 源代码进行批量处理。

* CMake：跨平台的外部构建工具，通过CmakeLists.txt 构建文件的定义生成 makefile 文件
* make: 批量处理工具，通过 makefile 文件中用户指定的命令对 c 源文件进行编译和链接
* NDK: Android NDK 是一组能将原生代码（c代码）嵌入到 Android 应用中的开发套件

![构建过程](https://github.com/dengzq/leo-collections/blob/main/doc/img/img2.png?raw=true)


### 2.ndk 使用 Cmake / make 

1) 通过 gradle 打包动态库并集成到 apk

1.  as 打开`sdk manager`配置面板，下载 `ndk` 和 `cmake`
2. 创建原生代码文件 `.h` 和 `.cpp` 文件，编写原生代码
3. cpp文件夹下创建 `CMakeLists.txt` 文件，编写默认配置
4. 配置项目app下的 `build.gradle` 文件

**ps: 每个版本的 AGP 都有一个对应的 ndk 版本，如果 ndk 版本和 CMake 版本不一致，也会有 .so 产物打包失败的问题。这个时候需要通过配置 ndkVersion 和 cmake version 使其能在同一版本环境下工作**
![](https://github.com/dengzq/leo-collections/blob/main/doc/img/img1.png?raw=true)

##### 1.使用 CMake / make 命令构建产物
手动使用CMake 和 make 命令会比使用 gradle 构建脚本更加灵活，大多数情况下我们的共享库不仅只针对 Android，同时也会需要构建其他平台的产物。

构建前先熟悉两个关键的 CMake 构建参数：

* `CMAKE_TOOLCHAIN_FILE` ：指定`CMAKE_TOOLCHAIN_FILE `工具链路径
* `-DANDROID_ABI ` : 指定 `ANDROID_ABI ` cpu架构 

以 Android 平台为例，cd 到目标工程的 cpp目录，比如 cd 到 `/src/main/cpp`.

1) 执行 CMake -D 命令并拼接 ndk 及架构参数:

```
cmake -DCMAKE_TOOLCHAIN_FILE=/Users/dengzq/Library/Android/sdk/ndk/25.2.9519653/build/cmake/android.toolchain.cmake -DANDROID_ABI=armeabi-v7a
```

此时经过 `CMakeLists.txt` 配置脚本，CMake 自动生成 makefile 

2) 在当前文件下执行make命令 `make`，会根据 CMake 生成的 makefile 脚本规则，打出对应的原生库产物.

![](https://github.com/dengzq/leo-collections/blob/main/doc/img/img3.png?raw=true)

