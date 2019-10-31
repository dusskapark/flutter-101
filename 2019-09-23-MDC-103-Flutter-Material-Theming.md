---
date: 2019-09-23
title: MDC-103 Flutter: Material Theming 
---



# 오늘 할 일

- 머테리얼 테마를 수정해서 플러터의 디자인을 변경해 봅시다. 
- theme에 대해서 이해하고, 디자이너 -> 개발자의 협업 방식에 대해서 논의합니다. 

<img src="https://codelabs.developers.google.com/codelabs/mdc-103-flutter/img/9346cdffc30760da.png" style="zoom: 25%;" />|<img src="https://codelabs.developers.google.com/codelabs/mdc-103-flutter/img/52920c70743adf6e.png" style="zoom: 25%;" />





# 사전 준비

1. 기존에 깃허브에서 클론한  `material-components-flutter-codelabs` 의 브랜치를 `103-starter_and_102-complete` 으로 변경합니다.
1. 플레이 버튼을 눌러서 현재 상태를 확인합니다.



<img src="https://codelabs.developers.google.com/codelabs/mdc-103-flutter/img/532fe80b3fa3db74.png" style="zoom:25%;" />



# 컬러 테마 추가 

- `lib/colors.dart` 파일을 새로 생성하고 아래 코드를 붙여넣습니다.

```dart
import 'package:flutter/material.dart';

const kShrinePink50 = const Color(0xFFFEEAE6);
const kShrinePink100 = const Color(0xFFFEDBD0);
const kShrinePink300 = const Color(0xFFFBB8AC);
const kShrinePink400 = const Color(0xFFEAA4A4);

const kShrineBrown900 = const Color(0xFF442B2D);

const kShrineErrorRed = const Color(0xFFC5032B);

const kShrineSurfaceWhite = const Color(0xFFFFFBFA);
const kShrineBackgroundWhite = Colors.white;
```



- `app.dart`에 아래 colors.dart를 추가하고, 코드에 반영합니다.

```dart
import 'colors.dart'; // 파일 import 추가 
...
  
// TODO: Build a Shrine Theme (103)
final ThemeData _kShrineTheme = _buildShrineTheme();

ThemeData _buildShrineTheme() {
  final ThemeData base = ThemeData.light();
  return base.copyWith(
    accentColor: kShrineBrown900,
    primaryColor: kShrinePink100,
    buttonTheme: base.buttonTheme.copyWith(
      buttonColor: kShrinePink100,
      textTheme: ButtonTextTheme.normal,
    ),
    scaffoldBackgroundColor: kShrineBackgroundWhite,
    cardColor: kShrineBackgroundWhite,
    textSelectionColor: kShrinePink100,
    errorColor: kShrineErrorRed,
  );
}

// TODO: Add a theme (103)
return MaterialApp(
  title: 'Shrine',
  home: HomePage(),
  initialRoute: '/login',
  onGenerateRoute: _getRoute,
  theme: _kShrineTheme, // 요기 코드를 추가합니다. 
);
```



- 결과 확인

<img src="https://codelabs.developers.google.com/codelabs/mdc-103-flutter/img/31adaca378656d60.png" style="zoom:50%;" />



# Typography 수정 

앱바의 색상이 흰색이라 가독성이 떨어지므로 텍스트 색상을 수정합니다.

- pubspec.yaml 에 아래 코드를 추가 후 package get을 눌러서 Rublic 폰트를 가져옵니다.

```yaml
  # TODO: Insert Fonts (103)
  fonts:
    - family: Rubik
      fonts:
        - asset: fonts/Rubik-Regular.ttf
        - asset: fonts/Rubik-Medium.ttf
          weight: 500
```



- app.dart 파일에서 `_buildShrineTheme()` 뒤에 `_buildShrineTextTheme` 이라는 함수를 추가합니다. 
  - 이 text-Theme 이 headlines, titles, captions의 폰트 를 변경시켜줍니다. 

```dart
// TODO: Build a Shrine Text Theme (103)
TextTheme _buildShrineTextTheme(TextTheme base) {
  return base.copyWith(
    headline: base.headline.copyWith(
      fontWeight: FontWeight.w500,
    ),
    title: base.title.copyWith(
        fontSize: 18.0
    ),
    caption: base.caption.copyWith(
      fontWeight: FontWeight.w400,
      fontSize: 14.0,
    ),
  ).apply(
    fontFamily: 'Rubik',
    displayColor: kShrineBrown900,
    bodyColor: kShrineBrown900,
  );
}
```



- text-Theme 을 _buildShrineTheme 에 적용해줍니다. 

 ```dart
// TODO: Add the text themes (103)

textTheme: _buildShrineTextTheme(base.textTheme),
primaryTextTheme: _buildShrineTextTheme(base.primaryTextTheme),
accentTextTheme: _buildShrineTextTheme(base.accentTextTheme),
 ```

- 텍스트에 맞춰서 아이콘도 색상을 맞춰줍니다.

```dart
    // TODO: Add the icon theme (103)
    primaryIconTheme: base.iconTheme.copyWith(
      color: kShrineBrown900
    ),
```



- 결과 확인

<img src="https://codelabs.developers.google.com/codelabs/mdc-103-flutter/img/9489cc4b3274b10a.png" style="zoom:25%;" />



## 