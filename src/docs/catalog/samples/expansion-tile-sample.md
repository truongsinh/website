---
title: ExpansionTile
description: Một ví dụ sử dụng ExpansionTiles.
deprecated: true
---

ExpansionTiles có thể được sử dụng để tạo ra các danh sách hai hoặc nhiều tầng.

<p>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-4">
        <div class="panel">
          <div class="panel-body">
            <img style="border:1px solid #000000" src="https://storage.googleapis.com/flutter-catalog/cb4a54db8fb3726bf4293b9cc5cb12ce16883803/expansion_tile_sample_small.png" alt="Android screenshot" class="img-fluid">
          </div>
          <!-- <div class="panel-footer">
            Android screenshot
          </div> -->
        </div>
      </div>
    </div>
  </div>
</p>

Ứng dụng này hiển thị dữ liệu phân cấp với ExpansionTiles. Nhấp vào một ô(tile)
sẽ hiển thị hoặc ẩn đi những phần tử con của nó. Khi một ô bị thu lại thì
những phần tử con của nó cũng bị xóa đi nên vùng nhớ chứa widget của danh sách chỉ 
phản ánh những gì có thể thấy được.

Khi hiển thị bên trong một cái có thể cuộn được (a scrollable) mà nó tạo danh sách các mục của nó một 
cách lười biếng, giống như một danh sách có thể cuộn được tạo với `ListView.builder()`, thì
ExpansionTiles có thể hoạt động khá hiệu quả, đặc biệt đối với những danh sách Material Design 
"mở rộng/thu lại".

Dùng thử ứng dụng này bằng cách tạo một dự án mới với `flutter create` và
thay thế nội dụng bên trong của `lib/main.dart` với đoạn mã nguồn sau.

```dart
// Copyright 2017 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

import 'package:flutter/material.dart';

class ExpansionTileSample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('ExpansionTile'),
        ),
        body: ListView.builder(
          itemBuilder: (BuildContext context, int index) =>
              EntryItem(data[index]),
          itemCount: data.length,
        ),
      ),
    );
  }
}

// One entry in the multilevel list displayed by this app.
class Entry {
  Entry(this.title, [this.children = const <Entry>[]]);

  final String title;
  final List<Entry> children;
}

// The entire multilevel list displayed by this app.
final List<Entry> data = <Entry>[
  Entry(
    'Chapter A',
    <Entry>[
      Entry(
        'Section A0',
        <Entry>[
          Entry('Item A0.1'),
          Entry('Item A0.2'),
          Entry('Item A0.3'),
        ],
      ),
      Entry('Section A1'),
      Entry('Section A2'),
    ],
  ),
  Entry(
    'Chapter B',
    <Entry>[
      Entry('Section B0'),
      Entry('Section B1'),
    ],
  ),
  Entry(
    'Chapter C',
    <Entry>[
      Entry('Section C0'),
      Entry('Section C1'),
      Entry(
        'Section C2',
        <Entry>[
          Entry('Item C2.0'),
          Entry('Item C2.1'),
          Entry('Item C2.2'),
          Entry('Item C2.3'),
        ],
      ),
    ],
  ),
];

// Displays one Entry. If the entry has children then it's displayed
// with an ExpansionTile.
class EntryItem extends StatelessWidget {
  const EntryItem(this.entry);

  final Entry entry;

  Widget _buildTiles(Entry root) {
    if (root.children.isEmpty) return ListTile(title: Text(root.title));
    return ExpansionTile(
      key: PageStorageKey<Entry>(root),
      title: Text(root.title),
      children: root.children.map(_buildTiles).toList(),
    );
  }

  @override
  Widget build(BuildContext context) {
    return _buildTiles(entry);
  }
}

void main() {
  runApp(ExpansionTileSample());
}
```

## Xem thêm

* Trang về thành phần Material [Lists]({{site.material}}/design/components/lists.html).
* Mã nguồn ở
  [examples/catalog/lib/expansion_tile_sample.dart]({{site.repo.flutter}}/tree/{{site.branch}}/examples/catalog/lib/expansion_tile_sample.dart).
