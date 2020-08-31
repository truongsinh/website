---
title: Thực hiện vuốt để loại bỏ
description: How to implement swiping to dismiss or delete.
prev:
  title: Handle taps
  path: /docs/cookbook/gestures/handling-taps
next:
  title: Display images from the internet
  path: /docs/cookbook/images/network-image
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Chế độ "vuốt để loại bỏ" rất được phổ biến trong các ứng dụng điện thoại.
Ví dụ với ứng dụng quản lí email,
người dùng có thể vuốt email
để xóa nó ra khỏi danh sách.

Flutter làm điều này dễ dàng với
[`Dismissible`][] widget.
Hãy tìm hiểu cách thực hiện thao tác này bằng những bước sau:

  1. Tạo danh sách items.
  2. Bao mỗi item với `Dismissible` widget.
  3. Cung cấp các "dấu hiệu"

## 1. Tạo danh sách items

Đầu tiên, tạo một danh sách items. Hướng dẫn
tạo một danh sách,
đọc ở bài viết [Working with long lists][].

### Tạo source dữ liệu

Ở ví dụ này,
bạn muốn danh sách với 20 items.
Để đơn giản, sinh ra danh sách của chuỗi.

<!-- skip -->
```dart
final items = List<String>.generate(20, (i) => "Item ${i + 1}");
```

### Chuyển đổi source dữ liệu vài danh sách

Hiển thị mỗi item trong danh sách lên màn hình. Người dùng
Người dùng không thể vuốt các items này ngay bây giờ.

<!-- skip -->
```dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListTile(title: Text('${items[index]}'));
  },
);
```

## 2. Bao mỗi item với Dismissible widget

Ở bước này,
ta sẽ cho phép người dùng vuốt item ra khỏi danh sách thông qua
[`Dismissible`][] widget.

Sau khi người dùng vuốt item
xóa item từ danh sách rồi hiển thị lên.
Trong thực tế, bạn cần phải thực hiện các thao tác logic phức tạp hơn, chẳng hạn như xóa các items từ các web service hay từ cơ sở dữ liệu.

Cập nhật hàm `itemBuilder()` để return `Dismissible` widget:

<!-- skip -->
```dart
Dismissible(
  // Each Dismissible must contain a Key. Keys allow Flutter to
  // uniquely identify widgets.
  key: Key(item),
  // Provide a function that tells the app
  // what to do after an item has been swiped away.
  onDismissed: (direction) {
    // Remove the item from the data source.
    setState(() {
      items.removeAt(index);
    });

    // Show a snackbar. This snackbar could also contain "Undo" actions.
    Scaffold
        .of(context)
        .showSnackBar(SnackBar(content: Text("$item dismissed")));
  },
  child: ListTile(title: Text('$item')),
);
```

## 3. Cung cấp các "dấu hiệu" 

Như ta đã bàn,
ứng dụng cho phép người dùng vuốt items ra khỏi danh sách, nhưng nó không đưa ra một dấu hiệu trực quan về những gì họ làm.
Để cung cấp một gợi ý rằng các item đã bị xóa,
hiển thị một "dấu hiệu" thể hiện item đã vuốt ra khỏi màn hình. Ở ví dụ này, dấu hiệu là nền đỏ.

Để thêm dấu hiệu,
cung cấp parameter của `background` cho `Dismissible`.

<!-- skip -->
```dart
Dismissible(
  // Show a red background as the item is swiped away.
  background: Container(color: Colors.red),
  key: Key(item),
  onDismissed: (direction) {
    setState(() {
      items.removeAt(index);
    });

    Scaffold
        .of(context)
        .showSnackBar(SnackBar(content: Text("$item dismissed")));
  },
  child: ListTile(title: Text('$item')),
);
```

## Ví dụ

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

// MyApp is a StatefulWidget. This allows updating the state of the
// widget when an item is removed.
class MyApp extends StatefulWidget {
  MyApp({Key key}) : super(key: key);

  @override
  MyAppState createState() {
    return MyAppState();
  }
}

class MyAppState extends State<MyApp> {
  final items = List<String>.generate(20, (i) => "Item ${i + 1}");

  @override
  Widget build(BuildContext context) {
    final title = 'Dismissing Items';

    return MaterialApp(
      title: title,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index) {
            final item = items[index];

            return Dismissible(
              // Each Dismissible must contain a Key. Keys allow Flutter to
              // uniquely identify widgets.
              key: Key(item),
              // Provide a function that tells the app
              // what to do after an item has been swiped away.
              onDismissed: (direction) {
                // Remove the item from the data source.
                setState(() {
                  items.removeAt(index);
                });

                // Then show a snackbar.
                Scaffold.of(context)
                    .showSnackBar(SnackBar(content: Text("$item dismissed")));
              },
              // Show a red background as the item is swiped away.
              background: Container(color: Colors.red),
              child: ListTile(title: Text('$item')),
            );
          },
        ),
      ),
    );
  }
}
```


<noscript>
  <img src="/images/cookbook/dismissible.gif" alt="Dismissible Demo" class="site-mobile-screenshot" />
</noscript>


[`Dismissible`]: {{site.api}}/flutter/widgets/Dismissible-class.html
[Working with long lists]: /docs/cookbook/lists/long-lists
