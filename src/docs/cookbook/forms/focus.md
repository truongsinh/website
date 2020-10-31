---
title: Focus và ô text
description: Giải thích cách focus tương tác với các ô để nhập text.
prev:
  title: Retrieve the value of a text field
  path: /docs/cookbook/forms/retrieve-input
next:
  title: Add Material touch ripples
  path: /docs/cookbook/gestures/ripples
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Khi một ô text được chọn và đang chờ được nhập vào gọi là trường text đó đang được focus (tập trung). Về cơ bản, người dùng sẽ focus vào một ô text bằng cách nhấn vào màn hình, còn lập trình viên sẽ focus một ô text bằng cách dùng các công cụ có trong bài hướng dẫn này.

Kiểm soát focus là công cụ nền tảng để tạo ra các biểu mẫu dễ dùng. Ví dụ, mình có một màn hình để tìm và tra cứu. Trên màn hình đó có một ô để nhập text. Khi người dùng mở ra màn hình này, mình có thể cho focus vào ô text đó. Như vậy, người dùng có thể nhập liệu vào ngay khi tới màn hình này mà không cần phải chọn vào ô text.

Bài viết này hướng dẫn focus một ô text theo hai cách:
  - Focus vào ô text ngay khi nó hiện ra
  - Focus khi một nút nào đó được nhấn 
## Focus vào một ô text ngay khi xuất hiện

Để focus vào một ô text ngay khi xuất hiện, mình dùng đặc tính `autofocus`.

<!-- skip -->
```dart
TextField(
  autofocus: true,
);
```

Xem phần [Forms][] để tìm hiểu thêm về cách tạo trường text và xử lý input.

## Focus vào ô text khi một nút nào đó

Sẽ có những lúc mình không cần focus ngay lập tức vào ô text, mà chỉ focus khi nào cần thiết thôi.
Trên thực tế, cũng có lúc mình cần phải focus vào ô text khi được gọi từ API hoặc các trường hợp khác. Trong ví dụ này, mình sẽ focus vào một ô text khi người dùng nhấn vào một nút bấm trên màn hình. Mình sẽ làm như sau:

  1. Tạo một cái `FocusNode`.
  2. Truyền `FocusNode` vào một ô `TextField`.
  3. Focus vào ô `TextField` khi nút được nhấn.

### 1. Tạo `FocusNode`

Trước tiên, mình cần tạo [`FocusNode`][].
Dùng `FocusNode` để xác định chính xác một ô `TextField` trong "cây focus" của Flutter. Như vậy, tiếp theo mình có thể focus vào ô `TextField`.

Vì những cái focus nodes là những object có vòng đời (lifecycle) rất dài, nên mình kiểm soát vòng đời của nó bằng object `State`. Sau đây là hướng dẫn để tạo ra một instance của `FocusNode` bên trong phương thức `initState()` của lớp `State`, và sau đó sẽ dọn dẹp bằng phương thức `dispose()`:

<!-- skip -->
```dart
// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

// Define a corresponding State class.
// This class holds data related to the form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Define the focus node. To manage the lifecycle, create the FocusNode in
  // the initState method, and clean it up in the dispose method.
  FocusNode myFocusNode;

  @override
  void initState() {
    super.initState();

    myFocusNode = FocusNode();
  }

  @override
  void dispose() {
    // Clean up the focus node when the Form is disposed.
    myFocusNode.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next step.
  }
}
```

### 2. Truyền `FocusNode` vào một trường `TextField`

Sau khi đã có `FocusNode`, mình sẽ truyền nó vào một trường `TextField` trong phương thức `build()`.

<!-- skip -->
```dart
class _MyCustomFormState extends State<MyCustomForm> {
  // Code to create the Focus node...

  @override
  Widget build(BuildContext context) {
    return TextField(
      focusNode: myFocusNode,
    );
  }
}
```

### 3. Focus vào trường `TextField` khi nhấn vào nút

Bây giờ, mình sẽ focus vào ô text field khi người dùng nhấn vào một nút nào đó. Để làm được điều này, mình sẽ dùng phương thức  [`requestFocus()`][].

<!-- skip -->
```dart
FloatingActionButton(
  // When the button is pressed, give focus to the text field using
  // myFocusNode.
  onPressed: () => myFocusNode.requestFocus(),
);
```

## Một số ví dụ

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Text Field Focus',
      home: MyCustomForm(),
    );
  }
}

// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

// Define a corresponding State class.
// This class holds data related to the form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Define the focus node. To manage the lifecycle, create the FocusNode in
  // the initState method, and clean it up in the dispose method.
  FocusNode myFocusNode;

  @override
  void initState() {
    super.initState();

    myFocusNode = FocusNode();
  }

  @override
  void dispose() {
    // Clean up the focus node when the Form is disposed.
    myFocusNode.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Text Field Focus'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            // The first text field is focused on as soon as the app starts.
            TextField(
              autofocus: true,
            ),
            // The second text field is focused on when a user taps the
            // FloatingActionButton.
            TextField(
              focusNode: myFocusNode,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // When the button is pressed,
        // give focus to the text field using myFocusNode.
        onPressed: () => myFocusNode.requestFocus(),
        tooltip: 'Focus Second Text Field',
        child: Icon(Icons.edit),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/focus.gif" alt="Text Field Focus Demo" class="site-mobile-screenshot" />
</noscript>


[fix has landed]: {{site.github}}/flutter/flutter/pull/50372
[`FocusNode`]: {{site.api}}/flutter/widgets/FocusNode-class.html
[Forms]: /docs/cookbook#forms
[flutter/flutter@bf551a3]: {{site.github}}/flutter/flutter/commit/bf551a31fe7ef45c854a219686b6837400bfd94c
[Issue 52221]: {{site.github}}/flutter/flutter/issues/52221
[`requestFocus()`]: {{site.api}}/flutter/widgets/FocusNode/requestFocus.html
[workaround]: {{site.github}}/flutter/flutter/issues/52221#issuecomment-598244655
