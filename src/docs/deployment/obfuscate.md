---
title: Kĩ thuật làm rối (obfuscation) Dart code 
description: Làm thế nào để xóa tên function và class trong mã Dart (Dart binary) của bạn.
---

*Trong bài này mình sẽ giữ nguyên từ chuyên môn: obfuscation để không làm thay đổi nội dung gốc.*

[Code obfuscation][] là quá trình điều chỉnh mã nhị phân của ứng dụng
(app's binary) để làm mã khó hiểu với con người.
Obfuscation giấu tên các function và class ở trong 
mã code Dart đã được biên dịch, để những kẻ tấn công khó có thể
dịch ngược ứng dụng chính chủ của bạn.

Danh sách dưới đây miêu tả những nền tảng nào
hỗ trợ quá trình obfuscation được miêu tả trong 
trang này

**Android**/**iOS**
: Được hỗ trợ từ Flutter 1.16.2.  Để obfuscate
  được xây từ phiên bản trước đó của Flutter,
  hãy sử dụng [hướng dẫn obfuscation][obfuscation instructions] trong Flutter wiki.

**macOS**
: macOS ([in alpha][] as of Flutter 1.13),
  hỗ trợ obfuscation từ Flutter 1.16.2.

**Linux**/**Windows**
: Chưa được hỗ trợ

**web**
: Obfuscation chưa được hỗ trợ cho ứng dụng web,
  nhưng ứng dụng web có thể được [minified][] (rút gọn code),
  cũng khá tương đương vơi obfuscation. Khi bạn xây
  bản phát hành cho một ứng dụng web Flutter, nó
  sẽ được tư động rút gọn. Để biết thêm thông tin,
  hãy xem [Xây dựng và phát hành một ứng dụng web][Build and release a web app].

**Obfuscation ở Flutter code, khi được hỗ trợ, sẽ chỉ hoạt động
với [bản phát hành][release build].**

## Obfuscating ứng dụng của bạn

Để obfuscate ứng dụng, xây một bản build phát hành
dùng thẻ flag `--obfuscate`,
kết hợp với `--split-debug-info`.
Thẻ flag `--split-debug-info` chỉ định thư mục
nơi mà Flutter xuất ra tệp gỡ lỗi (debug file).
Lệnh này tạo ra một symbol map
Các mục `apk`, `appbundle`, `ios`, và `ios-framework`
hiện tại đều được hỗ trợ. (`macos` được
hỗ trợ ở các kênh master và dev)
Ví dụ:

```terminal
flutter build apk --obfuscate --split-debug-info=/<project-name>/<directory>
```

Một khi bạn đã obfuscate mã (binary) của bạn, lưu
lưu lại symbols file đó. Banj sẽ cần file này nếu sau đó
bạn muốn giải mã lại (de-obfuscate) một stack trace.

**Lưu ý rằng thẻ flag `--split-debug-info` cũng có thể
được sử dụng bởi chính nó. Thực ra, nó có thể giảm đáng kể
kích cỡ của mã code. Để biết thêm thông tin về
kích cỡ ứng dụng, xem [Đo kích cỡ ứng dụng của bạn][Measuring your app's size].**

Để biết thêm thông tin về các thẻ flags này, chạy lệnh
help cho đối tượng target cụ thể của bạn, ví dụ:

```terminal
flutter build apk -h
```

Nếu thẻ flags nêu trên này không được hiện ra ở output,
chạy `flutter --version` để kiểm tra phiên bản Flutter.

## Đọc một obfuscated stack trace

Để gỡ lỗi một stack trace tạo ra bởi ứng dụng đã được obfuscated,
làm theo các bước sau để làm nó có thể đọc được bởi con người:

1. Tìm symbols file khớp với stack trace đó.
   Ví dụ, lỗi sập ứng dụng ở thiết bị Android arm64
   sẽ cần `app.android-arm64.symbols`.

1. Cung cấp cả stack trace (lưu trong một tệp)
   và symbols file tới lệnh `flutter symbolize`.
   Ví dụ:

```terminal
flutter symbolize -i <stack trace file> -d /out/android/app.android-arm64.symbols 
```

   Đê rbieets thêm thông tin về lệnh `symbolize`,
   chạy `flutter symbolize -h`.

## Cảnh báo trước

Hãy chú ý những điều sau khi code một ứng dụng
mà sẽ được obfuscated mã binary.

* Nếu code có phụ thuộc vào việc phải khớp tên class, function,
  hoặc tên thư viện sẽ không thể obfuscated.
  Ví dụ, lệnh gọi hàm dưới đây tới `expect()` sẽ không hoạt động
  trong mã (binary) được obfuscated:

<!-- skip -->
```dart
expect(foo.runtimeType.toString(), equals('Foo'))
```


[Build and release a web app]: /docs/deployment/web
[Code obfuscation]: https://en.wikipedia.org/wiki/Obfuscation_(software)
[in alpha]: /desktop
[Measuring your app's size]: /docs/perf/app-size
[minified]: https://en.wikipedia.org/wiki/Minification_(programming)
[obfuscation instructions]: {{site.github}}/flutter/flutter/wiki/Obfuscating-Dart-Code
[release build]: /docs/testing/build-modes
