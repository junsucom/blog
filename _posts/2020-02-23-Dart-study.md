---
layout: post
title: Dart Study
date: '2020-02-23 12:00:00'
categories: dart
---
# Dart

[https://dart.dev/guides/language/language-tour](https://dart.dev/guides/language/language-tour)

The new keyword became optional in Dart 2.

Unlike Java, Dart doesn’t have the keywords public, protected, and private. If an identifier starts with an underscore (_), it’s private to its library.

Built-in types

The Dart language has special support for the following types:

- numbers
- strings
- booleans
- lists (also known as *arrays*)

    ```dart
    var list = [1, 2, 3];
    var list2 = [0, ...list]; //Dart 2.3 부터 지원
    assert(list2.length == 4);

    var list;
    var list2 = [0, ...?list]; //Dart 2.3 부터 지원
    assert(list2.length == 1);

    var listOfInts = [1, 2, 3];
    var listOfStrings = [
      '#0',
      for (var i in listOfInts) '#$i'
    ];
    assert(listOfStrings[1] == '#1');
    ```

---
### sets

    ```dart
    var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
    var elements = <String>{};
    elements.add('fluorine');
    elements.addAll(halogens);
    ```

---
### maps

    ```dart
    var nobleGases = Map();
    nobleGases[2] = 'helium';
    nobleGases[10] = 'neon';
    nobleGases[18] = 'argon';

    var gifts = {'first': 'partridge'};
    gifts['fourth'] = 'calling birds'; // Add a key-value pair
    ```

---
### runes (for expressing Unicode characters in a string)

    ```dart
    var rune = 'Hi 🇩🇰';
    ```

---
### symbols

    ```dart
    assert(Symbol("foo") == #foo);
    assert(identical(const Symbol("foo"), #foo));
    ```
---
### Functions Basic

```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}

isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}

bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```

---
### Optional parameters

```dart
//호출시에 param 값 지정 필요
void enableFlags({bool bold, bool hidden}) {...} 
enableFlags();
enableFlags(bold: true, hidden: false);

const Scrollbar({Key key, @required Widget child})//child 는 필수

//호출시에 3번째 param 값을 꼭 넣을 필요 없음
String say(String from, String msg, [String device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}

//기본 param 지정
void doStuff(
    {List<int> list = const [1, 2, 3],
    Map<String, String> gifts = const {
      'first': 'paper',
      'second': 'cotton',
      'third': 'leather'
    }}) {
  print('list:  $list');
  print('gifts: $gifts');
}
```

---
### Lexical closures (어휘 폐쇄?)

```dart
Function makeAdder(num addBy) {
  return (num i) => addBy + i;
}

void main() {
  // Create a function that adds 2.
  var add2 = makeAdder(2);

  // Create a function that adds 4.
  var add4 = makeAdder(4);

  assert(add2(3) == 5);
  assert(add4(3) == 7);
}
```

---
### Operators

```dart
// ~/ : 나누고 정수 반환
assert(5 ~/ 2 == 2);

//?? : null 처리
String playerName([String name]) => name ?? 'Guest';
var a = 1, b;
print(a ??= 2); //1
print(b ??= 2); //2
print(playerName()); //Guest
```

---
### Cascade notation (..)

```dart
final addressBook = (AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();

//실제 객체를 반환하는 함수에만 사용 가능.
var sb = StringBuffer();
sb.write('foo')
  ..write('bar'); // Error: method 'write' isn't defined for 'void'.
```

---
### Conditional member access (?.) : 코틀린과 동일

```dart
foo?.bar
```

---
### Catch

```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e, s) {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}
```

```dart
//rethrow
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // Runtime error
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    rethrow; // Allow callers to see the exception.
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  } finally {
    cleanLlamaStalls(); // Then clean up.
  }
}
```

---
### identical : Check whether two references are to the same object.

```dart
var a = const ImmutablePoint(1, 1); // Creates a constant
var b = ImmutablePoint(1, 1); // Does NOT create a constant

assert(!identical(a, b)); // NOT the same instance!
```

---
### class basic

```dart
class Person {
  String firstName;
  
  Person(String firstName)
    : firstName = firstName;
	// same Person(this.firstName);
  // final 값 설정 가능

  Person.fromJson(Map data) {
    firstName = data["firstName"];
  }
  Person.origin() {
    firstName = "origin";
  }
}

main() {
  var p1 = Person("constructor");
  var p2 = Person.fromJson({"firstName":"json"});
  var p3 = Person.fromJson({"error":"error"});
  var p4 = Person.origin();
  
  
  print(p1.firstName);  //constructor
  print(p2.firstName);  //json
  print(p3.firstName);  //null
  print(p4.firstName);  //origin
}
```

---
### Abstract classes : java 와 다른거 없음..

```dart
abstract class AbstractContainer {
  // Define constructors, fields, methods...
  void updateChildren(); // Abstract method.
}
```

---
### Interfaces : 그냥 class 를 써버리네..

```dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy'))); //Hello, Bob. I am Kathy.
  print(greetBob(Impostor())); //Hi Bob. Do you know who I am?
}
```

---
### mixin : 멀티 상속?

```dart
class A {
  String getMessage() => 'A';
  String getA() => 'a';
}

class B {
  String getMessage() => 'B';
  String getB() => 'b';
}

class P {
  String getMessage() => 'P';
  String getP() => 'p';
}

class AB extends P with A, B {}

class BA extends P with B, A {}

void main() {
  AB ab = AB();
  BA ba = BA();
  print(ab.getMessage());//B
  print(ba.getMessage());//A
  print(ab.getA() + ab.getB() + ab.getP());//abp
}
```

---
### factory

```dart
class MyClass {
  //멤버는 팩토리 생성자에서 접근할 수 없으므로 static 키워드를 붙여 준다.
  static final MyClass _singleton = MyClass._internal();

  //factory 키워드는 생성자가 리턴을 할 수 있도록 해준다.
  factory MyClass() {
    return _singleton;
  }

  MyClass._internal() {}
}

main(List<String> args) {
  MyClass myObj1 = MyClass();
  MyClass myObj2 = MyClass();
  print(identical(myObj1, myObj2)); //결과 true
}
```

---
### generic : java 와 다른것 없다.

```dart
T first<T>(List<T> ts) {
  // Do some initial work or error checking, then...
  T tmp = ts[0];
  // Do some additional checking or processing...
  return tmp;
}
```

---
### import

```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Uses Element from lib1.
Element element1 = Element();
// Uses Element from lib2.
lib2.Element element2 = lib2.Element();

// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;

// Lazily loading a library
import 'package:greetings/hello.dart' deferred as hello;
Future greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

---
### part of : dart 파일을 선언한 파일의 일부분으로  선언 시킴. 하지만 아래의 export 의 사용을 추천

```dart
//a.dart 파일만 import 하면 class B 도 사용 할 수 있다.
part of 'a.dart';
class B {
}
```

---
### export : 여러 dart 파일의 하나의 dart 파일로 묶어 준다.

```dart
export 'src/cascade.dart';
export 'src/handler.dart';
export 'src/handlers/logger.dart';
```

---
### await : 비동기 

Future, async

Stream, async*

```dart
import 'dart:async';

void sumIterable(Iterable<int> iterable) {
  var sum = 0;
  for (var value in iterable) {
    sum += value;
    print("sumIterable $sum");
  }
  
  print("sumIterable result:$sum");
}

Iterable<int> makeIterable(int to) sync* {
  for (int i = 1; i <= to; i++) {
    print("makeIterable $i");
    yield i;
  }
}

void sumStream(Stream<int> stream) async {
  var sum = 0;
  await for (var value in stream) {
    sum += value;
    print("sumStream $sum");
  }
  
  print("sumStream result:$sum");
}

Stream<int> makeStream(int to) async* {
  for (int i = 1; i <= to; i++) {
    print("makeStream $i");
    await Future.delayed(const Duration(milliseconds: 100));
    yield i;
  }
}

main() async {
  print("main start");
  print("\nmain makeIterable");
  var iterable = makeIterable(3);
  print("\nmain sumIterable");
  sumIterable(iterable);
  
  print("\nmain makeStream");
  var stream = makeStream(3);
  print("\nmain sumStream");
  sumStream(stream);
  print("\nmain end");
}

/*===== Console =====
main start

main makeIterable

main sumIterable
makeIterable 1
sumIterable 1
makeIterable 2
sumIterable 3
makeIterable 3
sumIterable 6
sumIterable result:6

main makeStream

main sumStream

main end
makeStream 1
sumStream 1
makeStream 2
sumStream 3
makeStream 3
sumStream 6
sumStream result:6
===== Console =====*/
```

```dart
import 'dart:async';

void sumFuture(Future<List> future) async {
  var sum = 0;
  var list = await future;
  for (var value in list) {
    sum += value;
    print("sumFuture $sum");
  }
  
  print("sumFuture result:$sum");
}

Future<List> makeFuture(int to) async {
  var list = List(to);
  
  for (int i = 1; i <= to; i++) {
    print("makeFuture $i");
    await Future.delayed(const Duration(milliseconds: 100));
    list[i-1] = i;
  }
  return Future.value(list);
}

main() async {
  print("main start");
  print("\nmain makeFuture");
  var future = makeFuture(3);
  print("\nmain sumFuture");
  sumFuture(future);
  print("\nmain end");
}

/*===== Console =====
main start

main makeFuture
makeFuture 1

main sumFuture

main end
makeFuture 2
makeFuture 3
sumFuture 1
sumFuture 3
sumFuture 6
sumFuture result:6
===== Console =====*/
```

yield, yield*

```dart
import 'dart:async';

main() async {
  await for (int i in numbersDownFrom(5)) {
    print('$i bottles of beer');
  }
}

Stream numbersDownFrom(int n) async* {
  if (n >= 0) {
    await Future.delayed(Duration(milliseconds: 100));
    yield n;
    yield* numbersDownFrom(n - 1);
  }
}

/*===== Console =====
5 bottles of beer
4 bottles of beer
3 bottles of beer
2 bottles of beer
1 bottles of beer
0 bottles of beer
===== Console =====*/
```

FutureOr

---
### Design

[https://dart.dev/guides/language/effective-dart/design](https://dart.dev/guides/language/effective-dart/design)
