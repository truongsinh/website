---
title: Display a snackbar
description: How to implement a snackbar to display messages.
prev:
  title: Add a drawer to a screen
  path: /docs/cookbook/design/drawer
next:
  title: Export fonts from a package
  path: /docs/cookbook/design/package-fonts
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Khi một số thao tác xảy ra thì việc thông báo cho người dùng rất có ích.
Ví dụ, khi người dùng kéo một tin nhắn ra khỏi danh sách, bạn muốn thông báo
với họ rằng tin nhắn đó đã được xoá.
Bạn cũng muốn cung cấp cho họ cơ hội để hoàn tác hành động đó.

Trong Material Design, đây là công việc của [`SnackBar`][].
Phương thức này áp dụng snackbar thông qua những bước sau:

  1. Tạo `Scaffold`.
  2. Hiển thị `Snackbar`.
  3. Cung cấp thao tác phụ.

## 1. Tạo `Scaffold`

Khi tạo ứng dụng theo nguyên tắc Material Design,
hãy thiết kế cho ứng dụng đó một cấu trúc giao diện thống nhất.
Ở ví dụ này, hiển thị `SnackBar` ở dưới đáy màn hình mà không chồng lên
những widget quan trọng khác, như `FloatingActionButton`.

Widget [`Scaffold`][], từ [material library][],
tạo cấu trúc giao diện này và đảm bảo rằng các widget quan trọng
không chồng lên nhau.

<!-- skip -->
```dart
Scaffold(
  appBar: AppBar(
    title: Text('SnackBar Demo'),
  ),
  body: SnackBarPage(), // Complete this code in the next step.
);
```

## 2. Hiển thị `SnackBar`

Sau khi tạo `Scaffold` thì ta sẽ hiển thị `SnackBar`.
Đầu tiên, tạo `SnackBar`, và hiển thị nó thông qua `Scaffold`.

<!-- skip -->
```dart
final snackBar = SnackBar(content: Text('Yay! A SnackBar!'));

// Find the Scaffold in the widget tree and use it to show a SnackBar.
Scaffold.of(context).showSnackBar(snackBar);
```

## 3. Cung cấp thao tác phụ

Bạn có thể muốn cung cấp cho người dùng một thao tác khi
SnackBar được hiển thị.
Ví dụ, nếu người dùng vô tình xoá một tin nhắn,
có thể họ sẽ muốn sử dụng một thao tác phụ trong SnackBar để phục hồi lại
tin nhắn đó.

Đây là một ví dụ của việc cung cấp
một `thao tác` phụ cho `SnackBar`:

<!-- skip -->
```dart
final snackBar = SnackBar(
  content: Text('Yay! A SnackBar!'),
  action: SnackBarAction(
    label: 'Undo',
    onPressed: () {
      // Some code to undo the change.
    },
  ),
);
```

## Ví dụ

{{site.alert.note}}
  Trong ví dụ này, SnackBar được hiển thị khi người dùng ấn một nút.
  Để tìm hiểu thêm về các thao tác nhập của người dùng,
  hãy đến phần [Gestures][] của cẩm nang.
{{site.alert.end}}

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(SnackBarDemo());

class SnackBarDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'SnackBar Demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text('SnackBar Demo'),
        ),
        body: SnackBarPage(),
      ),
    );
  }
}

class SnackBarPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: RaisedButton(
        onPressed: () {
          final snackBar = SnackBar(
            content: Text('Yay! A SnackBar!'),
            action: SnackBarAction(
              label: 'Undo',
              onPressed: () {
                // Some code to undo the change.
              },
            ),
          );

          // Find the Scaffold in the widget tree and use
          // it to show a SnackBar.
          Scaffold.of(context).showSnackBar(snackBar);
        },
        child: Text('Show SnackBar'),
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/snackbar.gif" alt="SnackBar Demo" class="site-mobile-screenshot" />
</noscript>


[Gestures]: /docs/cookbook#gestures
[`Scaffold`]: {{site.api}}/flutter/material/Scaffold-class.html
[`SnackBar`]: {{site.api}}/flutter/material/SnackBar-class.html
[material library]: {{site.api}}/flutter/material/material-library.html
