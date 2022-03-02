## RT-Thread Settings Web化需求分析

#### 一、添加软件包

##### Studio与浏览器交互原理

```
BrowserFunction ： org.eclipse.swt.chromium.BrowserFunction   chrome内核浏览器

class CustomFunction extends BrowserFunction {
		CustomFunction(Browser browser, String name) {
			super(browser, name);
		}
		public Object function(Object[] arguments) {

		}	
}
```

- 如果在浏览器上JS调用了 name ,将会自动执行 function

##### 添加方式

- 软件包中心添加软件包
- 详细配置中添加软件包

##### 添加过程 （********）

- 前端发送数据到studio
- studio 在 function 中接受参数处理



####  二、常用组件和驱动 使能开关

- 组件和服务层
- 外设驱动
- 更多配置



#### 三、软件包详细配置

包括 内核、组件、软件包、硬件、示例、三方库的配置

##### 修改配置

- 监听树的改变

- 根据改变的情况发送 json 数据给后端

- 后端获取 json ，设置脏标记， 执行 json

  



#### 四、K2J与KCS交互

- [ ] K2J 和 KCS 能否在Web段使用，直接在 Web 端处理数据的变化

  kcs依赖于kconfiglib

##### 初始化 loadConfigMenus

- 通过kconfig文件生成 .rtmenus 文件：生成 run_congen.bat
  - 调用k2j生成.rtmenus :    k2j.exe --config ./.config --kconfig ./Kconfig --output json_menus ./.settings/.rtmenus
  - 删除 run_congen.bat

- 启动kcs.exe， 直到 Settings 关闭才销毁
- 创建配置界面
  - 读取 .rtmenus： read (obj, menuITem) 根据 jsonArray 构造 KConfigMenuItem 模型
  - 根据模型创建 UI 树 RTTConfigurationEditorTree
  - RTTConfigurationEditorDiagram 创建软件包界面
  - 搜索功能 KconfigSearchUi
  - 初始化各配置是否使能



##### 



#### 五、配置保存

在Studio中点击保存后，将RT-Thread Settings 中修改过的内容对工程重新配置

- 获取 kcs 的保存命令 并执行
- 脏标记设置为false
- 检查是否开启 C++ 功能
- 更新软件包