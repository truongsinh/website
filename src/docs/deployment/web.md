---
title: Xây dựng và phát hành một ứng dụng web
description: Cách chuẩn bị và phát hành một ứng dụng web.
short-title: Web
---

Trong một chu trình phát triển thông thường,
bạn có thể chạy thử ứng dụng bằng `flutter run -d chrome`
(ví dụ) ở cửa sổ command line.
Lệnh này sẽ xây một bản _gỡ lỗi_ (_debug_) cho ứng dụng của bạn.

Phần này sẽ giúp bạn chuẩn bị một bản _phát hành_ 
cho ứng dụng của bạn và bao gồm những chủ đề hướng dẫn sau đây:

* [Kĩ thuật rút gọn dung lượng](#minification)
* [Xây dựng ứng dụng để phát hành](#building-the-app-for-release)
* [Triển khai ứng dụng lên web](#deploying-to-the-web)

## Kĩ thuật rút gọn dung lượng

Kĩ thuật rút gọn dung lượng sẽ được xử lí cho bạn
khi bạn tạo một bản xây dựng để phát hành.

Bản dựng gỡ lỗi (_debug build_) sẽ không được rút gọn dung lượng và
không được áp dụng kĩ thuật _tree shaking_.

Bản dựng cá nhân (_profile build_) không được rút gọn dung lượng
được áp dụng kĩ thuật _tree shaking_.

Bản dựng phát hành (_release build_) vừa được rút gọn dung lượng
vừa được áp dụng kĩ thuật _tree shaking_.

## Xây dựng ứng dụng để phát hành

Để xây dựng ứng dụng cho việc triển khai ta dùng
dòng lệnh `flutter build web`.
Lệnh này tạo ra ứng dụng, bao gồm các tài nguyên tĩnh (_assets_),
và đặt các tệp vào thư mục `/build/web`
trong dự án của bạn.

Bản dựng phát hành (_release build_) của một ứng dụng đơn giản 
có cấu trúc sau đây:

```none
/build/web
  assets
    AssetManifest.json
    FontManifest.json
    LICENSE
    fonts
      MaterialIcons-Regular.ttf
      <other font files>
    <image files>
  index.html
  main.dart.js
  main.dart.js.map
```

Chạy máy chủ (_server_) của trang web (ví dụ,
`python -m SimpleHTTPServer 8000`,
hoặc sử dụng package [dhttpd][]),
và mở thư mục /build/web. Mở
`localhost:8000` trong trình duyệt của bạn
(ví dụ áp dụng với SimpleHTTPServer của python)
để xem bản phát hành ứng dụng của bạn.

## Nhúng ứng dụng Flutter vào trang HTML

Bạn có thể nhúng một ứng dụng web Flutter,
như cách bạn nhúng các nội dung khác,
trong thẻ [`iframe`][] của tệp HTML.
Trong ví dụ sau đây, thay đổi "URL"
với vị trí trang HTML của bạn:

```html
<iframe src="URL"></iframe>
```

## Triển khai lên trang web

Khi bạn đã sẵn sàng triển khai ứng dụng của bạn,
đăng tải gói phát hành
lên Firebase, đám mây, hoặc dịch vụ tương tự.
Dưới đây là một số ví dụ, nhưng vẫn sẽ có
nhiều lựa chọn khác:

* [Firebase Hosting][]
* [GitHub Pages][]
* [Google Cloud Hosting][]

Trong tương lai, chúng tôi dự định sẽ tạo ra các tệp cấu hình PWA
để hỗ trợ Progressive Web Apps.

[dhttpd]: {{site.pub}}/packages/dhttpd
[Firebase Hosting]: https://firebase.google.com/docs/hosting
[GitHub Pages]: https://pages.github.com/
[Google Cloud Hosting]: https://cloud.google.com/solutions/smb/web-hosting/
[`iframe`]: https://html.com/tags/iframe/

