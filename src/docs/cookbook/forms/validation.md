---
title: Xây dựng một form với tính xác thực
description: How to build a form that validates input.
prev:
  title: Work with tabs
  path: /docs/cookbook/design/tabs
next:
  title: Create and style a text field
  path: /docs/cookbook/forms/text-input
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Ứng dụng thường xuyên yêu cầu người dùng nhập thông tin vào text field.
Ví dụ, bạn bắt người dùng đăng nhập với địa chỉ email kết hợp với password.

Để ứng dụng tăng thêm bảo mật và dễ sử dụng, kiểm tra thông tin mà người dùng cung cấp. Nếu thông tin người dùng điền form chính xác, ta sé sử lí thông tin. Nếu thông tin người dùng nhập vô sai xác thực, ta sẽ hiển thị một thông báo thân thiện với người dùng để họ sửa lại lỗi sai.

Ở vị dụ này, ta hãy học cách thêm tính xác thực vào form có một text field bằng các bước sau:

  1. Tạo `Form` với `GlobalKey`.
  2. Thêm `TextFormField` với tính xác thực logic.
  3. Tạo button để xác thực và nộp form.

## 1. Tạo `Form` với `GlobalKey`

Đầu tiên, tạo [`Form`][].
`Form` widget được xem như một vật chứa cho các nhóm và xác thực cho nhiều form.

Khi tạo form, cung cấp [`GlobalKey`][].
Điều này tạo sự độc đáo để xác thực `Form`, và cho phép xác thực form vào các bước sau.

<!-- skip -->
```dart
// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  @override
  MyCustomFormState createState() {
    return MyCustomFormState();
  }
}

// Define a corresponding State class.
// This class holds data related to the form.
class MyCustomFormState extends State<MyCustomForm> {
  // Create a global key that uniquely identifies the Form widget
  // and allows validation of the form.
  //
  // Note: This is a `GlobalKey<FormState>`,
  // not a GlobalKey<MyCustomFormState>.
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    // Build a Form widget using the _formKey created above.
    return Form(
      key: _formKey,
      child: Column(
        children: <Widget>[
              // Add TextFormFields and RaisedButton here.
        ]
     )
    );
  }
}
```

{{site.alert.tip}}
  Using a `GlobalKey` is the recommended way to access a form.
  However, if you have a more complex widget tree,
  you can use the [`Form.of()`][] method to
  access the form within nested widgets.
{{site.alert.end}}

## 2. Thêm `TextFormField` với tính xác thực logic

Mặc dù `Form` đã được tạo, nhưng nó vẫn chưa cho phép người dùng nhập chữ vào.
Đó là công việc của [`TextFormField`][].
`TextFormField` widget sẽ xử lí text field đó rồi hiển thị lỗi khi được tìm thấy bởi hệ thống.

Xác thực input bởi hàm `validator()`  cho
`TextFormField`. Nếu input của người dùng không hợp lệ,
hàm `validator`  sẽ return`String` chứa thông báo lỗi.
Nếu không có lỗi, `validator` sẽ return null.

Trong ví dụ này, tạo `validator` để  bảo đảm
`TextFormField` không trống. Nếu nó trống,
return một tin nhắn thân thiện với người dùng.

<!-- skip -->
```dart
TextFormField(
  // The validator receives the text that the user has entered.
  validator: (value) {
    if (value.isEmpty) {
      return 'Please enter some text';
    }
    return null;
  },
);
```

## 3. Tạo button đẻ xác thực và tạo form

Bây giờ bạn đã có form với text field,
có button để người dùng có thể nhấn để nộp thông tin.

Khi người dùng nộp fomr, kiểm tra form có hợp lệ.
Nếu hợp lệ, hiển thị thông báo thành công.
Nếu không (text field không có nội dung) hiển thị thông báo lỗi.

<!-- skip -->
```dart
RaisedButton(
  onPressed: () {
    // Validate returns true if the form is valid, otherwise false.
    if (_formKey.currentState.validate()) {
      // If the form is valid, display a snackbar. In the real world,
      // you'd often call a server or save the information in a database.

      Scaffold
          .of(context)
          .showSnackBar(SnackBar(content: Text('Processing Data')));
    }
  },
  child: Text('Submit'),
);
```

### Nó hoạt động như thế nào?

Để xác thực form, sử dụng `_formKey` đã tạo ở
step 1. Bạn có thể dùng phương thức `_formKey.currentState()`
để truy cập vào [`FormState`][],
cái mà Flutter tự động tạo ra khi xây dựng `Form`.

Class `FormState` chứa phương thức `validate()`.
Khi phương thức `validate()` được gọi, nó sẽ khởi chạy hàm `validator()`
cho mỗi text field in form.
Nếu mọi thức tốt đẹp, phương thức `validate()` returns `true`.
Nếu một text field nào đó bị lỗi, phương thức `validate()`
xẽ xây dựng lại form để  hiển thị tin nhắn lỗi và returns `false`.

## Ví dụ

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final appTitle = 'Form Validation Demo';

    return MaterialApp(
      title: appTitle,
      home: Scaffold(
        appBar: AppBar(
          title: Text(appTitle),
        ),
        body: MyCustomForm(),
      ),
    );
  }
}

// Create a Form widget.
class MyCustomForm extends StatefulWidget {
  @override
  MyCustomFormState createState() {
    return MyCustomFormState();
  }
}

// Create a corresponding State class.
// This class holds data related to the form.
class MyCustomFormState extends State<MyCustomForm> {
  // Create a global key that uniquely identifies the Form widget
  // and allows validation of the form.
  //
  // Note: This is a GlobalKey<FormState>,
  // not a GlobalKey<MyCustomFormState>.
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    // Build a Form widget using the _formKey created above.
    return Form(
      key: _formKey,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          TextFormField(
            validator: (value) {
              if (value.isEmpty) {
                return 'Please enter some text';
              }
              return null;
            },
          ),
          Padding(
            padding: const EdgeInsets.symmetric(vertical: 16.0),
            child: RaisedButton(
              onPressed: () {
                // Validate returns true if the form is valid, or false
                // otherwise.
                if (_formKey.currentState.validate()) {
                  // If the form is valid, display a Snackbar.
                  Scaffold.of(context)
                      .showSnackBar(SnackBar(content: Text('Processing Data')));
                }
              },
              child: Text('Submit'),
            ),
          ),
        ],
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/form-validation.gif" alt="Form Validation Demo" class="site-mobile-screenshot" />
</noscript>


[`Form`]: {{site.api}}/flutter/widgets/Form-class.html
[`Form.of()`]: {{site.api}}/flutter/widgets/Form/of.html
[`FormState`]: {{site.api}}/flutter/widgets/FormState-class.html
[`GlobalKey`]: {{site.api}}/flutter/widgets/GlobalKey-class.html
[`TextFormField`]: {{site.api}}/flutter/material/TextFormField-class.html

