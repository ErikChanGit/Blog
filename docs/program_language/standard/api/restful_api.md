# Restful API 标准

## 安全
使用 https 传输, 保证数据安全

## 接口特征
接口特征表现，一看就知道是 api,   
https://api.baidu.com

## 版本共存
多数据版本共存: 保障老 app 能用
- https://api.baidu.com/v1
- https://api.baidu.com/v2

## 数据即资源
数据即资源，均使用名词  
https://api.baidu.com/books 而不是 		https://api.baidu.com/getAllbooks

## 请求方式
资源操作由请求方式决定 (method)
- get 方式, 获取
- post 方式, 新增
- put  修改
- patch 局部修改
- delete 删除

## 过滤
- limit=10， 限制数量
- offest， 偏移量
- page && per_page
- sortby && order= 
	
## 响应状态码
- 正常： 200，201
- 重定向： 300，301
- 客户端异常： 403:请求无权限，404：请求路径不存在，405：请求方法不存在
- 服务端异常： 500
	
## 错误处理
- 发生异常后，返回 error 原因
	
## 返回结果
- GET：数组
- POST： 返回新生成的资源， json 对象
- PUT： 返回完整的资源对象
- PATCH： 返回完整的资源对象
- DELETE： 返回一个空文档或者 error
	
## 需要 url 的可以带链接