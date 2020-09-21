---
title: Navigate to a new screen and back
description: How to navigate between routes.
prev:
  title: Animate a widget across screens
  path: /docs/cookbook/navigation/hero-animations
next:
  title: Navigate with named routes
  path: /docs/cookbook/navigation/named-routes
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Hầu hết app đều chứa một số màn hình để hiển thị các loại thông tin khác nhau.
Ví dụ, một app có thể màn hình hiển thị sản phẩm.
Khi người dùng chạm vào hình ảnh của sản phẩm, một màn hình sẽ xuất hiện để hiển thị chi tiết về sản phẩm.

{{site.alert.secondary}}
  **Terminology**: In Flutter, _screens_ and _pages_ are called _routes_.
  The remainder of this recipe refers to routes.
{{site.alert.end}}

Trong Android, route tương đương với Activity.
Trong iOS, route tương đương với ViewController.
Nhưng với Flutter, route chỉ là một widget.

Chuyển sang route mới bằng [`Navigator`][].

Một số phần tiếp theo cho ta biết cách điều hướng giwaxx hai routes, thông qua các bước sau:

  1. Tạo hai routes.
  2. Chuyển đến route thứ hai với Navigator.push().
  3. Trở về route thứ nhất thông qua Navigator.pop().

## 1. Tạo hai routes

Đầu tiên, tạo hai routes để ta thao tác. Vì đây là ví dụ cơ bản, mỗi route chỉ chứa một button. Chạm vào button ở route đầu tiên để chuyển đến route thứ hai. Chạm vào button ở route thứ hai để trở về route đầu tiên.

Ta cài đặt cấu trúc như sau:

```dart
class FirstRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First Route'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('Open route'),
          onPressed: () {
            // Navigate to second route when tapped.
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Second Route"),
      ),
      body: Center(
        child: RaisedButton(
          onPressed: () {
            // Navigate back to first route when tapped.
          },
          child: Text('Go back!'),
        ),
      ),
    );
  }
}
```

## 2. Chuyển đến route thứ hai với Navigator.push()

Để chuyển sang một route mới, dùng [`Navigator.push()`][]
method. `push()` method thêm một `Route` vào stack của routes được quản lí bởi `Navigator`. Là nơi mà bạn biết được `Route` tới từ đâu?
Bạn có thể tự tạo thủ công, hoặc dùng [`MaterialPageRoute`][],
công cụ hữu ích bởi vì nó chuyển đổi sang route mới bằng sử dụng nền tảng đặc biệt cho chuyển động.

Trong `build()` method của `FirstRoute` widget,
ta cập nhật `onPressed()`:

<!-- skip -->
```dart
// Within the `FirstRoute` widget
onPressed: () {
  Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => SecondRoute()),
  );
}
```

## 3. Trở về route thứ nhất thông qua Navigator.pop()
Làm các nào để đóng route thứ hai và trở về route đầu tiên?
Ta sẽ dùng [`Navigator.pop()`][] method.
`pop()` method sẽ xóa `Route` hiện tại từ stack của routes được quản lí bởi `Navigator`.

Để cài đặt cách trở về route gốc, ta cập nhật `onPressed()`
bên trong `SecondRoute` widget:

<!-- skip -->
```dart
// Within the SecondRoute widget
onPressed: () {
  Navigator.pop(context);
}
```

## Ví dụ

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    title: 'Navigation Basics',
    home: FirstRoute(),
  ));
}

class FirstRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First Route'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('Open route'),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondRoute()),
            );
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Second Route"),
      ),
      body: Center(
        child: RaisedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: Text('Go back!'),
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/navigation-basics.gif" alt="Navigation Basics Demo" class="site-mobile-screenshot" />
</noscript>


[`MaterialPageRoute`]: {{site.api}}/flutter/material/MaterialPageRoute-class.html
[`Navigator`]: {{site.api}}/flutter/widgets/Navigator-class.html
[`Navigator.pop()`]: {{site.api}}/flutter/widgets/Navigator/pop.html
[`Navigator.push()`]: {{site.api}}/flutter/widgets/Navigator/push.html
