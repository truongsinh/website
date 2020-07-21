---
title: Tạo ra một horizontal list
description: Triển khai một horizontal list.
prev:
  title: Use lists
  path: /docs/cookbook/lists/basic-list
next:
  title: Create a grid list
  path: /docs/cookbook/lists/grid-lists
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Đôi khi bạn muốn tạo ra một danh sách có thể lướt (scroll) ngang thay vì
lượt dọc. Widget [`ListView`][] sẽ hỗ trợ bạn xây dựng một horizontal list.

Bạn chỉ cần sử dụng constructor cơ bản của `ListView`, sau đó thêm
`scrollDirection` là 'horizontal'. Điều này sẽ override hướng scroll mặc định
của ListView là lướt dọc.

<!-- skip -->
```dart
ListView(
  // This next line does the trick.
  scrollDirection: Axis.horizontal,
  children: <Widget>[
    Container(
      width: 160.0,
      color: Colors.red,
    ),
    Container(
      width: 160.0,
      color: Colors.blue,
    ),
    Container(
      width: 160.0,
      color: Colors.green,
    ),
    Container(
      width: 160.0,
      color: Colors.yellow,
    ),
    Container(
      width: 160.0,
      color: Colors.orange,
    ),
  ],
)
```

## Một ví dụ tương tác

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Horizontal List';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: Container(
          margin: EdgeInsets.symmetric(vertical: 20.0),
          height: 200.0,
          child: ListView(
            scrollDirection: Axis.horizontal,
            children: <Widget>[
              Container(
                width: 160.0,
                color: Colors.red,
              ),
              Container(
                width: 160.0,
                color: Colors.blue,
              ),
              Container(
                width: 160.0,
                color: Colors.green,
              ),
              Container(
                width: 160.0,
                color: Colors.yellow,
              ),
              Container(
                width: 160.0,
                color: Colors.orange,
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/horizontal-list.gif" alt="Horizontal List Demo" class="site-mobile-screenshot" />
</noscript>


[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
