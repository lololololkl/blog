---
tags: [Web, CSS]
categories: 
- Web
- CSS
title: CSS基础知识
date: 2021-09-12 20:03:02
---

* html: 文档元数据、内容区域标签、文本内容标签、内联文本语义标签、图片与多媒体标签、表格内容标签、表单标签

* css: 基本语法、块级/内联元素、图片样式、背景图样式、盒子模型、边框与背景、字体样式、CSS嵌入方式、CSS优先级、定位、浮动、基础选择器、分组选择器、组合选择器、伪类选择器、溢出属性、颜色值标注、em/rem/px单位

## CSS
### 块级/内联元素
https://blog.csdn.net/xuanfuhuo4769/article/details/81326457

| 块级元素 | 内联元素
| ------------- | ----------------- |
| 独占一行,默认情况下，其宽度自动填满其父元素宽度 | 相邻的行内元素会排列在同一行里，直到一行排不下，才会换行，其宽度随元素的内容而变化 |
| 可以设置width，height属性 | 行内元素设置width，height属性无效|
| 可以设置margin和padding属性 | 行内元素起边距作用的只有margin-left、margin-right、padding-left、padding-right，其它属性不会起边距效果。|
| 对应于display:block | 对应于display:inline；|

<!-- more -->
### 图片样式
https://cloud.tencent.com/developer/article/1735108
* 一、图片大小
    * width
    * height

* 二、图片边框
    * border

* 三、图片对齐
    * 在父类使用：text-align、vertical-align

* 四、文字环绕（float）
    * float: left | right



### 背景图样式
https://blog.csdn.net/qq_48192192/article/details/119803230
* background-color

* background-image: none | url("") | linear-gradient(color1, color2)

* background-repeat: repeat | no-repeat | repeat-x | repeat-y

* background-position:  

* background-size 
* background-attachment 背景图像固定
* background-clip ⽤于设定背景裁切的⽅式
* background-origin ⽤于设定背景铺设的起点
* background 背景属性复合写法<br>
&nbsp;单独的属性需要写在复合属性的下方，否则复合属性会覆盖上方的单独属性。
    * 设置背景色半透明
    <br>background：rgba (r, g ,b , 透明度) 
### 盒子模型
* Margin(外边距) - 清除边框外的区域，外边距是透明的。
* Border(边框) - 围绕在内边距和内容外的边框。
* Padding(内边距) - 清除内容周围的区域，内边距是透明的。
* Content(内容) - width和height。 盒子的内容，显示文本和图像。

### 边框与背景
* 边框
    * border-style
    * border-width
    * border-color	
    * border-radius
    * box-shadow
    * border-image

### 字体样式
* font-family 字体类型
* font-size 字体大小
* color 字体颜色
* font-weight 字体粗细
* font-style 字体风格
* text-decoration 下划线
* font-variant 字体大小写

### CSS优先级
https://www.cnblogs.com/zxjwlh/p/6213239.html
* 在属性后面使用 !important 会覆盖页面内任何位置定义的元素样式。
* 作为style属性写在元素内的样式
* id选择器
* 类选择器
* 标签选择器
* 通配符选择器
* 浏览器自定义或继承

（排序：!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性同一级别）

### 定位
* position
    * static
    * relative
    * fixed
    * absolute
    * sticky

### 浮动
* float 
    * right
    * left 
    * none
    * inherit

### 基础选择器
基础选择器 | 作用
---|-----
标签选择器 | 可以选出所有相同的标签，比如：p
类选择器|可以选出 1 个或者 多个 标签
id 选择器|一次只能选择 1 个标签
通配符选择器|选择所有的标签


### 分组选择器
    ```css
    h1, h2, h3, h4, h5, h6 {
        color:blue;
    }
    ```
    ```css
    .class1{
        color:red;
        }
    .class2{
        color:blue;
        }
        /*共同样式*/
    .class1, class2{
        font-size:13px;
        text-decoration:underline;
    }
    ```

### 组合选择器
* A,B 多个元素选择
    ```css
    div, p { color:red; }
    ```
* A B 后代元素选择器，匹配所有属于A元素后代的B元素，A和B之间用空格分隔
    ```css
    #fuji .ziji{
        padding:10px;
        background-color:#ffffff;
        border:1px #000000 solid;
        color:red;
    }
    ```
* A > B 子元素选择器，匹配所有A元素的子元素B
    ```css
    #zys>span {
        padding:10px;
        background-color:#ffffff;
        border:1px #000000 solid;
        color:red;
    }
    ```
* A + B 毗邻元素选择器，匹配所有紧随A元素之后的同级元素B
    ```css
    #pl p+p{
        padding:10px;
        background-color:#ffffff;
        border:1px #000000 solid;
        color:red;
    }
    ```

### 伪类
静态伪类和动态伪类，伪类选择器分为两种。

* （1）静态伪类：只能用于超链接的样式。如下：

    * :link 超链接点击之前
    * :visited 链接被访问过之后
    
    PS：以上两种样式，只能用于超链接。

* （2）动态伪类：针对所有标签都适用的样式。如下：

    * :hover “悬停”：鼠标放到标签上的时候
    * :active “激活”： 鼠标点击标签，但是不松手时。
    * :focus 是某个标签获得焦点时的样式（比如某个输入框获得焦点）

### 溢出属性
* overflow 
    * visible - 默认。溢出没有被剪裁。内容在元素框外渲染
    * hidden - 溢出被剪裁，其余内容将不可见
    * scroll - 溢出被剪裁，同时添加滚动条以查看其余内容
    * auto - 与 scroll 类似，但仅在必要时添加滚动条
    
### 颜色值标注
https://www.w3school.com.cn/cssref/css_colors_legal.asp
* 十六进制颜色
* 带透明度的十六进制颜色
* RGB 颜色
* RGBA 颜色
* HSL 颜色
* HSLA 颜色
* 预定义/跨浏览器的颜色名称
* currentcolor 关键字

### em/rem/px单位
* em

    父元素的字体大小。em是相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。(1.3em—是其父嵌套字体大小的1.3倍。)

* rem

    根元素的字体大小。
* px

    px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。

