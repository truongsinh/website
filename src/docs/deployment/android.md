---
title: Xây dựng và phát hành một ứng dụng Android
description: Cách để chuẩn bị và phát hành một ứng dụng Android lên Play store
short-title: Android
---

Thông thường khi phát triển,
bạn có thể chạy thử ứng dụng của mình bằng dòng lệnh `flutter run` ở cửa sổ *command line*,
hoặc bằng lựa chọn **Run** và **Debug**
ở IDE của mình. Theo mặc định,
Flutter sẽ xây dựng ra (*build*) một phiên bản _debug_ (*gỡ lỗi*) cho ứng dụng của bạn.

Khi bạn đã sẵn sàng chuẩn bị một phiên bản _phát hành_ cho ứng dụng của mình,
ví dụ như [phát hành lên Google Play Store][play],
trang web này có thể cho bạn thêm thông tin. Trước khi phát hành,
có thể bạn sẽ muốn chỉnh sửa một số chi tiết để ứng dụng hoàn thiện hơn.
Dưới đây là một số hướng dẫn:

* [Thêm icon biểu tượng cho ứng dụng](#adding-a-launcher-icon)
* [Đăng kí ứng dụng](#signing-the-app)
* [Rút gọn code với R8](#shrinking-your-code-with-r8)
* [Kiểm tra bảng kê khai ứng dụng (app manifest)](#reviewing-the-app-manifest)
* [Xem lại cấu hình xây dựng (build) (build configuration)](#reviewing-the-build-configuration)
* [Xây dựng (build) ứng dụng để sẵn sàng phát hành](#building-the-app-for-release)
* [Phát hành ứng dụng lên Google Play Store](#publishing-to-the-google-play-store)
* [Cập nhật đánh số phiên bản của ứng dụng](#updating-the-apps-version-number)
* [Các câu hỏi thường gặp khi phát hành ứng dụng Android](#android-release-faq)

## Thêm icon biểu tượng cho ứng dụng

Khi một ứng dụng Flutter mới được tạo ra, nó có một icon biểu tượng mặc định.
Để thay đổi biểu tượng này, bạn có thể xem qua package sau đây:
[flutter_launcher_icons][].

Một phương án khác: Bạn có thể làm việc này thủ công theo các bước dưới đây:

1. Tham khảo hướng dẫn thiết kế icon biểu tượng [Material Design product
   icons][launchericons].

1. Trong danh mục `<app dir>/android/app/src/main/res/`,
   đặt các tệp (files) hình ảnh vào đây bằng cách
   dùng [configuration qualifiers][]. 
   Những thư mục `mipmap-` sẽ cho bạn thông tin về
   quy ước đặt tên sao cho đúng.

1. Ở tệp `AndroidManifest.xml`, thay đổi thuộc tính
   `android:icon` của thẻ [`application`][applicationtag]
   để trỏ đến icons biểu tưởng ở bước trước (Ví dụ:
   `<application android:icon="@mipmap/ic_launcher" ...`).

1. Để xác nhận bạn đã thay thành công icon biểu tượng,
   chạy ứng dụng của bạn và kiểm tra biểu tượng icon trong Launcher (Trình khởi chạy).

## Đăng kí ứng dụng

Để phát hành ứng dụng lên Play Store, bạn cần đăng kí cho ứng dụng của bạn
một chữ kí/con dấu điện tử. Dưới đây là hướng dẫn đăng kí.

### Tạo một keystore (kho lưu trữ các chứng chỉ bảo mật)

Nếu bạn đã có keystore, hãy xem sang bước tiếp theo.
Nếu chưa, chạy dòng lệnh dưới đây trong cửa sổ command line để tạo:

Ở Mac/Linux, dùng dòng lệnh dưới đây:

```terminal
keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
```

Ở Windows, dùng dòng lệnh dưới đây:

```terminal
keytool -genkey -v -keystore c:/Users/USER_NAME/key.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias key
```

Dòng lệnh sẽ lưu trữ tệp `key.jks` ở danh mục chính
của bạn. Nếu bạn muốn lưu nó ở chỗ khác, thay thế
đối số truyền vào *tham số* `-keystore`.
**Tuy nhiên, hãy giữ bảo mật tệp `keystore`; 
không đưa nó vào hệ thống quản lí công cộng!**

{{site.alert.note}}
* Lệnh `keytool` có thể không ở trong *path* của bạn&mdash;nó là
  một phần của Java, được cài đặt kèm trong
  Android Studio. Để tìm ra đường dẫn cụ thể (*path*),
  hãy chạy lệnh `flutter doctor -v`, đường dẫn sẽ nằm ở sau cụm chữ
  'Java binary at:'. Sau đó thay thế `java`
  (ở cuối đường dẫn đó) bằng `keytool`.
  Nếu đường dẫn của bạn có chứa những tên có dấu cách, 
  ví dụ như `Program Files`, hãy sử dụng 
  ký hiệu phù hợp với nền tảng của bạn.
  Ví dụ: ở Mac/Linux là `Program\ Files`, 
  và ở Windows là `"Program Files"`.

* Thẻ tag `-storetype JKS` chỉ bắt buộc cho Java 9 
  hoặc các phiên bản sau đó. Vi khi Java 9 phát hành,
  loại keystore mặc định là PKS12.
{{site.alert.end}}

### Tham chiếu keystore từ ứng dụng

Tạo một tệp tên `<app dir>/android/key.properties`
chứa tham chiếu tới keystore của bạn:

```
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=key
storeFile=<location of the key store file, such as /Users/<user name>/key.jks>
```

{{site.alert.warning}}
  Hãy giữ bảo mật tệp `key.properties`;
  không đưa nó vào hệ thống quản lí công cộng.
{{site.alert.end}}

### Cấu hình đăng kí ở gradle

Cấu hình đăng kí cho ứng dụng của bạn bằng cách chỉnh sửa tệp
`<app dir>/android/app/build.gradle`.

<ol markdown="1">
<li markdown="1"> Thêm vào trước block `android`...:

```
   android {
      ...
   }
```

   ...thông tin keystore từ tệp properties (*thuộc tính*)

```
   def keystoreProperties = new Properties()
   def keystorePropertiesFile = rootProject.file('key.properties')
   if (keystorePropertiesFile.exists()) {
       keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
   }

   android {
         ...
   }
```
   
   Tải tệp `key.properties` vào object `keystoreProperties`.

</li>

<li markdown="1"> Thêm vào trước block `buildTypes`...:

```
   buildTypes {
       release {
           // TODO: Add your own signing config for the release build.
           // Signing with the debug keys for now,
           // so `flutter run --release` works.
           signingConfig signingConfigs.debug
       }
   }
```

   ...thông tin cấu hình đăng kí (*signingConfigs*):

```
   signingConfigs {
       release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
           storePassword keystoreProperties['storePassword']
       }
   }
   buildTypes {
       release {
           signingConfig signingConfigs.release
       }
   }
```

   Cấu hình block `signingConfigs` ở tệp `build.gradle` trong module.

</li>
</ol>

Những bản phát hành ứng dụng của bạn từ giờ sẽ được đăng kí tự động.

{{site.alert.note}}
  Bạn có thể cần chạy lệnh `flutter clean` sau khi chỉnh sửa tệp gradle.
  Lệnh này hạn chế cache từ những bản _xây dựng_ (*build*) cũ ảnh hưởng 
  tới quá trình đăng kí.
{{site.alert.end}}

Để biết thêm thông tin về việc đăng ký ứng dụng, hãy xem
[Sign your app][] trên developer.android.com.

## Rút gọn code với R8

[R8][] là chương trình rút gọn code của Google, 
nó mặc định được bật khi xây dựng bản phát hành APK và AAB.
Để tắt R8, truyền flag (thẻ cờ) `--no-shrink`
vào `flutter build apk` hoặc `flutter build appbundle`.

{{site.alert.note}}
  Kĩ thuật làm rối code (*Obfuscation*) và rút gọn dung lượng code (*minification*) 
  có thể kéo dài đáng kể thời gian biên dịch ứng dụng Android.
{{site.alert.end}}

## Kiểm tra bảng kê khai ứng dụng (app manifest)

Kiểm tra tệp mặc định [App Manifest][manifest],
`AndroidManifest.xml`,
nằm trong thư mục `<app dir>/android/app/src/main` và xác nhận xem
các trường giá trị đã chính xác chưa, đặc biệt là các trường sau đây:

`application`
: Sửa `android:label` trong thẻ
  [`application`][applicationtag] để thay đổi
  tên cuối cùng của ứng dụng.

`uses-permission`
: Để `android.permission.INTERNET`
  [permission][permissiontag] nếu ứng dụng của bạn cần 
  truy cập Internet. Mẫu (_template_) tiêu chuẩn không bao gồm thẻ này vẫn nhưng cho phép
  truy cập Internet trong quá trình phát triển để giao tiếp giữa
  các công cụ Flutter và ứng dụng đang chạy.

## Kiếm tra cấu hình xây dựng (build configuration)

Xem lại tệp mặc định [Gradle build file][gradlebuild],
`build.gradle`, ở thư mục `<app dir>/android/app` và
kiểm tra xem các trường giá trị đã đúng chưa, đặc biệt là
các trường giá trị dưới đây trong block `defaultConfig`:

`applicationId`
: Đặt một Id độc nhất cho ứng dụng (Application Id) [appid]

`versionCode` & `versionName`
: Đánh số phiên bản cho ứng dụng
  và đánh số phiên bản hiển thị bằng một chuỗi, bằng cách cài đặt
  thuộc tính `version` ở tệp pubspec.yaml. Bạn hãy tham khảo
  hướng dẫn ở [versions documentation][versions].

`minSdkVersion` & `targetSdkVersion`
: Đặt cấp độ API tối thiểu,
  và cấp độ API mà ứng dụng được thiết kế để chạy.
  Tham khảo phần _cấp độ API_ (API level) trong [versions documentation][versions]
  for details.

## Xây dựng ứng dụng để phát hành

2 định dạng ứng dụng có thể phát hành trên Play Store:

* Gói ứng dụng (_app bundle_) (được ưa thích hơn)
* APK

{{site.alert.note}}
  Google Play Store ưu tiên định dạng gói ứng dụng hơn.
  Để biết thêm thông tin, xem [Android App Bundle][bundle] và
  [About Android App Bundles][bundle2].
{{site.alert.end}}

{{site.alert.warning}}

  Gần đây, đội ngũ Flutter đã nhận [một vài báo cáo][crash-issue]
  từ các nhà phát triển rằng họ gặp một vài sự cố sập ứng dụng
  khi phát triển trên Android 6.0. Nếu bạn cũng nhắm đến 
  phát triển ứng dụng trên Android 6.0, hãy làm theo các bước sau:

  * Nếu bạn xây dựng Gói ứng dụng (_app bundle_):
    Sửa `android/gradle.properties` và thêm một flag:
    `android.bundle.enableUncompressedNativeLibs=false`.

  * Nếu bạn xây dựng APK:
    Đảm bảo rằng tệp `android/app/src/AndroidManifest.xml`
    không để `android:extractNativeLibs=false`
    ở thẻ `<application>`.

  Để biết thêm thông tin, hãy xem [public issue][crash-issue].
{{site.alert.end}}

### Xây dựng gói ứng dụng (_app bundle_)

Phần này mô tả cách xây dựng một gói ứng dụng để phát hành.
Nếu bạn đã hoàn thành các bước đăng ký,
gói ứng dụng sẽ được ký kết.

Lúc này, bạn có thể xem xét kĩ thuật làm rối mã nguồn Dart (obfuscation) 
([obfuscating your Dart code][])
để mã nguồn khó bị đảo ngược hơn (_reverse engineer_). Để sử dụng 
kĩ thuật làm rối mã nguồn bạn cần thêm một vài thẻ flags 
vào câu lệnh để xây dựng (build) ứng dụng,
cũng như lưu trữ một vài tệp bổ sung để gỡ rối (de-obfuscate) 
dấu vết ngăn xếp (stack traces).

Ở cửa sổ command line:

1. Gõ `cd <app dir>`<br>
   (Thay thế `<app dir>` bằng tên thư mục ứng dụng.)
1. Chạy `flutter build appbundle`<br>
   (Chạy `flutter build` mặc định sẽ xây dựng ra một bản phát hành.)

Gói ứng dụng của bạn sẽ được tạo ra ở
`<app dir>/build/app/outputs/bundle/release/app.aab`.

Theo mặc định, gói ứng dụng chứa mã nguồn Dart của bạn và Flutter
thời gian chạy được biên dịch cho [armeabi-v7a][] (ARM 32-bit), [arm64-v8a][]
(ARM 64-bit), và [x86-64][] (x86 64-bit).

### Chạy thử gói ứng dụng (app bundle)

Gói ứng dụng có thể được chạy thử theo nhiều cách&mdash; phần này
sẽ miêu tả 2 cách

#### Ngoại tuyến bằng công cụ gói (bundle tool)

1. Nếu chưa tải, hãy tải `bundletool` từ
   [GitHub repository][].
1. [Tạo ra một tập APKs][apk-set] từ gói ứng dụng của bạn.
1. [Triển khai APKs][apk-deploy] đến những ứng dụng được kết nối.

#### Trực tuyến bằng Google Play

1. Đăng tải gói ứng dụng của bạn lên Google Play để chạy thử.
   Bạn có thể sử dụng track thử nghiệm nội bộ,
   hoặc các kênh alpha/beta để thử nghiệm gói trước khi
   phát hành.
2. Làm theo [các bước để đăng tải gói của bạn][upload-bundle]
   lên Play Store.

### Build an APK

Mặc dù các gối ứng dụng thường được ưu tiên hơn APK, vẫn có những cửa hàng lưu trữ ứng dụng
chưa hỗ trợ gói ứng dụng (app bundle). Trong trường hợp này, hãy xây dựng một bản APK phát hành
cho từng đối tượng ABI (Application Binary Interface) .

If you completed the signing steps,
the APK will be signed.
At this point, you might consider [obfuscating your Dart code][]
to make it more difficult to reverse engineer. Obfuscating
your code involves adding a couple flags to your build command.

From the command line:

1. Enter `cd <app dir>`<br>
   (Replace `<app dir>` with your application's directory.)
1. Run `flutter build apk --split-per-abi`<br>
   (The `flutter build` command defaults to `--release`.)

This command results in three APK files:

* `<app dir>/build/app/outputs/apk/release/app-armeabi-v7a-release.apk`
* `<app dir>/build/app/outputs/apk/release/app-arm64-v8a-release.apk`
* `<app dir>/build/app/outputs/apk/release/app-x86_64-release.apk`

Removing the `--split-per-abi` flag results in a fat APK that contains
your code compiled for _all_ the target ABIs. Such APKs are larger in
size than their split counterparts, causing the user to download
native binaries that are not applicable to their device's architecture.

### Install an APK on a device

Follow these steps to install the APK on a connected Android device.

From the command line:

1. Connect your Android device to your computer with a USB cable.
1. Enter `cd <app dir>` where `<app dir>` is your application directory.
1. Run `flutter install`.

## Publishing to the Google Play Store

For detailed instructions on publishing your app to the Google Play Store,
see the [Google Play launch][play] documentation.

## Updating the app's version number

The default version number of the app is `1.0.0`.
To update it, navigate to the `pubspec.yaml` file
and update the following line:

`version: 1.0.0+1`

The version number is three numbers separated by dots,
such as `1.0.0` in the example above, followed by an optional
build number such as `1` in the example above, separated by a `+`.

Both the version and the build number may be overridden in Flutter's
build by specifying `--build-name` and `--build-number`, respectively.

In Android, `build-name` is used as `versionName` while
`build-number` used as `versionCode`. Để biết thêm thông tin,
see [Version your app][] in the Android documentation.

## Android release FAQ

Here are some commonly asked questions about deployment for
Android apps.

### When should I build app bundles versus APKs?

The Google Play Store recommends that you deploy app bundles
over APKs because they allow a more efficient delivery of the
application to your users. However, if you’re distributing
your application by means other than the Play Store,
an APK may be your only option.

### What is a fat APK?

A [fat APK][] is a single APK that contains binaries for multiple
ABIs embedded within it. This has the benefit that the single APK
runs on multiple architectures and thus has wider compatibility,
but it has the drawback that its file size is much larger,
causing users to download and store more bytes when installing
your application. When building APKs instead of app bundles,
it is strongly recommended to build split APKs,
as described in [build an APK](#build-an-apk) using the
`--split-per-abi` flag.

### What are the supported target architectures?

When building your application in release mode,
Flutter apps can be compiled for [armeabi-v7a][] (ARM 32-bit),
[arm64-v8a][] (ARM 64-bit), and [x86-64][] (x86 64-bit).
Flutter does not currently support building for x86 Android
(See [Issue 9253][]).

### How do I sign the app bundle created by `flutter build appbundle`?

See [Signing the app](#signing-the-app).

### How do I build a release from within Android Studio?

In Android Studio, open the existing `android/`
folder under your app’s folder. Then,
select **build.gradle (Module: app)** in the project panel:

{% asset 'deployment/android/gradle-script-menu.png' alt='screenshot of gradle build script menu' %}

Next, select the build variant. Click **Build > Select Build Variant**
in the main menu. Select any of the variants in the **Build Variants**
panel (debug is the default):

{% asset 'deployment/android/build-variant-menu.png' alt='screenshot of build variant menu' %}

The resulting app bundle or APK files are located in
`build/app/outputs` within your app's folder.

{% comment %}
### Are there any special considerations with add-to-app?
{% endcomment %}


[apk-deploy]: {{site.android-dev}}/studio/command-line/bundletool#deploy_with_bundletool
[apk-set]: {{site.android-dev}}/studio/command-line/bundletool#generate_apks
[appid]: {{site.android-dev}}/studio/build/application-id
[applicationtag]: {{site.android-dev}}/guide/topics/manifest/application-element
[arm64-v8a]: {{site.android-dev}}/ndk/guides/abis#arm64-v8a
[armeabi-v7a]: {{site.android-dev}}/ndk/guides/abis#v7a
[bundle]: {{site.android-dev}}/platform/technology/app-bundle
[bundle2]: {{site.android-dev}}/guide/app-bundle
[configuration qualifiers]: {{site.android-dev}}/guide/topics/resources/providing-resources#AlternativeResources
[crash-issue]: https://issuetracker.google.com/issues/147096055
[fat APK]: https://en.wikipedia.org/wiki/Fat_binary
[Flutter wiki]: {{site.github}}/flutter/flutter/wiki
[flutter_launcher_icons]: {{site.pub}}/packages/flutter_launcher_icons
[GitHub repository]: {{site.github}}/google/bundletool/releases/latest
[gradlebuild]: {{site.android-dev}}/studio/build/#module-level
[Issue 9253]: {{site.github}}/flutter/flutter/issues/9253
[Issue 18494]: {{site.github}}/flutter/flutter/issues/18494
[launchericons]: {{site.material}}/design/iconography/
[manifest]: {{site.android-dev}}/guide/topics/manifest/manifest-intro
[manifesttag]: {{site.android-dev}}/guide/topics/manifest/manifest-element
[obfuscating your Dart code]: /docs/deployment/obfuscate
[permissiontag]: {{site.android-dev}}/guide/topics/manifest/uses-permission-element
[play]: {{site.android-dev}}/distribute/googleplay/start
[R8]: {{site.android-dev}}/studio/build/shrink-code
[Sign your app]: https://developer.android.com/studio/publish/app-signing.html#generate-key
[upload-bundle]: {{site.android-dev}}/studio/publish/upload-bundle
[Version your app]: {{site.android-dev}}/studio/publish/versioning
[versions]: {{site.android-dev}}/studio/publish/versioning
[x86-64]: {{site.android-dev}}/ndk/guides/abis#86-64
