---
Project: "[[web前端]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 
Connected:
---

## `<script>`元素
`<script>`元素有下列8个属性。

- *async* 
- charset
- crossorigin
- defer
- integrity
- language
- src
- type


在这段代码中的 `解构赋值` 语法出现在 `set(newValue)` 这个方法中：

```javascript
set(newValue) {
  [firstName.value, lastName.value] = newValue.split(' ')
}
```

解构赋值是一种简洁的语法，允许你从数组或对象中提取值，并将其赋给变量。

### 代码中的解构赋值

1. **`newValue.split(' ')`**：  
   `newValue` 是 `set` 方法的参数，这里使用了 `split(' ')`，它会将字符串按空格分割成一个数组。例如，如果 `newValue` 是 `'Jane Doe'`，那么 `newValue.split(' ')` 会返回 `['Jane', 'Doe']`。

2. **解构数组**：
   通过使用解构赋值语法 `[firstName.value, lastName.value]`，我们可以从这个数组中一次性提取出两个值，并分别赋给 `firstName.value` 和 `lastName.value`。这相当于手动逐个赋值：
   ```javascript
   const arr = newValue.split(' ');
   firstName.value = arr[0];
   lastName.value = arr[1];
   ```

   而使用解构赋值可以简化为一行代码：
   ```javascript
   [firstName.value, lastName.value] = newValue.split(' ')
   ```

### 解构赋值的基本概念
- **数组解构**：你可以通过数组解构来将数组中的元素分别赋值给多个变量。
  ```javascript
  const arr = [1, 2, 3];
  const [a, b, c] = arr; // a = 1, b = 2, c = 3
  ```

- **对象解构**：你也可以对对象进行解构赋值，从对象中提取指定属性并赋给变量。
  ```javascript
  const person = { name: 'Alice', age: 25 };
  const { name, age } = person; // name = 'Alice', age = 25
  ```

### 解释这段代码的执行过程
当你给 `fullName` 赋一个新值时，`set(newValue)` 方法会被调用。例如：
```javascript
fullName.value = 'Jane Doe';
```
`newValue` 是 `'Jane Doe'`，所以 `newValue.split(' ')` 返回 `['Jane', 'Doe']`。通过解构赋值，`firstName.value` 会变为 `'Jane'`，`lastName.value` 会变为 `'Doe'`。

这就是解构赋值在这段代码中的作用，它简化了从数组中提取值并赋值的过程。

## event对象
在 JavaScript 中，`event` 对象是浏览器中与 DOM 事件相关的一个核心对象，它包含了与事件有关的各种信息。当用户在页面上执行某些操作（例如点击、键盘输入、鼠标移动等）时，浏览器会触发特定的事件，并且会生成一个 `event` 对象传递给事件处理函数。

### 1. **`event` 对象**
   - **`event` 对象**（有时被称为事件对象）是浏览器在事件触发时自动创建并传递给事件处理器的对象。它封装了与事件相关的所有信息，例如触发事件的元素、鼠标的位置、按下的键、是否按下了修饰键（如 `Shift`、`Alt` 等）等。
   - 常见的 `event` 对象属性有：
     - `target`：触发事件的 DOM 元素。
     - `type`：事件类型（例如 `"click"`、`"keydown"`）。
     - `clientX`、`clientY`：鼠标事件中，鼠标指针相对于视口的 X 和 Y 坐标。
     - `key`：键盘事件中，按下的键。

   例如：
   ```javascript
   document.addEventListener('click', function(event) {
     console.log(event.target); // 输出触发点击事件的元素
     console.log(event.type);   // 输出 'click'
   });
   ```

### 2. **事件类型和事件对象的种类**
   JavaScript 中有很多不同类型的事件，每个事件类型会产生相应的事件对象。虽然它们都是从 `Event` 基类继承的，但不同的事件对象有不同的属性和方法，适应具体的事件类型。
   
   - **鼠标事件（MouseEvent）**：当鼠标在页面上发生操作时，会产生 `MouseEvent` 对象。常见的鼠标事件包括 `click`、`mousedown`、`mousemove` 等。
     ```javascript
     document.addEventListener('click', function(event) {
       console.log(event.clientX, event.clientY); // 鼠标点击的坐标
     });
     ```

   - **键盘事件（KeyboardEvent）**：当用户按下键盘上的按键时，会产生 `KeyboardEvent` 对象。常见的键盘事件包括 `keydown`、`keyup`。
     ```javascript
     document.addEventListener('keydown', function(event) {
       console.log(event.key); // 输出按下的键
     });
     ```

   - **焦点事件（FocusEvent）**：当元素获得或失去焦点时，会产生 `FocusEvent` 对象，常见的事件包括 `focus`、`blur`。
   
   - **表单事件（InputEvent/ChangeEvent）**：当表单中的元素（如输入框、下拉列表等）发生变化时，产生 `InputEvent` 或 `ChangeEvent` 对象。
   
   - **触摸事件（TouchEvent）**：在触摸屏设备上，当用户使用触摸操作时，产生 `TouchEvent` 对象。例如 `touchstart`、`touchmove`、`touchend` 等。

### 3. **是否有类似于 `event` 的其他事件元素？**
   `event` 是事件处理函数中常见的参数，表示当前事件对象，但实际上还有其他与事件相关的元素（对象或方法）可以参与到事件处理机制中。

   - **`currentTarget` vs `target`**：
     - `event.target`：指向触发事件的元素，即事件发生的真正目标元素。
     - `event.currentTarget`：指向绑定事件处理器的元素。比如在事件委托机制中，`currentTarget` 可以不同于 `target`。

     例如：
     ```javascript
     document.getElementById('parent').addEventListener('click', function(event) {
       console.log(event.target); // 点击的具体子元素
       console.log(event.currentTarget); // 始终是 parent 元素
     });
     ```

   - **`stopPropagation` 和 `preventDefault`**：
     - `stopPropagation()`：阻止事件冒泡，使得事件不再继续传递到父级元素。
     - `preventDefault()`：阻止默认行为。例如，阻止表单提交或者链接跳转。

     这些方法也是 `event` 对象的一部分，用于控制事件的行为。

### 总结
- `event` 是 JavaScript 中用来表示用户和页面交互的事件对象，封装了事件的各种信息。
- JavaScript 提供了多种事件对象（例如 `MouseEvent`、`KeyboardEvent`、`TouchEvent` 等）来处理不同类型的事件。
- 在事件处理过程中，还可以通过 `currentTarget`、`target` 等属性区分事件发生的元素和绑定事件的元素，以及使用 `stopPropagation` 和 `preventDefault` 等方法控制事件行为。