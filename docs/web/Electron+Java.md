## Electron+Java

#### 环境

- Gradle

- Electron

- JDK 11

- Node JS
- Vaddin ：可以用来构建复杂的 UI 的 Java框架，不用一行 html 或者 JS 代码。Vaddin 中的组件在 Java 中的写法与 SWT 类似，不同的是，Vaddin 的组件可以在浏览器中被渲染，在桌面程序中的浏览效果也和在浏览器中一样
- Jetty：轻量级servlet容器， 可以迅速为一些独立运行的Java应用提供网络和web连接。

注： gradle下载很慢，需要开代理

#### 工程组织

![image-20201105121247399](C:\Users\rt-thread\AppData\Roaming\Typora\typora-user-images\image-20201105121247399.png)

- packages.json：设置 IDE 名称、版本、JS 入口

- main.js：控制整个IDE的生命周期，将 UI 嵌入到 桌面  程序框架中
- Java 部分
  - 前端UI : 通过 vaadin 框架描绘界面
  - 服务连接器：通过 vaadin 中的注解 @WebServlet来指定一个服务连接器，在此服务连接器中指定 UI 的主入口，即可将 UI 注入到服务中
  - 开启服务：jetty 中的 ServletContextHandler 可设置服务连接器，在 Server 中可设置端口号和 ServletContextHandler ，直接 Server  start 即可开启服务，此时，可以在浏览器中预览 UI
  - 在 electron 的 main.js 中指定开启服务的路径，即可在 桌面程序中 浏览UI，和浏览器中的效果一样