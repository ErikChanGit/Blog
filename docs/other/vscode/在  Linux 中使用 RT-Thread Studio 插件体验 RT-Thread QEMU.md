## 在  Linux 中使用 RT-Thread Studio 插件体验 RT-Thread QEMU

分享如何在  Linux 中通过 Vscode 使用 RT-Thread Studio 插件使用 QEMU 模拟器体验 RT-Thread

### 准备

- 在 linux 中安装 Vscode

- 安装 QEMU:

  ```
  sudo apt-get install qemu-system-arm
  ```

- 安装工具链：

  ```
  wget https://armkeil.raw.core.windows.net/developer/Files/downloads/gnu-rm/6-2016q4/gcc-arm-none-eabi-6_2-2016q4-20161216-linux.tar.bz2
  ```

  ```
  tar xf ./gcc-arm-none-eabi-6_2-2016q4-20161216-linux.tar.bz2
  ```

- 安装 ncurses 库

  ```
  sudo apt-get install libncurses5
  ```

### 快速演示

![vscode_linux_qemu](https://oss-club.rt-thread.org/uploads/20210623/10044a957805443f6b5353976a854c38.gif)

### 打开工程

RT-Thread Studio 插件目前还没有支持创建工程

我们可以先打开一个现有的工程，可以使 RT-Thread bsp 工程或者由 Eclipse 版本 RT-Thread Studio 创建的工程。通过`工具`视图中的 `打开工程`  或者 `添加工程到工作区` 打开此工程

请打开一个 QEMU 模拟器支持的工程， 如果不知道你的 QEMU 支持哪些模拟器，可以通过命令 `qemu-system-arm -M help` 获取可用模拟器列表

![image-20210630164218173](C:\Users\rt-thread\AppData\Roaming\Typora\typora-user-images\image-20210630164218173.png)

### 编译

在 RT-Thread 工程视图中，鼠标聚焦到工程目录上，点击第一个 `编译` 按钮， 自动启动 scons 编译

编译之前，可能会提示需要配置工具链或者其他路径，按照实际情况配置即可

![image-20210630164606995](C:\Users\rt-thread\AppData\Roaming\Typora\typora-user-images\image-20210630164606995.png)

### 调试

点击第三个 `调试` 按钮， 插件会自动生成调试配置文件并启动调试

调试时，在视图左边，可以查看变量、调用栈等信息

全速运行后，可在模拟器终端中体验运行结果

![image-20210630164817136](C:\Users\rt-thread\AppData\Roaming\Typora\typora-user-images\image-20210630164817136.png)