---
title: Write Your First Flutter App, part 2-2
date: 2019-09-09

---

# 이번주 목표

![](https://codelabs.developers.google.com/codelabs/mdc-101-flutter/img/e8f2476968468376.png)

# 사전 준비

- 혹시 못 진도를 못 따라오셨나요? 걱정마세요! 우리 스터디는 완주가 목표입니다.
-  우선 **lib/main.dart** 의 모든 코드를 지우고 이 [file](https://github.com/flutter/codelabs/blob/b3293b5bb0c0187bdbe8112f7759f4d75f4c040a/startup_namer/step4_infinite_list/lib/main.dart)로 덮어쓰세요. 
- 그리고 **pubspec.yaml** 에 ` english_words: ^3.1.0` 를 추가하세요. 
- 다음에 우측 상단에 있는 click **Packages get** 라는 버튼을 클릭해서 업데이트 해주세요.
- 마지막으로 앱을 동작시켜보세요!



# Theme 추가하가

## Theme 이란?

- 서비스 전체에 적용되는 공통 디자인 규칙 
- 앱/웹/데스크톱 모두 Theme을 사용합니다. (예를 들어 다크모드!)

![](https://storage.googleapis.com/spec-host-backup/mio-design%2Fassets%2F1b7zteqiB7LCxy1R_NQwQZZ3_c8JqLE7T%2Ftheming-overview-applyingtheming.mp4)

## Theme 맛보기

맛보기로 메인 컬러(Primary Color)를 흰색으로 바꿔봅시다.  

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Startup Name Generator',
      theme: ThemeData(          // Add the 3 lines from here... 
        primaryColor: Colors.white,
      ),                         // ... to here.
      home: RandomWords(),
    );
  }
}
```

> TMI
> - https://material-ui.com/customization/color/#color-tool
> - https://material.io/resources/color/

-------

# 머테리얼 디자인 프로젝트 시작

## 준비하기

1) 아래 주소를 즐겨찾기 하세요!

- https://codelabs.developers.google.com/codelabs/mdc-101-flutter/index.html
- https://github.com/material-components/material-components-flutter-codelabs

2) Github 주소를 Fork & Clone 하세요. 

- 큰 상관은 없지만 그냥 Clone 하면 Push가 안되기 때문에 초반에는 혼란스러울 수 있습니다.
- 안전하게 Fork 후 Clone 하세요.

3) 가이드에 따라서 프로젝트를 추가하고, 실행하세요.

- 초반에 오류가 나는 것은 무시하세요.
- 오류가 계속되는 경우, Package-get을 실행한 뒤, Android 스튜디오를 재시작하세요.



