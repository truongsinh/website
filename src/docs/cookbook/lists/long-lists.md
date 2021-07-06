---
title: Làm việc với long lists
description: Sử dụng ListView.builder để triển khai một danh sách dài hoặc vô tận.
prev:
  title: Đặt floating app bar bên trên một danh sách
  path: /docs/cookbook/lists/floating-app-bar
next:
  title: Báo các lỗi cho hệ thống
  path: /docs/cookbook/maintenance/error-reporting
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Constructor tiêu chuẩn của [`ListView`][] chỉ làm việc hiệu quả với các danh
sách nhỏ. Để làm việc với các danh sách bao gồm một số lượng lớn item, chúng ta
nên sử dụng constructor [`ListView.builder`][].

Trái ngược với constructor mặc định của `ListView` yêu cầu tạo ra tất cả item
cùng một lúc, constructor `ListView.builder()` tạo ra các item thì người dùng
bắt đầu lướt đến nó (bắt đầu xuất hiện trong khung hình).

## 1. Tạo ra nguồn dữ liệu

Đầu tiên, bạn cần một nguồn dữ liệu. Ví dụ: nguồn dữ liệu có thể là danh sách
tin nhắn, kết quả tìm kiếm, hoặc danh sách sản phẩm trong một cửa hàng. Phần lớn
trường hợp thì dữ liệu này đén từ Internet hoặc từ cơ sở dữ liệu.

Ví dụ này đã tạo ra danh sách 10,000 chuỗi sử dụng constructor [`List.generate`][].

<!-- skip -->
```dart
final items = List<String>.generate(10000, (i) => "Item $i");
```

## 2. Biến dữ liệu thành các widget

Để hiện thị danh sách các chuổi, render mỗi chuỗi thành 1 widget sử dụng `ListView.builder()`.

Ví dụ dưới đây hiển thị mỗi chuỗi trên một hàng.

<!-- skip -->
```dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text('${items[index]}'),
    );
  },
);
```

## Ví dụ tương tác

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp(
    items: List<String>.generate(10000, (i) => "Item $i"),
  ));
}

class MyApp extends StatelessWidget {
  final List<String> items;

  MyApp({Key key, @required this.items}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final title = 'Long List';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index) {
            return ListTile(
              title: Text('${items[index]}'),
            );
          },
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/long-lists.gif" alt="Long Lists Demo" class="site-mobile-screenshot" />
</noscript>



[`List.generate`]: lh({{site.api}}/flutter/dart-core/List/List.generate.html)
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
[`ListView.builder`]: {{site.api}}/flutter/widgets/ListView/ListView.builder.html
