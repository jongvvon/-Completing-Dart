# Dart 언어 완성하기: 심화 학습 가이드

## 1. 객체지향 프로그래밍 심화

### 1.1 클래스와 상속

#### 기본 클래스 정의
```dart
class Person {
  String name;
  int age;
  
  // 생성자
  Person(this.name, this.age);
  
  // 명명된 생성자
  Person.guest() {
    name = '손님';
    age = 0;
  }
  
  void introduce() {
    print('안녕하세요, 저는 $name이고 $age살입니다.');
  }
}
```

#### 상속과 메서드 오버라이드
```dart
class Student extends Person {
  String school;
  
  Student(String name, int age, this.school) : super(name, age);
  
  @override
  void introduce() {
    print('안녕하세요, 저는 $school에 다니는 $name이고 $age살입니다.');
  }
  
  // 추가 메서드
  void study() {
    print('$name이(가) 공부하고 있습니다.');
  }
}
```

#### 추상 클래스
```dart
abstract class Shape {
  double calculateArea();
  double calculatePerimeter();
}

class Circle extends Shape {
  double radius;
  
  Circle(this.radius);
  
  @override
  double calculateArea() {
    return 3.14 * radius * radius;
  }
  
  @override
  double calculatePerimeter() {
    return 2 * 3.14 * radius;
  }
}
```

#### 실습 문제: 도형 계층
1. `Shape` 추상 클래스를 상속받는 `Rectangle`, `Triangle` 클래스를 구현하세요.
2. 각 클래스는 적절한 생성자와 면적/둘레 계산 메서드를 가져야 합니다.
3. 다양한 도형 객체들을 담는 리스트를 만들고 각 도형의 면적을 출력하세요.

### 1.2 인터페이스와 믹스인

#### 인터페이스 (Dart에서는 암시적)
```dart
class Flyable {
  void fly() {
    print('날고 있습니다.');
  }
}

// implements를 통한 인터페이스 구현
class Bird implements Flyable {
  @override
  void fly() {
    print('새가 날개로 날고 있습니다.');
  }
}

class Airplane implements Flyable {
  @override
  void fly() {
    print('비행기가 엔진으로 날고 있습니다.');
  }
}
```

#### 믹스인 (with 키워드)
```dart
mixin Logger {
  void log(String message) {
    print('로그: $message');
  }
}

mixin Validator {
  bool validate(String value) {
    return value.isNotEmpty;
  }
}

class DataProcessor with Logger, Validator {
  void process(String data) {
    if (validate(data)) {
      log('데이터 처리 시작: $data');
      // 데이터 처리 로직
      log('데이터 처리 완료');
    } else {
      log('유효하지 않은 데이터');
    }
  }
}
```

#### 실습 문제: 동물 시뮬레이션
1. `Animal` 기본 클래스를 만들고, `Walkable`, `Swimmable`, `Flyable` 믹스인을 정의하세요.
2. 다양한 동물 클래스(`Dog`, `Fish`, `Bird`, `Duck`)를 만들고 적절한 믹스인을 적용하세요.
3. 각 동물 인스턴스를 생성하고 해당 능력을 테스트하세요.

### 1.3 제네릭 타입

#### 기본 제네릭 클래스
```dart
class Box<T> {
  T value;
  
  Box(this.value);
  
  T getValue() {
    return value;
  }
  
  void setValue(T newValue) {
    value = newValue;
  }
}
```

#### 제네릭 함수
```dart
T first<T>(List<T> items) {
  if (items.isEmpty) {
    throw Exception('리스트가 비어있습니다.');
  }
  return items.first;
}

// 타입 제한
void printNumbers<T extends num>(List<T> numbers) {
  for (var number in numbers) {
    print(number);
  }
}
```

#### 제네릭 타입의 활용
```dart
class Pair<K, V> {
  K key;
  V value;
  
  Pair(this.key, this.value);
  
  @override
  String toString() {
    return '[$key: $value]';
  }
}

// 사용 예
var stringIntPair = Pair<String, int>('age', 30);
var doubleBoolPair = Pair<double, bool>(3.14, true);
```

#### 실습 문제: 데이터 구조 구현
1. 제네릭 `Stack<T>` 클래스를 구현하세요 (push, pop, peek, isEmpty 메서드 포함).
2. 제네릭 `Queue<T>` 클래스를 구현하세요 (enqueue, dequeue, peek, isEmpty 메서드 포함).
3. 각 데이터 구조에 다양한 타입의 데이터를 추가하고 꺼내보세요.

### 1.4 접근 제어자와 캡슐화

#### 프라이빗 멤버 (언더스코어로 시작)
```dart
class BankAccount {
  String _accountNumber;
  double _balance = 0;
  
  BankAccount(this._accountNumber);
  
  // 게터
  String get accountNumber => _accountNumber;
  double get balance => _balance;
  
  // 세터 (유효성 검증 포함)
  set balance(double amount) {
    if (amount < 0) {
      throw Exception('잔액은 0보다 작을 수 없습니다.');
    }
    _balance = amount;
  }
  
  void deposit(double amount) {
    if (amount <= 0) {
      throw Exception('입금액은 양수여야 합니다.');
    }
    _balance += amount;
  }
  
  bool withdraw(double amount) {
    if (amount <= 0) {
      throw Exception('출금액은 양수여야 합니다.');
    }
    if (_balance < amount) {
      return false;
    }
    _balance -= amount;
    return true;
  }
}
```

#### 불변 객체와 final 필드
```dart
class ImmutablePoint {
  final int x;
  final int y;
  
  // 생성자에서만 초기화 가능
  const ImmutablePoint(this.x, this.y);
  
  // 불변 객체에는 연산자를 제공하는 것이 좋습니다
  ImmutablePoint operator +(ImmutablePoint other) {
    return ImmutablePoint(x + other.x, y + other.y);
  }
  
  @override
  String toString() {
    return 'Point($x, $y)';
  }
}
```

#### 실습 문제: 학생 관리 시스템
1. `Student` 클래스를 만들고 이름, 학번, 성적 등의 속성을 캡슐화하세요.
2. 유효한 값만 설정할 수 있도록 세터에 유효성 검증을 추가하세요.
3. 성적을 추가하고 평균을 계산하는 메서드를 구현하세요.
4. 여러 학생 객체를 생성하고 정보를 수정/조회해보세요.

## 2. 비동기 프로그래밍

### 2.1 Future와 async/await

#### Future 기본
```dart
Future<String> fetchUserData() {
  return Future.delayed(Duration(seconds: 2), () {
    return '{"name": "홍길동", "email": "hong@example.com"}';
  });
}

void main() {
  print('데이터 요청 시작...');
  
  // Future 체이닝
  fetchUserData()
    .then((data) {
      print('데이터 수신: $data');
      return data.length;
    })
    .then((length) {
      print('데이터 길이: $length');
    })
    .catchError((error) {
      print('오류 발생: $error');
    })
    .whenComplete(() {
      print('작업 완료');
    });
  
  print('다른 작업 진행 중...');
}
```

#### async/await 사용
```dart
Future<void> processUserData() async {
  try {
    print('데이터 요청 시작...');
    
    // await로 Future 완료 대기
    String data = await fetchUserData();
    print('데이터 수신: $data');
    
    int length = data.length;
    print('데이터 길이: $length');
    
  } catch (error) {
    print('오류 발생: $error');
  } finally {
    print('작업 완료');
  }
}

void main() async {
  await processUserData();
  print('모든 작업 완료');
}
```

#### Future 유틸리티 메서드
```dart
Future<List<String>> fetchMultipleData() async {
  // 병렬로 여러 Future 실행
  List<Future<String>> futures = [
    fetchData('사용자1'),
    fetchData('사용자2'),
    fetchData('사용자3'),
  ];
  
  // 모든 Future가 완료될 때까지 대기
  List<String> results = await Future.wait(futures);
  return results;
}

// 최초 완료되는 Future 결과 사용
Future<String> fetchFastest() async {
  List<Future<String>> futures = [
    fetchWithDelay('서버1', 3),
    fetchWithDelay('서버2', 1),
    fetchWithDelay('서버3', 2),
  ];
  
  String result = await Future.any(futures);
  return result;
}
```

#### 실습 문제: 비동기 데이터 처리
1. 여러 개의 가상 API 호출 함수(`fetchUserProfile()`, `fetchUserPosts()`, `fetchUserFollowers()`)를 구현하세요.
2. `async`/`await`를 사용해 이 데이터를 순차적으로 가져오는 함수를 작성하세요.
3. `Future.wait`를 사용해 병렬로 데이터를 가져오는 함수를 작성하고 성능을 비교하세요.

### 2.2 Stream과 StreamController

#### 기본 Stream 생성과 사용
```dart
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}

void main() async {
  // Stream 구독
  Stream<int> stream = countStream(5);
  
  // await for 사용
  await for (int number in stream) {
    print('숫자: $number');
  }
  
  print('카운트 완료');
}
```

#### StreamController 사용
```dart
import 'dart:async';

class DataService {
  final StreamController<String> _dataController = StreamController<String>.broadcast();
  
  // Stream 접근자 제공
  Stream<String> get dataStream => _dataController.stream;
  
  // 데이터 추가
  void addData(String data) {
    _dataController.sink.add(data);
  }
  
  // 오류 전달
  void addError(Object error) {
    _dataController.sink.addError(error);
  }
  
  // 리소스 해제
  void dispose() {
    _dataController.close();
  }
}

void main() {
  final service = DataService();
  
  // 첫 번째 구독자
  final subscription1 = service.dataStream.listen(
    (data) => print('구독자1: $data'),
    onError: (error) => print('구독자1 오류: $error'),
    onDone: () => print('구독자1: 완료')
  );
  
  // 두 번째 구독자
  final subscription2 = service.dataStream.listen(
    (data) => print('구독자2: $data'),
    onError: (error) => print('구독자2 오류: $error'),
    onDone: () => print('구독자2: 완료')
  );
  
  // 데이터 추가
  service.addData('첫 번째 메시지');
  service.addData('두 번째 메시지');
  service.addError('네트워크 오류');
  service.addData('세 번째 메시지');
  
  // 5초 후 구독 취소 및 리소스 해제
  Future.delayed(Duration(seconds: 5), () {
    subscription1.cancel();
    subscription2.cancel();
    service.dispose();
  });
}
```

#### Stream 변환과 조작
```dart
void streamTransformations() async {
  Stream<int> numberStream = Stream.fromIterable([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);
  
  // 필터링
  Stream<int> evenNumbers = numberStream.where((number) => number % 2 == 0);
  
  // 변환
  Stream<String> numberStrings = evenNumbers.map((number) => '숫자: $number');
  
  // 데이터 변환 (평탄화)
  Stream<int> expandedStream = Stream.fromIterable([1, 2, 3])
      .asyncExpand((number) => 
          Stream.fromIterable([number, number * 10, number * 100]));
  
  // 데이터 결합
  Stream<int> combinedStream = 
      Stream.fromIterable([1, 2, 3]).asBroadcastStream();
  
  combinedStream.take(2).listen((data) => print('처음 2개: $data'));
  combinedStream.skip(1).listen((data) => print('첫 번째 이후: $data'));
}
```

#### 실습 문제: 실시간 데이터 모니터링
1. 온도 센서를 시뮬레이션하는 `TemperatureSensor` 클래스를 구현하세요 (StreamController 사용).
2. 온도가 특정 임계값을 초과하면 경고를 발생시키는 로직을 추가하세요.
3. 여러 구독자가 온도 데이터를 다른 방식으로 처리하도록 구현하세요 (예: 로깅, 평균 계산, 경고 표시).

### 2.3 Isolate를 통한 병렬 처리

#### 기본 Isolate 생성
```dart
import 'dart:isolate';

// Isolate에서 실행될 함수
void heavyComputation(SendPort sendPort) {
  int sum = 0;
  for (int i = 0; i < 1000000000; i++) {
    sum += i;
  }
  sendPort.send(sum);
}

Future<void> main() async {
  print('메인 Isolate 시작');
  
  // 통신을 위한 포트 생성
  final receivePort = ReceivePort();
  
  // 새 Isolate 생성
  await Isolate.spawn(heavyComputation, receivePort.sendPort);
  
  // 결과 수신
  final result = await receivePort.first;
  print('계산 결과: $result');
  
  print('메인 Isolate 종료');
}
```

#### 양방향 통신
```dart
import 'dart:isolate';

void computeFactorial(List<dynamic> args) {
  final SendPort sendPort = args[0];
  final int number = args[1];
  
  // 양방향 통신을 위한 ReceivePort 생성
  final receivePort = ReceivePort();
  sendPort.send(receivePort.sendPort);
  
  // 메시지 수신 대기
  receivePort.listen((message) {
    if (message is int) {
      int factorial = _calculateFactorial(message);
      sendPort.send(factorial);
    } else if (message == 'close') {
      receivePort.close();
    }
  });
  
  // 초기 요청 처리
  int factorial = _calculateFactorial(number);
  sendPort.send(factorial);
}

int _calculateFactorial(int n) {
  if (n <= 1) return 1;
  return n * _calculateFactorial(n - 1);
}

Future<void> main() async {
  final mainReceivePort = ReceivePort();
  await Isolate.spawn(computeFactorial, [mainReceivePort.sendPort, 10]);
  
  // Isolate로부터 SendPort 수신
  final SendPort isolateSendPort = await mainReceivePort.first;
  
  // 추가 계산 요청을 위한 ReceivePort
  final responseReceivePort = ReceivePort();
  
  // Isolate에 계산 요청 보내기
  isolateSendPort.send(responseReceivePort.sendPort);
  
  // 초기 계산 결과 수신
  final firstResult = await responseReceivePort.first;
  print('팩토리얼(10) = $firstResult');
  
  // 추가 계산 요청
  isolateSendPort.send(15);
  final secondResult = await responseReceivePort.first;
  print('팩토리얼(15) = $secondResult');
  
  // 정리
  isolateSendPort.send('close');
  mainReceivePort.close();
  responseReceivePort.close();
}
```

#### compute 함수 활용 (Flutter)
```dart
import 'package:flutter/foundation.dart';

// 무거운 계산 함수
int fibonacci(int n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

Future<void> main() async {
  print('계산 시작...');
  
  // Isolate에서 계산 실행
  final result = await compute(fibonacci, 40);
  
  print('피보나치(40) = $result');
  print('계산 완료');
}
```

#### 실습 문제: 이미지 처리 시뮬레이션
1. 픽셀 배열로 표현된 이미지 데이터를 처리하는 함수를 구현하세요 (예: 그레이스케일 변환).
2. Isolate를 사용해 대용량 이미지를 병렬로 처리하는 클래스를 만드세요.
3. 이미지를 여러 구역으로 나누어 여러 Isolate에서 동시에 처리한 후 결과를 합치는 로직을 구현하세요.

## 3. 함수형 프로그래밍

### 3.1 일급 함수 개념

#### 함수를 변수에 할당
```dart
void main() {
  // 함수를 변수에 할당
  var greet = (String name) => 'Hello, $name!';
  print(greet('홍길동'));  // 출력: Hello, 홍길동!
  
  // 함수 타입 명시
  String Function(String) welcome = (name) => 'Welcome, $name!';
  print(welcome('김철수'));  // 출력: Welcome, 김철수!
  
  // 기존 함수 참조
  int add(int a, int b) => a + b;
  var calculator = add;
  print(calculator(5, 3));  // 출력: 8
}
```

#### 함수를 인자로 전달
```dart
void processNumbers(List<int> numbers, bool Function(int) filter) {
  for (var number in numbers) {
    if (filter(number)) {
      print('통과: $number');
    }
  }
}

void main() {
  final numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  
  // 익명 함수를 인자로 전달
  processNumbers(numbers, (number) => number % 2 == 0);
  
  // 명명된 함수를 인자로 전달
  bool isOdd(int n) => n % 2 == 1;
  processNumbers(numbers, isOdd);
  
  // 함수 리터럴 직접 전달
  processNumbers(
    numbers,
    (n) => n > 5 && n < 9
  );
}
```

#### 함수를 반환하는 함수
```dart
Function makeMultiplier(int factor) {
  // 클로저 반환
  return (int number) => number * factor;
}

Function makeAdder(int addBy) {
  return (int value) => value + addBy;
}

void main() {
  var doubler = makeMultiplier(2);
  var tripler = makeMultiplier(3);
  
  print(doubler(5));  // 출력: 10
  print(tripler(5));  // 출력: 15
  
  var addFive = makeAdder(5);
  var addTen = makeAdder(10);
  
  print(addFive(3));  // 출력: 8
  print(addTen(3));   // 출력: 13
}
```

#### 실습 문제: 함수 합성
1. 두 함수를 합성하는 `compose` 함수를 구현하세요 (f(g(x)) 형태).
2. 여러 함수를 순차적으로 적용하는 `pipe` 함수를 구현하세요.
3. 이 함수들을 사용해 문자열 처리 파이프라인을 만들어보세요 (소문자 변환 → 공백 제거 → 길이 계산).

### 3.2 람다식과 클로저

#### 람다식 (익명 함수)
```dart
void main() {
  // 한 줄 람다식
  var add = (int a, int b) => a + b;
  
  // 여러 줄 람다식
  var calculate = (int a, int b, String operation) {
    switch (operation) {
      case 'add': return a + b;
      case 'subtract': return a - b;
      case 'multiply': return a * b;
      case 'divide': return a / b;
      default: throw Exception('알 수 없는 연산');
    }
  };
  
  print(add(5, 3));  // 출력: 8
  print(calculate(10, 5, 'multiply'));  // 출력: 50
}
```

#### 클로저 (Closures)
```dart
Function counter() {
  int count = 0;
  
  // 클로저: 외부 변수 count를 캡처
  return () {
    count += 1;
    return count;
  };
}

void main() {
  var increment = counter();
  
  print(increment());  // 출력: 1
  print(increment());  // 출력: 2
  print(increment());  // 출력: 3
  
  // 새로운 카운터 인스턴스
  var increment2 = counter();
  print(increment2());  // 출력: 1 (별도의 count 변수를 가짐)
}
```

#### 클로저를 사용한 메모이제이션
```dart
Function memoize(Function fn) {
  Map<String, dynamic> cache = {};
  
  return (dynamic arg) {
    String key = arg.toString();
    if (cache.containsKey(key)) {
      print('캐시에서 가져옴: $key');
      return cache[key];
    }
    
    print('계산 중: $key');
    var result = fn(arg);
    cache[key] = result;
    return result;
  };
}

int fibonacci(int n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

void main() {
  var memoizedFib = memoize((int n) {
    if (n <= 1) return n;
    return memoizedFib(n - 1) + memoizedFib(n - 2);
  });
  
  print(memoizedFib(6));  // 계산 및 캐싱
  print(memoizedFib(6));  // 캐시에서 가져옴
}
```

#### 실습 문제: 토글 함수
1. 호출할 때마다 두 상태(true/false) 사이를 전환하는 `toggle` 함수를 클로저로 구현하세요.
2. 여러 가지 모드를 순환하는 `cycleMode` 함수를 구현하세요 (예: 'light' → 'dark' → 'auto' → 'light'...).
3. 제한된 횟수만 호출할 수 있는 `limitedCall` 함수 래퍼를 구현하세요.

### 3.3 컬렉션 연산 (map, filter, reduce)

#### map 활용
```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  // 각 요소 제곱
  List<int> squared = numbers.map((number) => number * number).toList();
  print(squared);  // 출력: [1, 4, 9, 16, 25]
  
  // 객체에서 특정 속성 추출
  List<Map<String, dynamic>> users = [
    {'id': 1, 'name': '홍길동', 'age': 30},
    {'id': 2, 'name': '김철수', 'age': 25},
    {'id': 3, 'name': '이영희', 'age': 28}
  ];
  
  List<String> names = users.map((user) => user['name'] as String).toList();
  print(names);  // 출력: [홍길동, 김철수, 이영희]
}
```

#### where (filter) 활용
```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  
  // 짝수 필터링
  List<int> evens = numbers.where((number) => number % 2 == 0).toList();
  print(evens);  // 출력: [2, 4, 6, 8, 10]
  
  // 복합 조건 필터링
  List<Map<String, dynamic>> products = [
    {'id': 1, 'name': '노트북', 'price': 1200000, 'inStock': true},
    {'id': 2, 'name': '스마트폰', 'price': 800000, 'inStock': true},
    {'id': 3, 'name': '태블릿', 'price': 600000, 'inStock': false},
    {'id': 4, 'name': '이어폰', 'price': 200000, 'inStock': true}
  ];
  
  var availableExpensiveProducts = products
      .where((product) => product['inStock'] == true && product['price'] > 500000)
      .toList();
  
  print(availableExpensiveProducts); 
  // 출력: [{id: 1, name: 노트북, price: 1200000, inStock: true}, {id: 2, name: 스마트폰, price: 800000, inStock: true}]
}
```

#### reduce와 fold 활용
```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  // reduce로 합계 계산
  int sum = numbers.reduce((value, element) => value + element);
  print('합계: $sum');  // 출력: 합계: 15
  
  // reduce로 최대값 찾기
  int max = numbers.reduce((current, next) => current > next ? current : next);
  print('최대값: $max');  // 출력: 최대값: 5
  
  // fold로 시작값 지정
  int sumWithInitial = numbers.fold(10, (sum, element) => sum + element);
  print('초기값 포함 합계: $sumWithInitial');  // 출력: 초기값 포함 합계: 25
