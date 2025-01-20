---
tags:
  - IT/Linux
---

即脚本开头的：`#!`

[固定用`/usr/bin/~`下的！](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04s04.html#purpose20)

- `#!/usr/bin/bash`
- `#!/usr/bin/env python3`
- `# -*- coding:utf-8 -*-`

设置了Shebang符号就可以用`./`直接执行脚本，而不用每次都指定解释程序。
- python等推荐env的方式，env可以在系统的PATH目录中查找，防止用户没有将python装在默认的`/usr/bin/`里
- 但是从DOS环境直接复制到UNIX环境由于换行符不同，会报错找不到文件。
	- **解决**：[[换行符#转换与删除]]



![[Pasted image 20240304011212.png]]
