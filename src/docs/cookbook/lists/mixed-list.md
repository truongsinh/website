---
title: Tạo ra danh sách với nhiều loại items
description: Triển khai một danh sách gồm nhiều loại assets.
prev:
  title: Create a grid list
  path: /docs/cookbook/lists/grid-lists
next:
  title: Place a floating app bar above a list
  path: /docs/cookbook/lists/floating-app-bar
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Đôi khi bạn muốn tạo ra một danh sách có thể hiển thị nhiều loại nội dung khác
nhau. Ví dụ, bạn muốn tạo ra một danh sách với một tiêu đề, sau đó là những items
liên quan đến tiêu đề đó, và nối tiếp lại là một tiêu đề khác.

Để tạo ra được cấu trúc đó với Flutter, hãy:

  1. Tạo ra nguồn dữ liệu với nhiều loại items khác nhau.
  2. Chuyển nguồn dữ liệu thành danh sách các widget.

## 1. Tạo ra nguồn dữ liệu với nhiều loại items

### Loại items

Để biểu diễn nhiều loại items trong 1 danh sách, định nghĩa 1 class cho từng loại item.

Ví dụ dưới đây tạo ra một ứng dụng hiển thị một tiêu đề, theo sau là 5 tin nhắn.
Ta sẽ tạo ra 3 class: `ListItem`, `HeadingItem`, và `MessageItem`.

```dart
/// The base class for the different types of items the list can contain.
abstract class ListItem {
  /// The title line to show in a list item.
  Widget buildTitle(BuildContext context);

  /// The subtitle line, if any, to show in a list item.
  Widget buildSubtitle(BuildContext context);
}

/// A ListItem that contains data to display a heading.
class HeadingItem implements ListItem {
  final String heading;

  HeadingItem(this.heading);

  Widget buildTitle(BuildContext context) {
    return Text(
      heading,
      style: Theme.of(context).textTheme.headline,
    );
  }

  Widget buildSubtitle(BuildContext context) => null;
}

/// A ListItem that contains data to display a message.
class MessageItem implements ListItem {
  final String sender;
  final String body;

  MessageItem(this.sender, this.body);

  Widget buildTitle(BuildContext context) => Text(sender);

  Widget buildSubtitle(BuildContext context) => Text(body);
}
```

### Tạo ra danh sách items

Phần lớn thời gian, ta sẽ fetch dữ liệu từ internet hoặc từ một cơ sở dữ liệu
local và chuyển dữ liệu đó thành một danh sách các items.

Ví dụ dưới đây tạo ra 1 danh sách bao gồm 1 tiêu đề và 5 tin nhắn. Mỗi tin nhắn
sẽ thuộc 1 trong 3 loại: `ListItem`, `HeadingItem`, hoặc `MessageItem`.

<!-- skip -->
```dart
final items = List<ListItem>.generate(
  1200,
  (i) => i % 6 == 0
      ? HeadingItem("Heading $i")
      : MessageItem("Sender $i", "Message body $i"),
);
```

## 2. Chuyển nguồn dữ liệu thành các widget

Để chuyển mỗi item thành 1 widget, hãy sử dụng constructor [`ListView.builder()`][].

Tổng quát, hãy viết một hàm builder để kiểm tra loại item nào cần được tạo ra, và
trả về widget tương ứng phù hợp với loại item đó.

<!-- skip -->
```dart
ListView.builder(
  // Let the ListView know how many items it needs to build.
  itemCount: items.length,
  // Provide a builder function. This is where the magic happens.
  // Convert each item into a widget based on the type of item it is.
  itemBuilder: (context, index) {
    final item = items[index];

    return ListTile(
      title: item.buildTitle(context),
      subtitle: item.buildSubtitle(context),
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
    items: List<ListItem>.generate(
      1000,
      (i) => i % 6 == 0
          ? HeadingItem("Heading $i")
          : MessageItem("Sender $i", "Message body $i"),
    ),
  ));
}

class MyApp extends StatelessWidget {
  final List<ListItem> items;

  MyApp({Key key, @required this.items}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final title = 'Mixed List';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: ListView.builder(
          // Let the ListView know how many items it needs to build.
          itemCount: items.length,
          // Provide a builder function. This is where the magic happens.
          // Convert each item into a widget based on the type of item it is.
          itemBuilder: (context, index) {
            final item = items[index];

            return ListTile(
              title: item.buildTitle(context),
              subtitle: item.buildSubtitle(context),
            );
          },
        ),
      ),
    );
  }
}

/// The base class for the different types of items the list can contain.
abstract class ListItem {
  /// The title line to show in a list item.
  Widget buildTitle(BuildContext context);

  /// The subtitle line, if any, to show in a list item.
  Widget buildSubtitle(BuildContext context);
}

/// A ListItem that contains data to display a heading.
class HeadingItem implements ListItem {
  final String heading;

  HeadingItem(this.heading);

  Widget buildTitle(BuildContext context) {
    return Text(
      heading,
      style: Theme.of(context).textTheme.headline,
    );
  }

  Widget buildSubtitle(BuildContext context) => null;
}

/// A ListItem that contains data to display a message.
class MessageItem implements ListItem {
  final String sender;
  final String body;

  MessageItem(this.sender, this.body);

  Widget buildTitle(BuildContext context) => Text(sender);

  Widget buildSubtitle(BuildContext context) => Text(body);
}
```

<noscript>
  <img src="/images/cookbook/mixed-list.png" alt="Mixed list demo" class="site-mobile-screenshot" />
</noscript>


[`ListView.builder()`]: {{site.api}}/flutter/widgets/ListView/ListView.builder.html
