# Electron 使用手册                                                                                                   

#### 准备Electron环境

##### 安装 NodeJS

- 下载地址：https://nodejs.org/zh-cn/download/

- 安装完成后查询node和npm的版本，确认安装是否成功。

```
node -v
npm -v
```

如果上述命令均打印出一个版本号，就说明Node.js已经安装好了！

##### 注册 npm 镜像

- 由于 npm 命令在下载模块时速度很慢，可以利用淘宝镜像注册 cnpm 指令，加快下载的速度

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

##### 安装 Electron

- 国外站点速度太慢，直接使用cnpm来安装electron

```
cnpm install -g electron
```

#### 新建 Electron 配置项目

- 新建一个目录

```
mkdir Demo
```

- 进入目录

```
cd Demo
```

- 初始化项目，创建 package.json

```
npm init -y
```

- 添加 electron 依赖

```
cnpm install electron -S
```

- 打开项目 ( 要安装 VS Code 才能使用此命令打开 )

```
code .
```

- 修改 package.json

```json
{
  "name": "eledemo",
  "version": "1.0.0",
  "description": "This is an electron demo project.",
  "main": "main.js",
  "scripts": {
    "start": "electron ."
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "electron": "^8.2.5"
  }
}
```

- 新建 main.js 文件

```js
const electron = require('electron');
const {app, BrowserWindow} = require('electron');
const path = require('path');
const url = require('url');

let mainWindow;

function createWindow () {
    // Create the browser window.
    mainWindow = new BrowserWindow({
        width: 1024, 
        height: 640,
        transparent: false,
        frame: true,
        resizable : true //固定大小
    });

    const URL = url.format({
        pathname: path.join(__dirname, 'index.html'),
        protocol: 'file:',
        slashes: true
      })

    mainWindow.loadURL(URL);
    console.log(URL);
    mainWindow.openDevTools()

    mainWindow.on('closed', function () {
      mainWindow = null;
    });

}

app.on('ready', createWindow);

// Quit when all windows are closed.
app.on('window-all-closed', function () {
  // On OS X it is common for applications and their menu bar
  // to stay active until the user quits explicitly with Cmd + Q
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', function () {
  // On OS X it's common to re-create a window in the app when the
  // dock icon is clicked and there are no other windows open.
  if (mainWindow === null) {
    createWindow();
  }
});
```

- 新建 index.html 文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    hello , World.
</body>
</html>
```

- 运行项目

```
npm start
```

#### electron-packager 打包 Electron

- 安装全局打包模块 electron-packager

  ```
  cnpm install electron-packager -g
  ```

- 修改 package.json，在 scripts 中添加 package

  ```json
  "scripts": {
      "start": "electron .",
      "package": "electron-packager . 应用名称 --out 输出文件夹 --arch=x64 --overwrite"
    },
  ```

- 执行打包命令

  ```
  cnpm run package
  ```

打包结束后即可在根目录的输出文件夹下查看打包后的 electron 桌面程序，在打包后的目录下有个 .exe 文件，双击即可启动 IDE

### electron-builder 打包 Electron

- 安装全局打包模块 electron-builder

  ```
  npm install electron-builder --save-dev
  ```

- 修改 package.json，在 scripts 中添加 dist

  ```
  "dist": "electron-builder --win --x64",
  ```

- 执行打包命令

  ```
  npm run dist
  ```

#### Electron 装载网页的两种方式

- 直接载入 HTML

  ```js
  const URL = url.format({
       pathname: path.join(__dirname, 'index.html'), // index.html 为网页主入口
       protocol: 'file:',
       slashes: true
  })
  mainWindow.loadURL(URL);
  ```

- 载入 Service URL

  ```js
  let URL = 'http://localhost:1337'; // 此 url 可以为本地服务也可以是远程服务，例如 https://www.google.com/
  mainWindow.loadURL(URL);
  ```

#### 升级

"electron-builder": "^22.9.1",

"dist": "electron-builder --win --x64",

 npm run dist