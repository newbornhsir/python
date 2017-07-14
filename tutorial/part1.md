# 1.使用python解释器

## 启动

 安装好python并配置好环境变量，在命令行模式下输入`python`,进入交互模式，输入`quit()`退出交互模式

 ## 传递参数

 当打开交互解释器的时候，可以传递脚本名称和附加参数，它们将会转变成字符串列表的形式传入。可以通过sys的sys.argv 获取传入参数

 ```
    henghongweideMacBook-Pro:~ henghongwei$ python3 -
	Python 3.6.1 (default, Apr  4 2017, 09:40:21) 
	[GCC 4.2.1 Compatible Apple LLVM 8.1.0 (clang-802.0.38)] on darwin
	Type "help", "copyright", "credits" or "license" for more information.
	>>> import sys
	>>> sys.argv
	['-']
	>>> 

```

### source code Encoding

默认情况下，python文件编码格式是utf-8，也可以指定编码格式,有以下两种书写格式：

- `#-*- coding:utf-8 -*-`
- `# coding:utf-8`
