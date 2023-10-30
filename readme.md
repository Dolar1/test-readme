<details> 
<summary>Flutter diagnostics</summary>

```sh
[✓] Flutter (Channel stable, 3.13.8, on macOS 13.6 22G120 darwin-arm64, locale en-IN)
    • Flutter version 3.13.8 on channel stable at /Users/saurabhsingh/Downloads/development/flutter
    • Upstream repository https://github.com/flutter/flutter.git
    • Framework revision 6c4930c4ac (12 days ago), 2023-10-18 10:57:55 -0500
    • Engine revision 767d8c75e8
    • Dart version 3.1.4
    • DevTools version 2.25.0

[✓] Android toolchain - develop for Android devices (Android SDK version 33.0.0)
    • Android SDK at /Users/saurabhsingh/Library/Android/sdk
    • Platform android-33, build-tools 33.0.0
    • ANDROID_HOME = /Users/saurabhsingh/Library/Android/sdk
    • Java binary at: /Applications/Android Studio.app/Contents/jbr/Contents/Home/bin/java
    • Java version OpenJDK Runtime Environment (build 17.0.6+0-17.0.6b829.9-10027231)
    • All Android licenses accepted.

[✓] Xcode - develop for iOS and macOS (Xcode 14.0.1)
    • Xcode at /Applications/Xcode.app/Contents/Developer
    • Build 14A400
    • CocoaPods version 1.13.0

[✓] Chrome - develop for the web
    • Chrome at /Applications/Google Chrome.app/Contents/MacOS/Google Chrome

[✓] Android Studio (version 2022.3)
    • Android Studio at /Applications/Android Studio.app/Contents
    • Flutter plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/9212-flutter
    • Dart plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/6351-dart
    • Java version OpenJDK Runtime Environment (build 17.0.6+0-17.0.6b829.9-10027231)

[✓] VS Code (version 1.83.1)
    • VS Code at /Applications/Visual Studio Code.app/Contents
    • Flutter extension version 3.74.0

[✓] Connected device (3 available)
    • sdk gphone64 arm64 (mobile) • emulator-5554 • android-arm64  • Android 13 (API 33) (emulator)
    • macOS (desktop)             • macos         • darwin-arm64   • macOS 13.6 22G120 darwin-arm64
    • Chrome (web)                • chrome        • web-javascript • Google Chrome 118.0.5993.117

[✓] Network resources
    • All expected network resources are available.
```
</details>

1. We have made two dynamic components in our app
  - dynamicGallery
  - dynamicGallery2

2. Follow the docs from [Deferred components](https://docs.flutter.dev/perf/deferred-components).

3. Was able to build and test **apk** locally via local testing commands given in doc.
 - <details> 
    <summary>Local Testing Commands</summary>
    
    ``` sh
     java -jar bundletool.jar build-apks --bundle=<your_app_project_dir>/build/app/outputs/bundle/release/app-release.aab --output=<your_temp_dir>/app.apks --local-testing
     java -jar bundletool.jar install-apks --apks=<your_temp_dir>/app.apks
    ```

4. When we put dynamic delivery project folder inside android in the root of flutter project
 - i.e. => flutter_project/android/dynamicallery & dynamicallery2
 The folder structure as given below.

.
|── flutter_project/
     |── android/
        |── dynamicGallery/
        │   |── src/
        │   │   └── AndroidManifest.xml
        │   |── build.gradle
        │   |── dexguards-config.gradle
        |── dynamicGallery2/
        │   |── src/
        │   │   └── AndroidManifest.xml
        │   |── build.gradle
        │   |── dexguards-config.gradle

5.  We are able to compile with dexguard configs too.

6. It results in generating extra symbols for dynamicallery & dynamicallery2 based on the .part.so which is the content for the aab file. [check image below]
  <img width="321" alt="image-20231030-061501" src="https://github.com/flutter/flutter/assets/33690796/76944031-2eb2-4907-9e2e-3117b924c642">
 
7. Content of symbol file with & without adding deferred components check image below
<img width="786" alt="Screenshot 2023-10-30 at 2 30 13 PM" src="https://github.com/flutter/flutter/assets/33690796/5f45ac02-bf5b-46a3-9947-03bbedbdac6d">

8. As a result we are not able to upload .symbols-x.part.so file to firebase, as it results in error.
 - saying native library found
 
9. And the crashes are not able to be symbolicated for deferred component part of the android app.
