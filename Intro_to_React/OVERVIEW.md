# Overview

## What is React?

* Thư viện -> building giao diện
* Cho phép chia biên soạn giao diện phức tạp thành các phần nhỏ -> component

## What is component?

* Một class extends từ React.Component -> bên trong có 1 method **render** kết xuất UI

## What is React element?

* Mỗi **React element** là **Object** Javascript -> lưu trong 1 biến hoặc di chuyển quanh chương trình

## What is render method in React?

* **render** là method nằm trong class **React.Component** -> return **React element** (UI) -> dùng syntax JSX để viết

## What is JSX?

* Cú phép mở rộng JS -> Viết mã HTML chung với JS

## What is props?
 
* props (property) tham số truyền vào component

```diff
! Note
# DOM <button> element's -> onClick là thuộc tích được tích hợp sẵn
# For custom component như Square thì:
- Cách đặt tên props 
+ on[Event] -> prop sự kiện
+ handle[Event] -> method xử lý các sự kiện 
```