---
title: Hiệu ứng fade in và fade out cho widget
description: Cách tạo hiệu ứng fade in và fade out cho widget.
prev:
  title: Tạo hiệu ứng cho các thuộc tính của container
  path: /docs/cookbook/animation/animated-container
next:
  title: Thêm drawer vào một màn hình
  path: /docs/cookbook/design/drawer
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Các lập trình viên UI thường cần hiển thị và ẩn đi các thành phần trên màn hình. Tuy nhiên, các thành phần xuất hiện và biến mất nhanh chóng có thể gây khó chịu cho người dùng. Thay vì vậy, thêm hiệu ứng fade in/out kèm theo độ mờ sẽ tạo ra trải nghiệm mượt mà hơn.

Widget [`AnimatedOpacity`][] giúp ta tạo hiệu ứng mờ dễ dàng hơn.
Các bước như sau:
  1. Tạo một box sẽ dùng hiệu ứng fade in/out.
  2. Khai báo một `StatefulWidget`.
  3. Hiển thị một button để chuyển đổi chế độ hiển thị.
  4. Áp dụng hiệu ứng fade in/out vào cho box.

## 1. Tạo một box sẽ dùng hiệu ứng fade in/out

Đầu tiên, tạo một thứ gì đó để dùng hiệu ứng này. Ví dụ, vẽ một box màu xanh lá lên màn hình.

<!-- skip -->
```dart
Container(
  width: 200.0,
  height: 200.0,
  color: Colors.green,
);
```

## 2. Khai báo một `StatefulWidget`

Bây giờ bạn đã có một box màu xanh lá, bạn cần một cách để hiển thị nó.
Để làm được, hãy dùng [`StatefulWidget`][].

`StatefulWidget` là một lớp tạo ra một đối tượng `State`.
Đối tượng `State` lưu trữ một vài dữ liệu về ứng dụng và cung cấp một cách để cập nhật dữ liệu đó. Khi cập nhật dữ liệu, bạn cũng có thể yêu cầu Flutter rebuild lại UI có những thay đổi đó.

Trong trường hợp này, bạn có một ít dữ liệu sau:
một giá trị boolean cho thấy button có thể nhìn thấy được hay không.

Để khởi tạo một `StatefulWidget`, ta cần 2 lớp: một lớp
`StatefulWidget` và một lớp `State` tương ứng.
Mẹo hay: Các Flutter plugin cho Android Studio và VSCode bao gồm
snippet `stful` để tạo nhanh đoạn code này.

<!-- skip -->
```dart
// The StatefulWidget's job is to take data and create a State class.
// In this case, the widget takes a title, and creates a _MyHomePageState.
class MyHomePage extends StatefulWidget {
  final String title;

  MyHomePage({Key key, this.title}) : super(key: key);

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

// The State class is responsible for two things: holding some data you can
// update and building the UI using that data.
class _MyHomePageState extends State<MyHomePage> {
  // Whether the green box should be visible.
  bool _visible = true;

  @override
  Widget build(BuildContext context) {
    // The green box goes here with some other Widgets.
  }
}
```

## 3. Hiển thị một button để chuyển đổi chế độ hiển thị

Bây giờ bạn đã có một ít dữ liệu để xác định xem box màu xanh lá nhìn thấy hay không, bạn cần một cách để cập nhật dữ liệu đó.
Trong ví dụ này, nếu box nhìn thấy được, ẩn nó đi.
Nếu box đang bị ẩn, hiển thị nó ra.

Để xử lí việc này, hãy hiển thị một button. Khi người dùng bấm vào button,
chuyển giá trị boolean từ true thành false, hoặc false thành true.
Thực hiện thay đổi dùng [`setState()`][],
đây là một phương thước thuộc lớp `State`.
Điều này yêu cầu Flutter rebuild lại widget.

Để biết thêm thông tin về cách làm việc với dữ liệu đầu vào của người dùng,
xem phần [Gestures][] của cookbook.

<!-- skip -->
```dart
FloatingActionButton(
  onPressed: () {
    // Call setState. This tells Flutter to rebuild the
    // UI with the changes.
    setState(() {
      _visible = !_visible;
    });
  },
  tooltip: 'Toggle Opacity',
  child: Icon(Icons.flip),
);
```

## 4. Áp dụng hiệu ứng fade in/out vào cho box

Bạn có một box màu xanh lá trên màn hình và một button để chuyển đổi chế độ hiển thị
thành `true` hoặc `false`. Làm thế nào để áp dụng hiệu ứng fade in/out vào cho box với widget
[`AnimatedOpacity`][]?

Widget `AnimatedOpacity` yêu cầu 3 đối số  :

  * `opacity`: một giá trị từ 0.0 (không nhìn thấy được) đến 1.0 (nhìn thấy được hoàn toàn).
  * `duration`: Thời gian của hiệu ứng.
  * `child`: Widget cần dùng hiệu ứng. Trong trường hợp này là box màu xanh lá.

<!-- skip -->
```dart
AnimatedOpacity(
  // If the widget is visible, animate to 0.0 (invisible).
  // If the widget is hidden, animate to 1.0 (fully visible).
  opacity: _visible ? 1.0 : 0.0,
  duration: Duration(milliseconds: 500),
  // The green box must be a child of the AnimatedOpacity widget.
  child: Container(
    width: 200.0,
    height: 200.0,
    color: Colors.green,
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
    final appTitle = 'Opacity Demo';
    return MaterialApp(
      title: appTitle,
      home: MyHomePage(title: appTitle),
    );
  }
}

// The StatefulWidget's job is to take data and create a State class.
// In this case, the widget takes a title, and creates a _MyHomePageState.
class MyHomePage extends StatefulWidget {
  final String title;

  MyHomePage({Key key, this.title}) : super(key: key);

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

// The State class is responsible for two things: holding some data you can
// update and building the UI using that data.
class _MyHomePageState extends State<MyHomePage> {
  // Whether the green box should be visible
  bool _visible = true;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: AnimatedOpacity(
          // If the widget is visible, animate to 0.0 (invisible).
          // If the widget is hidden, animate to 1.0 (fully visible).
          opacity: _visible ? 1.0 : 0.0,
          duration: Duration(milliseconds: 500),
          // The green box must be a child of the AnimatedOpacity widget.
          child: Container(
            width: 200.0,
            height: 200.0,
            color: Colors.green,
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // Call setState. This tells Flutter to rebuild the
          // UI with the changes.
          setState(() {
            _visible = !_visible;
          });
        },
        tooltip: 'Toggle Opacity',
        child: Icon(Icons.flip),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/fade-in-out.gif" alt="Fade In and Out Demo" class="site-mobile-screenshot" />
</noscript>

[`AnimatedOpacity`]: {{site.api}}/flutter/widgets/AnimatedOpacity-class.html
[Gestures]: /docs/cookbook#gestures
[`StatefulWidget`]: {{site.api}}/flutter/widgets/StatefulWidget-class.html
[`setState()`]: {{site.api}}/flutter/widgets/State/setState.html
