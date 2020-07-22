---
title: Thêm Material touch ripples
description: Cách để biểu diễn hoạt ảnh sóng.
prev:
  title: Focus và text fields
  path: /docs/cookbook/forms/focus
next:
  title: Xử lý thao tác bấm
  path: /docs/cookbook/gestures/handling-taps
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Widgets tuân theo the Material Design hướng dẫn biểu diễn 
hiệu ứng sóng khi bấm.

Flutter cung cấp widget [`InkWell`][]
để biểu diễn hiệu ứng này.
Tạo một hiệu ứng sóng theo các bước sau:

  1. Tạo một widget hỗ trợ thao tác bấm.
  2. Cuốn nó trong một widget `InkWell` để quản lí bấm callbacks và
     hoạt ảnh sóng.

<!-- skip -->
```dart
// The InkWell wraps the custom flat button widget.
InkWell(
  // When the user taps the button, show a snackbar.
  onTap: () {
    Scaffold.of(context).showSnackBar(SnackBar(
      content: Text('Tap'),
    ));
  },
  child: Container(
    padding: EdgeInsets.all(12.0),
    child: Text('Flat Button'),
  ),
);
```

## Ví dụ

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'InkWell Demo';

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
    // The InkWell wraps the custom flat button widget.
    return InkWell(
      // When the user taps the button, show a snackbar.
      onTap: () {
        Scaffold.of(context).showSnackBar(SnackBar(
          content: Text('Tap'),
        ));
      },
      child: Container(
        padding: EdgeInsets.all(12.0),
        child: Text('Flat Button'),
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/ripples.gif" alt="Ripples Demo" class="site-mobile-screenshot" />
</noscript>


[`InkWell`]: {{site.api}}/flutter/material/InkWell-class.html
