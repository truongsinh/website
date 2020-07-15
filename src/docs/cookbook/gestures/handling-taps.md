---
title: Handle taps
description: Cách để xử lý bấm và kéo
prev:
  title: Add Material touch ripples
  path: /docs/cookbook/gestures/ripples
next:
  title: Implement swipe to dismiss
  path: /docs/cookbook/gestures/dismissible
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Bạn không muốn chỉ hiện thị thông tin đến người dùng,
bạn còn muốn người dùng tương tác với ứng dụng.
Sử dụng widget [`GestureDetector`][] để phản ứng 
với các thao tác cơ bản như bấm và kéo.

Hướng dẫn này chỉ cách tạo một nút bấm để hiển thị 
snackbar sau khi bấm với các thao tác sau:

  1. Tạo một nút.
  2. Bao nó trong một `GestureDetector` với hàm callback `onTap()`.

<!-- skip -->
```dart
// The GestureDetector wraps the button.
GestureDetector(
  // When the child is tapped, show a snackbar.
  onTap: () {
    final snackBar = SnackBar(content: Text("Tap"));

    Scaffold.of(context).showSnackBar(snackBar);
  },
  // The custom button
  child: Container(
    padding: EdgeInsets.all(12.0),
    decoration: BoxDecoration(
      color: Theme.of(context).buttonColor,
      borderRadius: BorderRadius.circular(8.0),
    ),
    child: Text('My Button'),
  ),
);
```

## Chú thích

  1. Về thông tin về cách thêm Material ripple effect cho 
  nút bấm của bạn, coi hướng dẫn trên [Add Material touch ripples][].
  2. Mặc dù ví dụ này tạo một nút thủ công,
     Flutter cung cấp nhiều thao tác tạo nút, như là:
     [`RaisedButton`][], [`FlatButton`][], và
     [`CupertinoButton`][].

## Ví dụ

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Gesture Demo';

    return MaterialApp(
      title: title,
      home: MyHomePage(title: title),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;

  MyHomePage({Key key, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(child: MyButton()),
    );
  }
}

class MyButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // The GestureDetector wraps the button.
    return GestureDetector(
      // When the child is tapped, show a snackbar.
      onTap: () {
        final snackBar = SnackBar(content: Text("Tap"));

        Scaffold.of(context).showSnackBar(snackBar);
      },
      // The custom button
      child: Container(
        padding: EdgeInsets.all(12.0),
        decoration: BoxDecoration(
          color: Theme.of(context).buttonColor,
          borderRadius: BorderRadius.circular(8.0),
        ),
        child: Text('My Button'),
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/handling-taps.gif" alt="Handle taps demo" class="site-mobile-screenshot" />
</noscript>


[Add Material touch ripples]: /docs/cookbook/gestures/ripples
[`CupertinoButton`]: {{site.api}}/flutter/cupertino/CupertinoButton-class.html
[`FlatButton`]: {{site.api}}/flutter/material/FlatButton-class.html
[`GestureDetector`]: {{site.api}}/flutter/widgets/GestureDetector-class.html
[`RaisedButton`]: {{site.api}}/flutter/material/RaisedButton-class.html
