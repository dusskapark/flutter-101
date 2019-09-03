---
title: Write Your First Flutter App, part 2
date: 2019-08-19
---

## 참고자료:

비개발자라면 State, Stateless, Stateful, Widget 의 개념을 지금 잡는 것은 상당히 어려운 입니다. 
당장 모든 것을 암기하거나 이해할 필요는 없습니다. 대신 Flutter를 공부하면서 조금씩 익숙해지는 것이 중요할 것 입니다. 

Flutter의 여러 블록에서 제가 찾은 자료를 아래와 같이 인용합니다. 

> ###  Widget: Everything is Widget
>
> 출처: https://medium.com/flutter-korea/get-started-with-flutter-9703c3f6bd4f
>
> 리액트, 앵귤러, Vue 같은 프론트엔드 라이브러리를 다뤄본 사람이라면 컴포넌트 단위로 레이아웃을 쪼개는 것에 대해서 굉장히 익숙할 것이다. HTML 요소를 여러개 묶어서 컴포넌트를 만들듯, Flutter는 Widget을 여러개 묶어서 또 다른 Widget을 만든다.
> 그렇기 때문에 어떤 Widget을 사용할 수 있는 지에 대해서만 알고 있다면 생각보다 Flutter를 다루는 것이 어렵지 않다. 예를 들어, 이미지를 사용하고 싶다면 Image 위젯을 사용하면 된다.
>
> ```dart
> Image.asset(
> 'images/lake.jpg'
> )
> ```
>
> 텍스트를 사용하고 싶다면 Text 위젯을 사용하면 된다.
> ```dart
> Text('Hello World')
> ```
>
> Flutter에서 이미 구현해둔 다양한 위젯들이 있기 때문에 Built in 위젯들을 여러개 조합해서 더 큰 덩어리를 만들고, 더 큰 덩어리를 더 크게 만들어서 위젯들을 조합하여 사용할 수 있다.
>



> ### State는 뭔가요?
>
> [왜 State Management를 할까?](https://medium.com/flutter-korea/flutter-state-management-시작-a5408f7a86dd)
>
> State는 Data라고 한마디로 정의 할 수 있다.
>
> 즉 Flutter로 만드는 Application에서 UI를 통해서 사용자에게 보여주는 Data, 또는 Application을 통해서 사용자와 상호작용 하면서 변화하는 Data (ex. checkbox의 check 상태, Drawer의 open 또는 close 상태 등), 또는 내부적으로만 사용하는 Data가 될 수 있다.
>
> ###  **StatelessWidget / StatefulWidget**
>
> Flutter의 **모든** UI요소들이 Widget이다. 크게 우리가 자주사용하게 될 Widget을 두 분류로 나눌 수 있는데, StatelessWidget과 StatefulWidget이다.
>
> StatelessWidget은 보통 상태를 가지지 않는다고 표현을 하는 분들이 많지만, 사실 **내부적으로 상태를 가진다**. 그러면 왜 Stateless인가? 라는 물음에 StatelessWidget의 State는 **Immutable** 하다고 할 수 있다. 즉 변경할수없는 상태를 가지는 Widget인 것이다. 그러면 당연히도 반대인 StatefulWidget은 변경 가능한 상태를 가지는(**mutable**) Widget인 것이다.
>
> ...
>



## 3. Stateful 위젯 만들기 

위에서 설명한 것처럼 우리가 만드는 앱은 위젯을 조립하면 할수록 더 복잡해질 것이고 스트레스를 받을 것 입니다. 따라서 기존의 코드를 밖으로 빼서.. Stateful 위젯을 만들어서 코드를 정리해보겠습니다. (정리만 하는 것이니, 결과는 똑같아야 합니다.)

hot reload button (![img](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt1/img/7f9a9e103c7b5e5.png)) 버튼을 눌러보면서 확인해보세요

#### 새로운 클래스 추가 

Google Codelab 가이드에 따라서 새로운 Class를 추가합니다. 그리고 동시에 기존에 위젯 안에 하드코딩으로 작성한 코드를 삭제합니다. 


```dart

import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // 이 코드를 삭제합니다.

    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: RandomWords(),// 그리고 여기를 바꿉니다.
        ),
      ),
    );
  }
}

// 랜덤 영단어를 제어하는 State를 만듭니다.
class RandomWordsState extends State<RandomWords> {
  @override
  Widget build(BuildContext context) {
    final WordPair wordPair = WordPair.random(); // 위에서 삭제한 코드를 이곳에 모아서 관리합니다.
    return Text(wordPair.asPascalCase); // 랜덤 영단어 조합을 리턴 합니다.
  }
}
// RandomWordsState를 StatefulWidget으로 생성합니다.
class RandomWords extends StatefulWidget {
  @override
  RandomWordsState createState() => new RandomWordsState();
}
```



## 4 리스트뷰로 만들기

다음으로는 이 두 영단어 쌍을 리스트를 만들어서 스크롤 할 수 있도록 만들어 보겠습니다.
`_suggestions` 이라는 이름의 List를 만들고, `_biggerFont`라는 변수를 통해서 폰트 사이즈를 18pt로 키워보겠습니다. 

```dart
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Startup Name Generator',
      home: RandomWords(),
    );
  }
}

class RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[]; // _suggestions에 WordPair를 넣습니다. 
  final _biggerFont = const TextStyle(fontSize: 18.0); // 텍스트 크기를 18로 키운 위젯 선언합니다. 

// 다음으로 _suggestions 을 만드는 위젯 입니다.
// 상세한 내용을 모두 알 필요는 없습니다. 무엇을 리턴하는지만 잘 파악하면 됩니다.  

  Widget _buildSuggestions() {
    return ListView.builder(
        padding: const EdgeInsets.all(16.0),
        itemBuilder: /*1*/ (context, i) {
          if (i.isOdd) return Divider(); /*2*/

          final index = i ~/ 2; /*3*/
          if (index >= _suggestions.length) {
            _suggestions.addAll(generateWordPairs().take(10)); /*4*/
          }
          // _suggestions (= WordPair) 을 넣어서 _buildRow를 리턴합니다.
          // 아래에 _buildRow 가 있어야겠죠?
          return _buildRow(_suggestions[index]);
        });
  }
  
  Widget _buildRow(WordPair pair) {
  // Build row는 WordPair를 넣으면 ListTile 을 되돌려줍니다. 
    return ListTile(
      title: Text(
        pair.asPascalCase,
        style: _biggerFont,
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
  // 마지막으로 RandomWordsState가 리턴하는 값을 수정합니다. 
    return Scaffold(
      appBar: AppBar(
        title: Text('Startup Name Generator'),
      ),
      body: _buildSuggestions(),
    );
  }
}
// #end RandomWordsState

class RandomWords extends StatefulWidget {
  @override
  RandomWordsState createState() => new RandomWordsState();
}

```



**성공**

이제 무한스크롤이 되는 리스트가 완성됐습니다!



![](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt1/img/d71590c8c3c2e6f8.png)