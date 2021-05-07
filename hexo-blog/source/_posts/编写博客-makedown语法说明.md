title: 编写博客--makedown语法说明
abbrlink: ed6b07f3
date: 2021-04-26 13:41:45
---
# 语法总览

## 正文语法

	``` bash
	> 1.=== title ===	最顶级标题
	> 2.---  title --- 二级标题
	> 3.# title/## title/###### title	一级/二级/六级标题
	> * tab缩进语法
	> + 字体加粗使用** text ** 或__text__
	> - 排序列表，无序列表使用（+-*），有序列表使用（1.数字加点）
	> - 换行，如果上下行无\n(换行操作)则不换行，如果有则换行
	> - 空行，如果上下行之间有空行，则会进行换行
	```
	
## 资源语法

```bash
链接：

> - [链接](baidu.com "百度这可省略，添加的属性值")
> + [链接2][1]...[链接3][id]
	[1]: baidu.com "属性值1可省略"
	[id]: zhihu.com "属性值2可省略" 

图片：

> - ![alt text](/path/.jpg "可省略，添加的属性值")
> + ![alt text][1]...![alt text][id可任意和下文保持一致即可]
	[1]: /path/.jpg "属性值1可省略"
	[id]: /path/.jpg "属性值2可省略" 

代码块：

> 1. `中间是代码块`
> 2. ```bash 中间是代码块 ```
> 3. 四个空格/一个tab操作后面的代码111
> 4.<!-- more --> 缩写/不展示全文
```

[参考](https://www.appinn.com/markdown/basic.html)
[参考2][1]

[1]: https://www.appinn.com/markdown/basic.html