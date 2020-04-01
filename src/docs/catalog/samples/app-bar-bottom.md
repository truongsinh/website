---
title: AppBar với Bottom widget tùy chỉnh
description: Một ví dụ sử dụng Bottom widget cho AppBar.
deprecated: true
---

Bất kỳ widget nào có sử dụng lớp PreferredSize đều có thể xuất hiện ở dưới cùng của AppBar.

<p>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-4">
        <div class="panel">
          <div class="panel-body">
            <img style="border:1px solid #000000" src="https://storage.googleapis.com/flutter-catalog/cb4a54db8fb3726bf4293b9cc5cb12ce16883803/app_bar_bottom_small.png" alt="Android screenshot" class="img-fluid">
          </div>
          <!-- <div class="panel-footer">
            Android screenshot
          </div> -->
        </div>
      </div>
    </div>
  </div>
</p>

Thông thường thì widget được sử dụng ở dưới cùng của AppBar là TabBar; tuy nhiên có thể thay thế nó bằng bất kỳ widget nào có chứa lớp PreferredSize. Trong ứng dụng này, widget được sử dụng là TabPageSelector, dùng để hiển thị vị trí tương đối của trang đang chọn trong TabBarView của ứng dụng so với các trang còn lại. Mũi tên trên thanh công cụ (một phần của AppBar) giúp người sử dụng chọn trang trước hoặc trang kế tiếp.

Khám phá ứng dụng sau đây bằng cách tạo một project mới với `flutter create` và thay thế nội dụng trong file `lib/main.dart` thành mã nguồn dưới đây.

```dart
// Copyright 2017 The Chromium Authors. All rights reserved.
// Việc sử dụng mã nguồn này được quản lý bởi giấy phép BSD.
// Xem thêm tại tập tin LICENSE.

import 'package:flutter/material.dart';

class AppBarBottomSample extends StatefulWidget {
  @override
  _AppBarBottomSampleState createState() => _AppBarBottomSampleState();
}

class _AppBarBottomSampleState extends State<AppBarBottomSample>
    with SingleTickerProviderStateMixin {
  TabController _tabController;

  @override
  void initState() {
    super.initState();
    _tabController = TabController(vsync: this, length: choices.length);
  }

  @override
  void dispose() {
    _tabController.dispose();
    super.dispose();
  }

  void _nextPage(int delta) {
    final int newIndex = _tabController.index + delta;
    if (newIndex < 0 || newIndex >= _tabController.length) return;
    _tabController.animateTo(newIndex);
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('AppBar Bottom Widget'),
          leading: IconButton(
            tooltip: 'Previous choice',
            icon: const Icon(Icons.arrow_back),
            onPressed: () {
              _nextPage(-1);
            },
          ),
          actions: <Widget>[
            IconButton(
              icon: const Icon(Icons.arrow_forward),
              tooltip: 'Next choice',
              onPressed: () {
                _nextPage(1);
              },
            ),
          ],
          bottom: PreferredSize(
            preferredSize: const Size.fromHeight(48.0),
            child: Theme(
              data: Theme.of(context).copyWith(accentColor: Colors.white),
              child: Container(
                height: 48.0,
                alignment: Alignment.center,
                child: TabPageSelector(controller: _tabController),
              ),
            ),
          ),
        ),
        body: TabBarView(
          controller: _tabController,
          children: choices.map((Choice choice) {
            return Padding(
              padding: const EdgeInsets.all(16.0),
              child: ChoiceCard(choice: choice),
            );
          }).toList(),
        ),
      ),
    );
  }
}

class Choice {
  const Choice({this.title, this.icon});

  final String title;
  final IconData icon;
}

const List<Choice> choices = const <Choice>[
  const Choice(title: 'CAR', icon: Icons.directions_car),
  const Choice(title: 'BICYCLE', icon: Icons.directions_bike),
  const Choice(title: 'BOAT', icon: Icons.directions_boat),
  const Choice(title: 'BUS', icon: Icons.directions_bus),
  const Choice(title: 'TRAIN', icon: Icons.directions_railway),
  const Choice(title: 'WALK', icon: Icons.directions_walk),
];

class ChoiceCard extends StatelessWidget {
  const ChoiceCard({Key key, this.choice}) : super(key: key);

  final Choice choice;

  @override
  Widget build(BuildContext context) {
    final TextStyle textStyle = Theme.of(context).textTheme.display1;
    return Card(
      color: Colors.white,
      child: Center(
        child: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: <Widget>[
            Icon(choice.icon, size: 128.0, color: textStyle.color),
            Text(choice.title, style: textStyle),
          ],
        ),
      ),
    );
  }
}

void main() {
  runApp(AppBarBottomSample());
}
```

## Xem thêm

* [Components-Tabs]({{site.material}}/guidelines/components/tabs.html)
  trong trang về [Material Design]({{site.material}}).
* Mã nguồn ở
  [examples/catalog/lib/app_bar_bottom.dart]({{site.repo.flutter}}/tree/{{site.branch}}/examples/catalog/lib/animated_list.dart).
