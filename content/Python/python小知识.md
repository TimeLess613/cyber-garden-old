---
tags:
  - IT/Python
---

## 布尔值

一般其他编程语言（JS、PHP、Ruby等）都是小写。
而python的算特殊，`True`和`False`首字母大写，可以理解为为了和内置变量`None`一直。



## `if __name__ == '__main__'`

- `__name__` ：内置属性，文件自己的模块名（即：xxx.py中的xxx）
- `__main__`：内置属性，当前被执行的模块名

即：如果现在这个文件的模块名就是当前被执行的，才执行下面缩进内的代码





## `__pycache__`文件夹

当模块被导入时，会在模块的位置创建`__pycache__`文件夹，里面存储模块的`.pyc` 文件。

Python 解释器会将源代码（`.py` 文件）编译成字节码（`.pyc` 文件）。字节码是一种优化后的、更易于快速执行的代码表示形式。




## 原始字符串

原始字符串告诉读者不要将反斜杠解释为转义字符。
`raw_string = r"C:\User\file.txt"`



## any与all

- `any` 函数用于检测可迭代对象中是否至少有一个元素为真值
- `all` 函数用于检测可迭代对象中的所有元素是否都为真值


## 数据类

*python3.7+*

```python
from dataclasses import dataclass, field

@dataclass(frozen=True)
class APIRequest:
    url_path: str
    query_params: dict
    custom_headers: dict = field(default_factory=dict)
    body: list = field(default_factory=list)
    purpose: str = ''
    method: str = 'GET'  ## Default to GET method
```

* 用field是防止可变类型的默认值会在不同实例间串戏——因为默认值在类的所有实例之间共享。
* frozen=True： 这个参数会将数据类变为不可变的，即创建实例后，不能再修改实例的属性值。这对于创建不可变对象来说很有用，可以防止无意中的修改，提高代码的安全性和可维护性。
* 不指定默认值的话就必须传参！——所以统一习惯，有默认值的都指定上。

## pip 代理
- `pip install --proxy="http://user:password@server:port" packagename`
- 用CA bundle方法：`--cert=`。（**cert可能需要放在自己的用户目录下**）
- 没配置pip的path的话，会导致导包需要用`python -m pip install`


## [Magic comment](https://qiita.com/KEINOS/items/6efc1147b917d7811b5b)——脚本开头预定义的编码

```python
## coding: utf-8

## -*- coding: utf-8 -*-  （这种写法是为了兼容[Emacs](https://ja.wikipedia.org/wiki/Emacs)，不用这个文本编辑器的话可不写两边的东西）
```
* 标准格式：`coding[=:]\s*([-\w.]+)*`

> [codecs](https://docs.python.org/3/library/codecs.html#standard-encodings)

> [[Linux命令#Shebang符号]]



## `pip requirements.txt`

![[Pasted image 20240304000628.png]]


## 源码方式安装包

### 下载源码包

```python
wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tgz
tar -zxvf Python-3.5.2.tgz
cd Python-3.5.2/
```


### 安装配置、编译&安装

*这里出错要重来的话先运行“make clean”删除编译文件。*

`./configure --prefix=/usr/local/bin/Python3.5`
- 虽然默认安装在/usr/local/bin/，但是一个包会生成多个bin，指定一下总的安装目录好删除（即卸载）
- 如果报错“configure: error: no acceptable C compiler found in $PATH”，说明没有合适的编译器，运行：yum install make gcc gcc-c++
`make && make install`

*python3更新踩坑：[SSL](https://stackoverflow.com/questions/53543477/building-python-3-7-1-ssl-module-failed)：`./configure --with-ssl`*



## 包管理


### import理解

import是把文件系统里的文件/文件夹（导入到内存）变成python的模块/包（都是对象）的过程。

**从哪里导入**：环境变量PATH。可用下面代码查看：
```python
import sys
print(sys.path)  # 返回一个包含PATH的列表，其中第一个为main脚本所在的目录。
```

实际上是找到import目标文件然后在另一个单独的命名空间里运行目标文件——即建立模块，完成这个模块对象后，把它赋值给import时用的字符串以作为一个变量——所以我们之后就可以把import的模块当做变量使用了。

而包就是一种特殊的模块。所以导包过程和导模块类似：其实包里不一定要有__init__.py这个文件，不过有的话在导包时会【在单独的命名空间里运行】__init__.py这个文件——即建立模块，只不过这个模块是特殊的——即：包，所以包名后来也能当做变量使用。

关于import package.module的形式：由于导包时只会运行__init__.py这个文件，所以Python并不知道目标文件夹里有其他模块文件。而想要import包下面的特定模块，那就用这种形式。




### `pip` 和 `pipx` 的区别：

通常来说，命令行工具用推荐用 `pipx`，python模块用 `pip`。、

> 简而言之，`pip` 主要用于安装项目的依赖库，而 `pipx` 则用于安装可以独立运行的命令行工具和应用程序。

- **`pip`**：    
    - 是 Python 的包管理器，用于安装和管理 Python 包和依赖项。
    - 通常安装的是库和模块，这些库和模块被包含在项目的依赖中。
- **`pipx`**：    
    - 专门用于安装和管理独立的 Python 应用程序，而不是库。
    - 安装的应用程序在虚拟环境中运行，并且可以全局访问，避免了依赖冲突。


`pipx` 创建的虚拟环境实际上是基于 `venv` 实现的。每个通过 `pipx` 安装的程序都有自己的虚拟环境，这些虚拟环境通常存储在 `pipx` 的专用目录中。  
默认情况下，`pipx` 会在用户主目录下的 `.local/share/pipx/venvs` 目录中创建虚拟环境。这些虚拟环境的位置可以通过该命令查看：`pipx list`。


## 虚拟环境


### venv（只支持3.3以上）

生成虚拟环境，**其python版本与执行 `venv` 时python命令的版本一致**。

[https://docs.python.org/3/tutorial/venv.html](https://docs.python.org/3/tutorial/venv.html)

在当前目录中创建虚拟环境的文件夹：  
`python3 -m venv <name, 如: .venv>` (可以先一个项目文件夹然后执行——模仿VScode)

启动虚拟环境：

- Windows：`.\venv\Scripts\activate.bat`
- Linux：`source /bin/activate`

退出虚拟环境：`deactivate`


### virtualenv（python2、3）

可以直接指定 Python 版本创建虚拟环境。（感觉和 `venv` 没太大差别，前提都是要有多个python版本）

如果存在多个python解释器，可以选择指定一个Python解释器（比如python2.7），没有指定则由系统默认的解释器来搭建： 
`virtualenv [-p /usr/bin/python2.7] my_project_env`



## 多版本管理

### Windows可直接用miniconda


### pyenv


> 可以用 `pyenv install <version>` 来安装不同版本，  
> 用 `pyenv global <version>` 来设置全局默认的 Python 版本，或使用 `pyenv local <version>` 为项目指定特定版本。

`pyenv install --list`：查看所有可安装版本。
`pyenv versions`：查看所有可用的python版本。
`pyenv uninstall <版本号>`：卸载指定版本。



### 源码安装

```python
cd ~
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libsqlite3-dev libreadline-dev libffi-dev wget libbz2-dev
wget https://www.python.org/ftp/python/3.9.7/Python-3.9.7.tgz
tar -xf Python-3.9.7.tgz
cd Python-3.9.7

./configure --enable-optimizations
make -j$(nproc)
sudo make altinstall

python3.9 --version
```

- altinstall：如果您要替换安装在 /usr/bin/python 上的原始 Python 二进制文件，您可能会弄乱已安装操作系统包的兼容性和依赖性。最安全的方法是使用 altinstall 来防止替换默认的 Python 文件。
- `whereis python`查看各版本路径：当你安装一个自定义版本的 Python（如 Python 3.9），它通常**不会**自动添加到系统的环境变量中。因此，`whereis python` 命令可能无法找到新安装的 Python 3.9。

可选，如果需要链接到“python3”（链接名）：`sudo ln -s /usr/local/bin/python3.9 /usr/local/bin/python3`
如果不创建链接，可以直接加入PATH：`export PATH="/usr/local/bin/python3.9:$PATH"`

`source ~/.bashrc`

#### 卸载

![[Pasted image 20240304001917.png]]


## 关于 `(object)`

python2中不继承object的类叫经典类，继承object的类叫做新式类。


## 静态方法

`@staticmethod`

静态方法是类中的函数，但不需要实例。更像是一种名称空间。
事实上，在python引入静态方法之前，通常是在全局名称空间中创建函数。
我们可以在类外面写一个简单的方法来做这些，但是这样做就扩散了类代码的关系到类定义的外面，导致以后代码维护的困难。


## 装饰器

*就是用闭包来实现*

装饰器通常用于在不修改原始函数的情况下，添加额外的功能或逻辑，以实现代码的可重用性和可维护性。
装饰器的基本工作原理是将一个函数作为参数传递给另一个函数，并返回一个新的函数，该新函数通常包装了传递进来的原始函数。

**例：**  
![[Pasted image 20240304003000.png]]

**原理理解：**  
![[Pasted image 20240304003035.png]]

- 函数也是变量
- 将@下面的函数放入@装饰器内的函数里运行
- **@定义后，最后的第17行才是调用**
- **注意看下图第9行：装饰器就是一个语法糖。**
	![[Pasted image 20240304003112.png]]



## 闭包

一种高阶函数。

> 高阶函数
> 接收函数作为参数，或者将函数作为返回值的函数是高阶函数
> 当我们使用一个函数作为参数时，实际上是将指定代码传递进了目标涵数

![[Pasted image 20240304003406.png]]
* 防止中途被改

闭包（Closure）是一个函数对象，它在定义时捕获了其所在作用域的变量，并可以在其定义的作用域之外被调用。闭包允许函数访问并修改定义在其外部作用域中的变量，即使外部作用域已经执行完毕。闭包通常用于实现函数工厂、创建私有变量和实现柯里化等。

在函数工厂中，我们定义一个函数，它接受某些参数，并根据这些参数来生成并返回一个新的函数



## 函数工厂（Function Factory）

是一种编程模式。
在函数工厂中，我们定义一个函数，它接受某些参数，并根据这些参数来生成并返回一个新的函数。



## 回调函数（Callback Function）

是将一个函数作为参数传递给另一个函数，然后在特定的条件或事件发生时，这个参数函数会被调用执行。回调函数通常用于处理异步操作、事件处理或回调模式等场景。


## 数组本质

> 如果你已经有指针相关的知识的话，只需要一句话就可以解释了：方括号是一个指针运算的语法糖，是将里外两个值相加然后取值的简写。

`D[36]`，即数组D的初始内存地址+偏移36个元素后的地址，然后解引用操作`*(D+36)`以获取该地址的值。





















---

> [[廖雪峰官网]]





