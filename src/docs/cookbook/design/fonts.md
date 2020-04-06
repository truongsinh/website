---
title: Use a custom font
description: How to use custom fonts.
prev:
  title: Update the UI based on orientation
  path: /docs/cookbook/design/orientation
next:
  title: Use themes to share colors and font styles
  path: /docs/cookbook/design/themes
---

Android và IOS đã cung cấp hệ thống fonts chữ chất lượng cao, nhưng yêu cầu về fonts chữ của các designers là được tùy chỉnh designers. Ví dụ, bạn muốn fonts chữ tùy chỉnh được xây dựng từ một designers, bạn có thể tải về từ [Google Fonts][].

{{site.alert.note}}
  Check out the [google_fonts][] package for direct access
  to almost 1000 open-sourced font families.
{{site.alert.end}}

Flutter sử dụng fonts tùy chỉnh và bạn có thể ứng dụng fonts tùy chỉnh trên toàn bộ ứng dụng hoặc vài widgets. Các bước để tạo một ứng dụng sử dụng fonts chữ tùy chỉnh

  1. Thêm fonts vào dự án.
  2. Khai báo font trong file.
  3. Đặt font làm mặc định.
  4. Sử dụng font trong các widget.

## 1. Thêm fonts vào dự án

Khi sử dụng fonts, bạn phải cài đặt các file fonts vào bên trong dự án. Các file thường được đặt trong thư mục `fonts` hoặc `assets`. của dự án.

Ví dụ bạn muốn thêm fonts Raleway và Roboto Mono 
vào dự án, cấu trúc thư mục có thể như sau:

```
awesome_app/
  fonts/
    Raleway-Regular.ttf
    Raleway-Italic.ttf
    RobotoMono-Regular.ttf
    RobotoMono-Bold.ttf
```

## 2. Khai báo font trong file

Bạn cần phải cài đặt đường dẫn để Flutter có thể tìm được fonts.
Một cách đơn giản là bạn có thể chỉ rõ đường dẫn bên trong `pubspec.yaml` file.

```yaml
flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: fonts/Raleway-Regular.ttf
        - asset: fonts/Raleway-Italic.ttf
          style: italic
    - family: RobotoMono
      fonts:
        - asset: fonts/RobotoMono-Regular.ttf
        - asset: fonts/RobotoMono-Bold.ttf
          weight: 700
```

### `pubspec.yaml` tùy chọn định nghĩa

`family` dùng để xác định tên của font, mà bạn dùng trong
 thuộc tính [`fontFamily`][] của một đối tượng [`TextStyle`][] .

`asset` là đường dẫn tới font file, có liên quan đến file `pubspec.yaml`.
Các file này chứa các khung, dàn cho font.
Khi xây ứng dụng, các files này thường nằm trong các gói hỗ trợ sẵn.

Một fonts có thể liên kết được nhiều file với nhiều nét chữ khác nhau:

  * Thuộc tính `weight` cho ta biết mức độ dày của fonts là
    các số nguyên bội số của 100, từ 100 tới 900.
    Các giá trị này tương ứng với[`FontWeight`][]
    và được sử dụng trong thuộc tính [`fontWeight`][] 
    của đối tượng [`TextStyle`][].

  * Thuộc tính `style` chỉ định nét chữ của fonts trong file là
    `italic` hay `normal`. Các giá trị này tương ứng
    [`FontStyle`][] và có thể dùng trong thuộc tính [`fontStyle`][] của đối tượng
    [`TextStyle`][].

## 3. Đặt font làm mặc định

Có 2 cách để sử dụng fonts cho các chữ trong ứng dụng: sử dụng fonts mặc định hoặc sử dụng fonts trong widgets.

Muốn đặt fonts chữ làm mặc định, đặt thuộc tính `fontFamily` là một phần của
`theme` ứng dụng. Các giá trị được cấp cho `fontFamily` phải khớp với tên `family`
đã được khai báo trong file `pubspec.yaml`.

<!-- skip -->
```dart
MaterialApp(
  title: 'Custom Fonts',
  // Set Raleway as the default app font.
  theme: ThemeData(fontFamily: 'Raleway'),
  home: MyHomePage(),
);
```

Nếu bạn muốn thêm thông tin về themes,
xem [Using Themes to share colors and font styles][].

## 4. Sử dụng fonts trong các widget

Nếu bạn muốn sử dụng font cho các widget,
chẳng hạn như `Text` widget,
cung cấp môjt [`TextStyle`][] cho widget.

Ở ví dụ này, ta dùng font RobotoMono cho `Text` widget.
Lưu ý,  `fontFamily` phải khớp với tên `family` đã được khai báo trong file
`pubspec.yaml`.

<!-- skip -->
```dart
Text(
  'Roboto Mono sample',
  style: TextStyle(fontFamily: 'RobotoMono'),
);
```

### TextStyle

Nếu đối tượng [`TextStyle`][] có độ dày, các kiểu chữ
không đúng với fonts đã khai báo,
máy sẽ dùng một trong những font chữ mặc định và cố gắng
phản hồi lại các yêu cầu về độ dày và kiểu chữ.

## Complete example

### Fonts

The Raleway and RobotoMono fonts were downloaded from
[Google Fonts][].

### `pubspec.yaml`

```yaml
name: custom_fonts
description: An example of how to use custom fonts with Flutter

dependencies:
  flutter:
    sdk: flutter

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: fonts/Raleway-Regular.ttf
        - asset: fonts/Raleway-Italic.ttf
          style: italic
    - family: RobotoMono
      fonts:
        - asset: fonts/RobotoMono-Regular.ttf
        - asset: fonts/RobotoMono-Bold.ttf
          weight: 700
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
      title: 'Custom Fonts',
      // Set Raleway as the default app font.
      theme: ThemeData(fontFamily: 'Raleway'),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // The AppBar uses the app-default Raleway font.
      appBar: AppBar(title: Text('Custom Fonts')),
      body: Center(
        // This Text widget uses the RobotoMono font.
        child: Text(
          'Roboto Mono sample',
          style: TextStyle(fontFamily: 'RobotoMono'),
        ),
      ),
    );
  }
}
```

![Custom Fonts Demo](/images/cookbook/fonts.png){:.site-mobile-screenshot}


[`fontFamily`]: {{site.api}}/flutter/painting/TextStyle/fontFamily.html
[`fontStyle`]: {{site.api}}/flutter/painting/TextStyle/fontStyle.html
[`FontStyle`]: {{site.api}}/flutter/dart-ui/FontStyle-class.html
[`fontWeight`]: {{site.api}}/flutter/painting/TextStyle/fontWeight.html
[`FontWeight`]: {{site.api}}/flutter/dart-ui/FontWeight-class.html
[Google Fonts]: https://fonts.google.com
[google_fonts]: {{site.pub-pkg}}/google_fonts
[`TextStyle`]: {{site.api}}/flutter/painting/TextStyle-class.html
[Using Themes to share colors and font styles]: /docs/cookbook/design/themes
