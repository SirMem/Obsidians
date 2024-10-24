---
Project: 
Status:
tags: Resources
Deadline:
CreateTime:
Connected:
---

 

## 什么是事件（Event）？
- 事件是 JavaScript 应用的核心，当我们与网页进行交互时，例如点击按钮、滚动页面等，事件就会被触发。每当这样的互动发生，都可以视为一个事件。事件处理器（Event Handler）则是负责响应这些事件的函数或代码块。

## 事件对象（Event Object）
### 介绍

- 当某个事件（如鼠标点击、键盘按下、表单提交等）触发时，浏览器会自动创建一个 **事件对象**（`Event Object`），并将该对象传递给事件处理函数。事件对象包含了与当前事件相关的所有信息，比如事件类型、触发事件的元素、鼠标或键盘的位置、按键状态等。

- 事件对象 `e` 为事件处理函数提供了事件相关的详细信息，包括触发事件的元素、事件类型、事件发生的位置等。事件对象可以非常有用，比如在游戏开发中，可以用来获取鼠标点击的位置。

- 通过事件对象，开发者可以详细了解事件发生时的具体信息，从而对用户的行为做出更精准的响应。

**示例代码：**

```javascript
var el = document.querySelector('.btn');
el.onclick = function(e){
  console.log(e);  // 打印事件对象信息
};
```

### 事件对象的常见属性和方法

以下是常见的事件对象属性和方法，它们适用于不同类型的事件（如鼠标、键盘、触摸事件等）：

#### 1. 常用属性

- **`type`**: 返回事件的类型，表示事件的名称，如 `click`、`keydown` 等。
  
  ```javascript
  element.addEventListener('click', function(event) {
    console.log(event.type);  // 输出 'click'
  });
  ```

- **`target`**: 返回触发事件的目标元素。即事件发生时所作用的元素。
  
  ```javascript
  element.addEventListener('click', function(event) {
    console.log(event.target);  // 输出被点击的元素
  });
  ```

- **`currentTarget`**: 返回当前正在处理事件的元素。和 `target` 不同，它总是指向绑定事件处理器的那个元素。

- **`bubbles`**: 返回一个布尔值，表示事件是否会冒泡。冒泡是指事件从子元素传递到父元素的过程。

- **`cancelable`**: 返回一个布尔值，表示事件是否可以被取消（即可以使用 `preventDefault()` 来阻止浏览器的默认行为）。

- **`defaultPrevented`**: 返回一个布尔值，表示是否调用了 `preventDefault()` 来阻止默认事件行为。

#### 2. 鼠标事件特有属性

适用于鼠标相关的事件，如 `click`、`mousemove` 等。

- **`clientX`** 和 **`clientY`**: 返回事件发生时鼠标相对于浏览器窗口左上角的水平和垂直坐标（以像素为单位）。
  
  ```javascript
  element.addEventListener('click', function(event) {
    console.log(event.clientX, event.clientY);  // 输出鼠标点击的坐标
  });
  ```

- **`pageX`** 和 **`pageY`**: 返回事件发生时鼠标相对于文档左上角的水平和垂直坐标（包括滚动距离）。

- **`button`**: 返回按下的鼠标按钮编号，`0` 表示左键，`1` 表示中键，`2` 表示右键。

#### 3. 键盘事件特有属性

适用于键盘事件，如 `keydown`、`keypress`、`keyup`。

- **`key`**: 返回按下的键的值（如 `Enter`、`a`、`1` 等），可以区分大小写。

  ```javascript
  document.addEventListener('keydown', function(event) {
    console.log(event.key);  // 输出按下的键名
  });
  ```

- **`code`**: 返回按下的物理按键的代码，如 `KeyA`、`Enter`、`ArrowUp` 等，和键盘布局无关。

- **`altKey`**、**`ctrlKey`**、**`shiftKey`**、**`metaKey`**: 返回一个布尔值，表示是否按下了对应的功能键（Alt、Ctrl、Shift 或 Meta 键）。

#### 4. 方法

- **`preventDefault()`**: 阻止浏览器执行与事件关联的默认行为。例如，在表单 `submit` 事件中阻止页面刷新。

  ```javascript
  form.addEventListener('submit', function(event) {
    event.preventDefault();  // 阻止表单的默认提交行为
  });
  ```

- **`stopPropagation()`**: 阻止事件冒泡，使事件不会传递到父元素。

  ```javascript
  element.addEventListener('click', function(event) {
    event.stopPropagation();  // 阻止事件冒泡
  });
  ```

- **`stopImmediatePropagation()`**: 除了阻止事件冒泡，还会阻止该元素上当前类型的其他事件处理器执行。

### 事件对象总结

事件对象为开发者提供了详细的事件信息，常见的事件对象包括：
- **`MouseEvent`**: 处理鼠标事件，如点击、移动、双击等。
- **`KeyboardEvent`**: 处理键盘事件，如按键按下、松开等。
- **`TouchEvent`**: 处理触摸事件，常用于移动设备。
- **`FocusEvent`**: 处理焦点事件，如 `focus`、`blur` 等。


## 事件绑定的不同方式

**事件绑定** 是在 JavaScript 中将事件处理函数（回调函数）与特定的 DOM 元素上的事件关联起来的过程。当该事件发生时，浏览器会执行与之绑定的处理函数。

1. **内联事件处理器（Inline Event Handlers）**

   不推荐使用，因为它们容易导致安全问题，如跨站脚本攻击（XSS）。

   ```html
   <input onclick="alert('say Hello')" type="button" class="btn" value="点击">
   ```

2. **属性级事件处理器（On-event Handlers）**

   每个事件只能绑定一个事件处理函数。如果尝试绑定多个处理函数，之前的会被覆盖。

   ```javascript
   var btn = document.querySelector('.btn');
   btn.onclick = function() {
     console.log('Clicked!');
   };
   ```

## 事件监听器（Event Listeners）

   推荐使用 `addEventListener` 方法，因为它允许在同一个元素上绑定多个事件处理器，且更灵活。

   **基本用法：**

   ```javascript
   var el = document.querySelector('.btn');
   el.addEventListener('click', function(e) {
     alert('Hello');
   }, false);
   ```

   **参数解释：**
   - **事件名称**：如 `"click"`，不需要加 `"on"` 前缀。
   - **回调函数**：事件被触发时调用的函数。
   - **事件传播模式**：`false` 表示冒泡阶段监听（默认），`true` 表示捕获阶段监听。

## 事件传播的三个阶段

1. **捕获阶段（capture phase）：**
   - 从 `window` 对象传导到目标节点（从上层传到底层）。
   
2. **目标阶段（target phase）：**
   - 在目标节点上触发事件。
   
3. **冒泡阶段（bubbling phase）：**
   - 从目标节点传回 `window` 对象（从底层传回上层）。
   
这种三阶段的传播模型使得同一个事件会在多个节点上触发。

```html
<!-- 样式部分 -->
<style>
  #box {
    width: 200px;
    height: 200px;
    background-color: wheat;
    cursor: pointer;
    position: relative;
  }
  
  #btn {
    width: 90px;
    height: 60px;
    cursor: pointer;
    border: none;
    position: absolute;
  }
</style>

<!-- 脚本部分 -->
<script>
  var box = document.getElementById('box');
  var btn = document.getElementById('btn');

  // 冒泡阶段事件监听
  box.addEventListener('click', function () {
    console.log('冒泡阶段触发的box');
  }, false);

  btn.addEventListener('click', function () {
    console.log('冒泡阶段触发的btn');
  }, false);

  // 捕获阶段事件监听
  btn.addEventListener('click', function () {
    console.log('捕获阶段触发的btn');
  }, true);

  box.addEventListener('click', function () {
    console.log('捕获阶段触发的box');
  }, true);

  // 打印事件路径
  document.body.addEventListener('click', function (e) {
    console.log(e.composedPath());  // 事件冒泡阶段经过的所有节点对象数组
  });
</script>
```

大多数情况下，我们使用事件冒泡模式，因为这更符合常见的事件处理逻辑。

## 事件委托（事件代理）

**目的：**
通过事件代理，我们可以只在父节点上注册事件监听器，而不必为每个子节点都注册，子节点触发的事件会冒泡到父节点，由父节点处理。

**例子：**
当点击 `li` 元素内容时，控制台打印相关内容，例如点击 `item2`，则在控制台打印 `item2`。

```html
<script>
  var lis = document.querySelectorAll('li');

  // 使用事件代理进行监听
  document.querySelector('ul').addEventListener('click', function(e) {
    if (e.target && e.target.nodeName === 'LI') {
      console.log(e.target.textContent);  // 打印点击的li内容
    }
  });
</script>
```

- 当然，如果只希望子节点触发事件而不希望父事件触发，可以通过if方式过滤事件的触发

## 事件传播与停止

在事件传播过程中，如果需要阻止事件继续传播，可以使用以下两种方法：

1. **`stopPropagation()`**：阻止事件继续传播到下一个节点。
2. **`stopImmediatePropagation()`**：不仅阻止事件传播，还会阻止同一元素上当前事件的其他处理程序。也就是同元素同类型同阶段的监听函数被彻底取消该事件

**例子：**
捕获阶段被阻止，冒泡事件不能正常发生；同元素同类型同阶段的监听函数，如果想要彻底取消该事件，可以使用 `stopImmediatePropagation()` 方法。

```html
<script>
  var box = document.getElementById('box');
  var btn = document.getElementById('btn');

  // 捕获阶段阻止事件传播
  btn.addEventListener('click', function (e) {
    e.stopImmediatePropagation();  // 阻止同阶段同类型的其他事件处理器
    console.log('捕获阶段触发的btn');
  }, true);

  box.addEventListener('click', function (e) {
    console.log('捕获阶段触发的box');
  }, true);

  // 冒泡阶段监听
  btn.addEventListener('click', function (e) {
    console.log('冒泡阶段触发的btn');
  }, false);

  box.addEventListener('click', function (e) {
    console.log('冒泡阶段触发的box');
  }, false);
</script>
```

**总结：**
- 捕获和冒泡是事件传播的两个主要阶段，事件委托通过冒泡机制来简化事件处理。
- 可以使用 `stopPropagation()` 和 `stopImmediatePropagation()` 来控制事件传播的行为。

## 鼠标事件

### 点击事件

在网页开发中，鼠标的点击动作会触发四个主要事件：

- `mousedown`: 按下鼠标键时触发。
- `mouseup`: 释放按下的鼠标键时触发。
- `click`: 按下并释放鼠标键时触发。它可以看作是 `mousedown` 和 `mouseup` 事件组合形成的。
- `dblclick`: 双击鼠标时触发。

#### 触发顺序：
1. `mousedown`
2. `mouseup`
3. `click`
4. `dblclick` (如果有双击)

这些事件的触发顺序意味着 `mousedown` 和 `mouseup` 必定先于 `click`，`dblclick` 最后触发。

### 移动事件

当鼠标在页面上移动时，也会触发一系列事件，常用的有以下几种：

- `mousemove`: 鼠标在一个节点内或节点内部移动时会频繁触发。如果没有设置限流，移动过程中会不断触发此事件。可以通过定时器限制触发频率
- `mouseenter`: 鼠标从外部进入一个节点时触发，不冒泡。
- `mouseover`: 鼠标从外部进入一个节点或子节点时触发，冒泡。
- `mouseleave`: 鼠标离开一个节点时触发，不冒泡。
- `mouseout`: 当鼠标从内外离开一个节点或子节点时触发，冒泡。


### 键盘属性

当鼠标事件与键盘修饰键（如 Ctrl、Alt、Shift 等）组合使用时，开发者可以获取以下属性：

- `altKey`: 是否按下了 `Alt` 键。
- `ctrlKey`: 是否按下了 `Ctrl` 键。
- `metaKey`: 是否按下了 `Meta` 键（在 Mac 中是 Command 键，在 Windows 中是 Windows 键）。
- `shiftKey`: 是否按下了 `Shift` 键。

例如，可以监听 `mousedown` 事件并检测这些键是否被按下，以实现更多组合操作的可能性：

```javascript
btn.addEventListener('mousedown', function (e) {
    console.log(e.altKey);    // 输出是否按下了Alt键
    console.log(e.ctrlKey);   // 输出是否按下了Ctrl键
    console.log(e.metaKey);   // 输出是否按下了Meta键
    console.log(e.shiftKey);  // 输出是否按下了Shift键
});
```

### 鼠标按钮属性

- `button`: 返回触发鼠标事件的鼠标键，值为：
  - `0`: 表示左键。
  - `1`: 表示中键（滚轮）。
  - `2`: 表示右键。

这个属性可以用来检测用户点击了哪个鼠标按钮。

###  坐标属性

鼠标事件中提供了几个用于确定鼠标位置的属性：

- `clientX` 和 `clientY`: 返回鼠标相对于浏览器窗口左上角的水平和垂直距离。这两个属性都是只读属性
  
- `pageX` 和 `pageY`: 返回鼠标相对于整个文档左上角的水平和垂直距离。这些属性包括了不可见的滚动区域。

- page针对于整个的页面，而client只针对于当前页面

```javascript
function showCoords(e) {
    console.log(e.clientX, e.clientY); // 输出鼠标相对浏览器窗口的坐标
    console.log(e.pageX, e.pageY) // 输出鼠标页面窗口的坐标
}
```

假设页面没有滚动，鼠标点击了页面中距离视口顶部 200 像素的地方：

- `clientY = 200`
- `pageY = 200`

现在如果你向下滚动了 100 像素后，再点击同样的地方：

- `clientY = 200`（相对视口的位置不变）
- `pageY = 300`（相对于页面顶部的位置变化了，因为页面向下滚动了 100 像素）

--- 

- `offsetX` 和 `offsetY`: 返回鼠标相对于目标元素左上角的水平和垂直距离，单位是像素。这个属性通常用于在元素内部进行精确定位。这两个属性都是只读属性

```javascript
box.addEventListener('mousedown', function(e) {
    console.log(e.offsetX, e.offsetY); // 输出鼠标相对于元素的坐标
});
```

### 示例

以下是一个示例的 HTML 结构及样式，展示了如何获取鼠标点击事件中的坐标信息：

```html
<style>
  #box {
    width: 300px;
    height: 300px;
    background-color: wheat;
    padding: 30px;
    border: 30px solid red;
    background-clip: content-box;
  }
</style>

<div id="box"></div>

<script>
  var box = document.getElementById('box');
  box.addEventListener('mousedown', function (e) {
    console.log(e.offsetX, e.offsetY); // 获取鼠标点击位置相对于 box 元素的坐标
  });
</script>
```

在这个例子中，当用户点击 `#box` 元素时，控制台会输出鼠标点击的位置相对于 `#box` 的左上角的水平和垂直距离。

## 键盘事件

### 键盘事件类型

在网页开发中，键盘事件可以分为三种主要类型：

- `keydown`: 用户按下任意键时触发。
- `keyup`: 用户释放任意键时触发。
- `keypress`: 用户按下键盘上的字符键时触发（已废弃，推荐使用 `keydown` 代替）。

```javascript
document.body.addEventListener('keydown', function () {
    console.log('keydown');
});

document.body.addEventListener('keyup', function () {
    console.log('keyup');
});

document.body.addEventListener('keypress', function () {
    console.log('keypress');
});
```

**补充说明**：  
- `keypress` 事件已不推荐使用，现代浏览器中推荐使用 `keydown` 和 `keyup` 来处理所有按键操作。
- 与 `keydown` 和 `keyup` 不同，`keypress` 仅触发字符键事件，无法检测功能键（如 F1-F12、Shift 等）。

### 键盘事件的属性

键盘事件的 `event` 对象包含了多个用于处理按键的属性，常用的有以下几种：

- `code`: 返回物理键盘的按键代码，表示按下的键的固定格式（区分大小写）。

  - 数字键 `0-9`: 返回 `Digit0` 到 `Digit9`。
  - 字母键 `A-Z`: 返回 `KeyA` 到 `KeyZ`。
  - 功能键 `F1-F12`: 返回 `F1` 到 `F12`。
  - 方向键: 返回 `ArrowUp`、`ArrowDown`、`ArrowLeft`、`ArrowRight`。
  - `Alt` 键: 返回 `AltLeft` 或 `AltRight`。
  - `Shift` 键: 返回 `ShiftLeft` 或 `ShiftRight`。
  - `Ctrl` 键: 返回 `ControlLeft` 或 `ControlRight`。

```javascript
document.body.addEventListener('keydown', function (e) {
    console.log(e.code);  // 输出物理键盘按键代码
});
```

- `key`: 返回按下的键对应的字符，不区分大小写。

  - 如果按下的是可打印的字符，则返回该字符。
  - 如果按下的是不可打印的特殊符号，则返回键名（如 `Shift`、`Control`、`Alt` 等）。

```javascript
document.body.addEventListener('keydown', function (e) {
    console.log(e.key);  // 输出按下的字符
});
```

#### `code` 和 `key` 的区别：
- `code` 返回物理按键位置，因此大小写状态不同，返回的值是固定的。
- `key` 返回按下的字符值，受键盘状态（如大小写锁定键、语言输入法等）的影响。

### 键盘修饰键的相关属性

修饰键是指 `Ctrl`、`Alt`、`Shift`、`Meta`（Windows 键或 Command 键）等，它们可以与其他键组合使用来触发不同的功能。

- `altKey`: 是否按下了 `Alt` 键。
- `ctrlKey`: 是否按下了 `Ctrl` 键。
- `metaKey`: 是否按下了 `Meta` 键（Mac 是 Command 键，Windows 是 Windows 键）。
- `shiftKey`: 是否按下了 `Shift` 键。

```javascript
document.body.addEventListener('keydown', function (e) {
    console.log(e.altKey);   // 输出是否按下了Alt键
    console.log(e.ctrlKey);  // 输出是否按下了Ctrl键
    console.log(e.metaKey);  // 输出是否按下了Meta键
    console.log(e.shiftKey); // 输出是否按下了Shift键
});
```

这些属性可以用来检测是否按下了某个修饰键，从而实现键盘组合键的功能。

### 示例：结合 `keydown` 和修饰键

通过监听 `keydown` 事件并结合修饰键，可以实现更多的组合键操作，例如检测 `Ctrl + A` 这样的操作。

```javascript
document.body.addEventListener('keydown', function (e) {
    if (e.ctrlKey && e.key === 'a') {
        console.log('Ctrl + A 被按下');
    }
});
```


## 表单事件

### 表单的输入事件
表单中的输入字段在用户输入内容时会触发多个事件，常见的事件包括：
- `input`: 用户在输入框中每次输入时都会触发，适用于监听内容的实时变化。
- `change`: 当输入框的内容发生改变并且失去焦点时触发。

```html
<input type="text" id="txt">
<script>
var txt = document.getElementById('txt');
var count1 = 0;
txt.addEventListener('input', function (e) {
    count1++;
    console.log(count1);  // 记录输入事件触发次数
    console.log(e.target.value);  // 输出当前输入框的值
});

var count2 = 0;
txt.addEventListener('change', function (e) {
    count2++;
    console.log(count2);  // 记录改变事件触发次数
    console.log(e.target.value);  // 输出当前输入框的值
});
</script>
```

**补充说明**：
- `input` 事件适合监听输入框的实时变化，每输入一个字符都会触发。
- `change` 事件则在输入完成并失去焦点时触发。

### 焦点事件
表单元素的焦点事件包括：
- `focus`: 当元素获得焦点时触发。
- `blur`: 当元素失去焦点时触发。

```html
<input type="text" id="txt">
<script>
var txt = document.getElementById('txt');
txt.addEventListener('focus', function (e) {
    console.log('focus');  // 元素获得焦点时触发
    console.log(e.target.value);
});

txt.addEventListener('blur', function (e) {
    console.log('blur');  // 元素失去焦点时触发
    console.log(e.target.value);
});
</script>
```

**补充说明**：
- `focus` 和 `blur` 事件不会冒泡（即事件不会传递到父元素）。

### 提交事件和重置事件
表单的提交和重置行为可以通过 `submit` 和 `reset` 事件进行控制。
- `submit`: 当表单被提交时触发，默认会刷新页面。
- `reset`: 当表单被重置时触发。

```html
<form action="./02-表单事件2.html">
    <input type="text" required id="txt">
    <button type="submit">提交</button>
    <button type="reset">重置</button>
</form>

<script>
var form = document.forms[0];
form.addEventListener('submit', function (e) {
    e.preventDefault();  // 阻止表单的默认提交行为
    console.log('表单提交');
    console.log(e.target);  // 输出提交的表单元素
});

form.addEventListener('reset', function (e) {
    console.log('表单重置');
    console.log(e.target);  // 输出重置的表单元素
});
</script>
```

**补充说明**：
- 在处理 `submit` 事件时，通常需要调用 `e.preventDefault()` 来阻止默认的表单提交行为（如页面刷新）。
- `reset` 事件触发时，表单的内容将被重置为初始值。

### 验证事件
表单字段验证的相关事件：
- `invalid`: 当输入框未通过验证时触发。

```html
<form action="./02-表单事件2.html">
    <input type="text" required id="txt">
    <button type="submit">提交</button>
    <button type="reset">重置</button>
</form>

<script>
var txt = document.getElementById('txt');
txt.addEventListener('invalid', function (e) {
    console.log(e.target);  // 输出未通过验证的元素
    console.log('必须输入至少一个字符');
});
</script>
```

**补充说明**：
- `invalid` 事件仅在表单提交时触发，如果表单字段未通过浏览器的内置验证，则会显示默认的错误提示信息。



## 事件循环（Event Loop）

### 理解

JavaScript 是单线程语言，但能够异步处理大量高并发请求，这主要依赖于 **事件循环机制**。

#### JavaScript 单线程

JavaScript 是运行在浏览器的 **渲染进程** 中的 **主线程** 上，所以它是单线程执行的。这意味着所有任务（包括同步和异步任务）都是在同一个线程中按顺序执行的。

#### 同步任务与异步任务

- **同步任务**：立即执行的任务，在主线程上排队执行，形成一个 **执行栈**。当前一个任务执行完毕后，才能继续执行下一个任务。
- **异步任务**：不进入主线程，而是进入 **任务队列**（Message Queue）中的任务。只有在主线程中的同步任务全部执行完毕后，任务队列中的异步任务才会被执行。

#### 任务队列

任务队列分为两类：**宏任务队列（Macrotask Queue）** 和 **微任务队列（Microtask Queue）**。

- **宏任务**：包含需要较长时间完成的任务，例如 `setTimeout`、`setInterval`、UI渲染任务、I/O操作、Ajax请求等。
- **微任务**：包含执行时间较短的任务，例如 `Promise.then()`、`catch()`、`async/await`、`MutationObserver`、`process.nextTick()`（Node.js 环境）。

#### 异步的核心：事件循环机制

- **事件循环是JavaScript实现异步的一种方法，也是JavaScript的执行机制。事件循环又叫消息循环，是浏览器渲染主线程的工作方式。**
- 当 JS 遇到异步任务时，它会将该任务交给其他进程或线程去处理，而不阻塞主线程。当异步任务完成后，相关的回调函数会被放入任务队列中等待执行。当 **执行栈** 清空（即同步任务执行完毕）后，事件循环机制将检查任务队列中的异步任务，并将它们加入主线程执行。

### 为什么需要异步？

如果 JavaScript 中不存在异步机制，所有任务只能按照自上而下的顺序执行。这意味着如果某个任务耗时较长，整个页面将会被阻塞，无法响应其他操作，从而影响用户体验。因此，引入异步机制的目的是确保页面流畅运行，避免长时间任务阻塞主线程。

### 事件循环机制详解

1. **同步任务**：JavaScript 先执行所有同步任务，它们在执行栈（Call Stack）中按顺序执行。
2. **遇到异步任务**：当遇到异步任务时，JavaScript 不会立即执行它们，而是将它们挂起，并交给 Web APIs 或其他线程处理，自己继续执行剩下的同步任务。
3. **执行异步任务的回调**：当异步任务完成时，对应的回调函数会被放入任务队列中。等到同步任务执行完毕后，JavaScript 引擎会检查 **微任务队列** 和 **宏任务队列** 中是否有需要执行的任务。

#### 执行顺序

1. 先执行所有同步任务，属于宏任务（Macrotask）。
2. 当执行完同步任务后，检查 **微任务队列** 中是否有任务。如果有，依次执行。
3. 如果微任务队列为空，执行下一个 **宏任务**。
4. 在执行宏任务时，如果宏任务产生了新的 **微任务**，这些微任务会被立即加入微任务队列中，确保在当前循环中执行。
5. 不断重复此过程，直到所有任务执行完毕。

![](https://img2018.cnblogs.com/i-beta/1402448/201912/1402448-20191205160502748-1705087452.png)


### 注意事项

- 所有的代码都要通过函数执行栈（主线程）中调用执行。
- 等到执行栈中的task执行完之后再回去执行任务队列之中的task。
- 任务队列中存放的是回调函数。
- 执行微任务过程中产生的新的微任务并不会推迟到下一个循环中执行，而是在当前的循环中继续执行。
- 当执行一个宏任务时，如果宏任务中产生了新的微任务，这些微任务不会立刻执行，而是会被放入到当前微任务队列中，在当前宏任务执行完毕后被依次执行。
- 微任务是一个需要异步执行的函数，可以通过Promise.reject 或者Promise.resolve 来触发，执行时机是在当前调用 reject 或者 resolve 函数执行结束之后、当前宏任务结束之前

### 示例代码

```js
setTimeout(function () {
  console.log('定时器setTimeout');
}, 0);

new Promise(function (resolve) {
  console.log('同步任务1');
  for (var i = 0; i < 10000; i++) {
    i == 99 && resolve();
  }
}).then(function () {
  console.log('Promise.then');
});

this.$nextTick(() => {
  console.log('nextTick');
});

console.log('同步任务2');
```

### 分析执行过程

1. 先执行 `script` 下的同步代码，遇到 `setTimeout`，0秒后定时器任务进入宏任务队列中。
2. 遇到 `new Promise`，立即执行，输出 `同步任务1`。
3. `then` 方法属于微任务，放入微任务队列。
4. 遇到 `$nextTick`，该方法也是微任务，将其放入微任务队列。
5. 输出 `同步任务2`。
6. 同步任务执行完毕后，检查微任务队列，发现 `nextTick` 和 `then`，依次执行，输出 `nextTick` 和 `Promise.then`。
7. 第一轮事件循环结束。
8. 第二轮事件循环执行 `setTimeout` 回调，输出 `定时器setTimeout`。

