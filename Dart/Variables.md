### 1.1 var keyword

- `var`로 변수를 선언하면 타입 추론이 일어난다.
- 한 번 타입이 정해지면 이후에는 다른 타입으로 변경할 수 없다.

```dart
var name = 'hehe';
name = 12; // Error
```

- 하지만 초기화하지 않고 선언만 하면 `dynamic`으로 취급된다.

```dart
void main() {
  var name;
  name = 12;
  name = 'hehe';
  if (name is String) {
    print('it\'s string');
  }
  name = 12;
  if (name is int) {
    print('it\'s int');
  }
  name = null;
  if (name == null) {
    print('it\'s null');
  }
}
```

### 1.2 dynamic type

- 변수의 타입 변경까지 허용하는 타입이다.
- 두 가지 경우 `dynamic`이 된다.
    - `var`로 선언만 하고 초기화하지 않은 경우
    - 직접 `dynamic` 으로 명시한 경우
- dynamic은 꼭 필요한 경우에만 쓰고, 남용하지 않는 것이 좋다. (타입 안정성이 필요할 경우 더더욱)
- null도 담을 수

```dart
dynamic name = 'hehe';
name = 12; // OK
name = null; // OK
```

### 1.3 Nullable Variables

- Dart에서는 기본적으로 변수에 null을 허용하지 않는다.
- null이 될 수 있는 변수는 **타입 뒤에 ?를 붙여서 선언한다.**

```dart
String? nickname = '오자두';
nickname = null; // OK
nickname?.isNotEmpty; // null이 아니라면 isNotEmpty 호출
```

### 1.4 final Variables

- 한 번 값이 할당되면 변경할 수 없는 변수, 즉 상수 선언
- 런타임 시점에 값이 결정된다.
- 타입 생략도 가능하지만(자동 타입 추론), 협업 시 타입 명시를 하는 것이 좋을 것 같다.

```dart
final name = '수빈';
final String nickname = '오자두';
```

### 1.5 late Variables

- 변수를 선언하되 나중에 초기화하고 싶을 때 사용한다.
- API 호출 결과 등을 나중에 할당할 때 유용하게 사용한다.
- `late final` 로 쓰면 한 번만 초기화가 가능하다.

```dart
late String name;
name = '수빈';

late final String nickname;
nickname = '오자두'; // 이후 재할당 불가
```

### 1.6 const Variables

- **컴파일 타임 상수**를 만든다.
- 컴파일 시점에 값이 결정되어야 한다.
    - 따라서 외부에서 동적으로 가져와 상수처럼 사용하고 싶은 데이터는 const로 선언이 불가하며, final을 사용하여야 한다.
- 앱에 항상 존재하고 변하지 않는 값을 담을 때 사용한다.

```dart
const title = '앱 이름';
```
