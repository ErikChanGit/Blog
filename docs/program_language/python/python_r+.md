#### 问题背景
想用 python 实现文件的读取，并修改部分内容，再写回去。 r+ 是最符合的权限，可读写，并且可以覆盖文件之前的内容。
但是实际使用时， 发现修改后的内容是追加的方式，而不是覆盖。
```python
 with open(gitignore, "r+") as f:
      ignore_data = f.read()
      if "cconfig.h" in ignore_data:
          ignore_data = ignore_data.replace("cconfig.h", "")
          f.write(ignore_data)
```
#### 原因分析
因为在使用read后，文档的指针已经指向了文本最后，而write写入的时候是以指针为起始，因此就产生了追加的效果。

#### 解决方案
如果想要覆盖，需要先seek（0），然后使用truncate()清除后，即可实现重新覆盖写入。
```python
 with open(gitignore, "r+") as f:
      ignore_data = f.read()
      if "cconfig.h" in ignore_data:
          ignore_data = ignore_data.replace("cconfig.h", "")
          f.seek(0) # 指向文本开头
          f.truncate() # 清除文本
          f.write(ignore_data)
```