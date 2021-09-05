# State and Lifecycle

* Có 2 cách để render lại UI:
   + Gọi ReactDOM.render()
   + Gọi hàm setState()

## Converting a Function to a Class

* Có 5 bước

1. Tạo class **ES6**, cùng tên, extends từ **React.Component**.
2. Thêm duy nhất một method trống -> **render()**.
3. Chuyển thân function vào hàm **render()**.
4. Thay **props** bằng **this.props** trong thân hàm **render()**.
5. Xóa khai báo function trống kia.

```JSX
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

```
to
```JSX
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

## Adding Local State to a Class

1. Thay **this.props.date** -> **this.state.date** trong **render()** method:

```JSX
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2. Thêm **contructor()** method -> gán giá trị ban đầu **this.state**:

```JSX
class Clock extends React.Component {
   constructor(props) {
      super(props)
      this.state = { date: new Date() };
   }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
   <Clock />,
   document.getElementById('root')
);
```

## Adding Lifecycle Methods to a Class

** Thiết lập bộ đếm thời gian khi -> **Clock** được hiển thị cho DOM lần đầu tiên -> **Mounting** 

** Xóa bộ đếm thời gian khi -> DOM do **Clock** tạo ra bị xóa -> **Unmounting**

```JSX
class Clock extends React.Component {
   constructor(props) {
      super(props)
      this.state = { date: new Date() };
   }

   componentDidMount() {

   }

   componentWillUnmount() {
      
   }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
   <Clock />,
   document.getElementById('root')
);
```

=> Các method này goi là **lifecycle method**

* **componentDidMount()** -> sau khi component được rendered vào DOM

```JSX
 componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

* Có thể thêm các field bổ sung -> nếu cần lưu trữ thứ gì đó **không tham gia vào data flow** (như ID hẹn giờ)

* Loại bỏ bộ đêm thời gian trong **componentWillUnmount()** lifecycle method -> khi component bị xóa khỏi DOM (Không render ra component hoặc khi redirect page, .etc)

```JSX
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

* Sử dụng **this.setState()** để lên lịch state local của component


```JSX
class Clock extends React.Component {
   constructor(props) {
      super(props)
      this.state = { date: new Date() };
   }

   componentDidMount() {
      this.timerID = setInterval(
         () => this.tick(),
         1000
      );
   }

   componentWillUnmount() {
      clearInterval(this.timerID);
   }

   tick() {
      this.setState({
         date: new Date()
      });
   }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
   <Clock />,
   document.getElementById('root')
);
```

* Nhờ **setState()** method mà React biết state đã thay đổi -> gọi lại **render()** method.

## Using State Correctly (Sử dụng đúng state)

* Có 3 điều cần biết về **setState()**

### Không sửa đổi trục tiếp state

```JSX
// Wrong
this.state.comment = 'Hello';
```

```JSX
// Correct
this.setState({comment: 'Hello'});
```

* Nơi duy nhất có thể gán **this.state** là hàm **constructor()**

### State Updates May Be Asynchronous (Update state không thể đồng bộ)

* Không nên dựa vào giá trị **this.props** và **this.state** để tính toán các state tiếp theo

```JSX
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

* Khắc phục -> dùng dạng thứ 2 của **setState()** nhận 1 function (state là argument đầu tiên, props là argument thứ 2) không phải object


```JSX
// Correct
this.setState((state, props) => ({ // arrow function
  counter: state.counter + props.increment
}));
```

```JSX
// Correct
this.setState(function(state, props) { // regular function
  return {
    counter: state.counter + props.increment
  };
});
```

### State Updates are Merged (Cập nhật state được hợp nhất)

* Khi gọi **setState()**, React hợp object được cung cấp vào state hiện tại


Ví dụ: trạng thái có thể chứa một số biến độc lập:

```JSX
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

* Có thể update một cách độc lập với các lời gọi **setState()** riêng biệt.

```JSX
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

## The Data Flows Down ( Dữ liệu chảy xuống )

* Component cha và con không thể biết component có state hay không, và ko nên quan tâm nó được định nghĩa từ một function hay class.

* Đây là lý state được gọi là local hoặc được unit. Nó không thẻ truy cập bên ngoài component chứa nó

* Một component có thể chuyền state xuống dưới dạng props cho các component con -> thường gọi là **top-down** hoặc **unidirectional(một chiều)** data flow.

Tạo component **<App>** hiển thị 3 component con **<Clock>** -> để thấy tất cả component thực sự bị cô lập
```JSX
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```