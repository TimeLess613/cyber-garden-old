---
tags:
  - IT/DevOps
---



- **仅做参考**：少用 `git pull`，用 `git pull --rebase`
	- 因为对有冲突的 `git pull` 会自动merge所以产生merge信息。未来搜索commit历史时不方便。
	- 配置默认使用rebase方式：`git config pull.rebase true`
	- 根本问题解决：不同人不共用branch。








---

## 渗透常用

#渗透/信息收集 

```bash
git log	
git show [commit id]	显示最近一次/指定的历史提交信息的完整信息

git log –p	输出每一个commit之间的差异信息
git log --all --graph --decorate: 	可视化历史记录（有向无环图）
git diff [filename]	显示工作区与暂存区的差异

git log -G<word>: 正则搜索commit中的某个词，如password。
git log --grep="search term"
```

