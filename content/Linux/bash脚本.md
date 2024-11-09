---
tags:
  - IT/Linux
---

> [[Shebang符号]]


### here-doc

```shell
[n]<<word
	here-document
delimiter

## 其中可选的n比哦啊是文件描述符编号。如果省略，则指的是标准输入（文件描述符0）
```
- `cat > file <<EOF`
- `cat <<EOF  > file`
	（下接多行字符串）


### shell执行

**2种方式**
1. `.`：source。**继承当前shell（父进程）的环境变量**。（不新建子进程）
2. `./` 或 `sh`：**新建子进程执行shell**。于是不继承当前shell（父进程）的环境变量（除非用export命令设置环境变量）

- source和sh/bash执行不需要x权限，只需要读取权限。因为它们不是执行脚本，而是从脚本文件中读取命令，然后在当前shell环境中逐行执行。
- 而`./`执行需要x权限，且脚本要写shebang语句。因为linux默认不会运行文件。

> sh方法执行的话，新建子进程，执行完后子进程也会退出。所以比如子进程里有移动目录等操作，也仅仅在子进程里执行，当回到父进程后，父进程的当前目录并不受子进程的影响。

- 子进程与父进程唯一不同的地方在于pid


### 后台运行

`nohup command > log文件(默认nohup.out，追加模式) 2>&1 &`
- nohup：no hang up
- &，放在>后表示文件描述符（`2>&1`，`&1`指文件描述符1）
- `&>file`是特殊用法，与 `>&file` 一样：都为 `>file 2>&1`。

![[Pasted image 20240310224905.png]]


## set

-e：exit不是0的话报错中途退出（不设置的话默认跑完整个脚本）
+e：设置了-e后中途想希望无视err的话就用这个。之后再设置set -e即可
-u：有未定义变量时中断
-x：输出每次执行的命令。其输出两个加号表示子进程。

## trap 

最后必执行，相当于python的`Finally`

> [https://webbibouroku.com/Blog/Article/shell-try-catch-finally](https://webbibouroku.com/Blog/Article/shell-try-catch-finally)





## 逻辑判断

- 比较整数需要用运算符：`-eq`等
- `==`、`!=`是比较字符串




## while

`while [[ 条件 ]]`    ## 不加条件就默认true

IFS：Internal Field Separator

```bash
while IFS=, read IP NAME BANDWIDTH
do
	echo $IP $NAME $BANDWIDTH
	sleep 0.2
done < $1
```


## test（与中括号等价）

中括号与双中括号： [https://serverfault.com/questions/52034/what-is-the-difference-between-double-and-single-square-brackets-in-bash/52050#52050](https://serverfault.com/questions/52034/what-is-the-difference-between-double-and-single-square-brackets-in-bash/52050#52050)

- `[[  ]]`里面的小括号不需要转义
- 仅用于字符/文件的比较，数字比较请用[数学计算](onenote:#linux命令&section-id={4CC5283B-ADC7-4A37-9BE7-6B45625639A4}&page-id={FFB70183-8310-4A23-BEA1-C36672005A75}&object-id={9E00EB48-60F9-F9E4-97C1-401ED728C273}&B&base-path=https://d.docs.live.net/d10d79d0adccf558/文档/我的笔记本/10.编程_运维/Linux.one)

`test`返回布尔值，以不在屏幕输出为特征。
用中括号的话里的东西两边必须要有空格！！如：`[ 一些条件 ]`

[https://shellscript.sunone.me/if_and_test.html](https://shellscript.sunone.me/if_and_test.html)

## 函数

返回值是rc，即0~255。省略的情况下返回最后一条命令的rc值。

**定义**
```bash
## 函数名后的括号建议必须
## "function"关键字可省略，但要统一
function 函数名() {
	命令
}
```

**调用**
- 函数名看作命令，直接使用“函数名”调用
- 传参也像用命令时，空格分开