---
title: Update the UI based on orientation
description: Respond to a change in the screen's orientation.
prev:
  title: Export fonts from a package
  path: /docs/cookbook/design/package-fonts
next:
  title: Use custom fonts
  path: /docs/cookbook/design/fonts
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Trong vài trường hợp,
bạn muốn thay đổi hiện thị của ứng dụng khi người dùng xoay màn hình từ chế độ chân dung sang chế độ phong cảnh. Ví dụ, ứng dụng sẽ hiển thị một item, trượt qua để thấy item tiếp theo, còn với chế độ phong cảnh, các item sẽ hiển thị ngang.

Trong Flutter, bạn có thể xây dựng nhiều lớp bố trí khác nhau dựa vào [`Orientation`][] đã cho.
Trong ví dụ này, xây dựng một danh sách hiển thị 2 cột trong chế độ chân dung và 3 cột trong chế độ phong cảnh thông qua các bước sau:

  1. Dùng `GridView` để hiện thị định dạng 2 cột .
  2. Sử dụng `OrientationBuilder` để thay đổi số lượng cột hiển thị.

## 1. Dùng `GridView` để hiện thị định dạng 2 cột

Đầu tiên, tạo một danh sách items để làm việc. Thay vì sử dụng một danh sách bình thường, ta tạo một danh sách các items sẽ hiện thí trong lưới. Tiếp theo, ta tạo một lưới với hai cột

<!-- skip -->
```dart
GridView.count(
  // A list with 2 columns
  crossAxisCount: 2,
  // ...
);
```

Tìm hiểu thêm về `GridViews`, xem [Creating a grid list][].

## 2. Sử dụng `OrientationBuilder` để thay đổi số lượng cột hiển thị

Để xác định `Orientation`  ứng dụng hiện tại, sử dụng
[`OrientationBuilder`][] widget.
 `OrientationBuilder` sẽ tính toán các `Orientation` bằng các so sánh chiều rộng và chiều cao có sẵn của các widget gốc, và xây dựng lại khi kích thước gốc bị thay đổi.

Sử dụng `Orientation`, xây dựng danh sách hiển thị hai cột ở chế độ chân dung hay ba cột ở chế độ phong cảnh

<!-- skip -->
```dart
OrientationBuilder(
  builder: (context, orientation) {
    return GridView.count(
      // Create a grid with 2 columns in portrait mode,
      // or 3 columns in landscape mode.
      crossAxisCount: orientation == Orientation.portrait ? 2 : 3,
    );
  },
);
```

{{site.alert.note}}
  If you're interested in the orientation of the screen,
  rather than the amount of space available to the parent,
  use `MediaQuery.of(context).orientation` instead of an
  `OrientationBuilder` widget.
{{site.alert.end}}

## Ví dụ

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-500px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final appTitle = 'Orientation Demo';

    return MaterialApp(
      title: appTitle,
      home: OrientationList(
        title: appTitle,
      ),
    );
  }
}

class OrientationList extends StatelessWidget {
  final String title;

  OrientationList({Key key, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(title)),
      body: OrientationBuilder(
        builder: (context, orientation) {
          return GridView.count(
            // Create a grid with 2 columns in portrait mode, or 3 columns in
            // landscape mode.
            crossAxisCount: orientation == Orientation.portrait ? 2 : 3,
            // Generate 100 widgets that display their index in the List.
            children: List.generate(100, (index) {
              return Center(
                child: Text(
                  'Item $index',
                  style: Theme.of(context).textTheme.headline,
                ),
              );
            }),
          );
        },
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/orientation.gif" alt="Orientation Demo" class="site-mobile-screenshot" />
</noscript>


[Creating a grid list]: /docs/cookbook/lists/grid-lists
[`Orientation`]: {{site.api}}/flutter/widgets/Orientation-class.html
[`OrientationBuilder`]: {{site.api}}/flutter/widgets/OrientationBuilder-class.html
