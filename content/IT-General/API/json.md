---
tags:
  - IT/API
  - IT/Python
---
python库：[[常用脚本与库#json]]


## json格式踩坑

### 引号

似乎（键）必须用“双引号”传递。 ^98d49a

```bash
## On Linux
--data '{"purge_everything": true}'

## On Windows
--data "{\"purge_everything\": true}"
```

> [!NOTE] 对于Windows
> 1. 将单引号替换为双引号
> 2. 用反斜杠转义双引号


### 布尔值

报错：Malformed JSON in request body.
解决：**json的布尔真值是小写true**


## jq命令

习惯：用单引号（如前文，[[json#^98d49a|似乎（键）必须用“双引号”传递]]）

用法：`...| jq -r '.'`


### 判断是否有元素

` | jq 'has("result")'`


### 获取数字key

![[Pasted image 20240309143104.png]]


### 删除output的双引号

`jq -r`，等价于`| jq … | tr -d '"'`



### json转csv

> [jq コマンドで JSON を CSV に変換する](https://medium.com/veltra-engineering/jq-supports-json-to-csv-fb5c951a9575)