# **Future**
> Based on version: Flutter 3.13.1


### aync/await 그리고 Future

aync 또는 aync*/await 키워드는 Future와 Stream 같은 클래스들과 함께 **비동기 프로그래밍**할 때 사용된다. 비동기 처리는 파일을 읽어 들인다거나 데이터베이스에 데이터 요청을 하는 등 시간이 좀 걸리는 처리를 할 때 사용할 수 있다. 

프로그램에서 다른 처리들을 블로킹하지 않고 언젠가 **미래**에 "완료될" 결과를 가져갈 Future 타입을 바로 반환한다.

<br>

<p align="center"><img src=resources/video_post_paused.png width=200> <img src=resources/comments_modal.png width=200></p>

왼쪽 화면의 댓글 아이콘을 탭하면 오른쪽 화면에 보이는 댓글 모달(팝업)창이 뜬다. 이 때, 댓글 창이 떠 있는 동안에는 백그라운드에서 재생되던 영상은 정지되고 창을 닫으면 재개된다.


```dart
bottom_sheet.dart

Future<T?> showModalBottomSheet<T>({
  required BuildContext context,
  required WidgetBuilder builder,
  ...
```

Flutter에서 제공하는 showModalBottomSheet 함수는 Future<T?> 타입을 반환한다. 모달 창을 아래에서부터 열어주는 함수이다.

```dart
void _onTapComments(BuildContext context) async {
    if (_videoPlayerController.value.isPlaying) {
      _onTogglePause();
    }

    await showModalBottomSheet(
      isScrollControlled: true,
      backgroundColor: Colors.transparent,
      context: context,
      builder: (context) => const VideoComments(),
    );

    _onTogglePause(); // 댓글 창을 닫으면 호출된다.
  }
```

댓글 창을 여는 순간 영상이 재생되고 있다면 정지시키고 창을 닫으면 다시 재개해주는 건 Future, async/await 덕분이다. 창이 닫힐 때까지 await 하였다가 닫히면 재생/정지 토글 함수가 호출되면서 영상이 재개된다. 이렇게 프로그램의 흐름을 블로킹하지 않고(논-블로킹) 완료될 때까지 기다리지 않고 미래9(Future)에 결과 값이 나오면 **비동기적으로** 후처리가 진행된다.

#### Error checking

```dart
Future<bool> fileContains(String path, String needle) async {
  try {
    var haystack = await File(path).readAsString();
    return haystack.contains(needle);
  } on FileSystemException catch (exception, stack) {
    _myLog.logError(exception, stack);
    return false;
  }
}
```

일반적인 try/catch를 이용해서 에러 핸들링해주면 된다.

```dart
Future<int> future = getFuture();
future.then((value) => handleValue(value))
      .catchError((error) => handleError(error));
```

자체적으로 콜백을 지정해서 에러 핸들링 해줄 수도 있다.

--- 
### **References**

*dart:async library - Dart API.* https://api.flutter.dev/flutter/dart-async/dart-async-library.html.

*Future class - dart:async library - Dart API.* https://api.flutter.dev/flutter/dart-async/Future-class.html.

*showModalBottomSheet function - material library - Dart API.* https://api.flutter.dev/flutter/material/showModalBottomSheet.html.