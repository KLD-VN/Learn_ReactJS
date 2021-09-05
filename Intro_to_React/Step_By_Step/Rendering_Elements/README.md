# Rendering Elements

* Element mô tả những gì bạn muốn thấy trên màn hình

```JSX
const element = <h1>Hello, world</h1>;
```

## Rendering an Element int the DOM

* Đây là **root** DOM node bởi vì nó được quản lý bởi React DOM.
```JSX
<div id="root"></div>
```

* Các app thường chỉ có 1 node DON duy nhất.

* Chuyển nó vào một root DOM node -> ReactDOM.render()

```JSX
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

## Updating the Rendered Element

* Các React elememnt là bất biến -> không thể thay đổi sau khi tạo

```JSX
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

```diff
+ Thực tế, hầu hết các app React chỉ gọi ReactDOM.render() một lần. 
```


## React Only Updates What’s Necessary

* React DOM so sánh element và element con với element trước đó -> chỉ áp dụng bản update DOM cần thiết để đưa DOM về trạng thái mong muốn

