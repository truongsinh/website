---
title: Làm việc với ảnh được lưu trữ
description: Cách để làm việc với ảnh được lưu trữ.
prev:
  title: Làm ảnh mờ dần với một placeholder
  path: /docs/cookbook/images/fading-in-images
next:
  title: Sử dụng lists
  path: /docs/cookbook/lists/basic-list
---

Trong một số trường hơp sẽ thuận tiện hơn khi lưu trữ hình ảnh khi chúng được tải từ trên web, khi đó chúng có thể sử dụng một cách offline. Cho mục đích này, sử dụng gói [`cached_network_image`][].

Ngoài việc lưu trữ, gói `cached_network_image`
còn hỗ trợ placeholders và làm mờ ảnh
trong khi ảnh còn đang tải.

<!-- skip -->
```dart
CachedNetworkImage(
  imageUrl: 'https://picsum.photos/250?image=9',
);
```

## Adding a placeholder

Gói `cached_network_image` cho phép bạn sử dụng bất kì widget như là một placeholder. Trong ví dụ này, biểu diễn một spinner trong khi ảnh được tải.

<!-- skip -->
```dart
CachedNetworkImage(
  placeholder: (context, url) => CircularProgressIndicator(),
  imageUrl: 'https://picsum.photos/250?image=9',
);
```

## Ví dụ

<!-- skip -->
```dart
import 'package:flutter/material.dart';
import 'package:cached_network_image/cached_network_image.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Cached Images';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: Center(
          child: CachedNetworkImage(
            placeholder: (context, url) => CircularProgressIndicator(),
            imageUrl:
                'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}
```


[`cached_network_image`]: {{site.pub-pkg}}/cached_network_image
