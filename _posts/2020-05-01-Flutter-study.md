---
layout: post
title: Flutter Study
date: '2020-05-01 10:07:32'
intro_paragraph: ''
categories: flutter
---
# Flutter

---
## 설치

- [https://flutter-ko.dev/docs/get-started/install](https://flutter-ko.dev/docs/get-started/install)
- flutter doctor
- NDK 특정 버전 필요함
    - 20.0.5594570

        ![Preferences_for_New_Projects](/assets/img/uploads/Preferences_for_New_Projects.png)

---
## 중요 문서

- CookBook : [https://flutter.dev/docs/cookbook](https://flutter.dev/docs/cookbook)
- Flutter for Android developers : [https://flutter-ko.dev/docs/get-started/flutter-for/android-devs](https://flutter-ko.dev/docs/get-started/flutter-for/android-devs)
- Flutter for iOS developers : [https://flutter.dev/docs/get-started/flutter-for/ios-devs](https://flutter.dev/docs/get-started/flutter-for/ios-devs)

---
## 기본 구조

- Widget ([https://flutter.dev/docs/development/ui/widgets](https://flutter.dev/docs/development/ui/widgets))
    - StatelessWidget : 정적인 Widget, Button
        - @override Widget build(BuildContext context) 만들어 줘야함
        - 화면 구성
    - StatefulWidget : 업데이트 가능한 Widget, Form
        - @override State<StatefulWidget> createState() 만들어 줘야함
        - 화면 구성 + Controller 에 대한 처리

    - MaterialApp : For a Material app, you can use a Scaffold widget; it provides a default banner, background color, and has API for adding drawers, snack bars, and bottom sheets. Then you can add the Center widget directly to the body property for the home page.
    - Scaffold : Implements the basic Material Design visual layout structure. This class provides APIs for showing drawers, snack bars, and bottom sheets.
    - Basic widgets([https://flutter.dev/docs/development/ui/widgets/basics](https://flutter.dev/docs/development/ui/widgets/basics))
        - Appbar
        - Column
        - Container : Widget 을 커스터마이징 할 수 있는 위젯(여백, 간격, 테두리, 정렬, 배경색...)
        - Icon : A Material Design icon.
        - Image
        - Placeholder
        - RaisedButton
        - Row
        - Scaffold
        - Text
    - ListView
        - 표준 ListView생성자는 작은 목록에 적합합니다. 많은 수의 항목이 포함 된 목록으로 작업하려면 ListView.builder생성자 를 사용하는 것이 가장 좋습니다 .
        ([https://flutter.dev/docs/cookbook/lists/long-lists](https://flutter.dev/docs/cookbook/lists/long-lists))
    - Layout ([https://flutter.dev/docs/development/ui/widgets/layout](https://flutter.dev/docs/development/ui/widgets/layout))
        - Param 으로 child 또는 children 값을 가짐
        - Single-child layout widgets
            - Align
            - Center
            - Padding
            - Container
            - ...
        - Multi-child layout widgets
            - Column
            - Row
            - GridView
            - Stack
            - ...
    - Navigator Widget
        - Route 객체의 스택 관리
        - [https://flutter.dev/docs/cookbook/navigation](https://flutter.dev/docs/cookbook/navigation)
    - Cupertino Widgets : iOS 디자인 위젯들도 지원한다.
        - CupertinoTabScaffold
        - CupertinoAlertDialog
        - CupertinoButton
        - ...
    - Key param : [https://flutter.dev/docs/development/ui/widgets-intro#keys](https://flutter.dev/docs/development/ui/widgets-intro#keys)
        - 동일한 상태의 동일 위젯으로 구성된 컬렉션을 재정렬하거나, 추가/삭제하는 등의 작업을 할때 사용.?
        - [https://nsinc.tistory.com/214](https://nsinc.tistory.com/214)
        - [https://youtu.be/kn0EOS-ZiIc](https://youtu.be/kn0EOS-ZiIc)

- State
    - [https://flutter.dev/docs/development/ui/interactive](https://flutter.dev/docs/development/ui/interactive)
    - [https://medium.com/flutter-korea/flutter-state-management-시작-a5408f7a86dd](https://medium.com/flutter-korea/flutter-state-management-%EC%8B%9C%EC%9E%91-a5408f7a86dd)

        ![image](/assets/img/uploads/1_uyRJ8g2st03YvpEgsrf2Ow.png)

    - BLOC + RxDart 추천

- Assets
    - pubspec.yaml 에서 assets 위치를 지정한다.
    - 디렉토리의 모든파일을 지정하려면 마지막에 "/" 를 넣어 준다.
    - 서브 디렉토리에 있는 파일을 추가하려면 디렉토리마다 지정해야 한다.

        ```yaml
        flutter:
          assets:
        		- assets/my_icon.png
            - directory/
            - directory/subdirectory/
        ```

        - 다양한 해상도의 이미지를 지원하기 위해서는 디렉토리 하위에 "2.0x", "3.0x" 등의 하위 디렉토리에 동일한 파일을 둔다.
        - 안드로이드 해상도 단위	Flutter 픽셀 비율

            ```
            ldpi	0.75x
            mdpi	1.0x
            hdpi	1.5x
            xhdpi	2.0x
            xxhdpi	3.0x
            xxxhdpi	4.0x
            ```

### Hot reload 지원

- UI 적인 부분만 수정시에 번개 모양 클릭으로 빠르게 적용
- [https://flutter.dev/docs/development/tools/hot-reload](https://flutter.dev/docs/development/tools/hot-reload)
- Hot reload : loads code changes into the VM and re-builds the widget tree, preserving the app state; it doesn’t rerun main() or initState(). (⌘\ in Intellij and Android Studio, ⌃F5 in VSCode)
- Hot restart : loads code changes into the VM, and restarts the Flutter app, losing the app state. (⇧⌘\ in IntelliJ and Android Studio, ⇧⌘F5 in VSCode)
- Full restart : restarts the iOS, Android, or web app. This takes longer because it also recompiles the Java / Kotlin / ObjC / Swift code. On the web, it also restarts the Dart Development Compiler. There is no specific keyboard shortcut for this; you need to stop and start the run configuration.

---
## 플랫폼 별 라이브러리 코드 작성

([https://flutter.dev/docs/development/platform-integration/platform-channels](https://flutter.dev/docs/development/platform-integration/platform-channels))

![assets/img/uploads/PlatformChannels.png](/assets/img/uploads/PlatformChannels.png)

- 그냥 사람들이 올려 놓은 샘플 참고 하자.
    - get_ip([https://github.com/lotrofans/get_ip_flutter](https://github.com/lotrofans/get_ip_flutter))

---
## Flutter 내부에서 플렛폼별 소스 구분

```dart
import ‘dart:io’ show Platform;
if (Platform.isAndroid) {
 // Android-specific code
} else if (Platform.isIOS) {
 // iOS-specific code
}
```

---
## Networking

- [https://flutter.dev/docs/development/data-and-backend/networking](https://flutter.dev/docs/development/data-and-backend/networking)
- http ([https://pub.dev/packages/http](https://pub.dev/packages/http))

---
## JSON

- [https://flutter.dev/docs/development/data-and-backend/json](https://flutter.dev/docs/development/data-and-backend/json)
- gson 같은 라이브러리는 없다.
    - dart 에는 dartson 이 있지만, flutter 는 성능적인 문제로 runtime reflection 을 지원하지 않기 때문에 호환되지 않는다.
    - [https://flutter.dev/docs/development/data-and-backend/json#is-there-a-gsonjacksonmoshi-equivalent-in-flutter](https://flutter.dev/docs/development/data-and-backend/json#is-there-a-gsonjacksonmoshi-equivalent-in-flutter)
- 소규모 프로젝트에서는 데이터 직렬화 이용.
- 대규모 프로젝트에서는 json_annotation 라이브러리 사용

---
## 기존 앱에 Flutter 화면 적용 가능 ([https://flutter.dev/docs/development/add-to-app](https://flutter.dev/docs/development/add-to-app))

- Android : AAR + FlutterActivity
- iOS : FlutterViewController

---
## 다국어 지원

[https://flutter.dev/docs/development/accessibility-and-localization/internationalization](https://flutter.dev/docs/development/accessibility-and-localization/internationalization)

 ([https://software-creator.tistory.com/24](https://software-creator.tistory.com/24))

- 아직은 너무 복잡함.
    - messages.dart 만들기
    - commend 로 intl_messages.arb 생성하기
        - `flutter pub run intl_translation:extract_to_arb --output-dir=lib/i18n lib/i18n/messages.dart`
        - ARB : Application Resource Bundle ([https://github.com/google/app-resource-bundle](https://github.com/google/app-resource-bundle))
        - json 형식
    - 내용을 번역해서 intl_ko.arb, intl_en.arb ... 만들기
    - commend 로 messages_all.dart, messages_ko.dart, message_en.dart ... 만들기
        - `flutter pub run intl_translation:generate_from_arb --output-dir=lib/i18n --no-use-deferred-loading lib/i18n/messages.dart lib/i18n/intl_*.arb`
    - LocalizationsDelegate 파일 생성(app_localizations.dart)및 설정
        - MaterialApp 의 localizationsDelegates param
    - iOS의 경우 Info.plist 에 번역 하려는 locale을 추가
    - Use

        ```dart
        import '/i18n/messages.dart';
        final msg = Messages()

        //in widget
        Text(msg.appName)
        ```

---
## 기타

- 데이터 저장
    - Key - Value 데이터 저장 : shared_preferences ([https://pub.dev/packages/shared_preferences](https://pub.dev/packages/shared_preferences))
    - Sqlite : [https://pub.dev/packages/sqflite](https://pub.dev/packages/sqflite)
- Work with cached images
    - [https://flutter.dev/docs/cookbook/images/cached-images](https://flutter.dev/docs/cookbook/images/cached-images)
    - [https://pub.dev/packages/cached_network_image](https://pub.dev/packages/cached_network_image)
- 난독화
    - Flutter 1.16.2 이후
    - --obfuscate 사용 : [https://flutter.dev/docs/deployment/obfuscate](https://flutter.dev/docs/deployment/obfuscate)
- flutter clean 으로 build cache clear
- Web 용 개발중.. : [https://flutter.dev/web](https://flutter.dev/web)
- Desktop 용 개발중.. : [https://flutter.dev/desktop](https://flutter.dev/desktop)

---
## Design pattern

- BLOC : [https://bloclibrary.dev/#/gettingstarted](https://bloclibrary.dev/#/gettingstarted)

    | ![bloc_architecture.png](/assets/img/uploads/bloc_architecture.png)

    | ![flutter_infinite_list_flow.png](/assets/img/uploads/flutter_infinite_list_flow.png)

    Naming Conventions : \[https://bloclibrary.dev/#/blocnamingconventions](https://bloclibrary.dev/#/blocnamingconventions)