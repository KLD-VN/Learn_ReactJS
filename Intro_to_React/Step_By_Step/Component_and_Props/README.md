# Component and Props


## What is Component?

* Giống các function JavaScript. Nhận input là **props** và return về UI

## Why is Component?

* Cho phép chia giao diện người dùng thành các phần độc lập, có thể tái sử dụng, và suy nghĩ về mỗi phần một cách riêng biệt

## Function and Class Components

* Đơn giản nhất để define một component là viết một JavaScript function:

```JSX
function Hello(props) {
   return <h1>Hello, {props.name}</h1>;
}
```

* Hàm này là React component hợp lệ vì nhận một đối số là "props" và trả về một React element -> **Function Component**

* Có thể dùng class Es6 để tạo component:

```JSX
class Hello extends React.Component {
   render() {
      return <h1>Hello, {this.props.name}</h1>
   }
}
```

## Rendering a Component

* Các **element** cũng có thể đại diện cho các component tự define

```JSX
function Hello(props) {
   return <h1>Hello, {props.name}</h1>;
}

const element = <Hello name="khanh" />;
ReactDOM.render(
   element,
   document.getElementById('Root')
)
```

```diff
- Lưu ý: Tên component luôn bắt đầu bằng chữ in hoa
- Chữ thường -> React hiểu là các DOM tags <div> hoặc <span> đại diện thẻ HTML -> tạo ra chuỗi 'div' hoặc 'span'
- Chữ viết hoa chữ cái đầu -> compile React.createElement(Name) -> tương ứng với component trong tệp JavaScript của bạn.
```

## Composing Components

* Các component có thể tham chiếu tới các component khác -> cho phép tạo các component level thấp hơn như **input**, **button**, **dialog** 

```JSX
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

## Extracting Components

* Đừng ngại chia nhỏ components -> components nhỏ hơn

```JSX
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

* Tách **Avatar**:

```JSX
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

* Tách **UserInfo** component -> kết xuất **Avatar** cạnh tên user

```JSX
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

* Đơn giản hóa **Comment**:

```JSX
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

## Props are Read-Only

** **Tất cả các component React hoạt động giống các pure function đối với props** (tức là với cùng 1 inptu sẽ cho ra cùng 1 output)