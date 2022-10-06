# 플루터 공부

## 삼항연산자

```dart
var visibility = isPublic ? 'public' : 'private'; // Public 일 경우 public 출력, 아니면 private 출력
String plyerName(String name) => name ?? 'Guest'; // 입력받은 name 이 Guest와 같은가 비교한다.
```

---

```dart
printInteger(int aNumber){ // printInteger 메소드 생성 매개변수는 int aNumber 이다.
    print('The number is $aNumber.'); // print(); 출력시 $변수명 으로 한다.
}

main(){
    var number = 42; // var 형 변수 추론해서 알아서 자료형을 정한다.
    printInteger(number); // 함수 실행
}
```

## 비동기 함수

```dart
Future => 저장했다가 출력함
async => 비동기 함수
await => 언제 끝날지 모르는 함수, 가장 나중에 사용됨
then.() => 비동기 함수의 반환값을 받을 때 사용함
```

```dart
void main() async{ // async 비동기 함수
  await getVersionName().then((value) => { // 비동기 함수 내에서 반환하는 값을 처리하기 위해서 then() 함수를 이용함
    print(value) // print()
  }); // then 함수는 여기까지 
  print('end process'); // print();
}

Future<String> getVersionName() async{ // Future 클래스 이므로 비동기 이며, 반환값을 <String>으로 지정한다. async 비동기 함수.
  var versionName = await lookUpVersionName(); // var 추론 데이터형, await 결과값이 필요하기 때문에 지정.
  return versionName; // String 반환함
}

String lookUpVersionName(){ // 메소드 선언
  return 'Android Q'; // String을 반환함
}
```

> 메인 함수가 실행된 후 await 가 붙은 함수 순서대로 실행됨

---

## json 파일 사용

```dart
import 'dart:convert';
```

> 일반 딕셔너리의 리스트 형식에도 json을 사용가능하다.

---

## 스트림 통신

> 데이터나 네트워크에 의해서 원하는 흐름대로 작동하지 못할때 사용함

---
