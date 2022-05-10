# Vscode 命令与路径
## 原生命令

```js
// 打开目录， true 表示在新的窗口打开
vscode.commands.executeCommand("vscode.openFolder", vscode.Uri.file(url), true);

// 打开设置
vscode.commands.executeCommand("_workbench.action.openFolderSettings", folder.uri); // 当前工程的设置
vscode.commands.executeCommand("workbench.action.openWorkspaceSettings"); // 当前工作区的设置
vscode.commands.executeCommand("workbench.action.openGlobalSettings"); // 全局设置
vscode.commands.executeCommand("workbench.action.openSettings", query); // query: 设置中查找的内容

// 打开 console
vscode.commands.executeCommand('workbench.action.output.toggleOutput');

// 隐藏 ActivityBar
vscode.commands.executeCommand("workbench.action.toggleActivityBarVisibility");

// 聚焦视图
vscode.commands.executeCommand("workbench.action.focusActivityBar");
vscode.commands.executeCommand("rt-smart_project_view.focus");
```

## 系统路径

```js
// 插件路径
vscode.extensions.getExtension(extensionId)?.extensionPath
```
