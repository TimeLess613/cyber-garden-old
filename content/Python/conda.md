---
tags:
  - IT/Python
---

> [!NOTE] Anaconda太大了，[[python小知识#Windows可直接用miniconda]]。

> [!NOTE] [[python小知识#pyenv|pyenv]]更轻量。

## 安装

### Windows

![[Pasted image 20240304162600.png]]
* 多python版本的环境中自动加入PATH会变奇怪，推荐不加

#### 配置PATH/环境变量

- `E:\Anaconda`（必须。Python需要） 
- `E:\Anaconda\Scripts`（必须。conda自带脚本） 
- `E:\Anaconda\Library\bin`（必须。jupyter notebook动态库） 
- `E:\Anaconda\Library\mingw-w64\bin`（重要。使用C with python的时候，编译用） 
- `E:\Anaconda\Library\usr\bin`（？）

## 一般命令

`conda --version`
`conda env list`：列出所有环境
`conda info`：列出当前环境信息
	![[Pasted image 20240304162909.png]]

## 配置

`conda config --show`

### 配置代理

```python
conda config --set proxy_servers.http http://id:pw@address:port
conda config --set ssl_verify /path/to/your/certificate.pem
```
* 默认：`ssl_verify: True`
* 证明书可能需要放在自己的用户目录下。

## 查看环境下安装的包

`conda list`：查看通过conda方式安装的包
【对比】`pip list`：查看通过pip方式安装的包【自己物理环境和conda虚拟机环境的pip是分开的】



## 安装包

优先：`conda install <package>`
没有的话：`pip install <package>`

## 创建 虚拟环境

`conda create -n py38  python=3.8`【最好只指定py，其他包进环境后安装】
- 不指定python就好像是一个空环境？

## 激活/切换 指定环境

`activate py38`
* `conda activate py38`报错，`activate py38`可
* conda4.4之前的版本是：`source activate <envName>`

## 退出 当前环境

`conda deactivate`


## 删除环境

`conda remove -n py38 --all`


## VScode结合conda

1. 各种安装：conda、VScode（及其python扩展）
2. 创建Anaconda环境
3. VScode中选择 Anaconda 环境作为 Python 解释器
	1. `Ctrl+Shift+P`
	2. 输入：`Python: Select Interpreter`


