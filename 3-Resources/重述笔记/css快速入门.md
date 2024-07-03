---
Project: "[[web前端]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-06-22
Connected:
---

CSS（Cascading Style Sheets，层叠样式表）是一种用于描述HTML或XML文档外观和格式的样式表语言。通过CSS，可以控制网页的布局、颜色、字体等视觉效果，使网页更加美观和用户友好。下面是对CSS的详细讲解：

  

## 1. CSS基本语法

  

CSS的基本语法由选择器和声明块组成。声明块包含一个或多个声明，每个声明由属性和属性值组成。

  

```css

selector {

    property: value;

    property: value;

}

```

  

例如：

  

```css

p {

    color: red;

    font-size: 16px;

}

```

  

上面的CSS代码将所有 `<p>` 元素的文本颜色设置为红色，字体大小设置为16像素。

  

## 2. CSS选择器

  
https://github.com/lecepin/code-lab/tree/main/css-selector
选择器用于选择HTML元素，以便应用样式。常见的选择器有：

  

- **元素选择器**：选择所有指定元素。

  ```css

  p {

      color: blue;

  }

  ```

  

- **类选择器**：选择具有指定类属性的元素。使用`.`表示类选择器。

  ```css

  .my-class {

      background-color: yellow;

  }

  ```

  

- **ID选择器**：选择具有指定ID属性的元素。使用`#`表示ID选择器。

  ```css

  #my-id {

      border: 1px solid black;

  }

  ```

  

- **属性选择器**：选择具有指定属性的元素。

  ```css

  input[type="text"] {

      width: 200px;

  }

  ```

  

- **伪类选择器**：选择特定状态的元素。

  ```css

  a:hover {

      color: green;

  }

  ```

  

## 3. CSS盒模型

CSS 盒模型（Box Model）是网页布局和设计的基础概念之一。它描述了一个元素在网页中的空间占用情况，包括内容、内边距（padding）、边框（border）和外边距（margin）。理解盒模型有助于你更好地控制元素的布局和样式。下面是对 CSS 盒模型的详细讲解。
https://www.bilibili.com/video/BV13V411z7do/?spm_id_from=333.337.search-card.all.click&vd_source=8b450300cfa6415cb0312754cf65ba30
  

### 1. 盒模型的组成部分

  

盒模型由以下几个部分组成，从内到外依次是：

  

1. **内容（Content）**：元素的实际内容，如文本、图像等。

2. **内边距（Padding）**：内容与边框之间的空白区域。

3. **边框（Border）**：包围内容和内边距的边框。

4. **外边距（Margin）**：元素与其他元素之间的空白区域。

  

### 2. 盒模型的视觉表示

  

```

+---------------------------+

|        Margin             |

|  +---------------------+  |

|  |      Border         |  |

|  |  +---------------+  |  |

|  |  |   Padding     |  |  |

|  |  |  +---------+  |  |  |

|  |  |  | Content |  |  |  |

|  |  |  +---------+  |  |  |

|  |  +---------------+  |  |

|  +---------------------+  |

+---------------------------+

```

  

### 3. 各部分的 CSS 属性

  

#### 3.1 内容（Content）

  

内容是元素的实际显示部分，如文本、图像等。内容的大小可以通过 `width` 和 `height` 属性来设置。

  

```css

div {

    width: 200px;

    height: 100px;

}

```

  

#### 3.2 内边距（Padding）

  

内边距是内容与边框之间的空白区域。可以通过 `padding` 属性设置。你可以单独设置每个方向的内边距，也可以一次性设置四个方向的内边距。

  

```css

div {

    padding: 10px; /* 四个方向的内边距都为10px */

}

  

div {

    padding-top: 10px;    /* 上边内边距 */

    padding-right: 20px;  /* 右边内边距 */

    padding-bottom: 10px; /* 下边内边距 */

    padding-left: 20px;   /* 左边内边距 */

}

```

  

#### 3.3 边框（Border）

  

边框是包围内容和内边距的边框。可以通过 `border` 属性设置边框的宽度、样式和颜色。

  

```css

div {

    border: 2px solid black; /* 边框宽度为2px，样式为实线，颜色为黑色 */

}

  

div {

    border-width: 2px;       /* 边框宽度 */

    border-style: solid;     /* 边框样式 */

    border-color: black;     /* 边框颜色 */

}

```

  

#### 3.4 外边距（Margin）

  

外边距是元素与其他元素之间的空白区域。可以通过 `margin` 属性设置。你可以单独设置每个方向的外边距，也可以一次性设置四个方向的外边距。

  

```css

div {

    margin: 20px; /* 四个方向的外边距都为20px */

}

  

div {

    margin-top: 20px;    /* 上边外边距 */

    margin-right: 10px;  /* 右边外边距 */

    margin-bottom: 20px; /* 下边外边距 */

    margin-left: 10px;   /* 左边外边距 */

}

```

  

### 4. 盒模型的计算

  

默认情况下，元素的总宽度和高度是由内容、内边距、边框和外边距共同决定的。计算公式如下：

  

- **总宽度** = 内容宽度 + 左内边距 + 右内边距 + 左边框 + 右边框 + 左外边距 + 右外边距

- **总高度** = 内容高度 + 上内边距 + 下内边距 + 上边框 + 下边框 + 上外边距 + 下外边距

  

例如：

  

```css

div {

    width: 200px;

    height: 100px;

    padding: 10px;

    border: 5px solid black;

    margin: 20px;

}

```

  

总宽度 = 200px（内容宽度） + 10px（左内边距） + 10px（右内边距） + 5px（左边框） + 5px（右边框） + 20px（左外边距） + 20px（右外边距） = 270px

  

总高度 = 100px（内容高度） + 10px（上内边距） + 10px（下内边距） + 5px（上边框） + 5px（下边框） + 20px（上外边距） + 20px（下外边距） = 170px

  

### 5. `box-sizing` 属性

  

`box-sizing` 属性用于改变盒模型的计算方式。它有两个常用值：

  

- **content-box**（默认值）：宽度和高度只包括内容部分，不包括内边距和边框。

- **border-box**：宽度和高度包括内容、内边距和边框。

  

使用 `border-box` 可以更容易地控制元素的总宽度和高度。

  

```css

div {

    box-sizing: border-box;

    width: 200px;

    height: 100px;

    padding: 10px;

    border: 5px solid black;

    margin: 20px;

}

```

  

在 `box-sizing: border-box;` 模式下，元素的总宽度和高度是固定的，内容区域的大小会根据内边距和边框的大小自动调整。

  

总宽度 = 200px（包括内容、内边距和边框）

总高度 = 100px（包括内容、内边距和边框）

  

### 6. 实际示例

  

下面是一个完整的示例，展示了如何使用盒模型属性来控制元素的布局和样式。

  

#### HTML

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>CSS 盒模型示例</title>

    <link rel="stylesheet" href="styles.css">

</head>

<body>

    <div class="box">这是一个盒模型示例。</div>

</body>

</html>

```

  

#### CSS（styles.css）

  

```css

body {

    font-family: Arial, sans-serif;

    background-color: #f0f0f0;

    display: flex;

    justify-content: center;

    align-items: center;

    height: 100vh;

    margin: 0;

}

  

.box {

    width: 200px;

    height: 100px;

    padding: 20px;

    border: 5px solid black;

    margin: 30px;

    background-color: lightblue;

    box-sizing: border-box;

}

```

  

### 解释

  

- `body` 设置为 flex 布局，使内容居中对齐。

- `.box` 类应用了盒模型属性，包括宽度、高度、内边距、边框和外边距。

- `box-sizing: border-box;` 确保元素的总宽度和高度包括内容、内边距和边框。

  

通过这个示例，你可以看到如何使用盒模型属性来控制元素的布局和样式。理解和掌握盒模型是进行网页设计和布局的基础。希望这些信息对你有所帮助！如果你有任何问题或需要进一步的讲解，请随时告诉我。

  

## 4. CSS布局

  

CSS 布局是网页设计的重要组成部分，它决定了页面元素如何排列和显示。CSS 提供了多种布局技术，包括块级布局、内联布局、浮动布局、弹性盒模型（Flexbox）和网格布局（Grid）。下面是对这些布局技术的详细讲解。

  

### 1. 块级布局和内联布局

  

#### 1.1 块级元素

  

块级元素独占一行，并且宽度默认填满其父容器的宽度。常见的块级元素包括 `<div>`、`<p>`、`<h1>` 等。

  

```html

<div>这是一个块级元素。</div>

<p>这是一个段落。</p>

```

  

#### 1.2 内联元素

  

内联元素不会独占一行，它们会在同一行中排列。常见的内联元素包括 `<span>`、`<a>`、`<img>` 等。

  

```html

<span>这是一个内联元素。</span>

<a href="#">这是一个链接。</a>

```

  

### 2. 浮动布局

  

浮动布局使用 `float` 属性使元素向左或向右浮动，从而实现文本环绕或多列布局。

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>浮动布局示例</title>

    <style>

        .container {

            width: 100%;

        }

        .left {

            float: left;

            width: 50%;

            background-color: lightblue;

        }

        .right {

            float: right;

            width: 50%;

            background-color: lightgreen;

        }

        .clearfix::after {

            content: "";

            display: table;

            clear: both;

        }

    </style>

</head>

<body>

    <div class="container clearfix">

        <div class="left">左侧内容</div>

        <div class="right">右侧内容</div>

    </div>

</body>

</html>

```

  

### 3. 弹性盒模型（Flexbox）

  

Flexbox 是一种一维布局模型，适用于水平或垂直方向上的元素排列。它通过设置父容器的 `display` 属性为 `flex` 来启用。

  

#### 3.1 基本用法

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Flexbox 布局示例</title>

    <style>

        .container {

            display: flex;

            justify-content: space-between; /* 主轴对齐方式 */

            align-items: center; /* 交叉轴对齐方式 */

            height: 100vh;

        }

        .item {

            background-color: lightblue;

            padding: 20px;

            margin: 10px;

        }

    </style>

</head>

<body>

    <div class="container">

        <div class="item">项目 1</div>

        <div class="item">项目 2</div>

        <div class="item">项目 3</div>

    </div>

</body>

</html>

```

  

#### 3.2 Flexbox 属性

  

- **`display: flex`**：定义一个弹性容器。

- **`flex-direction`**：设置主轴方向（`row`、`row-reverse`、`column`、`column-reverse`）。

- **`justify-content`**：设置主轴对齐方式（`flex-start`、`flex-end`、`center`、`space-between`、`space-around`）。

- **`align-items`**：设置交叉轴对齐方式（`flex-start`、`flex-end`、`center`、`stretch`、`baseline`）。

- **`flex`**：设置弹性项目的伸缩能力。

  

### 4. 网格布局（Grid）

  

Grid 是一种二维布局模型，适用于同时处理行和列的布局。通过设置父容器的 `display` 属性为 `grid` 来启用。

  

#### 4.1 基本用法

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Grid 布局示例</title>

    <style>

        .container {

            display: grid;

            grid-template-columns: repeat(3, 1fr); /* 定义三列，每列等宽 */

            grid-gap: 10px; /* 网格间距 */

            height: 100vh;

        }

        .item {

            background-color: lightblue;

            padding: 20px;

        }

    </style>

</head>

<body>

    <div class="container">

        <div class="item">项目 1</div>

        <div class="item">项目 2</div>

        <div class="item">项目 3</div>

        <div class="item">项目 4</div>

        <div class="item">项目 5</div>

        <div class="item">项目 6</div>

    </div>

</body>

</html>

```

  

#### 4.2 Grid 属性

  

- **`display: grid`**：定义一个网格容器。

- **`grid-template-columns`**：定义列的数量和宽度。

- **`grid-template-rows`**：定义行的数量和高度。

- **`grid-gap`**：定义网格项目之间的间距。

- **`grid-column`**：定义项目在网格中的列位置。

- **`grid-row`**：定义项目在网格中的行位置。

  

### 5. 定位布局

  

定位布局通过 `position` 属性来控制元素的位置。`position` 属性有五个值：`static`（默认值）、`relative`、`absolute`、`fixed` 和 `sticky`。

  

#### 5.1 相对定位（relative）

  

相对定位元素相对于其正常位置进行定位。

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>相对定位示例</title>

    <style>

        .relative {

            position: relative;

            top: 20px; /* 向下移动20px */

            left: 30px; /* 向右移动30px */

            background-color: lightblue;

            padding: 20px;

        }

    </style>

</head>

<body>

    <div class="relative">这是一个相对定位的元素。</div>

</body>

</html>

```

  

#### 5.2 绝对定位（absolute）

  

绝对定位元素相对于最近的已定位祖先元素进行定位，如果没有已定位祖先元素，则相对于初始包含块（通常是 `<html>` 元素）进行定位。

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>绝对定位示例</title>

    <style>

        .container {

            position: relative;

            height: 200px;

            background-color: lightgray;

        }

        .absolute {

            position: absolute;

            top: 50px; /* 距离父容器顶部50px */

            left: 50px; /* 距离父容器左侧50px */

            background-color: lightblue;

            padding: 20px;

        }

    </style>

</head>

<body>

    <div class="container">

        <div class="absolute">这是一个绝对定位的元素。</div>

    </div>

</body>

</html>

```

  

#### 5.3 固定定位（fixed）

  

固定定位元素相对于浏览器窗口进行定位，即使页面滚动它也不会移动。

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>固定定位示例</title>

    <style>

        .fixed {

            position: fixed;

            top: 10px; /* 距离窗口顶部10px */

            right: 10px; /* 距离窗口右侧10px */

            background-color: lightblue;

            padding: 20px;

        }

    </style>

</head>

<body>

    <div class="fixed">这是一个固定定位的元素。</div>

    <div style="height: 2000px;">滚动测试</div>

</body>

</html>

```

  

当然，接下来我们继续讲解粘性定位（sticky）和其他布局属性。

  

#### 5.4 粘性定位（sticky）

  

粘性定位元素在跨越特定阈值前表现为相对定位，在跨越后表现为固定定位。

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>粘性定位示例</title>

    <style>

        .sticky {

            position: sticky;

            top: 0; /* 当滚动到顶部时，元素固定 */

            background-color: lightblue;

            padding: 20px;

        }

        .content {

            height: 2000px;

            background-color: lightgray;

        }

    </style>

</head>

<body>

    <div class="sticky">这是一个粘性定位的元素。</div>

    <div class="content">滚动测试</div>

</body>

</html>

```

  

### 6. 多列布局

  

多列布局（Multi-column Layout）允许你轻松地将内容分成多列，类似于报纸的布局。

  

#### 6.1 基本用法

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>多列布局示例</title>

    <style>

        .columns {

            column-count: 3; /* 分成三列 */

            column-gap: 20px; /* 列间距 */

        }

    </style>

</head>

<body>

    <div class="columns">

        <p>这是一个多列布局示例。内容会自动分成三列。</p>

        <p>这是一个多列布局示例。内容会自动分成三列。</p>

        <p>这是一个多列布局示例。内容会自动分成三列。</p>

        <p>这是一个多列布局示例。内容会自动分成三列。</p>

        <p>这是一个多列布局示例。内容会自动分成三列。</p>

    </div>

</body>

</html>

```

  

#### 6.2 多列布局属性

  

- **`column-count`**：定义列的数量。

- **`column-gap`**：定义列之间的间距。

- **`column-width`**：定义列的宽度。

- **`column-rule`**：定义列之间的分隔线。

  

### 7. 表格布局

  

表格布局（Table Layout）使用 CSS 表格属性来控制元素的布局，类似于 HTML 表格。

  

#### 7.1 基本用法

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>表格布局示例</title>

    <style>

        .table {

            display: table;

            width: 100%;

        }

        .row {

            display: table-row;

        }

        .cell {

            display: table-cell;

            padding: 10px;

            border: 1px solid #ccc;

        }

    </style>

</head>

<body>

    <div class="table">

        <div class="row">

            <div class="cell">单元格 1</div>

            <div class="cell">单元格 2</div>

        </div>

        <div class="row">

            <div class="cell">单元格 3</div>

            <div class="cell">单元格 4</div>

        </div>

    </div>

</body>

</html>

```

  

#### 7.2 表格布局属性

  

- **`display: table`**：定义一个表格容器。

- **`display: table-row`**：定义一个表格行。

- **`display: table-cell`**：定义一个表格单元格。

- **`border-collapse`**：设置表格边框是否合并。

  

### 8. 综合布局示例

  

以下是一个综合示例，展示了如何结合使用上述布局技术来创建一个复杂的网页布局。

  

#### HTML

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>综合布局示例</title>

    <link rel="stylesheet" href="styles.css">

</head>

<body>

    <header class="header">页眉</header>

    <nav class="nav">导航栏</nav>

    <main class="main">

        <aside class="sidebar">侧边栏</aside>

        <section class="content">主要内容</section>

    </main>

    <footer class="footer">页脚</footer>

</body>

</html>

```

  

#### CSS（styles.css）

  

```css

body {

    font-family: Arial, sans-serif;

    margin: 0;

    display: flex;

    flex-direction: column;

    height: 100vh;

}

  

.header, .footer {

    background-color: #333;

    color: white;

    text-align: center;

    padding: 10px;

}

  

.nav {

    background-color: #444;

    color: white;

    text-align: center;

    padding: 10px;

}

  

.main {

    display: flex;

    flex: 1;

}

  

.sidebar {

    background-color: #555;

    color: white;

    width: 200px;

    padding: 10px;

}

  

.content {

    flex: 1;

    padding: 10px;

    background-color: #f0f0f0;

}

```

  

### 解释

  

- **页眉和页脚**：使用 `flex-direction: column` 将其放置在页面的顶部和底部。

- **导航栏**：位于页眉下方，使用背景颜色和内边距来区分。

- **主要内容区域**：使用 `display: flex` 将其分为侧边栏和主要内容区域。

- **侧边栏**：设置固定宽度，使用背景颜色和内边距来区分。

- **主要内容区域**：使用 `flex: 1` 使其占据剩余的空间。
## 5. CSS颜色和背景





### 1.1 颜色表示法

  

CSS 支持多种颜色表示法，包括颜色名称、十六进制值、RGB、RGBA、HSL 和 HSLA。

  

#### 颜色名称

  

CSS 支持 140 种颜色名称，如 `red`、`blue`、`green` 等。

  

```css

p {

    color: red;

}

```

  

#### 十六进制值

  

十六进制颜色值以 `#` 开头，后跟三或六个十六进制数字。

  

```css

h1 {

    color: #ff5733;

}

```

  

#### RGB 值

  

RGB 值表示红、绿、蓝三种颜色的组合，每种颜色的值在 0 到 255 之间。

  

```css

div {

    color: rgb(255, 87, 51);

}

```

  

#### RGBA 值

  

RGBA 值在 RGB 的基础上增加了一个透明度（Alpha）值，范围在 0 到 1 之间。

  

```css

span {

    color: rgba(255, 87, 51, 0.5);

}

```

  

#### HSL 值

  

HSL 值表示色相、饱和度和亮度。色相值在 0 到 360 之间，饱和度和亮度值在 0% 到 100% 之间。

  

```css

a {

    color: hsl(9, 100%, 60%);

}

```

  

#### HSLA 值

  

HSLA 值在 HSL 的基础上增加了一个透明度（Alpha）值，范围在 0 到 1 之间。

  

```css

button {

    color: hsla(9, 100%, 60%, 0.5);

}

```

  

### 1.2 颜色属性

  

#### `color`

  

设置文本颜色。

  

```css

p {

    color: blue;

}

```

  

#### `background-color`

  

设置元素的背景颜色。

  

```css

div {

    background-color: lightgray;

}

```

  


  

### 2.1 背景属性

  

#### `background-color`

  

设置元素的背景颜色。

  

```css

header {

    background-color: #f0f0f0;

}

```

  

#### `background-image`

  

设置元素的背景图片。

  

```css

section {

    background-image: url('background.jpg');

}

```

  

#### `background-repeat`

  

设置背景图片是否重复。可选值有 `repeat`、`no-repeat`、`repeat-x`、`repeat-y`。

  

```css

section {

    background-image: url('background.jpg');

    background-repeat: no-repeat;

}

```

  

#### `background-position`

  

设置背景图片的位置。可选值有 `left top`、`center center`、`right bottom` 等。

  

```css

section {

    background-image: url('background.jpg');

    background-position: center center;

}

```

  

#### `background-size`

  

设置背景图片的大小。可选值有 `auto`、`cover`、`contain`、具体尺寸（如 `100px 100px`）。

  

```css

section {

    background-image: url('background.jpg');

    background-size: cover;

}

```

  

#### `background-attachment`

  

设置背景图片是否固定。可选值有 `scroll`、`fixed`、`local`。

  

```css

section {

    background-image: url('background.jpg');

    background-attachment: fixed;

}

```

  

#### `background`

  

`background` 是一个简写属性，可以同时设置背景颜色、背景图片、背景重复、背景位置、背景大小和背景附件。

  

```css

section {

    background: url('background.jpg') no-repeat center center / cover;

}

```

  

### 2.2 示例

  

下面是一个完整的示例，展示了如何使用颜色和背景属性。

  

#### HTML

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>CSS 颜色和背景示例</title>

    <link rel="stylesheet" href="styles.css">

</head>

<body>

    <header>

        <h1>我的网站</h1>

    </header>

    <section>

        <h2>欢迎来到我的网站</h2>

        <p>这是一个示例页面，用于展示 CSS 颜色和背景的用法。</p>

    </section>

    <footer>

        <p>&copy; 2024 我的公司</p>

    </footer>

</body>

</html>

```

  

#### CSS

  

```css

/* 设置基本样式 */

body {

    font-family: Arial, sans-serif;

    margin: 0;

    padding: 0;

}

  

/* 设置头部样式 */

header {

    background-color: #333;

    color: white;

    padding: 20px;

    text-align: center;

}

  

/* 设置段落文本颜色 */

p {

    color: #555;

}

  

/* 设置段落背景颜色 */

p {

    background-color: #f0f0f0;

    padding: 10px;

    border-radius: 5px;

}

  

/* 设置节区背景图片 */

section {

    background-image: url('background.jpg');

    background-repeat: no-repeat;

    background-position: center center;

    background-size: cover;

    padding: 50px;

    color: white;

}

  

/* 设置页脚样式 */

footer {

    background-color: #333;

    color: white;

    text-align: center;

    padding: 10px;

}

```

  

### 总结

  

通过掌握 CSS 中的颜色和背景属性，你可以大大增强网页的视觉效果。颜色属性包括 `color` 和 `background-color`，而背景属性则包括 `background-image`、`background-repeat`、`background-position`、`background-size`、`background-attachment` 和 `background` 简写属性。通过这些属性，你可以创建丰富多彩和功能强大的网页设计。

  

## 6. CSS字体和文本

  
  

### 1.1 字体属性

  

#### `font-family`

  

设置字体系列。可以指定多个字体，浏览器会按顺序选择第一个可用的字体。

  

```css

p {

    font-family: "Arial", "Helvetica", sans-serif;

}

```

  

#### `font-size`

  

设置字体大小。可以使用绝对单位（如 `px`、`pt`）或相对单位（如 `%`、`em`、`rem`）。

  

```css

h1 {

    font-size: 24px;

}

  

p {

    font-size: 1.2em;

}

```

  

#### `font-weight`

  

设置字体的粗细。常用值有 `normal`、`bold` 以及数字值（如 `100` 到 `900`）。

  

```css

strong {

    font-weight: bold;

}

```

  

#### `font-style`

  

设置字体样式。常用值有 `normal`、`italic` 和 `oblique`。

  

```css

em {

    font-style: italic;

}

```

  

#### `font-variant`

  

设置字体的变体。常用值有 `normal` 和 `small-caps`。

  

```css

p {

    font-variant: small-caps;

}

```

  

#### `line-height`

  

设置行高，可以使用绝对单位或相对单位。

  

```css

p {

    line-height: 1.5;

}

```

  

#### `font`

  

`font` 是一个简写属性，可以同时设置字体样式、字体变体、字体粗细、字体大小、行高和字体系列。

  

```css

p {

    font: italic small-caps bold 16px/1.5 "Arial", sans-serif;

}

```

  

### 1.2 示例

  

```css

body {

    font-family: "Arial", "Helvetica", sans-serif;

    font-size: 16px;

    line-height: 1.5;

}

  

h1 {

    font-size: 2em;

    font-weight: bold;

}

  

em {

    font-style: italic;

}

  

strong {

    font-weight: bold;

}

```

  



  

### 2.1 文本属性

  

#### `color`

  

设置文本颜色。

  

```css

p {

    color: #333;

}

```

  

#### `text-align`

  

设置文本对齐方式。常用值有 `left`、`right`、`center` 和 `justify`。

  

```css

h1 {

    text-align: center;

}

```

  

#### `text-decoration`

  

设置文本装饰。常用值有 `none`、`underline`、`overline` 和 `line-through`。

  

```css

a {

    text-decoration: none;

}

```

  

#### `text-transform`

  

设置文本转换。常用值有 `none`、`capitalize`、`uppercase` 和 `lowercase`。

  

```css

p {

    text-transform: uppercase;

}

```

  

#### `text-indent`

  

设置文本缩进。

  

```css

p {

    text-indent: 2em;

}

```

  

#### `letter-spacing`

  

设置字母间距。

  

```css

p {

    letter-spacing: 0.1em;

}

```

  

#### `word-spacing`

  

设置单词间距。

  

```css

p {

    word-spacing: 0.2em;

}

```

  

#### `line-height`

  

设置行高。

  

```css

p {

    line-height: 1.5;

}

```

  

#### `white-space`

  

设置如何处理空白字符。常用值有 `normal`、`nowrap`、`pre`、`pre-line` 和 `pre-wrap`。

  

```css

pre {

    white-space: pre;

}

```

  

### 2.2 示例

  

```css

body {

    font-family: "Arial", "Helvetica", sans-serif;

    font-size: 16px;

    line-height: 1.5;

    color: #333;

}

  

h1 {

    text-align: center;

    text-transform: uppercase;

}

  

p {

    text-indent: 2em;

    letter-spacing: 0.1em;

    word-spacing: 0.2em;

}

  

a {

    text-decoration: none;

    color: blue;

}

  

a:hover {

    text-decoration: underline;

}

```

  

### 3. 综合示例

  

下面是一个完整的 HTML 和 CSS 示例，展示了如何使用字体和文本属性。

  

#### HTML

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>CSS 字体和文本示例</title>

    <link rel="stylesheet" href="styles.css">

</head>

<body>

    <header>

        <h1>我的网站</h1>

    </header>

    <section>

        <h2>欢迎来到我的网站</h2>

        <p>这是一个示例页面，用于展示 CSS 字体和文本的用法。</p>

        <p><em>这是斜体文本。</em> <strong>这是粗体文本。</strong></p>

        <p>这是一个<a href="#">链接</a>。</p>

    </section>

    <footer>

        <p>&copy; 2024 我的公司</p>

    </footer>

</body>

</html>

```

  

#### CSS

  

```css

/* 设置基本样式 */

body {

    font-family: "Arial", "Helvetica", sans-serif;

    font-size: 16px;

    line-height: 1.5;

    color: #333;

}

  

/* 设置头部样式 */

header {

    background-color: #333;

    color: white;

    padding: 20px;

    text-align: center;

}

  

/* 设置标题样式 */

h1 {

    font-size: 2em;

    text-transform: uppercase;

}

  

h2 {

    font-size: 1.5em;

    font-weight: bold;

}

  

/* 设置段落样式 */

p {

    text-indent: 2em;

    letter-spacing: 0.1em;

    word-spacing: 0.2em;

}

  

/* 设置链接样式 */

a {

    text-decoration: none;

    color: blue;

}

  

a:hover {

    text-decoration: underline;

}

  

/* 设置斜体和粗体文本 */

em {

    font-style: italic;

}

  

strong {

    font-weight: bold;

}

  

/* 设置页脚样式 */

footer {

    background-color: #333;

    color: white;

    text-align: center;

    padding: 10px;

}

```

  

### 4. 总结

  

通过掌握 CSS 中的字体和文本属性，你可以大大提升网页的可读性和美观性。字体属性包括 `font-family`、`font-size`、`font-weight`、`font-style`、`font-variant`、`line-height` 和 `font` 简写属性。文本属性包括 `color`、`text-align`、`text-decoration`、`text-transform`、`text-indent`、`letter-spacing`、`word-spacing`、`line-height` 和 `white-space`。通过这些属性，你可以创建丰富多彩和功能强大的网页设计。

## 7. CSS动画


  

### 1.1 过渡属性

  

#### `transition`

  

`transition` 是一个简写属性，用于设置一个或多个过渡效果。可以指定过渡的属性、持续时间、过渡曲线和延迟时间。

  

```css

.element {

    transition: property duration timing-function delay;

}

```

  

#### `transition-property`

  

指定要应用过渡效果的 CSS 属性。

  

```css

.element {

    transition-property: background-color;

}

```

  

#### `transition-duration`

  

指定过渡效果的持续时间。

  

```css

.element {

    transition-duration: 2s;

}

```

  

#### `transition-timing-function`

  

指定过渡效果的速度曲线。常用值有 `ease`、`linear`、`ease-in`、`ease-out`、`ease-in-out` 和 `cubic-bezier`。

  

```css

.element {

    transition-timing-function: ease-in-out;

}

```

  

#### `transition-delay`

  

指定过渡效果的延迟时间。

  

```css

.element {

    transition-delay: 0.5s;

}

```

  

### 1.2 示例

  

#### HTML

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>CSS 过渡示例</title>

    <link rel="stylesheet" href="styles.css">

</head>

<body>

    <div class="box"></div>

</body>

</html>

```

  

#### CSS

  

```css

/* 设置基本样式 */

body {

    display: flex;

    justify-content: center;

    align-items: center;

    height: 100vh;

    margin: 0;

}

  

.box {

    width: 100px;

    height: 100px;

    background-color: red;

    transition: background-color 1s ease-in-out, transform 1s ease-in-out;

}

  

.box:hover {

    background-color: blue;

    transform: scale(1.5);

}

```

  

在这个示例中，当你将鼠标悬停在 `.box` 元素上时，背景颜色会在 1 秒内从红色变为蓝色，同时元素会放大 1.5 倍。

  


  

### 2.1 关键帧动画属性

  

#### `@keyframes`

  

定义动画的关键帧。关键帧指定动画在特定时间点的样式。

  

```css

@keyframes animationName {

    from { /* 起始状态 */ }

    to { /* 结束状态 */ }

    /* 或者使用百分比 */

    0% { /* 起始状态 */ }

    100% { /* 结束状态 */ }

}

```

  

#### `animation`

  

`animation` 是一个简写属性，用于设置一个或多个动画效果。可以指定动画名称、持续时间、速度曲线、延迟时间、播放次数、方向和填充模式。

  

```css

.element {

    animation: animationName duration timing-function delay iteration-count direction fill-mode;

}

```

  

#### `animation-name`

  

指定要应用的关键帧动画名称。

  

```css

.element {

    animation-name: slidein;

}

```

  

#### `animation-duration`

  

指定动画的持续时间。

  

```css

.element {

    animation-duration: 2s;

}

```

  

#### `animation-timing-function`

  

指定动画的速度曲线。

  

```css

.element {

    animation-timing-function: ease-in-out;

}

```

  

#### `animation-delay`

  

指定动画的延迟时间。

  

```css

.element {

    animation-delay: 0.5s;

}

```

  

#### `animation-iteration-count`

  

指定动画的播放次数。可以是具体的数字，也可以是 `infinite`（无限循环）。

  

```css

.element {

    animation-iteration-count: infinite;

}

```

  

#### `animation-direction`

  

指定动画的播放方向。常用值有 `normal`、`reverse`、`alternate` 和 `alternate-reverse`。

  

```css

.element {

    animation-direction: alternate;

}

```

  

#### `animation-fill-mode`

  

指定动画在播放前后如何应用样式。常用值有 `none`、`forwards`、`backwards` 和 `both`。

  

```css

.element {

    animation-fill-mode: forwards;

}

```

  

### 2.2 示例

  

#### HTML

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>CSS 动画示例</title>

    <link rel="stylesheet" href="styles.css">

</head>

<body>

    <div class="box"></div>

</body>

</html>

```

  

#### CSS

  

```css

/* 设置基本样式 */

body {

    display: flex;

    justify-content: center;

    align-items: center;

    height: 100vh;

    margin: 0;

}

  

.box {

    width: 100px;

    height: 100px;

    background-color: red;

    animation: slidein 3s ease-in-out infinite alternate;

}

  

@keyframes slidein {

    0% {

        transform: translateX(0);

    }

    100% {

        transform: translateX(300px);

    }

}

```

  

在这个示例中，`.box` 元素会在 3 秒内从左向右移动 300 像素，然后返回原位，并无限循环。

  

### 3. 综合示例

  

下面是一个更复杂的示例，结合过渡和关键帧动画。

  

#### HTML

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>CSS 过渡和动画示例</title>

    <link rel="stylesheet" href="styles.css">

</head>

<body>

    <div class="container">

        <div class="box"></div>

    </div>

</body>

</html>

```

  

#### CSS

  

```css

/* 设置基本样式 */

body {

    display: flex;

    justify-content: center;

    align-items: center;

    height: 100vh;

    margin: 0;

}

  

.container {

    width: 400px;

    height: 400px;

    border: 2px solid #333;

    position: relative;

}

  

.box {

    width: 50px;

    height: 50px;

    background-color: red;

    position: absolute;

    top: 0;

    left: 0;

    transition: background-color 1s ease-in-out;

    animation: move 5s linear infinite;

}

  

.box:hover {

    background-color: blue;

}

  

@keyframes move {

    0% {

        top: 0;

        left: 0;

    }

    25% {

        top: 0;

        left: 100%;

        transform: translateX(-100%);

    }

    50% {

        top: 100%;

        left: 100%;

        transform: translate(-100%, -100%);

    }

    75% {

        top: 100%;

        left: 0;

        transform: translateY(-100%);

    }

    100% {

        top: 0;

        left: 0;

    }

}

```

  

在这个示例中，`.box` 元素会沿着 `.container` 的边缘移动，并且在悬停时会改变颜色。

  

### 4. 总结

  

通过掌握 CSS 中的过渡和关键帧动画属性，你可以创建丰富多彩和动态的网页效果。过渡属性包括 `transition`、`transition-property`、`transition-duration`、`transition-timing-function` 和 `transition-delay`。关键帧动画属性包括 `@keyframes`、`animation`、`animation-name`、`animation-duration`、`animation-timing-function`、`animation-delay`、`animation-iteration-count`、`animation-direction` 和 `animation-fill-mode`。通过这些属性，你可以创建各种动画效果，使你的网页更加生动和吸引人。
  

## 8. 媒体查询

### 1. 媒体查询的基本语法

  

媒体查询的基本语法如下：

  

```css

@media [media-type] and (media-feature) {

    /* CSS 规则 */

}

```

  

- `media-type`：指定媒体类型，如 `screen`（屏幕）、`print`（打印）、`all`（所有设备）等。可以省略，默认是 `all`。

- `media-feature`：指定媒体特性，如宽度、高度、分辨率等。

  

### 示例

  

```css

@media screen and (max-width: 600px) {

    body {

        background-color: lightblue;

    }

}

```

  

在这个示例中，当屏幕宽度小于或等于 600 像素时，`body` 元素的背景颜色将变为 `lightblue`。

  



  

### 2.1 宽度和高度

  

- `width`：视口的宽度。

- `height`：视口的高度。

- `min-width`：最小宽度。

- `max-width`：最大宽度。

- `min-height`：最小高度。

- `max-height`：最大高度。

  

### 示例

  

```css

@media screen and (max-width: 768px) {

    .container {

        width: 100%;

    }

}

```

  

### 2.2 设备宽度和高度

  

- `device-width`：设备的宽度。

- `device-height`：设备的高度。

- `min-device-width`：最小设备宽度。

- `max-device-width`：最大设备宽度。

- `min-device-height`：最小设备高度。

- `max-device-height`：最大设备高度。

  

### 示例

  

```css

@media screen and (min-device-width: 320px) and (max-device-width: 480px) {

    .container {

        padding: 10px;

    }

}

```

  

### 2.3 分辨率

  

- `resolution`：设备的分辨率。

- `min-resolution`：最小分辨率。

- `max-resolution`：最大分辨率。

  

### 示例

  

```css

@media screen and (min-resolution: 2dppx) {

    .high-res {

        background-image: url('high-res-image.png');

    }

}

```

  

### 2.4 方向

  

- `orientation`：设备的方向。常用值有 `portrait`（纵向）和 `landscape`（横向）。

  

### 示例

  

```css

@media screen and (orientation: landscape) {

    .landscape {

        display: block;

    }

}

```

  

### 3. 综合示例

  

下面是一个完整的 HTML 和 CSS 示例，展示了如何使用媒体查询实现响应式设计。

  

### HTML

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>CSS 媒体查询示例</title>

    <link rel="stylesheet" href="styles.css">

</head>

<body>

    <header>

        <h1>我的网站</h1>

    </header>

    <nav>

        <ul>

            <li><a href="#">首页</a></li>

            <li><a href="#">关于我们</a></li>

            <li><a href="#">服务</a></li>

            <li><a href="#">联系我们</a></li>

        </ul>

    </nav>

    <main>

        <section>

            <h2>欢迎来到我的网站</h2>

            <p>这是一个示例页面，用于展示 CSS 媒体查询的用法。</p>

        </section>

    </main>

    <footer>

        <p>&copy; 2024 我的公司</p>

    </footer>

</body>

</html>

```

  

### CSS

  

```css

/* 基本样式 */

body {

    font-family: Arial, sans-serif;

    margin: 0;

    padding: 0;

}

  

header {

    background-color: #333;

    color: white;

    padding: 20px;

    text-align: center;

}

  

nav ul {

    list-style-type: none;

    padding: 0;

    display: flex;

    justify-content: center;

    background-color: #444;

}

  

nav ul li {

    margin: 0 15px;

}

  

nav ul li a {

    color: white;

    text-decoration: none;

    padding: 10px;

}

  

main {

    padding: 20px;

}

  

footer {

    background-color: #333;

    color: white;

    text-align: center;

    padding: 10px;

}

  

/* 媒体查询 */

@media screen and (max-width: 768px) {

    nav ul {

        flex-direction: column;

        align-items: center;

    }

  

    nav ul li {

        margin: 10px 0;

    }

}

  

@media screen and (max-width: 480px) {

    header {

        padding: 10px;

    }

  

    nav ul li a {

        padding: 5px;

    }

  

    main {

        padding: 10px;

    }

}

```

  

在这个示例中，我们使用了两个媒体查询：

  

1. 当屏幕宽度小于或等于 768 像素时，导航菜单从水平布局变为垂直布局。

2. 当屏幕宽度小于或等于 480 像素时，标题和主内容的内边距减少，导航链接的内边距也减少。

  


  

### 4.1 移动优先设计

  

采用移动优先设计（Mobile First Design），即首先为移动设备设计样式，然后使用媒体查询为更大屏幕添加样式。

  

```css

/* 移动设备样式 */

body {

    font-size: 14px;

}

  

/* 平板设备样式 */

@media screen and (min-width: 768px) {

    body {

        font-size: 16px;

    }

}

  

/* 桌面设备样式 */

@media screen and (min-width: 1024px) {

    body {

        font-size: 18px;

    }

}

```

  

### 4.2 使用相对单位

  

使用相对单位（如 `em`、`rem`、百分比）而不是绝对单位（如 `px`），以确保样式在不同设备上都能很好地适应。

  

```css

body {

    font-size: 1rem;

}

  

.container {

    width: 80%;

    max-width: 1200px;

    margin: 0 auto;

}

```

  

### 4.3 测试和优化

  

在不同设备和浏览器上测试你的网页，确保媒体查询和响应式设计效果良好。可以使用浏览器的开发者工具模拟不同设备。

  

### 5. 总结

  

通过掌握 CSS 媒体查询，你可以创建响应式网页设计，确保网页在各种设备上都有良好的显示效果。媒体查询的基本语法包括 `@media` 规则和各种媒体特性，如宽度、高度、分辨率和方向。采用移动优先设计、使用相对单位，并在不同设备上测试和优化你的网页，可以帮助你创建更加灵活和适应性强的网页设计。


## 9. CSS 属性
CSS 提供了一系列属性，用于控制 HTML 元素的外观和布局。
  
### 1. 尺寸属性

  

#### 1.1 `width` 和 `height`

- **`width`**：设置元素的宽度。

- **`height`**：设置元素的高度。

  

```css

div {

    width: 300px;  /* 元素宽度 */

    height: 150px; /* 元素高度 */

}

```

  

#### 1.2 `max-width` 和 `max-height`

- **`max-width`**：设置元素的最大宽度。

- **`max-height`**：设置元素的最大高度。

  

```css

img {

    max-width: 100%; /* 最大宽度为100%，防止图片超过容器宽度 */

    max-height: 400px; /* 最大高度为400px */

}

```

  

#### 1.3 `min-width` 和 `min-height`

- **`min-width`**：设置元素的最小宽度。

- **`min-height`**：设置元素的最小高度。

  

```css

div {

    min-width: 200px; /* 最小宽度为200px */

    min-height: 100px; /* 最小高度为100px */

}

```

  

### 2. 边距和内边距属性

  

#### 2.1 `margin`

设置元素的外边距（外部空间）。可以单独设置每个方向的边距，或者一次性设置四个方向的边距。

  

```css

div {

    margin: 20px; /* 四个方向的边距都为20px */

}

  

div {

    margin-top: 10px;    /* 上边距 */

    margin-right: 15px;  /* 右边距 */

    margin-bottom: 20px; /* 下边距 */

    margin-left: 25px;   /* 左边距 */

}

```

  

#### 2.2 `padding`

设置元素的内边距（内部空间）。可以单独设置每个方向的内边距，或者一次性设置四个方向的内边距。

  

```css

div {

    padding: 10px; /* 四个方向的内边距都为10px */

}

  

div {

    padding-top: 5px;    /* 上内边距 */

    padding-right: 10px; /* 右内边距 */

    padding-bottom: 15px;/* 下内边距 */

    padding-left: 20px;  /* 左内边距 */

}

```

  

### 3. 边框属性

  

#### 3.1 `border`

设置元素的边框。可以定义边框的宽度、样式和颜色。

  

```css

div {

    border: 2px solid black; /* 边框宽度为2px，样式为实线，颜色为黑色 */

}

```

  

#### 3.2 `border-width`

单独设置边框的宽度。

  

```css

div {

    border-width: 5px; /* 边框宽度为5px */

}

```

  

#### 3.3 `border-style`

单独设置边框的样式。

  

```css

div {

    border-style: dashed; /* 边框样式为虚线 */

}

```

  

#### 3.4 `border-color`

单独设置边框的颜色。

  

```css

div {

    border-color: red; /* 边框颜色为红色 */

}

```

  

#### 3.5 `border-radius`

设置元素的边框圆角。

  

```css

div {

    border-radius: 10px; /* 边框圆角半径为10px */

}

```

  

### 4. 背景属性

  

#### 4.1 `background-color`

设置元素的背景颜色。

  

```css

div {

    background-color: lightblue; /* 背景颜色为浅蓝色 */

}

```

  

#### 4.2 `background-image`

设置元素的背景图像。

  

```css

div {

    background-image: url('background.jpg'); /* 背景图像为背景图片 */

}

```

  

#### 4.3 `background-repeat`

设置背景图像是否重复。

  

```css

div {

    background-repeat: no-repeat; /* 背景图像不重复 */

}

```

  

#### 4.4 `background-size`

设置背景图像的大小。

  

```css

div {

    background-size: cover; /* 背景图像覆盖整个元素 */

}

```

  

### 5. 文本属性

  

#### 5.1 `color`

设置文本颜色。

  

```css

p {

    color: blue; /* 文本颜色为蓝色 */

}

```

  

#### 5.2 `font-size`

设置字体大小。

  

```css

p {

    font-size: 16px; /* 字体大小为16px */

}

```

  

#### 5.3 `font-weight`

设置字体粗细。

  

```css

p {

    font-weight: bold; /* 字体加粗 */

}

```

  

#### 5.4 `text-align`

设置文本对齐方式。

  

```css

p {

    text-align: center; /* 文本居中对齐 */

}

```

  

#### 5.5 `text-decoration`

设置文本装饰，如下划线、删除线等。

  

```css

a {

    text-decoration: none; /* 移除链接的下划线 */

}

```

  

### 6. 布局属性

  

#### 6.1 `display`

设置元素的显示类型。

  

```css

div {

    display: block; /* 设置为块级元素 */

}

  

span {

    display: inline; /* 设置为内联元素 */

}

```

  

#### 6.2 `position`

设置元素的定位方式。

  

```css

div {

    position: relative; /* 相对定位 */

    top: 10px;          /* 相对于其正常位置向下移动10px */

    left: 20px;         /* 相对于其正常位置向右移动20px */

}

  

div {

    position: absolute; /* 绝对定位 */

    top: 50px;          /* 相对于最近的已定位祖先元素的顶部边缘移动50px */

    left: 100px;        /* 相对于最近的已定位祖先元素的左边缘移动100px */

}

```

  

#### 6.3 `float`

设置元素的浮动方式。

  

```css

img {

    float: left; /* 元素向左浮动 */

}

  

p {

    float: right; /* 元素向右浮动 */

}

```

  

#### 6.4 `flex`

设置弹性盒模型属性。

  

```css

.container {

    display: flex; /* 设置为弹性盒容器 */

    justify-content: center; /* 主轴对齐方式为居中 */

    align-items: center; /* 交叉轴对齐方式为居中 */

}

  

.item {

    flex: 1; /* 弹性元素，自动分配剩余空间 */

}

```

  

### 7. 其他常用属性

  

#### 7.1 `opacity`

设置元素的不透明度。

  

```css

div {

    opacity: 0.5; /* 不透明度为50% */

}

```

  

#### 7.2 `overflow`

设置元素内容的溢出行为。

  

```css

div {

    overflow: hidden; /* 隐藏溢出内容 */

}

  

div {

    overflow: scroll; /* 添加滚动条 */

}

```

  

#### 7.3 `visibility`

设置元素的可见性。

  

```css

div {

    visibility: hidden; /* 元素不可见，但仍占据空间 */

}

```