## 2.0 Primitive Types & num
- Dart의 모든 데이터 타입은 **객체**이며, **클래스**로 이루어져 있다.
- 대표 타입들:
    - `String`
    - `bool`
    - `int`
    - `double`
- `int`와 `double`은 모두 **num** 클래스를 상속한다.
    - 즉, `num` 타입은 정수와 실수 모두 담을 수 있다.

```dart
num x = 12;
x = 1.1; // OK
```

---

## 2.1 Lists

- Dart에서 배열은 **List**로 표현한다.
- `var`로 선언 시 타입을 자동 추론한다.

```dart
var numbers = [1, 2, 3, 4, 5];
```

- `Collection if` : 내부에 **if문**을 쓸 수 있다. (엄청 편리!)

```dart
var giveMeFive = true;
var numbers = [
  1,
  2,
  3,
  4,
  if (giveMeFive) 5,
];
print(numbers); // [1, 2, 3, 4, 5]
```

---

## 2.2 String Interpolation

- 문자열 안에 변수 값을 쉽게 넣을 수 있다.
- `$변수명`으로 간단히 쓸 수 있고, 계산이나 표현식을 넣고 싶다면 `${}`를 사용한다.

```dart
var name = 'subbni';
var greeting = 'Hello, everyone, my name is $name.';
print(greeting);
```

```dart
var age = 25;
var greeting = 'Next year, I\'ll be ${age + 1}.';
print(greeting);
```

---

## 2.3 Collection For

- **컬렉션 내에서 for문**을 쓸 수 있다.
- 리스트를 동적으로 생성할 때 매우 유용하다.

```dart
void main() {
  var oldFriends = ['subbni', 'hyang', 'zoo'];
  var newFriends = [
    'lewis',
    'ralph',
    'darren',
    for (var friend in oldFriends) "⭐️ $friend",
  ];
  print(newFriends);
  // ['lewis', 'ralph', 'darren', '⭐️ subbni', '⭐️ hyang', '⭐️ zoo']
}

```

---

## 2.4 Maps

- **Key-Value** 쌍으로 이루어진 자료구조
- `var`로 선언 시 타입 추론이 가능하다.

```dart
var player = {
  'name': 'subbni',
  'age': 23,
  'power': 88,
};
```

- 직접 타입을 명시할 수 있다.

```dart
Map<String, int> scores = {
  'nico': 99,
  'lynn': 85,
};
```

- `var`와 함께 **타입**도 선언 가능하다.

```dart
var player = <String, Object>{
  'name': 'subbni',
  'age': 23,
};
```

---

## 2.5 Sets

- 중복을 허용하지 않는 **집합 자료형**
- `Set<타입>`으로 선언 가능하다.

```dart
Set<int> numbers = {1, 2, 3, 4};
```

- `var`로 선언 시 타입 추론 가능

```dart
var objects = {1, 2, 3, 4, 'hi'}; // Set<Object> 로 추론됨
```
