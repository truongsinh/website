---
title: Animate a widget across screens
description: How to animate a widget from one screen to another
prev:
  title: Report errors to a service
  path: /docs/cookbook/maintenance/error-reporting
next:
  title: Navigate to a new screen and back
  path: /docs/cookbook/navigation/navigation-basics
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Thông thường để hướng dẫn người dùng sử dụng app, ta thường chuyển động từ màn hình
này sang màn hình khác. Một kỹ thuật chung trong việc chuyển đổi widget sang màn hình kế tiếp. Điều này tạo hiệu ứng trực quan giữa hai màn hình.

Dùng [`Hero`][] widget
để chuyển đổi sang màn hình kế tiếp.
THướng dẫn gồm các bước:

  1. Tạo hai màn hình hiển thị cùng một ảnh.
  2. Thêm `Hero` widget vào màn hình đầu tiên.
  3. Thêm `Hero` widget vào màn hình thứ hai.

## 1. Tạo hai màn hình hiển thị cùng một ảnh

Ở ví dụ này, ta hiển thị một ảnh ở cả hai màn hình.
Chuyển sang màn hình thứ hai khi người dùng chạm vào ảnh. Bây giờ, tạo một cấu trúc ảo;
để quản lí chuyển động trong cho các bước tiếp theo.

{{site.alert.note}}
  This example builds upon the
  [Navigate to a new screen and back][]
  and [Handle taps][] recipes.
{{site.alert.end}}

```dart
class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Main Screen'),
      ),
      body: GestureDetector(
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (_) {
            return DetailScreen();
          }));
        },
        child: Image.network(
          'https://picsum.photos/250?image=9',
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        onTap: () {
          Navigator.pop(context);
        },
        child: Center(
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}
```

## 2. Thêm `Hero` widget vào màn đầu tiên

Để kết nối hai màn hình với nhau trong một chuyển động, đóng khung `Image` widget ở cả hai màn hình với `Hero` widget.
`Hero` widget yêu cầu hai arguments:

<dl>
  <dt>`tag`</dt>
  <dd>Đối tượng sẽ nhận dạng `Hero`.
      Tham số phải giống nhau ở hai màn hình.</dd>
  <dt>`child`</dt>
  <dd>widget sẽ chuyển động.</dd>
</dl>

<!-- skip -->
```dart
Hero(
  tag: 'imageHero',
  child: Image.network(
    'https://picsum.photos/250?image=9',
  ),
);
```

## 3. Thêm `Hero` widget vào màn hình thứ hai

Để hoàn thành kết nối với màn hình thứ nhất,
đóng khung `Image` trong màn hình thứ hai với `Hero`
widget, `tag` phaỉ giống với `Hero` ở màn hình đầu tiên.

Sau khi cài đặt `Hero` widget ở màn hình thứ hai,
chuyển động giữa hai màn hình sẽ thực hiện.

<!-- skip -->
```dart
Hero(
  tag: 'imageHero',
  child: Image.network(
    'https://picsum.photos/250?image=9',
  ),
);
```

{{site.alert.note}}
  This code is identical to what you have on the first screen.
  As a best practice, create a reusable widget instead of
  repeating code. This example uses identical code for both
  widgets, for simplicity.
{{site.alert.end}}

## Interactive example

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(HeroApp());

class HeroApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Transition Demo',
      home: MainScreen(),
    );
  }
}

class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Main Screen'),
      ),
      body: GestureDetector(
        child: Hero(
          tag: 'imageHero',
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (_) {
            return DetailScreen();
          }));
        },
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        child: Center(
          child: Hero(
            tag: 'imageHero',
            child: Image.network(
              'https://picsum.photos/250?image=9',
            ),
          ),
        ),
        onTap: () {
          Navigator.pop(context);
        },
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/hero.gif" alt="Hero demo" class="site-mobile-screenshot" />
</noscript>


[Handle taps]: /docs/cookbook/gestures/handling-taps
[`Hero`]: {{site.api}}/flutter/widgets/Hero-class.html
[Navigate to a new screen and back]: /docs/cookbook/navigation/navigation-basics
