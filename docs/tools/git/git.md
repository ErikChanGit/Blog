# Git 使用

## github 中使用 ssh
从去年开始，github 对于 https 方式下载的仓库已经不支持直接 commit、push 等操作，由于 https 方式简单快捷，对于大部分只是对 git 初步熟悉的用户来说是非常合适的，因为不需要做任何配置，只需要一个 github 账号，一个仓库链接就可以将开源项目的代码拉取下来，并且贡献代码等。但是由于安全原因等考虑，github 对 https 做了一些限制，直接导致部分用户用着用着，提交不了代码了。所以，我们需要开始学习 ssh 方式啦。

#### 生成密钥文件

打开 `gitbash`, 输入命令

```shell
ssh-keygen -t rsa -C "username"   // username为你git上的用户名
```

接下里命令行可能会返回以下内容

```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/username/.ssh/id_rsa):
```

我们直接回车，命令行继续提示

```
/Users/your username/.ssh/id_rsa already exists.Overwrite (y/n)?
```

输入 y，命令行继续提示

```
Enter passphrase(empty for no passphrase)
```

直接回车， 然后会显示一长串内容其中还有一些..o.. o oo .oS. 之类的代码，这说明SSH key就已经生成了。文件目录就是：username/.ssh/id_rsa.pub.

#### 在 github 添加 ssh 密钥

- 在系统找到 .ssh 目录，用记事本打开 `id_rsa.pub`, 并复制其中全部内容。

- 打开 https://github.com/, 登录自己的账户, 进入 Settings , 找到:
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d29aa4873ec4dbfb497ac7f78a736fa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARXJpa0NoYW4uaGs=,size_20,color_FFFFFF,t_70,g_se,x_16)
- 点击 `New SSH Key`, 新增一个 ssh，将刚才复制的内容粘贴到下图所示的 `key` 中, Title 可以随意写， 最后确认即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/fcb72f2de40944b19535331ff7a4b855.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARXJpa0NoYW4uaGs=,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 设置 gitbash
在 bash.exe 中输入

```shell
ssh -T git@github.com
```

然后会跳出一堆内容你只需输入 yes 回车就完事了，然后他会提示你成功了。

后续就可以正常使用 git 了。