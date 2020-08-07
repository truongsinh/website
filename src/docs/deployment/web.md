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

Minification is handled for you when you
create a release build.

A debug build of a web app is not minified and
tree shaking has not been performed.

A profile build is not minified and tree shaking
has been performed.

A release build is both minified and tree shaking
has been performed.

## Building the app for release

Build the app for deployment using the
`flutter build web` command.
This generates the app, including the assets,
and places the files into the `/build/web`
directory of the project.

The release build of a simple app has the
following structure:

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

Launch a web server (for example,
`python -m SimpleHTTPServer 8000`,
or by using the [dhttpd][] package),
and open the /build/web directory. Navigate to
`localhost:8000` in your browser
(given the python SimpleHTTPServer example)
to view the release version of your app.

## Embedding a Flutter app into an HTML page

You can embed a Flutter web app,
as you would embed other content,
in an [`iframe`][] tag of an HTML file.
In the following example, replace "URL"
with the location of your HTML page:

```html
<iframe src="URL"></iframe>
```

## Deploying to the web

When you are ready to deploy your app,
upload the release bundle
to Firebase, the cloud, or a similar service.
Here are a few possibilities, but there are
many others:

* [Firebase Hosting][]
* [GitHub Pages][]
* [Google Cloud Hosting][]

In future, we plan to generate PWA configuration files
to support Progressive Web Apps.

[dhttpd]: {{site.pub}}/packages/dhttpd
[Firebase Hosting]: https://firebase.google.com/docs/hosting
[GitHub Pages]: https://pages.github.com/
[Google Cloud Hosting]: https://cloud.google.com/solutions/smb/web-hosting/
[`iframe`]: https://html.com/tags/iframe/

