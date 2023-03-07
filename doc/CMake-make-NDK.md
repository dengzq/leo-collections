
# CMake / make / ndk

### 1.CMake / make / ndk 是什么

在理解构建工具前，首先需要理解什么是动态库与静态库。动态库与静态库都是一种能将原生代码（C/C++代码）集成到目标 app 的一种方式。对于 Android 平台，动态库以 `.so` 的方式呈现，静态库以 `.a` 的方式呈现。它们的主要区别在于：

* 静态库需要预先编译到应用内部，动态可以以动态下载的方式加载使用
* 静态库 `.a` 文件体积相较于动态库 `.so` 而言更大


对于 `C/C++`源代码，可以使用 `gcc`编译器将原生代码编译成动态库或静态库。但当程序包含多个源文件时，直接使用 gcc 进行产物编译就会特别麻烦。因此我们选择使用构建工具对 C/C++ 源代码进行批量处理。

* CMake：跨平台的外部构建工具，通过CmakeLists.txt 构建文件的定义生成 makefile 文件
* make: 批量处理工具，通过 makefile 文件中用户指定的命令对 c 源文件进行编译和链接
* NDK: Android NDK 是一组能将原生代码（c代码）嵌入到 Android 应用中的开发套件

![构建过程](https://raw.githubusercontent.com/dengzq/leo-collections/main/doc/img/img2.png?token=GHSAT0AAAAAAB4RDCU2WT7DB7OUCUHWZIRKZAHMCGA)


### 2.ndk 使用 Cmake / make 

1) 通过 gradle 打包动态库并集成到 apk

![build.gradle 配置](https://raw.githubusercontent.com/dengzq/leo-collections/main/doc/img/img1.png)


#####1.使用cmake 手动打 makefile
cd 到目标工程的 cpp目录，比如 cd 到 `/src/main/cpp`， 执行以下命令:

`-DCMAKE_TOOLCHAIN_FILE` 是 cmake `-D` 命令，指定`CMAKE_TOOLCHAIN_FILE `工具链路径

`-DANDROID_ABI ` 是 cmake '-D' 命令，指定 `ANDROID_ABI ` cpu架构

```
cmake -DCMAKE_TOOLCHAIN_FILE=/Users/dengzq/Library/Android/sdk/ndk/25.2.9519653/build/cmake/android.toolchain.cmake -DANDROID_ABI=armeabi-v7a
```
