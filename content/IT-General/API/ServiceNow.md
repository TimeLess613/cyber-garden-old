---
tags:
  - IT/API
---
- ticket界面的description，换行符要用`<be>`。`\n`和`\\n`等都无效。
- 当传入list时，各个元素中的特殊值会被编码（如`<>`，似乎就是用Unicode）。而直接传入字符串的话则不会。

