## Electron+Java Demo 分析

Demo： https://github.com/cuba-labs/java-electron-tutorial

#### 环境

- Gradle

- Electron

- JDK 11

- Node JS
- Vaddin ：可以用来构建复杂的 UI 的 Java框架，不用一行 html 或者 JS 代码。Vaddin 中的组件在 Java 中的写法与 SWT 类似，不同的是，Vaddin 的组件可以在浏览器中被渲染，在桌面程序中的浏览效果也和在浏览器中一样
- Jetty：轻量级servlet容器， 可以迅速为一些独立运行的Java应用提供网络和web连接。

注： gradle下载很慢，需要开代理

#### 框架

![image](/uploads/87864798e8c315b563cc29db033f731b/image.png)

- Electron 模块
  - Electron由三个主要支柱组成：
     - Chromium 用于显示网页内容。
     - Node.js 用于本地文件系统和操作系统。
     - 自定义 APIs 用于使用经常需要的 OS 本机函数。

   在此Demo中相当于一个框架和容器，将本地服务加载到Electron自带的浏览器中
  - packages.json：设置 IDE 名称、版本、JS 入口
  - main.js：控制整个IDE的生命周期，将 UI 嵌入到 桌面程序框架中，通过 loadURL的方式将本地起的服务load到electron中
![image](/uploads/dbdfe57e833ca2b9194cee74c19fc5c8/image.png)
- Java 模块

    前后端代码都用 java 来编写，通过 jetty 挂接到 server 中
  - 前端UI : 通过 vaadin 框架描绘界面
  - 服务连接器：通过 vaadin 中的注解 @WebServlet来指定一个服务连接器，在此服务连接器中指定 UI 的主入口，即可将 UI 注入到服务中
  - 开启服务：jetty 中的 ServletContextHandler 可设置服务连接器，在 Server 中可设置端口号和 ServletContextHandler ，直接 Server  start 即可开启服务，此时，可以在浏览器中预览 UI
  - 在 electron 的 main.js 中指定开启服务的路径，即可在 桌面程序中 浏览UI，和浏览器中的效果一样

#### 在 studio 中的 Web 化方案

可与原方案 #794 比较，原方案中，eclipse 自带的浏览器版本很久不升级，之后也不知会不会升级

要做到前后端分离，不完全采用上面的方式

- 前端: Electron （自带Chromium，版本升级快） + JS (调用本地服务接口) + HTML +CSS
  - 前端 ajax 发送接收数据
    ![image](/uploads/ec19f924fab8fb3254328b297ece32c9/image.png)
- 后端: Java + Server (为前端提供本地服务接口)，不使用 vaddin 的 UI
  - 后台开启本地服务示例：
    ![image](/uploads/75a5e3aa3d12cea155435beacb1f2e22/image.png)
    ![image](/uploads/d49ad7cb28a75d838dcb19a1611714db/image.png)
- 问题
  - 可以直接平面化地嵌入到 studio 中
    OS.SetParent(hwnd, composite.handle); // hwnd为程序的窗口句柄，composite为父容器
    
  - eclctron打包后的体积 ： 
    
    -  大于120 M
    
  - 配置后的保存到工程的方式
     - Studio中 Ctrl + S 或者点击保存按钮
     - 前端加一个【保存】按钮，通知后端
     
  - 如何支持同时打开多个工程的 electron 
  
     - 将 electron 平面化地嵌入到 MultiPageEditorPart中
  
     - 每个 electron 都通过不同的端口开启服务，要做生成随机的可用端口号
  
     - 假设开启了 n 个 electron ，则需要在后台开启 n 个本地 Server，electron时，也需要关闭对应端口的本地服务
  
     - 如何保持前后端 端口号 一致？
  
       - 后端随机生成可用端口号 port
  
       - 后端调用 electronAPP.exe  arg1  arg2... 开启 electron，发送端口号，其中  electronAPP.exe 为  启动的可执行程序
  
       - 前端获取端口号
  
         ```
         process.argv.forEach(function (val, index, array) {
         
             console.log(val); // val 为所有从 exe 传过来的参数
         
          });
         ```
  
     - 每个 Server 处理相对应的 electron 的请求，完全保持独立
  
     - 怎么确定每个 electron 配置后的保存操作不会互相影响？  由于每个 electron 都独立地嵌套在各自的 MultiPageEditorPart 中，有自己的 doSave 操作，不会互相影响
  
  - electron 在studio的存放形式？ 
    
    - 以工具的形式
    
  - 更新
     调用update.exe
    ![image](/uploads/1668227c837c337cc4bcf54f0bc7a92a/image.png)