---
title: AnimatedList
description: Một ví dụ sử dụng AnimatedList.
deprecated: true
---

AnimatedList sẽ hiển thị danh sách các <i>thẻ</i> (card) được đồng bộ hóa với ListModel trong ứng dụng đó. Khi môt mục được thêm vào hoặc xóa khỏi model thì thẻ tương ứng cũng sẽ xuất hiện hoặc biến mất.

<p>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-4">
        <div class="panel">
          <div class="panel-body">
            <img style="border:1px solid #000000" src="https://storage.googleapis.com/flutter-catalog/cb4a54db8fb3726bf4293b9cc5cb12ce16883803/animated_list_small.png" alt="Android screenshot" class="img-fluid">
          </div>
          <!-- <div class="panel-footer">
            Android screenshot
          </div> -->
        </div>
      </div>
    </div>
  </div>
</p>

Nhấn vào một mục để chọn, nhấn lần nữa để bỏ chọn. Nhấn '+' để thêm mục mới tại vị trí đang chọn, và nhấn '-' để xóa đi mục đang chọn. Bộ xử lý các thao tác nhấn này sẽ thêm hoặc xóa các mục trong `ListModel<E>`, một <i>đóng gói</i> (encapsulation) đơn giản của `List<E>`. Việc sử dụng ListModel sẽ giúp đồng bộ hóa các thao tác trên AnimatedList. Ngoài ra, list model này còn dùng một GlobalKey để gọi các <i>phương thức</i> (methods) insertItem (thêm mục) và removeItem (xóa mục) được định nghĩa bởi AnimatedListState.

Khám phá ứng dụng sau đây bằng cách tạo một project mới với `flutter create` và thay thế nội dụng trong file `lib/main.dart` thành mã nguồn dưới đây.

```dart
// Copyright 2017 The Chromium Authors. All rights reserved.
// Việc sử dụng mã nguồn này được quản lý bởi giấy phép BSD.
// Xem thêm tại tập tin LICENSE.

import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

class AnimatedListSample extends StatefulWidget {
  @override
  _AnimatedListSampleState createState() => _AnimatedListSampleState();
}

class _AnimatedListSampleState extends State<AnimatedListSample> {
  final GlobalKey<AnimatedListState> _listKey = GlobalKey<AnimatedListState>();
  ListModel<int> _list;
  int _selectedItem;
  int _nextItem; // Mục tiếp theo sẽ được thêm vào khi người dùng nhấn '+'.

  @override
  void initState() {
    super.initState();
    _list = ListModel<int>(
      listKey: _listKey,
      initialItems: <int>[0, 1, 2],
      removedItemBuilder: _buildRemovedItem,
    );
    _nextItem = 3;
  }

  // Build các mục chưa bị xóa.
  Widget _buildItem(
      BuildContext context, int index, Animation<double> animation) {
    return CardItem(
      animation: animation,
      item: _list[index],
      selected: _selectedItem == _list[index],
      onTap: () {
        setState(() {
          _selectedItem = _selectedItem == _list[index] ? null : _list[index];
        });
      },
    );
  }

  // Build các mục đã bị xóa khỏi danh sách. Ta cần phương thức này vì 
  // những mục bị xóa vẫn sẽ hiển thị cho tới khi animation đã hoàn tất
  // (mặc dù trong ListModel thì mục này đã biến mất). Widget mà phương 
  // thức này trả về sẽ làm tham số [AnimatedListRemovedItemBuilder] 
  // trong phương thức [AnimatedListState.removeItem].
  Widget _buildRemovedItem(
      int item, BuildContext context, Animation<double> animation) {
    return CardItem(
      animation: animation,
      item: item,
      selected: false,
      // Không có detector cho thao thác nhấn vì mục bị xóa không cần tương tác
    );
  }

  // Thêm mục tiếp theo vào list model.
  void _insert() {
    final int index =
        _selectedItem == null ? _list.length : _list.indexOf(_selectedItem);
    _list.insert(index, _nextItem++);
  }

  // Xóa mục đang chọn khỏi list model.
  void _remove() {
    if (_selectedItem != null) {
      _list.removeAt(_list.indexOf(_selectedItem));
      setState(() {
        _selectedItem = null;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('AnimatedList'),
          actions: <Widget>[
            IconButton(
              icon: const Icon(Icons.add_circle),
              onPressed: _insert,
              tooltip: 'insert a new item',
            ),
            IconButton(
              icon: const Icon(Icons.remove_circle),
              onPressed: _remove,
              tooltip: 'remove the selected item',
            ),
          ],
        ),
        body: Padding(
          padding: const EdgeInsets.all(16.0),
          child: AnimatedList(
            key: _listKey,
            initialItemCount: _list.length,
            itemBuilder: _buildItem,
          ),
        ),
      ),
    );
  }
}

/// Tạo ra một Dart List để đồng bộ với AnimatedList.
///
/// Hai phương thức [insert] và [removeAt] sẽ thêm hoặc xóa mục trong cả 
/// list nội bộ (internal list) - Dart List và animated list, thuộc về listKey.
///
/// Lớp này chỉ thể hiện một phần của Dart List API cần dùng trong ứng dụng mẫu.
/// Có thể thêm nhiều phương thức liên quan đến list khác, nhưng một khi đã làm
/// thay đối list thì cần phải có sự thay đổi tương ứng ở animated list đồng
/// nghĩa với việc gọi [AnimatedListState.insertItem] hoặc [AnimatedList.removeItem].
class ListModel<E> {
  ListModel({
    @required this.listKey,
    @required this.removedItemBuilder,
    Iterable<E> initialItems,
  })  : assert(listKey != null),
        assert(removedItemBuilder != null),
        _items = List<E>.from(initialItems ?? <E>[]);

  final GlobalKey<AnimatedListState> listKey;
  final dynamic removedItemBuilder;
  final List<E> _items;

  AnimatedListState get _animatedList => listKey.currentState;

  void insert(int index, E item) {
    _items.insert(index, item);
    _animatedList.insertItem(index);
  }

  E removeAt(int index) {
    final E removedItem = _items.removeAt(index);
    if (removedItem != null) {
      _animatedList.removeItem(index,
          (BuildContext context, Animation<double> animation) {
        return removedItemBuilder(removedItem, context, animation);
      });
    }
    return removedItem;
  }

  int get length => _items.length;

  E operator [](int index) => _items[index];

  int indexOf(E item) => _items.indexOf(item);
}

/// Mỗi thẻ hiển thị một số nguyên tương ứng dưới dạng 'item N'
/// và màu được dựa trên giá trị trên thẻ. Màu chữ sẽ đổi sang 
/// xanh lá cây nếu thẻ đang được chọn. Chiều cao của thẻ dựa 
/// trên tham số animation, dao động từ 0 đến 128 khi giá trị 
/// animation dao động từ 0.0 đến 1.0.
class CardItem extends StatelessWidget {
  const CardItem(
      {Key key,
      @required this.animation,
      this.onTap,
      @required this.item,
      this.selected: false})
      : assert(animation != null),
        assert(item != null && item >= 0),
        assert(selected != null),
        super(key: key);

  final Animation<double> animation;
  final VoidCallback onTap;
  final int item;
  final bool selected;

  @override
  Widget build(BuildContext context) {
    TextStyle textStyle = Theme.of(context).textTheme.display1;
    if (selected)
      textStyle = textStyle.copyWith(color: Colors.lightGreenAccent[400]);
    return Padding(
      padding: const EdgeInsets.all(2.0),
      child: SizeTransition(
        axis: Axis.vertical,
        sizeFactor: animation,
        child: GestureDetector(
          behavior: HitTestBehavior.opaque,
          onTap: onTap,
          child: SizedBox(
            height: 128.0,
            child: Card(
              color: Colors.primaries[item % Colors.primaries.length],
              child: Center(
                child: Text('Item $item', style: textStyle),
              ),
            ),
          ),
        ),
      ),
    );
  }
}

void main() {
  runApp(AnimatedListSample());
}
```

## Xem thêm

* [Components-Lists: Controls]({{site.material}}/guidelines/components/lists-controls.html#)
  trong trang về [Material Design]({{site.material}}).
* Mã nguồn trong
  [examples/catalog/lib/animated_list.dart]({{site.repo.flutter}}/tree/{{site.branch}}/examples/catalog/lib/animated_list.dart).
