# What is JSX?

* Cú pháp mở rộng của JS

# Why JSX?

* Render Logic đi đôi với UI logic -> React cho kết hợp cả 2
* Cho cái nhìn trực quan khi làm việc với giao diện bên trong JavaScript code.

# Embedding Expressions in JSX

* Nhúng variable hoặc expresstion bên trong dấu ngoặc nhọn

```JSX
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```