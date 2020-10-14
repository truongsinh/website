---
title: Export fonts from a package
description: How to export fonts from a package.
prev:
  title: Display a snackbars
  path: /docs/cookbook/design/snackbars
next:
  title: Update the UI based on orientation
  path: /docs/cookbook/design/orientation
---

Thay vì ta khai báo font là một phần của ứng dụng, bạn có thể sử dụng fonts từ trong các package có sẵn. Cách này rất thuận tiện để chia sẻ cùng một fonts trên nhiều dự án khác nhau, các lập trình viên thường công bố các package trên [pub.dev][].
Cách này gồm các bước như sau:

  1. Thêm font vào gói package.
  2. Thêm gói package và fonts vào ứng dụng.
  3. Sử dụng fonts.

{{site.alert.note}}
  Check out the [google_fonts][] package for direct access
  to almost 1000 open-sourced font families.
{{site.alert.end}}

## 1. Thêm font vào gói package

Để có thê xuất fonts từ các gói package, bạn nên để fonts files vào thư mục `lib` của package dự án. Bạn có thể để font files vào thư mục `lib`hoặc trong một thư mục con, như là `lib/fonts`.

Ở ví dụ này, giả sử bạn có một thư viện Flutter có tên `awesome_package` có các fonts nằm trong thư mục`lib/fonts`.

```
awesome_package/
  lib/
    awesome_package.dart
    fonts/
      Raleway-Regular.ttf
      Raleway-Italic.ttf
```

## 2. Thêm gói package và fonts vào ứng dụng

Bây giờ bạn có thể sử dụng font trong package bằng cách cập nhật file `pubspec.yaml` trong thư mục gốc của *ứng dụng*.

### Thêm gói package vào ứng dụng

```yaml
dependencies:
  awesome_package: <latest_version>
```

### Khai báo fonts

Sau khi đã thêm fonts vào package, bạn phải nói cho Flutter biết đường dẫn tới fonts từ bên trong `awesome_package`.

Để khai báo các gói fonts package, đường dẫn tới fonts được bắt đầu với
`packages/awesome_package`.
Để Flutter tìm fonts từ thư mục `lib` của các gói package.

```yaml
flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: packages/awesome_package/fonts/Raleway-Regular.ttf
        - asset: packages/awesome_package/fonts/Raleway-Italic.ttf
          style: italic
```

## 3. Sử dụng fonts

Dùng một [`TextStyle`][] để cho ta thấy sự thay đổi của các chữ viết.
Khi sử dụng các gói font package, Khai báo kiểu font bạn muốn dùng và gói package chứa fonts đó.

<!-- skip -->
```dart
Text(
  'Using the Raleway font from the awesome_package',
  style: TextStyle(
    fontFamily: 'Raleway',
    package: 'awesome_package',
  ),
);
```

## Hoàn thành ví dụ

### Fonts

Hai fonts Raleway và RobotoMono sẽ được tải về từ
[Google Fonts][].

### `pubspec.yaml`

```yaml
name: package_fonts
description: An example of how to use package fonts with Flutter

dependencies:
  awesome_package:
  flutter:
    sdk: flutter

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: packages/awesome_package/fonts/Raleway-Regular.ttf
        - asset: packages/awesome_package/fonts/Raleway-Italic.ttf
          style: italic
  uses-material-design: true
```

### `main.dart`

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Package Fonts',
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // The AppBar uses the app-default font.
      appBar: AppBar(title: Text('Package Fonts')),
      body: Center(
        // This Text widget uses the Raleway font.
        child: Text(
          'Using the Raleway font from the awesome_package',
          style: TextStyle(
            fontFamily: 'Raleway',
            package: 'awesome_package',
          ),
        ),
      ),
    );
  }
}
```

![Package Fonts Demo](/images/cookbook/package-fonts.png){:.site-mobile-screenshot}

[Google Fonts]: https://fonts.google.com
[google_fonts]: {{site.pub-pkg}}/google_fonts
[pub.dev]: {{site.pub}}
[`TextStyle`]: {{site.api}}/flutter/painting/TextStyle-class.html
