# **State management**
>Based on version: Flutter 3.13.1

<p align="center"><img src=resources/state-management-explainer.gif width=500><br>Fig. 1. State Management Explainer</p>

### State

앱 내에서 다른 화면들 간 서로 상태 공유가 되어야할 때가 있다. Fig.1 그림과 같이 예를 들면, 쇼핑 앱에선 카탈로그 화면에서 물건을 장바구니에 담고 최종적으로 장바구니에서 담은 물건들을 내 장바구니 화면에서 확인하여 로그인/결제까지 이어지게 된다. 내가 무슨 물건을 담았다는 **상태**를 장바구니 화면도 알아야 되는 상태이다.

<p align="center"><img src=resources/ui-equals-function-of-state.png width=500><br>Fig. 2.</p>

플러터에서는 다른 많은 UI프레임워크들과 달리 현재 앱의 상태가 바뀌면 해당되는 UI를 처음부터 재빌드한다. 기존의 상태/인스턴스를 바꾸는 것(명령형, Imperative)이 아닌 새로운 상태를 만들어 주입한다. 이것을 **선언적(Declarative) 프로그래밍**이라고 한다.


```dart
// Declarative style
return ViewB(
  color: red,
  child: const ViewC(),
);
```

```swift
// Imperative style
b.setColor(red)
b.clearChildren()
ViewC c3 = new ViewC(...)
b.add(c3)
```

선언적(Declarative) 스타일의 플러터 위젯은 변경할 수가 없고 가벼운 '청사진(blueprints)'정도로만 생각하면 된다. UI를 바꾸려면 해당 위젯은 자체적으로 재빌드한다. 일반적으로 StatefulWidgets의 setState()를 호출해서 재빌드하며 새로운 위젯 서브트리를 만든다.

### Ephemeral vs. App state

> **Ephemeral state**


**Ephemeral state**는 단명하는 상태를 말하는데, 흔히 local state 또는 UI state라고 한다. 싱글 위젯에 포함시킬 수 있는데 예를 들면 다음과 같다.

- [PageView]("https://api.flutter.dev/flutter/widgets/PageView-class.html") 안에 현재 Page
- 복잡한 애니메이션의 현재 진행 상태
- `BottomNavigationBar` 안에 현재 선택된 탭

이런 종류의 상태에는 Riverpod, ScopedModel, Redux, 등과 같은 상태 관리 기법을 사용하지 않고 StatefulWidget만으로도 충분하다.

```dart
class MyHomepage extends StatefulWidget {
  const MyHomepage({super.key});

  @override
  State<MyHomepage> createState() => _MyHomepageState();
}

class _MyHomepageState extends State<MyHomepage> {
  int _index = 0;

  @override
  Widget build(BuildContext context) {
    return BottomNavigationBar(
      currentIndex: _index,
      onTap: (newIndex) {
        setState(() {
          _index = newIndex;
        });
      },
      // ... items ...
    );
  }
}
```

`BottomNavigationBar` 위젯을 사용하는 `MyHomepage`라는 StatefulWidget에서는 현재 선택된 탭을 가리키는 `_index`를 setState() 함수를 사용하여 접근/변경한다. 앱 내 다른 어떤 부분도 `_index`를 필요로 하지 않는다. 유저가 앱을 껐다 켜도 `_index`가 초기화되어 0이 되어도 상관이 없는 부분이다.


> *App state*

**App state**는 ephemeral state와 달리 단명하지 않고 앱 전반적으로 공유하고 싶은 상태이다. `공유 상태`라고도 하는데 예를 들면 다음과 같다.

- 사용자 환경 설정
- 로그인 정보
- SNS앱에서의 알림
- 이커머스앱에서의 장바구니
- 뉴스앱에서의 읽음/안읽음 상태

<p align="center"><img src=resources/ephemeral-vs-app-state.png width=500><br>Fig. 3.</p>

정확하게 이 상태는 ephemeral state이고 저 상태는 app state라고 정해진 것은 없다. `BottomNavigationBar`의 `_index`를 외부 클래스(위젯)에서 접근해서 변경해야 된다면 `_index`는 app state로 관리해야 한다. 앱 개발 초기에는 ephemeral state였다가 앱이 커지면서 app state로 리팩토링해야 되는 경우도 있을 것이다.

이 두 컨셉의 상태는 어느 플러터 앱에나 있다. 어떻게 쓰느냐는 상관없기 때문에 복잡도와 선호도에 따라 적재적소에 사용하면 된다.



--- 
### **References**

*State management. Flutter.* https://docs.flutter.dev/data-and-backend/state-mgmt/intro.