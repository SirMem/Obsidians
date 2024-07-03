---
Project: 
Status:
tags: Resources
Deadline:
CreateTime:
Connected:
---

### HTML基础知识

  

#### 1. HTML简介

  

**什么是HTML**

  

- HTML（HyperText Markup Language）是用于创建网页的标准标记语言。
- HTML描述了网页的结构，并通过标签（tags）来定义网页内容。

  

**HTML文档结构**

  

- 一个基本的HTML文档结构如下：
```html
  <!DOCTYPE html>
  <html>
  <head>
      <title>页面标题</title>
  </head>
  <body>
      <h1>这是一个标题</h1>
      <p>这是一个段落。</p>
  </body>
  </html>
```

  

#### 2. 基本标签

  

**文本标签**

  

- `<h1>` - `<h6>`：标题标签，`<h1>` 是最大的标题，`<h6>` 是最小的标题。

  

```html
  <h1>这是一个一级标题</h1>
  <h2>这是一个二级标题</h2>
```

  

- `<p>`：段落标签，用于定义一个段落。

  





```html
  <p>这是一个段落。</p>
```

  

- `<br>`：换行标签，用于在文本中插入换行。



```html
  这是第一行。<br>这是第二行。
```

  

- `<hr>`：水平线标签，用于在页面中插入一条水平线。

  



```html
  <hr>
```

  

**格式化标签**

  

- `<b>`：加粗文本。

  


```html
  <b>这是加粗的文本。</b>
```

  

- `<i>`：斜体文本。

  


```html
  <i>这是斜体的文本。</i>
```

  

- `<u>`：下划线文本。

  

```html
  <u>这是带下划线的文本。</u>
```

  

- `<strong>`：重要文本，通常显示为加粗。

  


```html
  <strong>这是重要的文本。</strong>
```

  

- `<em>`：强调文本，通常显示为斜体。

  


```html
  <em>这是强调的文本。</em>
```

  

**列表**

  

- 有序列表 `<ol>` 和列表项 `<li>`：有序列表中的每个项目都有一个编号。

  


```html
  <ol>
      <li>第一项</li>
      <li>第二项</li>
  </ol>
```

  

- 无序列表 `<ul>` 和列表项 `<li>`：无序列表中的每个项目都有一个项目符号。



```html
  <ul>
      <li>第一项</li>
      <li>第二项</li>
  </ul>
```

  

#### 3. 链接和图像

  

**超链接 `<a>`**

  

- 用于创建从一个页面到另一个页面的链接。

  


```html
  <a href="https://www.example.com">这是一个链接</a>
```

  

**图像 `<img>`**

  

- 用于在网页中嵌入图像。

  


```html
  <img src="image.jpg" alt="描述文字">
```

  

#### 4. 表格

  

**表格结构**

  

- `<table>`：定义表格。
- `<tr>`：定义表格行。
- `<td>`：定义表格单元格。
- `<th>`：定义表格头单元格。

  

```html
  <table>
      <tr>
          <th>标题1</th>
          <th>标题2</th>
      </tr>
      <tr>
          <td>数据1</td>
          <td>数据2</td>
      </tr>
  </table>
```

  

**表格属性**

  

- `colspan`：合并列。

  

```html
  <td colspan="2">合并两列</td>
```

  

- `rowspan`：合并行。



```html
  <td rowspan="2">合并两行</td>
```

  

#### 5. 表单

  

**表单元素**

  

- `<form>`：定义表单。

  

```html
  <form action="/submit" method="post">
      <input type="text" name="username">
      <input type="submit" value="提交">
  </form>
```

  

- `<input>`：定义输入控件。

  

```html
  <input type="text" name="username">
  <input type="password" name="password">
  <input type="submit" value="提交">
```

  

- `<textarea>`：定义多行文本输入控件。

  

```html
  <textarea name="message"></textarea>
```

  

- `<button>`：定义按钮。

  


```html
  <button type="button">按钮</button>
```

  

- `<select>` 和 `<option>`：定义下拉列表。

  


```html
  <select name="options">
      <option value="1">选项1</option>
      <option value="2">选项2</option>
  </select>
```

  

#### 6. 其他标签

  

**分区和容器**

  

- `<div>`：块级容器，用于分组其他元素。

  

```html
  <div>
      <p>这是一个段落。</p>
  </div>
```

  

- `<span>`：内联容器，用于分组内联元素。

  


```html
  <span>这是内联文本。</span>
```

  

**嵌入内容**

  

- `<iframe>`：嵌入另一个HTML页面。

  


```html
  <iframe src="https://www.example.com"></iframe>
```

  

- `<video>`：嵌入视频。



```html
  <video controls>
      <source src="movie.mp4" type="video/mp4">
  </video>
```

  

- `<audio>`：嵌入音频。



```html
  <audio controls>
      <source src="audio.mp3" type="audio/mpeg">
  </audio>
```

  

#### 7. HTML5新增特性

  

**语义化标签**

  

- `<header>`：定义页面或部分的头部。
- `<footer>`：定义页面或部分的底部。
- `<nav>`：定义导航链接。
- `<article>`：定义文章。
- `<section>`：定义页面中的节。
- `<aside>`：定义侧边栏。

  

**多媒体**

  

- `<video>`：嵌入视频。
- `<audio>`：嵌入音频。
- `<canvas>`：用于绘制图形。

  

**表单增强**

  

- 新的输入类型：`email`, `url`, `number`, `date`, `range` 等。
- 新的属性：`placeholder`, `required`, `pattern` 等。

  

### 示例：创建一个简单的HTML页面

  

```html
<!DOCTYPE html>
<html>
<head>
    <title>我的网页</title>
</head>
<body>
    <header>
        <h1>欢迎来到我的网页</h1>
    </header>
    <nav>
        <a href="#section1">部分1</a>
        <a href="#section2">部分2</a>
    </nav>
    <section id="section1">
        <h2>部分1</h2>
        <p>这是部分1的内容。</p>
    </section>
    <section id="section2">
        <h2>部分2</h2>
        <p>这是部分2的内容。</p>
    </section>
    <footer>
        <p>版权所有 &copy; 2024</p>
    </footer>
</body>
</html>
```

