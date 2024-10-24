---
Project: 
Status:
tags: Resources
Deadline:
CreateTime:
Connected:
---
好的！下面我将为你详细介绍 `fetch` API，帮助你理解它的用法。

### 1. **什么是 `fetch` API？**
`fetch` API 是浏览器中内置的 JavaScript API，用于发起网络请求并处理响应。它是现代替代方案，比老旧的 `XMLHttpRequest` 更加简洁和灵活。

### 2. **基本用法**
`fetch()` 函数用于发起请求，返回一个 `Promise`，当请求完成时可以获取到服务器的响应。

#### **基本示例：**
```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())  // 解析为JSON数据
  .then(data => console.log(data))    // 处理数据
  .catch(error => console.error('Error:', error)); // 捕获错误
```

### 3. **步骤解析**
#### **(1) 发起请求**
```javascript
fetch('https://api.example.com/data')
```
- `fetch` 的第一个参数是请求的 URL。
- 默认情况下，`fetch` 发起的请求是 `GET` 请求。

#### **(2) 处理响应**
- `fetch()` 返回一个 `Promise`，它会解析为一个 `Response` 对象。
- `Response` 对象可以通过多种方法读取数据，例如 `.json()`、`.text()`、`.blob()` 等。

#### **常用的 `Response` 处理方法：**
- `response.json()`：解析响应为 JSON 格式。
- `response.text()`：将响应体解析为纯文本。
- `response.blob()`：用于处理二进制数据。

#### **(3) 捕获错误**
```javascript
.catch(error => console.error('Error:', error));
```
- 在网络请求失败或解析数据出错时，`catch()` 会捕获异常并处理。

### 4. **使用 POST 请求**
除了 `GET` 请求，`fetch` 也可以发送其他 HTTP 请求，例如 `POST` 请求。我们可以通过第二个参数配置请求的方法和请求头。

#### **POST 请求示例：**
```javascript
fetch('https://api.example.com/submit', {
  method: 'POST', // 指定方法为POST
  headers: {
    'Content-Type': 'application/json'  // 设置请求头
  },
  body: JSON.stringify({
    name: 'John',
    age: 30
  })  // 发送JSON数据
})
  .then(response => response.json())
  .then(data => console.log('Success:', data))
  .catch(error => console.error('Error:', error));
```

- `method`：指定请求方法（如 `POST`、`PUT` 等）。
- `headers`：设置 HTTP 头，例如 `Content-Type` 为 `application/json` 表示发送 JSON 数据。
- `body`：请求体，通常用于 `POST`、`PUT` 等请求，可以是 JSON 字符串、表单数据等。

### 5. **处理 HTTP 状态码**
`fetch` 不会自动捕获 HTTP 错误（如 404 或 500 错误），你需要手动检查 `response.ok` 来判断请求是否成功。

#### **示例：**
```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```
- `response.ok` 是一个布尔值，表示请求是否成功（状态码在 200-299 之间）。
- 如果请求失败（如 404），代码会进入 `catch` 块处理错误。

### 6. **完整请求配置**
可以通过传递一个配置对象来完全自定义请求行为。

#### **配置示例：**
```javascript
fetch('https://api.example.com/data', {
  method: 'POST',         // 请求方法
  headers: {              // 请求头
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token'
  },
  body: JSON.stringify({  // 请求体
    userId: 123,
    message: 'Hello World!'
  }),
  mode: 'cors',           // 请求模式（同源、跨域、no-cors）
  credentials: 'include'  // 包含凭证（cookies等）
})
  .then(response => response.json())
  .then(data => console.log('Success:', data))
  .catch(error => console.error('Error:', error));
```

### 7. **常用选项**
- **method**: HTTP 方法（如 `GET`, `POST`, `PUT`, `DELETE`）。
- **headers**: 请求头，用于设置 `Content-Type`、授权信息等。
- **body**: 请求体，用于发送数据（一般为 `POST` 请求时使用）。
- **mode**: 请求模式，默认为 `cors`，可选 `same-origin`（同源），`no-cors`（允许跨域但不能读取数据）。
- **credentials**: 是否发送凭证（如 cookies）。可选 `same-origin`、`include` 或 `omit`。

### 8. **异常处理**
`fetch` 仅在网络请求出错时才会抛出异常，例如断网或 DNS 解析失败。对于 HTTP 错误状态码（如 404、500），需要手动检查 `response.ok`。

#### **捕获异常示例：**
```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Fetch error:', error));
```

### 总结：
1. `fetch` 是现代浏览器中用于发起 HTTP 请求的 API，替代了 `XMLHttpRequest`。
2. 它返回一个 `Promise`，可通过链式 `.then()` 处理请求结果和 `.catch()` 处理错误。
3. 通过配置请求的 `method`、`headers`、`body` 等可以发起各种 HTTP 请求。
4. 手动检查 `response.ok` 来处理 HTTP 错误状态码。

这个基础概念和示例可以帮助你熟练掌握 `fetch` API。如果你有任何具体问题或场景，欢迎提问！

### 1. **处理超时 (Timeout)**
`fetch` 本身并没有内置的超时机制，这意味着它会一直等待响应，直到服务器返回结果。我们可以通过 `Promise.race()` 实现超时控制。

#### **实现请求超时的示例：**
```javascript
function fetchWithTimeout(url, options = {}, timeout = 5000) {
  // 创建一个超时的Promise
  const timeoutPromise = new Promise((_, reject) =>
    setTimeout(() => reject(new Error('Request timed out')), timeout)
  );

  // 使用Promise.race() 来处理超时和fetch请求
  return Promise.race([fetch(url, options), timeoutPromise]);
}

fetchWithTimeout('https://api.example.com/data', {}, 3000)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```
在这个例子中，`fetchWithTimeout` 会在指定的时间内等待响应，否则会抛出超时错误。

### 2. **并行请求**
当你需要同时发起多个请求时，可以通过 `Promise.all()` 并行处理多个 `fetch` 请求，这样可以加快性能，减少等待时间。

#### **并行请求示例：**
```javascript
const urls = [
  'https://api.example.com/data1',
  'https://api.example.com/data2',
  'https://api.example.com/data3'
];

// 使用Promise.all来并行发起请求
Promise.all(urls.map(url => fetch(url)))
  .then(responses => Promise.all(responses.map(res => res.json())))
  .then(data => console.log(data))  // 得到一个包含所有数据的数组
  .catch(error => console.error('Error:', error));
```
`Promise.all()` 会等待所有请求完成，只有在全部请求成功时才会继续。如果其中任何一个请求失败，它会立即触发 `catch()`。

### 3. **序列化请求 (Request Queue)**
有时需要一个请求完成后再发起下一个请求，这种情况可以通过 `async`/`await` 或 `reduce()` 方法来实现。

#### **顺序请求示例：**
```javascript
async function fetchSequential(urls) {
  for (let url of urls) {
    try {
      let response = await fetch(url);
      let data = await response.json();
      console.log(data);
    } catch (error) {
      console.error('Error:', error);
    }
  }
}

const urls = [
  'https://api.example.com/data1',
  'https://api.example.com/data2',
  'https://api.example.com/data3'
];

fetchSequential(urls);
```
这样可以确保一个请求完成后再开始下一个请求，适用于对请求顺序有依赖的场景。

### 4. **请求重试机制**
在网络不稳定或请求偶尔失败的情况下，自动重试机制非常有用。可以用递归或循环来实现重试逻辑。

#### **请求重试示例：**
```javascript
async function fetchWithRetry(url, options = {}, retries = 3, delay = 1000) {
  for (let i = 0; i < retries; i++) {
    try {
      const response = await fetch(url, options);
      if (!response.ok) throw new Error('Failed to fetch');
      return await response.json();
    } catch (error) {
      if (i < retries - 1) {
        console.log(`Retrying request... Attempt ${i + 1}`);
        await new Promise(resolve => setTimeout(resolve, delay));  // 等待一段时间后重试
      } else {
        throw error;  // 如果重试次数用完，抛出错误
      }
    }
  }
}

fetchWithRetry('https://api.example.com/data', {}, 3, 2000)
  .then(data => console.log('Success:', data))
  .catch(error => console.error('Failed after retries:', error));
```
这里的 `fetchWithRetry` 函数会在失败后等待一段时间（`delay`）并自动重试，直到成功或重试次数用尽。

### 5. **请求取消 (Abort Controller)**
在某些情况下，你可能希望能够取消一个正在进行的请求。`AbortController` 是 `fetch` 提供的一个 API，允许我们中止请求。

#### **取消请求示例：**
```javascript
const controller = new AbortController();
const signal = controller.signal;

fetch('https://api.example.com/data', { signal })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => {
    if (error.name === 'AbortError') {
      console.log('Fetch request was aborted');
    } else {
      console.error('Error:', error);
    }
  });

// 在某些条件下取消请求
setTimeout(() => {
  controller.abort();  // 取消请求
}, 1000);
```
在这个例子中，如果请求超过 1 秒还未完成，我们会通过调用 `controller.abort()` 来取消请求，并触发 `AbortError`。

### 6. **缓存 (Cache Control)**
`fetch` 支持控制请求缓存行为，你可以通过设置 `cache` 选项来配置请求如何处理缓存。

#### **缓存控制示例：**
```javascript
fetch('https://api.example.com/data', { cache: 'no-cache' })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

`cache` 选项可以是以下几种值：
- `'default'`：默认缓存行为。
- `'no-store'`：不使用缓存，也不存储缓存。
- `'reload'`：从网络上重新获取资源。
- `'no-cache'`：使用缓存，但要求服务器验证资源是否被修改。
- `'force-cache'`：使用缓存，即使缓存已经过期。
- `'only-if-cached'`：只使用缓存，不访问网络（仅限同源请求）。

### 7. **跨域请求 (CORS)**
浏览器的安全策略默认限制跨域请求，但你可以通过配置 CORS（跨域资源共享）来允许请求其他域的数据。服务器需要在响应头中设置 `Access-Control-Allow-Origin` 来允许跨域访问。

#### **跨域请求示例：**
```javascript
fetch('https://api.example.com/data', {
  mode: 'cors',  // 设置为跨域模式
  credentials: 'include'  // 如果需要发送凭证（如Cookies），设置为 'include'
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```
`mode: 'cors'` 指定请求是跨域请求，`credentials: 'include'` 表示即使是跨域请求，也要发送凭证（如 cookies）。

### 8. **处理流式响应 (Streaming Response)**
`fetch` 还可以用于处理流式响应数据，特别是当你需要处理较大文件时，可以通过流的方式逐步读取响应。

#### **流式响应示例：**
```javascript
fetch('https://api.example.com/large-file')
  .then(response => {
    const reader = response.body.getReader();
    const stream = new ReadableStream({
      start(controller) {
        function push() {
          reader.read().then(({ done, value }) => {
            if (done) {
              controller.close();
              return;
            }
            controller.enqueue(value);  // 逐块处理数据
            push();
          });
        }
        push();
      }
    });

    return new Response(stream);
  })
  .then(response => response.blob())
  .then(blob => {
    // 处理文件，例如将其显示为图片
    const url = URL.createObjectURL(blob);
    document.getElementById('image').src = url;
  })
  .catch(error => console.error('Error:', error));
```
这个例子展示了如何使用 `ReadableStream` 来逐块处理大文件，避免一次性加载全部数据。

---

### 总结：
- **超时控制**：通过 `Promise.race()` 实现超时。
- **并行请求**：使用 `Promise.all()` 提高性能。
- **顺序请求**：通过 `async/await` 实现请求按顺序执行。
- **请求重试**：通过循环和递归实现自动重试机制。
- **请求取消**：使用 `AbortController` 中止请求。
- **缓存控制**：通过 `fetch` 的 `cache` 选项控制缓存行为。
- **跨域请求**：通过 `mode: 'cors'` 处理跨域请求。
- **流式响应**：使用 `ReadableStream` 处理大数据或流式数据。

这些进阶技巧能帮助你应对更复杂的场景，更灵活高效地使用 `fetch` API。如果你有任何问题，随时可以问我！