---
title: Write Your First Flutter App, part 2
date: 2019-09-02
---

# 이번주 목표

| 지난주까지 진도                                              | 이번주 목표                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt1/img/e3624172607d5551.gif) | ![](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2/img/b17de15fa7831a1c.gif) |



# 사전 준비

- 혹시 못 진도를 못 따라오셨나요? 걱정마세요! 우리 스터디는 완주가 목표입니다.
-  우선 **lib/main.dart** 의 모든 코드를 지우고 이 [file](https://github.com/flutter/codelabs/blob/b3293b5bb0c0187bdbe8112f7759f4d75f4c040a/startup_namer/step4_infinite_list/lib/main.dart)로 덮어쓰세요. 
- 그리고 **pubspec.yaml** 에 ` english_words: ^3.1.0` 를 추가하세요. 
- 다음에 우측 상단에 있는 click **Packages get** 라는 버튼을 클릭해서 업데이트 해주세요.
- 마지막으로 앱을 동작시켜보세요!



# 즐겨찾기 버튼 만들기!

## 아이콘 추가하기

> https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2/index.html?index=..%2F..index#4

`RandomWordsState`에 _saved 라는 function을 추가합니다.

```dart
final Set<WordPair> _saved = Set<WordPair>();   // Add this line.
```

`_buildRow` 에 이미 추가된 wordPair가 있는지를 확인하는 AlreadySaved라는 기능을 추가합니다. 

```dart
  final bool alreadySaved = _saved.contains(pair);  // Add this line.
```

`_buildRow`에 아래 코드를 추가하고 빌드해보세요. 즐겨찾기 버튼이 활성화될 것 입니다!

```dart
Widget _buildRow(WordPair pair) {
  final bool alreadySaved = _saved.contains(pair);
  return ListTile(
    title: Text(
      pair.asPascalCase,
      style: _biggerFont,
    ),
    trailing: Icon(   // 아이콘을 추가합니다. 
      alreadySaved ? Icons.favorite : Icons.favorite_border,
      color: alreadySaved ? Colors.red : null,
    ),                // ... 여기까지 붙여넣으세요. 
  );
}
```

## 아이콘을 클릭하도록 만들자!

`_buildRow`에 아래 onTap 펑션을 추가해서 즐겨찾기 버튼이 클릭 가능하도록 만들어봅시다!

```dart
    onTap: () {      // Add 9 lines from here...
      setState(() {
        if (alreadySaved) {
          _saved.remove(pair);
        } else { 
          _saved.add(pair); 
        } 
      });
    },               // ... to here.
```

# 네비게이션도 수정해봅시다.

이제 네비게이션을 수정해서 여러 페이지를 왔다갔다 할 수 있도록 만들어 봅시다.

먼저 RandomWordsState 클래스에 아래 코드를 추가합니다. 

```dart
class RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];
  final Set<WordPair> _saved = Set<WordPair>();
  final _biggerFont = const TextStyle(fontSize: 18.0);
  // #enddocregion RWS-var

  void _pushSaved() { // 여기부터 
  } // 여기까지 붙여넣기 해주세요.
  ...
```



## 액션 아이콘을 추가하자

다음으로는 RandomWordsState에서 Appbar를 컨트롤 하는 부분을 찾아서 아이콘을 추가해봅시다.

```dart
class RandomWordsState extends State<RandomWords> {
  ...
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Startup Name Generator'),
        actions: <Widget>[      // Add 3 lines from here...
          IconButton(icon: Icon(Icons.list), onPressed: _pushSaved),
        ],                      // ... to here.
      ),
      body: _buildSuggestions(),
    );
  }
  ...
}
```

이제 빌드를 돌려보세요! 아이콘이 잘 보이나요?



## 라우팅 해보자

이제 _pushSaved 버튼을 수정해보겠습니다. 

```dart
void _pushSaved() {
  Navigator.of(context).push(
    MaterialPageRoute<void>(
      builder: (BuildContext context) {
        final Iterable<ListTile> tiles = _saved.map(
          (WordPair pair) {
            return ListTile(
              title: Text(
                pair.asPascalCase,
                style: _biggerFont,
              ),
            );
          },
        );
        final List<Widget> divided = ListTile
          .divideTiles(
            context: context,
            tiles: tiles,
          )
              .toList();

        return Scaffold(         // Add 6 lines from here...
          appBar: AppBar(
            title: Text('Saved Suggestions'),
          ),
          body: ListView(children: divided),
        );                       // ... to here.
      },
    ),
  );
}
```

