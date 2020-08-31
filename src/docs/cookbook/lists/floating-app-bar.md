---
title: Place a floating app bar above a list
description: How to place a floating app bar above a list.
prev:
  title: Create lists with different types of items
  path: /docs/cookbook/lists/mixed-list
next:
  title: Work with long lists
  path: /docs/cookbook/lists/long-lists
js:
  - defer: true
    url: https://dartpad.dev/inject_embed.dart.js
---

Để người dùng dễ dàng xem danh sách các items,
bạn muốn ẩn app bar khi người dùng cuộn danh sách xuống.
Điều này có thể  thực hiện được nếu hiển thị một danh sách ứng dụng
có chiều cao chiếm nhiều không gian theo chiều dọc.

Thông thường, bạn tạo một app bar bằng cách cung cấp thuộc tính `appBar` cho
`Scaffold` widget. Điều này tạo ra một app bar cố định luôn nằm phía trên
`body` của `Scaffold`.

Khi chuyển app bar từ `Scaffold` widget vào
[`CustomScrollView`][] sẽ cho phép bạn có thể tạo một app bar
có thể  cuộn khỏi màn hình khi bạn cuộn qua danh sách các item
có trong `CustomScrollView`.

Bài viết này cho ta biết cách dùng `CustomScrollView` để hiển thị danh sách
items với một app bar ở trên cùng, khi người dùng cuộn danh sách thông qua các bước sau:
down the list using the following steps:

  1. Tạo `CustomScrollView`.
  2. Dùng `SliverAppBar` để thêm floating app bar.
  3. Tạo danh sách items bằng `SliverList`.

## 1. Tạo `CustomScrollView`

Để tạo floating app bar, ta đặt app bar bên trong
`CustomScrollView` mà chứa danh sách các items.
Điều này đồng bộ hóa vị trí cuộn của app bả và danh sách items.
Bạn sẽ nghĩ `CustomScrollView` widget như là `ListView`,
thứ cho phép bạn kết hợp nhiều hình thức cuộn danh sách và
widgets lại với nhau.

Các danh sách và widgets cung cấp cho 
`CustomScrollView` các phần tử được gọi là _slivers_. Có rất nhiều loại
slivers, chẳng hạn `SliverList`, `SliverGridList`, và `SliverAppBar`.
Thực tế, `ListView` và `GridView` widgets sử dụng `SliverList` và
`SliverGrid` widgets để thực hiện cho việc cuộn.

Trong ví dụ này, tạo `CustomScrollView` để chứa
`SliverAppBar` và `SliverList`. Ngoài ra, gỡ bỏ app bars mà bạn
cung cấp cho `Scaffold` widget.

<!-- skip -->
```dart
Scaffold(
  // No appBar property provided, only the body.
  body: CustomScrollView(
    // Add the app bar and list of items as slivers in the next steps.
    slivers: <Widget>[]
  ),
);
```

### 2. Dùng `SliverAppBar` để thêm floating app bar

Tiếp theo, thêm app bar vào [`CustomScrollView`][].
Flutter cung cấp [`SliverAppBar`][] widget,
giống như `AppBar` widget,
Sử dụng `SliverAppBar` để hiện thị tiêu đề,
tabs, hình ảnh và nhiều thứ khác.

Tuy nhiên, `SliverAppBar` cũng cung cấp cho bạn khả năng khởi tạo "floating"
app bar cuộn màn hình khi người dùng cuộn danh sách xuống.
Hơn nữa, bạn có thể tùy chỉnh `SliverAppBar` để thu nhỏ và 
mở rộng khi người dùng cuộn.

Để tạo hiệu ứng này:

  1. Bắt đầu app bar chỉ hiện thị một tiêu đề.
  2. Đặt thuộc tính `floating` là `true`.
     Điều này sẽ cho phép người dùng nhanh chóng tiết lộ thanh ứng dụng
     khi họ cuộn danh sách lên.
  3. Thêm `flexibleSpace` widget để lấp đầy phần
     `expandedHeight`.

<!-- skip -->
```dart
CustomScrollView(
  slivers: <Widget>[
    SliverAppBar(
      title: Text('Floating app bar'),
      // Allows the user to reveal the app bar if they begin scrolling back
      // up the list of items.
      floating: true,
      // Display a placeholder widget to visualize the shrinking size.
      flexibleSpace: Placeholder(),
      // Make the initial height of the SliverAppBar larger than normal.
      expandedHeight: 200,
    ),
  ],
);
```

{{site.alert.tip}}
  Play around with the
  [various properties you can pass to the `SliverAppBar` widget][],
  and use hot reload to see the results. For example, use an `Image`
  widget for the `flexibleSpace` property to create a background image that
  shrinks in size as it's scrolled offscreen.
{{site.alert.end}}


### 3. Tạo danh sách items bằng `SliverList`

Bây giờ bạn đã có một thanh app bar, thêm một danh sách items
`CustomScrollView`. Bạn có hai lựa chọn: [`SliverList`][]
hoặc [`SliverGrid`][].  Nếu bạn cần hiển thị danh sách các items lần lược,
use the `SliverList` widget.
Nếu bạn muốn hiển thị danh sách với dạng lứoi, dùng `SliverGrid` widget.

`SliverList` và `SliverGrid` widgets cần một parameter: 
[`SliverChildDelegate`][], sẽ cung cấp
từ widgets tới `SliverList` hoặc `SliverGrid`.
Ví dụ, [`SliverChildBuilderDelegate`][]
cho phép bạn tạo một danh sách items được xây dựng một cách đơn giản khi bạn cuộn,
giống như `ListView.builder` widget.

<!-- skip -->
```dart
// Create a SliverList.
SliverList(
  // Use a delegate to build items as they're scrolled on screen.
  delegate: SliverChildBuilderDelegate(
    // The builder function returns a ListTile with a title that
    // displays the index of the current item.
    (context, index) => ListTile(title: Text('Item #$index')),
    // Builds 1000 ListTiles
    childCount: 1000,
  ),
)
```

## Interactive example

```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  MyApp({Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final title = 'Floating App Bar';

    return MaterialApp(
      title: title,
      home: Scaffold(
        // No appbar provided to the Scaffold, only a body with a
        // CustomScrollView.
        body: CustomScrollView(
          slivers: <Widget>[
            // Add the app bar to the CustomScrollView.
            SliverAppBar(
              // Provide a standard title.
              title: Text(title),
              // Allows the user to reveal the app bar if they begin scrolling
              // back up the list of items.
              floating: true,
              // Display a placeholder widget to visualize the shrinking size.
              flexibleSpace: Placeholder(),
              // Make the initial height of the SliverAppBar larger than normal.
              expandedHeight: 200,
            ),
            // Next, create a SliverList
            SliverList(
              // Use a delegate to build items as they're scrolled on screen.
              delegate: SliverChildBuilderDelegate(
                // The builder function returns a ListTile with a title that
                // displays the index of the current item.
                (context, index) => ListTile(title: Text('Item #$index')),
                // Builds 1000 ListTiles
                childCount: 1000,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/images/cookbook/floating-app-bar.gif" alt="Use list demo" class="site-mobile-screenshot"/> 
</noscript>


[`CustomScrollView`]: {{site.api}}/flutter/widgets/CustomScrollView-class.html
[`SliverAppBar`]: {{site.api}}/flutter/material/SliverAppBar-class.html
[`SliverChildBuilderDelegate`]: {{site.api}}/flutter/widgets/SliverChildBuilderDelegate-class.html
[`SliverChildDelegate`]: {{site.api}}/flutter/widgets/SliverChildDelegate-class.html
[`SliverGrid`]: {{site.api}}/flutter/widgets/SliverGrid-class.html
[`SliverList`]: {{site.api}}/flutter/widgets/SliverList-class.html
[various properties you can pass to the `SliverAppBar` widget]: {{site.api}}/flutter/material/SliverAppBar/SliverAppBar.html
