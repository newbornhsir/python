<h1 id="script">5.1.Executing modules as script</h1>

模块可以包含可执行的声明以及函数。这些声明用来初始化模块。它们在模块被import之后第一时间执行。

每个模块有自己的private symbol table(我理解为私有的作用域)并作为在这个模块里面定义的函数的global symbol table。因此，模块的所有者可以使用全局变量而不用但心和其它模块发生冲突。

模块可以import其它模块。import声明放在模块内容最开始的地方（通常但不是强制规定）

eg: `import somemodule`

执行模块

`python module.py <arguments>`


<h1 id="search">5.2 module search path</h1>

sys.path列表中是模块查找的路径

<h1 id="compile"> compile python file</h1>

为了加快加载模块文件，python会缓存每个编译过的模块文件，生成__pycache__目录。


python 包含许多标准库。内建的dir函数可以查看所给模块名称的定义，它返回一个有序列表








