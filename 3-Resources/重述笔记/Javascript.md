---
Project: "[[web前端]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-06-23
Connected:
---

## 1. 变量
### 1. `var`

`var`是JavaScript中最早使用的变量声明方式，但它有一些缺点，因此在现代JavaScript开发中不推荐使用。

  

#### 特点：

- **函数作用域**：`var`声明的变量在函数内是局部的，但在其他地方是全局的。

- **变量提升**：`var`声明的变量会被提升到其作用域的顶部，但不会初始化。

  

#### 示例：

```javascript

function example() {

    console.log(x); // 输出: undefined

    var x = 10;

    console.log(x); // 输出: 10

}

example();

```

在上面的代码中，`var x`会被提升到函数的顶部，但初始化仍然在原来的位置。

  

### 2. `let`

`let`是ES6（ECMAScript 2015）引入的，用于声明块级作用域的变量。

  

#### 特点：

- **块级作用域**：`let`声明的变量在其所在的块内是局部的。

- **不允许重复声明**：在同一个作用域内，不能使用`let`重复声明同一个变量名。

- **不会变量提升**：`let`声明的变量不会被提升到块的顶部。

  

#### 示例：

```javascript

function example() {

    if (true) {

        let y = 20;

        console.log(y); // 输出: 20

    }

    console.log(y); // 报错: y is not defined

}

example();

```

在上面的代码中，`y`只在`if`块内有效，块外访问会报错。

  

### 3. `const`

`const`也是ES6引入的，用于声明常量。常量的值一旦声明就不能改变。

  

#### 特点：

- **块级作用域**：与`let`相同，`const`声明的变量在其所在的块内是局部的。

- **不允许重新赋值**：`const`声明的变量必须在声明时初始化，且不能重新赋值。

- **不允许重复声明**：与`let`相同，不能使用`const`重复声明同一个变量名。

  

#### 示例：

```javascript

function example() {

    const z = 30;

    console.log(z); // 输出: 30

    z = 40; // 报错: Assignment to constant variable.

}

example();

```

在上面的代码中，尝试重新赋值给`z`会导致错误。

  

### 总结

- **`var`**：具有函数作用域和变量提升，不推荐在现代开发中使用。

- **`let`**：具有块级作用域，适用于需要重新赋值的变量。

- **`const`**：具有块级作用域，适用于常量声明，不可重新赋值。

  

### 示例代码

以下是一个综合示例，展示了`var`、`let`和`const`的使用：

```javascript

function example() {

    var a = 1;

    let b = 2;

    const c = 3;

  

    if (true) {

        var a = 10; // 重新声明和赋值

        let b = 20; // 块级作用域内的新变量

        const c = 30; // 块级作用域内的新常量

        console.log(a); // 输出: 10

        console.log(b); // 输出: 20

        console.log(c); // 输出: 30

    }

  

    console.log(a); // 输出: 10 (var变量提升并重新赋值)

    console.log(b); // 输出: 2 (let变量在块外不受影响)

    console.log(c); // 输出: 3 (const变量在块外不受影响)

}

example();

```

  

希望这些解释能帮助你更好地理解JavaScript中的变量。如果还有其他问题或需要更详细的解释，请告诉我！
  

## 2. 数据类型

### 基本类型（Primitive Types）

基本类型是不可变的（即值本身不能被改变），包括以下几种：

  

1. **Number**

   - 表示数字，包括整数和浮点数。

   - 特殊值：`NaN`（Not-a-Number），`Infinity`和`-Infinity`。

   ```javascript

   let intNum = 42;      // 整数

   let floatNum = 3.14;  // 浮点数

   let notANumber = NaN; // 非数字

   let infinity = Infinity; // 无穷大

   ```

  

2. **String**

   - 表示字符串，可以使用单引号、双引号或反引号（模板字符串）。

   ```javascript

   let singleQuoteStr = 'Hello';

   let doubleQuoteStr = "World";

   let templateStr = `Hello, ${singleQuoteStr} ${doubleQuoteStr}!`; // 模板字符串

   ```

  

3. **Boolean**

   - 表示布尔值，只有两个可能的值：`true`和`false`。

   ```javascript

   let isTrue = true;

   let isFalse = false;

   ```

  

4. **Null**

   - 表示空值，只有一个值：`null`。

   ```javascript

   let emptyValue = null;

   ```

  

5. **Undefined**

   - 表示未定义的值，变量声明但未赋值时默认值为`undefined`。

   ```javascript

   let undefinedValue;

   console.log(undefinedValue); // 输出: undefined

   ```

  

6. **Symbol**（ES6引入）

   - 表示唯一的标识符，主要用于对象属性的唯一性。

   ```javascript

   let symbol1 = Symbol();

   let symbol2 = Symbol('description');

   console.log(symbol1 === symbol2); // 输出: false

   ```

  

7. **BigInt**（ES11引入）

   - 表示任意精度的整数，用于处理超过`Number`类型安全范围的整数。

   ```javascript

   let bigIntNum = 1234567890123456789012345678901234567890n;

   let anotherBigInt = BigInt(1234567890123456789012345678901234567890);

   ```

  

### 引用类型（Reference Types）

引用类型是可变的，主要包括对象（Object）、数组（Array）和函数（Function）等。

  

1. **Object**

   - 表示键值对的集合。

   ```javascript

   let person = {

       name: "Alice",

       age: 25,

       greet: function() {

           console.log("Hello, " + this.name);

       }

   };

   console.log(person.name); // 输出: Alice

   person.greet(); // 输出: Hello, Alice

   ```

  

2. **Array**

   - 表示有序的值的集合，数组的元素可以是任何类型。

   ```javascript

   let numbers = [1, 2, 3, 4, 5];

   let mixedArray = [1, "two", true, null, undefined, {name: "Alice"}];

   console.log(numbers[0]); // 输出: 1

   console.log(mixedArray[5].name); // 输出: Alice

   ```

  

3. **Function**

   - 表示可执行的代码块，函数也是对象，可以赋值给变量或作为参数传递。

   ```javascript

   function add(a, b) {

       return a + b;

   }

   let sum = add(2, 3);

   console.log(sum); // 输出: 5

  

   let multiply = function(a, b) {

       return a * b;

   };

   console.log(multiply(2, 3)); // 输出: 6

   ```

  

4. **Date**

   - 表示日期和时间。

   ```javascript

   let now = new Date();

   console.log(now); // 输出当前日期和时间

   ```

  

5. **RegExp**

   - 表示正则表达式，用于模式匹配。

   ```javascript

   let pattern = /hello/;

   let str = "hello world";

   console.log(pattern.test(str)); // 输出: true

   ```

  

6. **Map**

   - 表示键值对的集合，键可以是任何类型。

   ```javascript

   let map = new Map();

   map.set('key1', 'value1');

   map.set('key2', 'value2');

   console.log(map.get('key1')); // 输出: value1

   ```

  

7. **Set**

   - 表示值的集合，每个值都是唯一的。

   ```javascript

   let set = new Set();

   set.add(1);

   set.add(2);

   set.add(1); // 重复值不会添加

   console.log(set.size); // 输出: 2

   ```

  

### 总结

JavaScript的数据类型丰富且灵活，了解并熟练掌握这些数据类型是编写高效和健壮代码的基础。以下是一个综合示例，展示了各种数据类型的使用：

  

```javascript

// 基本类型

let num = 42;

let str = "Hello, World!";

let bool = true;

let nothing = null;

let notDefined;

let uniqueId = Symbol('id');

let bigIntNum = 1234567890123456789012345678901234567890n;

  

// 引用类型

let obj = { name: "Alice", age: 25 };

let arr = [1, 2, 3, 4, 5];

let func = function() { console.log("Hello!"); };

let date = new Date();

let regex = /hello/;

let map = new Map([['key1', 'value1'], ['key2', 'value2']]);

let set = new Set([1, 2, 3, 4, 5]);

  

console.log(num, str, bool, nothing, notDefined, uniqueId, bigIntNum);

console.log(obj, arr, func, date, regex, map, set);

```

  

## 3. 函数

### 1. 函数声明（Function Declaration）

  

函数声明使用`function`关键字，可以在代码的任何位置调用（因为函数声明会被提升）。

  

```javascript

function greet(name) {

    return "Hello, " + name + "!";

}

  

// 调用函数

console.log(greet("Alice")); // 输出: Hello, Alice!

```

  

### 2. 函数表达式（Function Expression）

  

函数表达式将函数赋值给一个变量，函数不会被提升，只能在定义之后调用。

  

```javascript

const greet = function(name) {

    return "Hello, " + name + "!";

};

  

// 调用函数

console.log(greet("Bob")); // 输出: Hello, Bob!

```

  

### 3. 箭头函数（Arrow Function）

  

箭头函数是ES6引入的更简洁的函数定义方式，尤其适用于匿名函数。它没有自己的`this`、`arguments`、`super`或`new.target`绑定。

  

```javascript

const greet = (name) => {

    return "Hello, " + name + "!";

};

  

// 简写形式（当函数体只有一条语句时）

const greetShort = name => "Hello, " + name + "!";

  

// 调用函数

console.log(greet("Charlie")); // 输出: Hello, Charlie!

console.log(greetShort("Dave")); // 输出: Hello, Dave!

```

  

### 4. 函数参数

  

#### 默认参数值

  

可以为函数参数设置默认值，当调用函数时未传递相应参数时使用默认值。

  

```javascript

function greet(name = "Guest") {

    return "Hello, " + name + "!";

}

  

console.log(greet()); // 输出: Hello, Guest!

console.log(greet("Eve")); // 输出: Hello, Eve!

```

  

#### 剩余参数（Rest Parameters）

  

使用剩余参数语法（`...`）可以将不定数量的参数收集到一个数组中。

  

```javascript

function sum(...numbers) {

    return numbers.reduce((acc, curr) => acc + curr, 0);

}

  

console.log(sum(1, 2, 3, 4)); // 输出: 10

console.log(sum(5, 10)); // 输出: 15

```

  

### 5. 函数返回值

  

函数可以返回一个值，使用`return`语句。如果没有`return`语句，默认返回`undefined`。

  

```javascript

function add(a, b) {

    return a + b;

}

  

console.log(add(2, 3)); // 输出: 5

  

function noReturn() {

    let x = 10;

}

  

console.log(noReturn()); // 输出: undefined

```

  

### 6. 立即执行函数表达式（IIFE）

  

立即执行函数表达式是一种设计模式，定义后立即执行，通常用于创建独立的作用域。

  

```javascript

(function() {

    console.log("This is an IIFE!");

})(); // 输出: This is an IIFE!

  

// 带参数的IIFE

(function(name) {

    console.log("Hello, " + name + "!");

})("Frank"); // 输出: Hello, Frank!

```

  

### 7. 回调函数（Callback Function）

  

回调函数是作为参数传递给另一个函数的函数，通常用于异步操作。

  

```javascript

function fetchData(callback) {

    setTimeout(() => {

        let data = "Fetched Data";

        callback(data);

    }, 1000);

}

  

function handleData(data) {

    console.log(data);

}

  

fetchData(handleData); // 1秒后输出: Fetched Data

```

  

### 8. 高阶函数（Higher-Order Function）

  

高阶函数是指接受一个或多个函数作为参数，或返回一个函数的函数。

  

```javascript

function createMultiplier(multiplier) {

    return function(number) {

        return number * multiplier;

    };

}

  

const double = createMultiplier(2);

const triple = createMultiplier(3);

  

console.log(double(5)); // 输出: 10

console.log(triple(5)); // 输出: 15

```

  

### 9. 函数作用域和闭包（Closure）

  

#### 作用域

  

JavaScript中的作用域决定了变量的可见性。函数内部声明的变量在函数外部不可见。

  

```javascript

function scopeExample() {

    let localVariable = "I'm local";

    console.log(localVariable); // 输出: I'm local

}

  

scopeExample();

console.log(localVariable); // 报错: localVariable is not defined

```

  

#### 闭包

  

闭包是指函数可以访问其外部作用域的变量，即使在函数外部调用时。

  

```javascript

function createCounter() {

    let count = 0;

    return function() {

        count++;

        return count;

    };

}

  

const counter = createCounter();

console.log(counter()); // 输出: 1

console.log(counter()); // 输出: 2

console.log(counter()); // 输出: 3

```

  

### 10. 函数绑定（Function Binding）

  

使用`bind`方法可以创建一个新的函数，新的函数永远绑定到指定的`this`值。

  

```javascript

const person = {

    name: "Alice",

    greet: function() {

        console.log("Hello, " + this.name);

    }

};

  

const greet = person.greet;

greet(); // 输出: Hello, undefined (因为this指向全局对象)

  

const boundGreet = person.greet.bind(person);

boundGreet(); // 输出: Hello, Alice

```

  

### 总结

  

JavaScript中的函数是非常灵活和强大的工具，了解和掌握各种函数定义和使用方式是编写高效和可维护代码的关键。以下是一个综合示例，展示了各种函数的使用：

  

```javascript

// 函数声明

function add(a, b) {

    return a + b;

}

  

// 函数表达式

const subtract = function(a, b) {

    return a - b;

};

  

// 箭头函数

const multiply = (a, b) => a * b;

  

// 默认参数值

function greet(name = "Guest") {

    return "Hello, " + name + "!";

}

  

// 剩余参数

function sum(...numbers) {

    return numbers.reduce((acc, curr) => acc + curr, 0);

}

  

// 立即执行函数表达式（IIFE）

(function() {

    console.log("This is an IIFE!");

})();

  

// 回调函数

function fetchData(callback) {

    setTimeout(() => {

        let data = "Fetched Data";

        callback(data);

    }, 1000);

}

  

// 高阶函数

function createMultiplier(multiplier) {

    return function(number) {

        return number * multiplier;

    };

}

  

// 闭包

function createCounter() {

    let count = 0;

    return function() {

        count++;

        return count;

    };

}

  

// 函数绑定

const person = {

    name: "Alice",

    greet: function() {

        console.log("Hello, " + this.name);

    }

};

  

const boundGreet = person.greet.bind(person);

  

// 测试

console.log(add(2, 3)); // 输出: 5

console.log(subtract(5, 2)); // 输出: 3

console.log(multiply(3, 4)); // 输出: 12

console.log(greet()); // 输出: Hello, Guest!

console.log(sum(1, 2, 3, 4)); // 输出: 10

  

fetchData(data => console.log(data)); // 1秒后输出: Fetched Data

  

const double = createMultiplier(2);

console.log(double(5)); // 输出: 10

  

const counter = createCounter();

console.log(counter()); // 输出: 1

console.log(counter()); // 输出: 2

  

boundGreet(); // 输出: Hello, Alice

```

  

## 4. 条件语句

条件语句用来根据不同的条件执行不同的代码。

```javascript

let age = 18;

  

if (age >= 18) {

    console.log("You are an adult.");

} else {

    console.log("You are a minor.");

}

```

  

## 5. 循环

循环语句用于重复执行代码块。

```javascript

// for循环

for (let i = 0; i < 5; i++) {

    console.log(i); // 输出: 0, 1, 2, 3, 4

}

  

// while循环

let count = 0;

while (count < 5) {

    console.log(count); // 输出: 0, 1, 2, 3, 4

    count++;

}

```

  

## 6. 事件处理

### 1. 事件类型

  

JavaScript支持多种事件类型，常见的包括：

  

- **鼠标事件**：`click`、`dblclick`、`mousedown`、`mouseup`、`mousemove`、`mouseover`、`mouseout`等。

- **键盘事件**：`keydown`、`keyup`、`keypress`。

- **表单事件**：`submit`、`change`、`focus`、`blur`。

- **窗口事件**：`load`、`resize`、`scroll`。

  

### 2. 添加事件监听器

  

你可以使用以下三种方法来添加事件监听器：

  

#### 2.1 使用`addEventListener`方法

  

这是推荐的方式，因为它允许添加多个事件监听器，并且可以控制事件的捕获和冒泡。

  

```javascript

const button = document.getElementById('myButton');

  

button.addEventListener('click', function() {

    console.log('Button clicked!');

});

```

  

#### 2.2 使用HTML属性

  

你可以在HTML标签中直接添加事件处理函数。这种方式不推荐，因为它与HTML结构混在一起，不利于维护。

  

```html

<button id="myButton" onclick="handleClick()">Click me</button>

  

<script>

function handleClick() {

    console.log('Button clicked!');

}

</script>

```

  

#### 2.3 使用`on`前缀的属性

  

你可以通过设置元素的事件属性来添加事件监听器。这种方式只能添加一个事件监听器，后添加的会覆盖先添加的。

  

```javascript

const button = document.getElementById('myButton');

  

button.onclick = function() {

    console.log('Button clicked!');

};

```

  

### 3. 事件对象

  

事件对象包含了与事件相关的所有信息。你可以在事件处理函数中访问事件对象，以获取更多的事件信息。

  

```javascript

const button = document.getElementById('myButton');

  

button.addEventListener('click', function(event) {

    console.log('Button clicked!');

    console.log('Event type:', event.type);

    console.log('Event target:', event.target);

});

```

  

### 4. 事件传播

  

事件传播分为三个阶段：捕获阶段、目标阶段和冒泡阶段。默认情况下，事件在目标阶段和冒泡阶段触发事件处理函数。你可以通过设置第三个参数来控制事件监听器在捕获阶段还是冒泡阶段触发。

  

#### 4.1 捕获阶段

  

事件从文档的根节点向目标元素传播。

  

```javascript

document.getElementById('parent').addEventListener('click', function() {

    console.log('Parent clicked (capture phase)');

}, true); // 第三个参数为true表示在捕获阶段触发

```

  

#### 4.2 冒泡阶段

  

事件从目标元素向文档的根节点传播。

  

```javascript

document.getElementById('parent').addEventListener('click', function() {

    console.log('Parent clicked (bubble phase)');

}, false); // 第三个参数为false表示在冒泡阶段触发

```

  

### 5. 阻止事件传播

  

你可以使用`event.stopPropagation()`方法来阻止事件的进一步传播。

  

```javascript

const button = document.getElementById('myButton');

  

button.addEventListener('click', function(event) {

    console.log('Button clicked!');

    event.stopPropagation(); // 阻止事件传播

});

  

document.getElementById('parent').addEventListener('click', function() {

    console.log('Parent clicked!');

});

```

  

### 6. 取消默认行为

  

你可以使用`event.preventDefault()`方法来取消事件的默认行为。例如，取消表单提交或链接跳转。

  

```javascript

const link = document.getElementById('myLink');

  

link.addEventListener('click', function(event) {

    event.preventDefault(); // 取消链接跳转

    console.log('Link clicked, but default behavior prevented.');

});

```

  

### 综合示例

  

以下是一个综合示例，展示了各种事件处理的使用：

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Event Handling Example</title>

</head>

<body>

    <div id="parent" style="padding: 20px; border: 1px solid black;">

        Parent

        <button id="myButton">Click me</button>

        <a href="https://example.com" id="myLink">Go to example.com</a>

    </div>

  

    <script>

        // 添加事件监听器

        const button = document.getElementById('myButton');

        button.addEventListener('click', function(event) {

            console.log('Button clicked!');

            console.log('Event type:', event.type);

            console.log('Event target:', event.target);

            event.stopPropagation(); // 阻止事件传播

        });

  

        // 捕获阶段事件处理

        document.getElementById('parent').addEventListener('click', function() {

            console.log('Parent clicked (capture phase)');

        }, true);

  

        // 冒泡阶段事件处理

        document.getElementById('parent').addEventListener('click', function() {

            console.log('Parent clicked (bubble phase)');

        }, false);

  

        // 取消默认行为

        const link = document.getElementById('myLink');

        link.addEventListener('click', function(event) {

            event.preventDefault(); // 取消链接跳转

            console.log('Link clicked, but default behavior prevented.');

        });

    </script>

</body>

</html>

```
## 7. DOM操作

### 1. 查找DOM元素

  

#### 1.1 `getElementById`

通过元素的ID查找元素，返回一个元素对象。

  

```javascript

const element = document.getElementById('myElement');

console.log(element);

```

  

#### 1.2 `getElementsByClassName`

通过类名查找元素，返回一个包含所有匹配元素的类数组（HTMLCollection）。

  

```javascript

const elements = document.getElementsByClassName('myClass');

console.log(elements);

```

  

#### 1.3 `getElementsByTagName`

通过标签名查找元素，返回一个包含所有匹配元素的类数组（HTMLCollection）。

  

```javascript

const elements = document.getElementsByTagName('div');

console.log(elements);

```

  

#### 1.4 `querySelector`

通过CSS选择器查找元素，返回第一个匹配的元素对象。

  

```javascript

const element = document.querySelector('.myClass');

console.log(element);

```

  

#### 1.5 `querySelectorAll`

通过CSS选择器查找元素，返回一个包含所有匹配元素的NodeList。

  

```javascript

const elements = document.querySelectorAll('.myClass');

console.log(elements);

```

  

### 2. 创建DOM元素

  

使用`document.createElement`方法创建新元素。

  

```javascript

const newElement = document.createElement('div');

newElement.textContent = 'Hello, World!';

console.log(newElement);

```

  

### 3. 插入DOM元素

  

#### 3.1 `appendChild`

将新元素作为子元素插入到指定元素的末尾。

  

```javascript

const parent = document.getElementById('parentElement');

const newElement = document.createElement('div');

newElement.textContent = 'Hello, World!';

parent.appendChild(newElement);

```

  

#### 3.2 `insertBefore`

将新元素插入到指定元素之前。

  

```javascript

const parent = document.getElementById('parentElement');

const newElement = document.createElement('div');

newElement.textContent = 'Hello, World!';

const referenceElement = document.getElementById('referenceElement');

parent.insertBefore(newElement, referenceElement);

```

  

#### 3.3 `innerHTML`

使用`innerHTML`属性直接插入HTML内容。

  

```javascript

const parent = document.getElementById('parentElement');

parent.innerHTML = '<div>Hello, World!</div>';

```

  

### 4. 修改DOM元素

  

#### 4.1 修改元素内容

  

使用`textContent`或`innerHTML`属性修改元素的内容。

  

```javascript

const element = document.getElementById('myElement');

element.textContent = 'New Content';

element.innerHTML = '<span>New HTML Content</span>';

```

  

#### 4.2 修改元素属性

  

使用`setAttribute`方法或直接修改属性。

  

```javascript

const element = document.getElementById('myElement');

element.setAttribute('data-custom', 'value');

element.id = 'newId';

element.className = 'newClass';

```

  

#### 4.3 修改元素样式

  

使用`style`属性修改元素的内联样式。

  

```javascript

const element = document.getElementById('myElement');

element.style.color = 'red';

element.style.fontSize = '20px';

```

  

### 5. 删除DOM元素

  

#### 5.1 `removeChild`

从父元素中删除子元素。

  

```javascript

const parent = document.getElementById('parentElement');

const child = document.getElementById('childElement');

parent.removeChild(child);

```

  

#### 5.2 `remove`

直接删除元素（现代浏览器支持）。

  

```javascript

const element = document.getElementById('myElement');

element.remove();

```

  

### 6. 事件处理

  

为DOM元素添加事件监听器，使其能够响应用户操作。

  

```javascript

const button = document.getElementById('myButton');

  

button.addEventListener('click', function() {

    alert('Button clicked!');

});

```

  

### 综合示例

  

以下是一个综合示例，展示了如何查找、创建、修改和删除DOM元素：

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>DOM Manipulation Example</title>

</head>

<body>

    <div id="parentElement">

        <div id="childElement">Child Element</div>

    </div>

    <button id="myButton">Click me</button>

  

    <script>

        // 查找DOM元素

        const parent = document.getElementById('parentElement');

        const child = document.getElementById('childElement');

        const button = document.getElementById('myButton');

  

        // 创建新元素

        const newElement = document.createElement('div');

        newElement.textContent = 'Hello, World!';

  

        // 插入新元素

        parent.appendChild(newElement);

  

        // 修改元素内容

        child.textContent = 'Updated Child Element';

  

        // 修改元素属性

        newElement.setAttribute('data-custom', 'value');

        newElement.id = 'newElementId';

        newElement.className = 'newElementClass';

  

        // 修改元素样式

        newElement.style.color = 'blue';

        newElement.style.fontSize = '18px';

  

        // 删除元素

        parent.removeChild(child);

  

        // 事件处理

        button.addEventListener('click', function() {

            alert('Button clicked!');

        });

    </script>

</body>

</html>

```


## 解构
通过{}对一个对象进行解构，获得一个对象的属性