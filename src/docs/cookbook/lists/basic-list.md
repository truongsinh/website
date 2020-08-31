---
title: Use lists
description: How to implement a list.
prev:
  title: Work with cached images
  path: /docs/cookbook/images/cached-images
next:
  title: Create a horizontal list
  path: /docs/cookbook/lists/horizontal-list
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Hiển thị danh sách data là chức năng cơ bản của ứng dụng mobile.
Flutter hỗ trợ [`ListView`][]
widget để ta có thể thao tác với danh sách dễ dàng.

## Tạo ListView

Sử dụng constructor `ListView` cơ bản
là phương án tốt nhất cho việc hiển thị danh sách ít items.
[`ListTile`][] widget được xây dựng
theo hướng để items có thể dễ dàng trực quan.

<!-- skip -->
```dart
ListView(
  children: <Widget>[
    ListTile(
      leading: Icon(Icons.map),
      title: Text('Map'),
    ),
    ListTile(
      leading: Icon(Icons.photo_album),
      title: Text('Album'),
    ),
    ListTile(
      leading: Icon(Icons.phone),
      title: Text('Phone'),
    ),
  ],
);
```

## Ví dụ

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Basic List';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: ListView(
          children: <Widget>[
            ListTile(
              leading: Icon(Icons.map),
              title: Text('Map'),
            ),
            ListTile(
              leading: Icon(Icons.photo_album),
              title: Text('Album'),
            ),
            ListTile(
              leading: Icon(Icons.phone),
              title: Text('Phone'),
            ),
          ],
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/basic-list.png" alt="Basic List Demo" class="site-mobile-screenshot" /> 
</noscript>


[`ListTile`]: {{site.api}}/flutter/material/ListTile-class.html
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
