## Node 做 studio 浏览器后端分析

### 优势

- 方便移植到 Electron
- 统一前后端编程语言都为 JS
- 方便便开发边调试
- 减少插件代码量

### 缺点

- node 环境打包成 exe 后，至少有 40 M

### node 与前端通信

前端:

```js
  axios.get("http://localhost:8888/getStorage", {
          params: {
            'dialog_type':"folder",
          },
        })
        .then((response) => {
           obj.value = response.data['result'];
           this.inputChanged(obj);
        });
    },
```

node:

```js
app.get('/getStorage', function (req, res) {
  let type = req.query['dialog_type']
  res.set({"Access-Control-Allow-Origin": "*",})
  res.json({})
})
```

### node项目打包成 exe

```js
pkg index.js -o rtt_studio_server
```

