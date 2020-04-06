---
title: Animate the properties of a container
description: How to animate properties of a container using implicit animations.
prev:
  title: Animate a widget using a physics simulation
  path: /docs/cookbook/animation/physics-simulation
next:
  title: Fade a widget in and out
  path: /docs/cookbook/animation/opacity-animation
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

<!-- ---
title: Animate the properties of a container
description: How to animate properties of a container using implicit animations.
prev:
  title: Animate a widget using a physics simulation
  path: /docs/cookbook/animation/physics-simulation
next:
  title: Fade a widget in and out
  path: /docs/cookbook/animation/opacity-animation
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
--- -->

<!--The [`Container`][] class provides a convenient way
to create a widget with specific properties:
width, height, background color, padding, borders, and more.-->
Lớp [`Container`][] cung cấp một cách thuận tiện để tạo một widget với những thuộc tính riêng biệt sau: width, height, background color, padding, borders, ...

<!--Simple animations often involve changing these properties over time.
For example,
you might want to animate the background color from grey to green to
indicate that an item has been selected by the user.-->
Việc đơn giản hoá những hoạt ảnh bao gồm thay đổi những thuộc tính này theo thời gian. Ví dụ như bạn muốn chuyển đổi màu của phong nền từ xám đến xanh lá để cho thấy là một mục đã được chọn bởi người dùng.

<!--To animate these properties,
Flutter provides the [`AnimatedContainer`][] widget.
Like the `Container` widget, `AnimatedContainer` allows you to define
the width, height, background colors, and more. However, when the
`AnimatedContainer` is rebuilt with new properties, it automatically
animates between the old and new values. In Flutter, these types of
animations are known as "implicit animations."-->
Để tạo chuyển động cho những thuộc tính này, Flutter cung cấp widget ['AnimatedContainer']. Cũng như `Container` widget, `AnimatedContainer` cho phép người dùng tự định nghĩa chiều rộng, chiều dài, màu phông nền, vân vân. Tuy nhiên, khi `AnimatedContainer` được xây dựng lại với các thuộc tính mới, nó tự động chuyển đổi giữa các giá trị cũ và mới. Trong Flutter, những kiểu hoạt ảnh này được biết đến như là "hoạt ảnh ngầm."

<!--This recipe describes how to use an `AnimatedContainer` to animate the size,
background color, and border radius when the user taps a button
using the following steps:

  1. Create a StatefulWidget with default properties.
  2. Build an `AnimatedContainer` using the properties.
  3. Start the animation by rebuilding with new properties.-->
Hướng dẫn này mô tả cách sử dụng một `AnimatedContainer` để tạo chuyển đổi kích thước, màu phông nền cùng với bán kính biên khi người dùng bấn nút sử dụng những bước sau:

  1. Tạo một StatefulWidget với thuộc tính mặc định.
  2. Xây dựng một `AnimatedContainer` sử dụng các thuộc tính.
  3. Tạo nên hoạt ảnh bằng cách xây dựng lại với những thuộc tính mới.

<!--
## 1. Create a StatefulWidget with default properties

To start, create [`StatefulWidget`][] and [`State`][] classes.
Use the custom State class to define the properties that change over
time. In this example, that includes the width, height, color, and border
radius. You can also define the default value of each property.

These properties belong to a custom `State` class so they
can be updated when the user taps a button. --->

## 1. Tạo một StatefulWidget với những thuộc tính mặc định

Đầu tiên, hãy tạo lớp [`StatefulWidget`][] và lớp [`State`][]. Sử dụng lớp thủ công State để định nghĩa các thuộc tính thay đổi theo thời gian. Trong ví dụ này, chúng bao gồm width, height, color và border radius. Bạn cũng có thể định nghĩa giá trị mặc định của từng thuộc tính.

Những thuộc tính này thuộc về lớp thủ công State nên chúng có thể được cập nhật khi người dùng bấm nút.

<!-- skip -->
```dart
class AnimatedContainerApp extends StatefulWidget {
  @override
  _AnimatedContainerAppState createState() => _AnimatedContainerAppState();
}

class _AnimatedContainerAppState extends State<AnimatedContainerApp> {
  // Define the various properties with default values. Update these properties
  // when the user taps a FloatingActionButton.
  double _width = 50;
  double _height = 50;
  Color _color = Colors.green;
  BorderRadiusGeometry _borderRadius = BorderRadius.circular(8);

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next steps.
  }
}
```
<!--
## 2. Build an `AnimatedContainer` using the properties

Next, build the `AnimatedContainer` using the properties defined in the
previous step. Furthermore, provide a `duration` that defines how long
the animation should run.-->

## 2. Xây dựng một `AnimatedContainer` sử dụng các thuộc tính

Tiếp theo hãy xây dựng `AnimatedContainer` sử dụng những thuộc tính được định nghĩa ở bước 1. Ngoài ra hãy cung cấp một `khoảng thời gian` để cho biết mỗi hoạt ảnh tốn bao lâu để chạy hết.

<!-- skip -->
```dart
AnimatedContainer(
  // Use the properties stored in the State class.
  width: _width,
  height: _height,
  decoration: BoxDecoration(
    color: _color,
    borderRadius: _borderRadius,
  ),
  // Define how long the animation should take.
  duration: Duration(seconds: 1),
  // Provide an optional curve to make the animation feel smoother.
  curve: Curves.fastOutSlowIn,
);
```

<!-- ## 3. Start the animation by rebuilding with new properties

Finally, start the animation by rebuilding the
`AnimatedContainer` with the new properties.
How to trigger a rebuild?
Use the [`setState()`][] method.

Add a button to the app. When the user taps the button, update
the properties with a new width, height, background color and border radius
inside a call to `setState()`.

A real app typically transitions between fixed values (for example,
from a grey to a green background). For this app,
generate new values each time the user taps the button. -->

## 3. Bắt đầu animation bằng các xây dụng lại với các thuộc tính mới

Cuối cùng, bắt đầu chạy hoạt ảnh bằng cách xây dựng lại `AnimatedContainer` với các thuộc tính mới.
Làm cách nào để kích hoạt việc xây dụng lại ?
Hãy sử dụng phương phức [`setState()`][]

Thêm một nút bấm cho ứng dụng. Khi người dùng bấm nút, cập nhật thuộc tính với chiều dài, chiều rộng, màu phông nền và bán kính biên mới trong bên hàm gọi tới `setState()`.

Một ứng dụng thực tế thường chuyển đổi giữa các giá trị cố định (ví dụ như, chuyển đổi từ phông nền màu xám thành phông nền màu xanh). Đối với ứng dụng này, tạo giá trị mới mỗi lần người dùng bấm nút.

<!-- skip -->
```dart
FloatingActionButton(
  child: Icon(Icons.play_arrow),
  // When the user taps the button
  onPressed: () {
    // Use setState to rebuild the widget with new values.
    setState(() {
      // Create a random number generator.
      final random = Random();

      // Generate a random width and height.
      _width = random.nextInt(300).toDouble();
      _height = random.nextInt(300).toDouble();

      // Generate a random color.
      _color = Color.fromRGBO(
        random.nextInt(256),
        random.nextInt(256),
        random.nextInt(256),
        1,
      );

      // Generate a random border radius.
      _borderRadius =
          BorderRadius.circular(random.nextInt(100).toDouble());
    });
  },
);
```

<!-- ## Interactive example -->
## Ví dụ tương tác

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'dart:math';

import 'package:flutter/material.dart';

void main() => runApp(AnimatedContainerApp());

class AnimatedContainerApp extends StatefulWidget {
  @override
  _AnimatedContainerAppState createState() => _AnimatedContainerAppState();
}

class _AnimatedContainerAppState extends State<AnimatedContainerApp> {
  // Define the various properties with default values. Update these properties
  // when the user taps a FloatingActionButton.
  double _width = 50;
  double _height = 50;
  Color _color = Colors.green;
  BorderRadiusGeometry _borderRadius = BorderRadius.circular(8);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('AnimatedContainer Demo'),
        ),
        body: Center(
          child: AnimatedContainer(
            // Use the properties stored in the State class.
            width: _width,
            height: _height,
            decoration: BoxDecoration(
              color: _color,
              borderRadius: _borderRadius,
            ),
            // Define how long the animation should take.
            duration: Duration(seconds: 1),
            // Provide an optional curve to make the animation feel smoother.
            curve: Curves.fastOutSlowIn,
          ),
        ),
        floatingActionButton: FloatingActionButton(
          child: Icon(Icons.play_arrow),
          // When the user taps the button
          onPressed: () {
            // Use setState to rebuild the widget with new values.
            setState(() {
              // Create a random number generator.
              final random = Random();

              // Generate a random width and height.
              _width = random.nextInt(300).toDouble();
              _height = random.nextInt(300).toDouble();

              // Generate a random color.
              _color = Color.fromRGBO(
                random.nextInt(256),
                random.nextInt(256),
                random.nextInt(256),
                1,
              );

              // Generate a random border radius.
              _borderRadius =
                  BorderRadius.circular(random.nextInt(100).toDouble());
            });
          },
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/animated-container.gif" alt="AnimatedContainer demo showing a box growing and shrinking in size while changing color and border radius" class="site-mobile-screenshot" />
</noscript>


[`AnimatedContainer`]: {{site.api}}/flutter/widgets/AnimatedContainer-class.html
[`Container`]: {{site.api}}/flutter/widgets/Container-class.html
[`setState()`]: {{site.api}}/flutter/widgets/State/setState.html
[`State`]: {{site.api}}/flutter/widgets/State-class.html
[`StatefulWidget`]: {{site.api}}/flutter/widgets/StatefulWidget-class.html
