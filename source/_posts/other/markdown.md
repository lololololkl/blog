---
tags: [Markdown]
categories: 
- other
- Markdown
title: 'Markdown 快捷键和语法'
date: 2020-03-20 14:40:00
created: '2021-01-22T08:44:24.800Z'
modified: '2021-05-13T01:25:43.323Z'
---

### key bindings
| ShortKeys | Description |
|-------------|---------------|
| Ctrl+Alt+V |在选定的文本上创建或粘贴剪贴板的内容作为内联链接。
| Ctrl+Alt+R | 创建或粘贴剪贴板的内容作为引用链接。
| Shift+Win+K | 创建或将剪贴板的内容作为内联图像粘贴到选定的文本上。
| Alt+B Alt+I | 这些必须是粗体和斜体。有选择和没有选择都适用。如果没有选择，他们只会转换光标下的单词。
| Ctrl+1…6 | 这些将为标题添加相应数量的哈希标记。与上述标题工具一起处理空行和选定文本。如果您选择一个完整的现有标题，当前的哈希标记将被删除并替换为您请求的标题级别。这个命令尊重mde.match\头\哈希首选项设置。
| Alt+Shift+6 | 插入脚注。
| Shift+Tab | 折叠/展开当前节。
| Ctrl+Shift+Tab | 将所有章节折叠到某一级别的标题下。
| Ctrl+Alt+Shift+PageUp Ctrl+Alt+Shift+PageDown | 转到相同或更高级别的上一个/下一个标题
| Ctrl+Shift+PageUp Ctrl+Shift+PageDown | 转到上一个/下一个标题
| Ctrl+Shift+H | 打开主页
| Ctrl+Shift+D | 在光标下打开wiki页面
| Ctrl+Shift+J | 打开今天的日记页
| Ctrl+Shift+B | 列出反向链接 |
<!-- more -->

### Topic
* 分割线：
 `---`
* 代码块：
```java
int i=0;
```
* 插入链接：
`[链接文字](链接网址 "标题")`
如：[百度](http://www.baidu.com "ABC")
* 插入图片：
`[文字](路径 "标签)`
[图片](C:\Users\Administer\Pictures\11_Moment.jpg "标签")
    - 直接显示图片
![图片](C:\Users\Administer\Pictures\11_Moment.jpg)
<img src="C:\Users\Administer\Pictures\11_Moment.jpg" width="256", height="150">
* 换行：
`<br>`
<br>
这是下一行
* 首行缩进：
`&emsp;&emsp;`
<br>&emsp;&emsp; 两个全角的空格（用的比较多）<br>
`&emsp;`&emsp;  表示一个全角的空格&ensp; or &#8194; 表示一个半角的空格<br>
