---
title: Tạo kiểu cho Text field
description: How to implement a text field.
prev:
  title: Build a form with validation
  path: /docs/cookbook/forms/validation
next:
  title: Handle changes to a text field
  path: /docs/cookbook/forms/text-field-changes
---

Người dùng sẽ sử dụng Text fields để nhập chữ vào ứng dụng. Họ thường dùng để  tạo forms, gửi tin nhắn, tạo dữ liệu tìm kiếm, và những thứ khác. Ở bài viết này, hãy tìm hiểu cách tạo kiểu cho text fields.

Flutter cung cấp 2 loại text fields:
[`TextField`][] và [`TextFormField`][].

## `TextField`

[`TextField`][] được sử dụng phổ biến trong việc nhập text.

Mặc định, một `TextField` được hiển thị với dấu gạch dứoi.
Bạn có thể nhập nhãn dán, icon, thông tin gợi ý, và lỗi thông qua
[`InputDecoration`][] như [`decoration`][]
là một thuộc tính của `TextField`.
Để  loại bỏ các hiển thị bên trong (bao gồm cả dấu gạch dưới và các nhãn dán được đảo ngược),ta đặt `decoration` là null.

<!-- skip -->
```dart
TextField(
  decoration: InputDecoration(
    border: InputBorder.none,
    hintText: 'Enter a search term'
  ),
);
```

Để biết thêm thông tin khi có sự thay đổi,
tìm hiểu ở bài viết [Handle changes to a text field][].

## `TextFormField`

[`TextFormField`][] bao một `TextField` và kết hợi với [`Form`][] để tạo ra một vùng. Điều này cung cấp thêm một chức năng, chẳng hạn kết hợp và tính xác thực nó với [`FormField`][] widgets.

<!-- skip -->
```dart
TextFormField(
  decoration: InputDecoration(
    labelText: 'Enter your username'
  ),
);
```

Để có thêm thông tin về xác thực dữ liệu vào, tìm hiểu thêm bài viết [Building a form with validation][].


[Building a form with validation]: /docs/cookbook/forms/validation/
[`decoration`]: {{site.api}}/flutter/material/TextField/decoration.html
[`Form`]: {{site.api}}/flutter/widgets/Form-class.html
[`FormField`]: {{site.api}}/flutter/widgets/FormField-class.html
[Handle changes to a text field]: /docs/cookbook/forms/text-field-changes/
[`InputDecoration`]: {{site.api}}/flutter/material/InputDecoration-class.html
[`TextField`]: {{site.api}}/flutter/material/TextField-class.html
[`TextFormField`]: {{site.api}}/flutter/material/TextFormField-class.html
