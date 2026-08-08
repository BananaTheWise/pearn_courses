# Dart Course: Files 9 &amp; 10 - Generics, Collections, and Advanced Topics

This combined file covers **Generics, Collections, and Advanced Dart Topics** - the final comprehensive chapters of our Dart course.

---

# 📚 Chapter 9: Generics and Collections

## 9.1 Generics

Generics allow you to write flexible, reusable code that can work with different types while maintaining type safety.

### 9.1.1 Why Use Generics?

```dart
// Without generics - loses type information
List list = [1, 2, 3];
list.add('string'); // This is allowed but wrong!

// With generics - type safe
List<int> intList = [1, 2, 3];
// intList.add('string'); // Compile-time error
```

### 9.1.2 Generic Classes

```dart
class Box<T> {
  T value;
  
  Box(this.value);
  
  T getValue() => value;
  void setValue(T newValue) => value = newValue;
}

void main() {
  Box<int> intBox = Box(42);
  Box<String> stringBox = Box('Hello');
  
  print(intBox.getValue());    // 42
  print(stringBox.getValue()); // Hello
}
```

### 9.1.3 Generic Functions

```dart
T first<T>(List<T> list) {
  return list.first;
}

void main() {
  List<int> numbers = [1, 2, 3];
  List<String> strings = ['a', 'b', 'c'];
  
  int firstNumber = first(numbers);
  String firstString = first(strings);
  
  print(firstNumber); // 1
  print(firstString); // a
}
```

### 9.1.4 Generic Methods

```dart
class Utils {
  static T max<T extends num>(T a, T b) {
    return a > b ? a : b;
  }
}

void main() {
  print(Utils.max(5, 10));      // 10
  print(Utils.max(3.14, 2.71)); // 3.14
}
```

### 9.1.5 Bounded Generics

Restrict the types that can be used with generics.

```dart
// T must extend num
class NumericPair<T extends num> {
  T first;
  T second;
  
  NumericPair(this.first, this.second);
  
  T getSum() => first + second;
}

void main() {
  NumericPair<int> intPair = NumericPair(5, 10);
  NumericPair<double> doublePair = NumericPair(3.14, 2.71);
  
  print(intPair.getSum());    // 15
  print(doublePair.getSum()); // 5.85
  
  // NumericPair<String> stringPair = NumericPair('a', 'b'); // ERROR
}
```

### 9.1.6 Generic Interfaces

```dart
abstract class Repository<T> {
  Future<List<T>> getAll();
  Future<T?> getById(int id);
  Future<void> save(T item);
  Future<void> delete(int id);
}

class UserRepository implements Repository<User> {
  @override
  Future<List<User>> getAll() async => [];
  
  @override
  Future<User?> getById(int id) async => null;
  
  @override
  Future<void> save(User item) async {}
  
  @override
  Future<void> delete(int id) async {}
}
```

### 9.1.7 Generic Extensions

```dart
extension ListExtensions<T> on List<T> {
  T? findFirst(bool Function(T) test) {
    for (T item in this) {
      if (test(item)) return item;
    }
    return null;
  }
}

void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  int? even = numbers.findFirst((n) => n % 2 == 0);
  print(even); // 2
}
```

## 9.2 Collections in Depth

### 9.2.1 Lists

#### Creating Lists:

```dart
List<int> numbers = [1, 2, 3];
var names = ['Ahmed', 'Badr'];
List<double> emptyList = [];
List<dynamic> mixed = [1, 'text', true];
```

#### List Methods:

```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  // Adding elements
  numbers.add(6);              // [1, 2, 3, 4, 5, 6]
  numbers.addAll([7, 8]);      // [1, 2, 3, 4, 5, 6, 7, 8]
  numbers.insert(0, 0);         // [0, 1, 2, 3, 4, 5, 6, 7, 8]
  
  // Removing elements
  numbers.remove(0);           // [1, 2, 3, 4, 5, 6, 7, 8]
  numbers.removeAt(0);         // [2, 3, 4, 5, 6, 7, 8]
  numbers.removeLast();        // [2, 3, 4, 5, 6, 7]
  
  // Accessing elements
  int first = numbers.first;  // 2
  int last = numbers.last;    // 7
  int atIndex = numbers[1];   // 3
  
  // Sublists
  List<int> sublist = numbers.sublist(1, 3); // [3, 4]
  
  // Searching
  bool contains = numbers.contains(3); // true
  int index = numbers.indexOf(4);      // 2
  
  // Sorting
  numbers.sort();                      // [2, 3, 4, 5, 6, 7]
  numbers.sort((a, b) => b.compareTo(a)); // [7, 6, 5, 4, 3, 2]
  
  // Functional operations
  List<int> doubled = numbers.map((n) => n * 2).toList();
  List<int> evens = numbers.where((n) => n % 2 == 0).toList();
  int sum = numbers.reduce((a, b) => a + b);
  bool allPositive = numbers.every((n) => n > 0);
  bool hasEven = numbers.any((n) => n % 2 == 0);
}
```

### 9.2.2 Sets

#### Creating Sets:

```dart
Set<int> numbers = {1, 2, 3};
var names = {'Ahmed', 'Badr', 'Ahmed'}; // {'Ahmed', 'Badr'}
Set<String> emptySet = {};
```

#### Set Operations:

```dart
void main() {
  Set<int> a = {1, 2, 3};
  Set<int> b = {3, 4, 5};
  
  // Union
  Set<int> union = a.union(b); // {1, 2, 3, 4, 5}
  
  // Intersection
  Set<int> intersection = a.intersection(b); // {3}
  
  // Difference
  Set<int> difference = a.difference(b); // {1, 2}
  
  // Subset
  bool isSubset = a.containsAll(b); // false
  
  // Adding/Removing
  a.add(4);       // {1, 2, 3, 4}
  a.addAll({5, 6}); // {1, 2, 3, 4, 5, 6}
  a.remove(1);     // {2, 3, 4, 5, 6}
  a.clear();       // {}
}
```

### 9.2.3 Maps

#### Creating Maps:

```dart
Map<String, int> ages = {'Ahmed': 25, 'Badr': 30};
var user = {'name': 'Ahmed', 'age': 25};
Map<String, dynamic> emptyMap = {};
```

#### Map Methods:

```dart
void main() {
  Map<String, int> ages = {'Ahmed': 25, 'Badr': 30};
  
  // Accessing
  int? age = ages['Ahmed']; // 25
  int ageWithDefault = ages['Unknown'] ?? 0; // 0
  
  // Adding/Updating
  ages['Mohamed'] = 22;      // {'Ahmed': 25, 'Badr': 30, 'Mohamed': 22}
  ages['Ahmed'] = 26;        // {'Ahmed': 26, 'Badr': 30, 'Mohamed': 22}
  
  // Removing
  ages.remove('Badr');       // {'Ahmed': 26, 'Mohamed': 22}
  
  // Checking
  bool hasKey = ages.containsKey('Ahmed'); // true
  bool hasValue = ages.containsValue(22);   // true
  
  // Iterating
  ages.forEach((key, value) => print('$key: $value'));
  
  // Keys and Values
  Iterable<String> keys = ages.keys;
  Iterable<int> values = ages.values;
  
  // Length
  int length = ages.length;
  
  // Clearing
  ages.clear();
}
```

### 9.2.4 Iterables

#### Creating Iterables:

```dart
Iterable<int> numbers = [1, 2, 3];
Iterable<int> range = Iterable.generate(5, (i) => i); // [0, 1, 2, 3, 4]
```

#### Iterable Methods:

```dart
void main() {
  Iterable<int> numbers = [1, 2, 3, 4, 5];
  
  // Converting
  List<int> list = numbers.toList();
  Set<int> set = numbers.toSet();
  
  // Mapping
  Iterable<int> doubled = numbers.map((n) => n * 2);
  
  // Filtering
  Iterable<int> evens = numbers.where((n) => n % 2 == 0);
  Iterable<int> firstThree = numbers.take(3);
  Iterable<int> skipFirstTwo = numbers.skip(2);
  
  // Aggregating
  int sum = numbers.reduce((a, b) => a + b);
  int product = numbers.fold(1, (a, b) => a * b);
  
  // Checking
  bool allPositive = numbers.every((n) => n > 0);
  bool hasEven = numbers.any((n) => n % 2 == 0);
  bool isEmpty = numbers.isEmpty;
  
  // Expanding
  Iterable<int> expanded = numbers.expand((n) => [n, n * 2]);
  // [1, 2, 2, 4, 3, 6, 4, 8, 5, 10]
}
```

### 9.2.5 Collection Methods

#### Common Methods Across Collections:

```dart
void main() {
  List<int> list = [1, 2, 3];
  Set<int> set = {1, 2, 3};
  
  // Adding elements
  list.add(4);
  set.add(4);
  
  // Length
  print(list.length); // 4
  print(set.length);  // 4
  
  // Checking if empty
  print(list.isEmpty); // false
  print(set.isEmpty);  // false
  
  // Clearing
  list.clear();
  set.clear();
  
  // Converting
  Set<int> fromList = list.toSet();
  List<int> fromSet = set.toList();
}
```

### 9.2.6 Collection If and Spread Operator

```dart
void main() {
  bool isAdmin = true;
  bool isGuest = false;
  
  // Collection if
  List<String> permissions = [
    'read',
    if (isAdmin) 'write',
    if (isAdmin) 'delete',
    if (isGuest) 'guest_access',
  ];
  print(permissions); // [read, write, delete]
  
  // Spread operator
  List<int> list1 = [1, 2, 3];
  List<int> list2 = [4, 5, 6];
  List<int> combined = [...list1, ...list2];
  print(combined); // [1, 2, 3, 4, 5, 6]
  
  // Null-aware spread
  List<int>? nullList;
  List<int> safeCombined = [...list1, ...?nullList];
  print(safeCombined); // [1, 2, 3]
}
```

## 9.3 Collection Exercises

### Practice Exercise 9.1

Write a generic function that finds the maximum value in a list of comparable items.

```dart
T max<T extends Comparable<T>>(List<T> list) {
  if (list.isEmpty) throw ArgumentError('List cannot be empty');
  T maxValue = list[0];
  for (T item in list) {
    if (item.compareTo(maxValue) > 0) {
      maxValue = item;
    }
  }
  return maxValue;
}
```

### Practice Exercise 9.2

Create a generic `Pair` class that can hold two values of any type.

```dart
class Pair<T1, T2> {
  T1 first;
  T2 second;
  
  Pair(this.first, this.second);
  
  @override
  String toString() => 'Pair($first, $second)';
}
```

### Practice Exercise 9.3

Write a function that merges two maps, with values from the second map taking precedence.

```dart
Map<K, V> mergeMaps<K, V>(Map<K, V> map1, Map<K, V> map2) {
  return {...map1, ...map2};
}
```

### Practice Exercise 9.4

Write a function that removes all duplicates from a list while preserving order.

```dart
List<T> removeDuplicates<T>(List<T> list) {
  Set<T> seen = {};
  return list.where((item) => seen.add(item)).toList();
}
```

### Practice Exercise 9.5

Write a function that groups a list of items by a key function.

```dart
Map<K, List<T>> groupBy<T, K>(Iterable<T> items, K Function(T) keyFunc) {
  Map<K, List<T>> result = {};
  for (T item in items) {
    K key = keyFunc(item);
    result.putIfAbsent(key, () => []).add(item);
  }
  return result;
}
```

---

# 📚 Chapter 10: Advanced Dart Topics

## 10.1 Isolates and Concurrency

Dart uses **isolates** instead of threads for concurrency. Each isolate has its own memory heap and runs independently.

### 10.1.1 Basic Isolate Usage

```dart
import 'dart:isolate';

// Function to run in isolate
void isolateFunction(SendPort sendPort) {
  // Perform heavy computation
  int result = 0;
  for (int i = 0; i < 1000000000; i++) {
    result += i;
  }
  
  // Send result back to main isolate
  sendPort.send(result);
}

void main() async {
  // Create a receive port
  ReceivePort receivePort = ReceivePort();
  
  // Spawn isolate
  Isolate isolate = await Isolate.spawn(
    isolateFunction,
    receivePort.sendPort,
  );
  
  // Listen for messages
  receivePort.listen((message) {
    print('Received from isolate: $message');
    receivePort.close();
    isolate.kill();
  });
}
```

### 10.1.2 Isolate Communication

```dart
import 'dart:isolate';

class Message {
  final String content;
  final SendPort? replyPort;
  
  Message(this.content, [this.replyPort]);
}

void echoIsolate(SendPort mainSendPort) {
  ReceivePort receivePort = ReceivePort();
  
  // Send our receive port to main
  mainSendPort.send(receivePort.sendPort);
  
  // Listen for messages
  receivePort.listen((message) {
    if (message is Message) {
      print('Isolate received: ${message.content}');
      if (message.replyPort != null) {
        message.replyPort.send(Message('Reply: ${message.content}'));
      }
    }
  });
}

void main() async {
  ReceivePort receivePort = ReceivePort();
  
  Isolate isolate = await Isolate.spawn(
    echoIsolate,
    receivePort.sendPort,
  );
  
  // Wait for isolate to send its receive port
  SendPort isolateSendPort = await receivePort.first as SendPort;
  
  // Send message to isolate
  Message message = Message('Hello from main!', receivePort.sendPort);
  isolateSendPort.send(message);
  
  // Listen for replies
  receivePort.listen((reply) {
    if (reply is Message) {
      print('Main received: ${reply.content}');
      receivePort.close();
      isolate.kill();
    }
  });
}
```

### 10.1.3 Using `compute` Function

For simpler cases, use the `compute` function from `package:flutter/foundation.dart` (or `dart:isolate` in pure Dart).

```dart
import 'package:flutter/foundation.dart';

int fibonacci(int n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

void main() async {
  // Run expensive computation in isolate
  int result = await compute(fibonacci, 40);
  print('Fibonacci(40) = $result');
}
```

## 10.2 Callable Classes

Classes can be made callable by implementing the `call()` method.

```dart
class Adder {
  int base;
  
  Adder(this.base);
  
  // Make class callable
  int call(int x) => base + x;
}

void main() {
  Adder add5 = Adder(5);
  print(add5(10)); // 15
  
  // Can be passed as function
  List<int> numbers = [1, 2, 3];
  var results = numbers.map(add5).toList();
  print(results); // [6, 7, 8]
}
```

## 10.3 Extension Methods

Extensions allow you to add new functionality to existing classes without modifying them or using inheritance.

### 10.3.1 Basic Extensions

```dart
// Extend String class
extension StringExtensions on String {
  String capitalize() {
    return this[0].toUpperCase() + this.substring(1);
  }
  
  bool isNumeric() {
    return double.tryParse(this) != null;
  }
}

void main() {
  String name = 'ahmed';
  print(name.capitalize()); // Ahmed
  print('123'.isNumeric()); // true
  print('abc'.isNumeric()); // false
}
```

### 10.3.2 Extensions on Generic Types

```dart
// Extend List<T>
extension ListExtensions<T> on List<T> {
  T? firstWhereOrNull(bool Function(T) test) {
    for (T item in this) {
      if (test(item)) return item;
    }
    return null;
  }
}

void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  int? even = numbers.firstWhereOrNull((n) => n % 2 == 0);
  print(even); // 2
}
```

### 10.3.3 Extensions with Parameters

```dart
// Extend with generic parameter
extension CompareExtensions<T> on T {
  bool isEqualTo(T other) => this == other;
}

void main() {
  int a = 5;
  int b = 10;
  print(a.isEqualTo(5));  // true
  print(a.isEqualTo(b));  // false
}
```

## 10.4 Metadata (Annotations)

Metadata provides a way to add additional information to your code. Annotations are prefixed with `@`.

### 10.4.1 Using Built-in Annotations

```dart
class Todo {
  final String what;
  final String who;
  
  const Todo(this.what, this.who);
}

@Todo('Implement this method', 'Ahmed')
void implementLater() {
  // Method implementation
}

// Deprecation annotation
@deprecated
void oldMethod() {
  print('This method is deprecated');
}
```

### 10.4.2 Creating Custom Annotations

```dart
class Route {
  final String path;
  
  const Route(this.path);
}

class Controller {
  final String name;
  
  const Controller(this.name);
}

@Controller('UserController')
class UserController {
  @Route('/users')
  void getUsers() {
    print('Getting users');
  }
  
  @Route('/users/:id')
  void getUser(int id) {
    print('Getting user $id');
  }
}
```

### 10.4.3 Accessing Metadata at Runtime

```dart
import 'dart:mirrors';

@Todo('Test this', 'Developer')
void testMethod() {}

void main() {
  // Get metadata using reflection
  InstanceMirror mirror = reflect(testMethod);
  
  for (var metadata in mirror.metadata) {
    if (metadata.reflectee is Todo) {
      Todo todo = metadata.reflectee as Todo;
      print('Todo: ${todo.what}, Assigned to: ${todo.who}');
    }
  }
}
```

## 10.5 Functional Programming

### 10.5.1 First-Class Functions

Functions are first-class citizens in Dart.

```dart
void main() {
  // Assign function to variable
  var greet = (String name) => 'Hello, $name!';
  print(greet('Ahmed'));
  
  // Pass function as argument
  List<String> names = ['Ahmed', 'Badr', 'Mohamed'];
  names.forEach((name) => print(greet(name)));
  
  // Return function from function
  Function createMultiplier(int factor) {
    return (int number) => number * factor;
  }
  
  var double = createMultiplier(2);
  print(double(5)); // 10
}
```

### 10.5.2 Pure Functions

A pure function has no side effects and always returns the same output for the same input.

```dart
// Pure function
int add(int a, int b) => a + b;

// Impure function (has side effect)
int counter = 0;
int impureAdd(int a, int b) {
  counter++;
  return a + b;
}

// Pure version
int pureAdd(int a, int b, {int counter = 0}) => a + b;
```

### 10.5.3 Function Composition

```dart
// Compose two functions
Function compose(Function f, Function g) {
  return (dynamic x) => f(g(x));
}

void main() {
  var add5 = (int x) => x + 5;
  var multiply2 = (int x) => x * 2;
  
  var composed = compose(multiply2, add5);
  print(composed(10)); // (10 + 5) * 2 = 30
}
```

### 10.5.4 Currying

```dart
// Regular function
int add(int a, int b, int c) => a + b + c;

// Curried version
Function curriedAdd(int a) {
  return (int b) {
    return (int c) {
      return a + b + c;
    };
  };
}

void main() {
  // Using curried function
  var add5 = curriedAdd(5);
  var add5And10 = add5(10);
  print(add5And10(15)); // 5 + 10 + 15 = 30
  
  // Or inline
  print(curriedAdd(1)(2)(3)); // 6
}
```

## 10.6 Typedefs

Typedefs create aliases for function types or generic types.

### 10.6.1 Function Typedefs

```dart
// Define a function type
typedef IntToString = String Function(int);

typedef Compare<T> = int Function(T a, T b);

void main() {
  IntToString converter = (int n) => n.toString();
  print(converter(42)); // 42
  
  Compare<int> compareInts = (int a, int b) => a - b;
  print(compareInts(5, 10)); // -5
}
```

### 10.6.2 Generic Typedefs

```dart
// Generic function type
typedef ListMapper<T, R> = R Function(T);
typedef ListPredicate<T> = bool Function(T);

void main() {
  ListMapper<int, String> mapper = (int n) => 'Number: $n';
  ListPredicate<int> isEven = (int n) => n % 2 == 0;
  
  List<int> numbers = [1, 2, 3, 4];
  List<String> mapped = numbers.map(mapper).toList();
  List<int> evens = numbers.where(isEven).toList();
  
  print(mapped); // [Number: 1, Number: 2, Number: 3, Number: 4]
  print(evens);  // [2, 4]
}
```

### 10.6.3 Typedefs for Classes

```dart
typedef IntList = List<int>;
typedef StringMap = Map<String, String>;
typedef Json = Map<String, dynamic>;

void main() {
  IntList numbers = [1, 2, 3];
  StringMap config = {'host': 'localhost', 'port': '8080'};
  Json data = {'name': 'Ahmed', 'age': 25};
}
```

## 10.7 Enums

Enums (enumerations) represent a fixed set of constants.

### 10.7.1 Basic Enums

```dart
enum Status {
  pending,
  approved,
  rejected,
  completed
}

void main() {
  Status currentStatus = Status.pending;
  
  if (currentStatus == Status.pending) {
    print('Waiting for approval');
  }
  
  // Iterate through all values
  for (Status status in Status.values) {
    print(status);
  }
}
```

### 10.7.2 Enhanced Enums (Dart 2.17+)

```dart
enum VehicleType {
  car(
    maxSpeed: 200,
    wheels: 4,
    description: 'A car with 4 wheels',
  ),
  bicycle(
    maxSpeed: 50,
    wheels: 2,
    description: 'A bicycle with 2 wheels',
  ),
  motorcycle(
    maxSpeed: 150,
    wheels: 2,
    description: 'A motorcycle with 2 wheels',
  );
  
  final int maxSpeed;
  final int wheels;
  final String description;
  
  const VehicleType({
    required this.maxSpeed,
    required this.wheels,
    required this.description,
  });
}

void main() {
  VehicleType vehicle = VehicleType.car;
  print(vehicle.maxSpeed);    // 200
  print(vehicle.wheels);       // 4
  print(vehicle.description); // A car with 4 wheels
}
```

### 10.7.3 Enum Methods

```dart
enum LogLevel {
  debug,
  info,
  warning,
  error
}

extension LogLevelExtension on LogLevel {
  String get color {
    switch (this) {
      case LogLevel.debug:
        return '\x1B[36m'; // Cyan
      case LogLevel.info:
        return '\x1B[32m'; // Green
      case LogLevel.warning:
        return '\x1B[33m'; // Yellow
      case LogLevel.error:
        return '\x1B[31m'; // Red
    }
  }
  
  String get label {
    switch (this) {
      case LogLevel.debug:
        return 'DEBUG';
      case LogLevel.info:
        return 'INFO';
      case LogLevel.warning:
        return 'WARNING';
      case LogLevel.error:
        return 'ERROR';
    }
  }
}

void log(LogLevel level, String message) {
  print('${level.color}[${level.label}] $message\x1B[0m');
}

void main() {
  log(LogLevel.info, 'Application started');
  log(LogLevel.error, 'Failed to connect');
}
```

## 10.8 Records

Records (Dart 3.0+) allow you to combine multiple objects into a single object without defining a class.

### 10.8.1 Basic Records

```dart
void main() {
  // Create a record
  var record = (1, 'Ahmed', true);
  
  // Access fields
  print(record.$1); // 1
  print(record.$2); // Ahmed
  print(record.$3); // true
  
  // Named fields
  var namedRecord = (id: 1, name: 'Ahmed', isActive: true);
  print(namedRecord.id);      // 1
  print(namedRecord.name);    // Ahmed
  print(namedRecord.isActive); // true
}
```

### 10.8.2 Record Types

```dart
// Explicit type
(int, String) userInfo = (1, 'Ahmed');

// With named fields
({int id, String name}) user = (id: 1, name: 'Ahmed');

// Generic record
(List<int>, Map<String, int>) complex = ([1, 2, 3], {'a': 1, 'b': 2});
```

### 10.8.3 Record Methods

```dart
void main() {
  var record = (1, 'Ahmed');
  
  // Records have all the methods of Object
  print(record.toString()); // (1, Ahmed)
  print(record.hashCode);    // Hash code
  
  // Pattern matching
  var (id, name) = record;
  print('ID: $id, Name: $name');
}
```

### 10.8.4 Records with Classes

```dart
class Point {
  final int x;
  final int y;
  
  Point(this.x, this.y);
}

void main() {
  // Record containing a class instance
  var record = (name: 'Origin', point: Point(0, 0));
  
  print(record.name); // Origin
  print(record.point.x); // 0
}
```

## 10.9 Patterns

Dart 3.0+ introduced pattern matching.

### 10.9.1 Destructuring

```dart
void main() {
  // List destructuring
  var list = [1, 2, 3];
  var [a, b, c] = list;
  print('$a, $b, $c'); // 1, 2, 3
  
  // Record destructuring
  var record = (name: 'Ahmed', age: 25);
  var {'name': name, 'age': age} = record;
  print('$name is $age years old');
  
  // Object destructuring
  class Point {
    final int x;
    final int y;
    Point(this.x, this.y);
  }
  
  var point = Point(10, 20);
  // Note: Requires pattern support in class
}
```

### 10.9.2 Pattern Matching in Switch

```dart
String describeObject(Object? object) {
  return switch (object) {
    null => 'null',
    int n when n > 0 => 'Positive integer: $n',
    int n => 'Non-positive integer: $n',
    String s when s.isEmpty => 'Empty string',
    String s => 'String: $s',
    List<int> list => 'List of integers: $list',
    _ => 'Unknown'
  };
}

void main() {
  print(describeObject(42));      // Positive integer: 42
  print(describeObject(-5));      // Non-positive integer: -5
  print(describeObject(''));      // Empty string
  print(describeObject('Hello')); // String: Hello
  print(describeObject([1, 2]));   // List of integers: [1, 2]
  print(describeObject(null));    // null
}
```

### 10.9.3 List Patterns

```dart
bool isFirstElementOne(List<int> list) {
  return switch (list) {
    [1, ...] => true,
    _ => false
  };
}

void main() {
  print(isFirstElementOne([1, 2, 3])); // true
  print(isFirstElementOne([2, 1, 3])); // false
}
```

### 10.9.4 Record Patterns

```dart
String describeRecord(Object? record) {
  return switch (record) {
    (int, String) => 'Integer and String',
    (String, int) => 'String and Integer',
    (int, int, int) => 'Three integers',
    (name: String name, age: int age) => 'Named: $name, $age',
    _ => 'Unknown pattern'
  };
}

void main() {
  print(describeRecord((1, 'Ahmed')));          // Integer and String
  print(describeRecord(('Ahmed', 25)));          // String and Integer
  print(describeRecord((1, 2, 3)));              // Three integers
  print(describeRecord((name: 'Ahmed', age: 25))); // Named: Ahmed, 25
}
```

## 10.10 Advanced Exercises

### Challenge Exercise 10.1

Create a generic `Cache` class that stores values with a TTL (time-to-live) using a map and timers.

```dart
class Cache<T> {
  final Duration ttl;
  final Map<String, _CacheEntry<T>> _cache = {};
  
  Cache(this.ttl);
  
  void set(String key, T value) {
    _cache[key] = _CacheEntry(value, DateTime.now().add(ttl));
    
    // Schedule removal
    Future.delayed(ttl, () {
      _cache.remove(key);
    });
  }
  
  T? get(String key) {
    _CacheEntry<T>? entry = _cache[key];
    if (entry == null) return null;
    
    if (DateTime.now().isAfter(entry.expiresAt)) {
      _cache.remove(key);
      return null;
    }
    
    return entry.value;
  }
  
  void clear() => _cache.clear();
}

class _CacheEntry<T> {
  final T value;
  final DateTime expiresAt;
  
  _CacheEntry(this.value, this.expiresAt);
}
```

### Challenge Exercise 10.2

Create a generic `PriorityQueue` that implements a priority-based queue using generics and custom comparators.

```dart
class PriorityQueue<T> {
  final List<_QueueItem<T>> _items = [];
  final int Function(T, T) _compare;
  
  PriorityQueue([int Function(T, T)? compare]) 
    : _compare = compare ?? (a, b) => 0;
  
  void add(T value, int priority) {
    _items.add(_QueueItem(value, priority));
    _items.sort((a, b) => b.priority.compareTo(a.priority));
  }
  
  T? remove() {
    if (_items.isEmpty) return null;
    return _items.removeLast().value;
  }
  
  bool get isEmpty => _items.isEmpty;
  int get length => _items.length;
}

class _QueueItem<T> {
  final T value;
  final int priority;
  
  _QueueItem(this.value, this.priority);
}
```

### Challenge Exercise 10.3

Create a generic `Observer` pattern implementation with subscriptions and notifications.

```dart
class Observer<T> {
  final List<void Function(T)> _listeners = [];
  
  void subscribe(void Function(T) listener) {
    _listeners.add(listener);
  }
  
  void unsubscribe(void Function(T) listener) {
    _listeners.remove(listener);
  }
  
  void notify(T data) {
    for (var listener in _listeners) {
      listener(data);
    }
  }
  
  void clear() => _listeners.clear();
}

void main() {
  Observer<String> observer = Observer();
  
  // Subscribe
  observer.subscribe((data) => print('Listener 1: $data'));
  observer.subscribe((data) => print('Listener 2: $data'));
  
  // Notify
  observer.notify('Hello, World!');
  // Output:
  // Listener 1: Hello, World!
  // Listener 2: Hello, World!
}
```

### Challenge Exercise 10.4

Create a generic `StateManager` class that manages state changes with undo/redo functionality.

```dart
class StateManager<T> {
  final List<T> _history = [];
  int _currentIndex = -1;
  
  T? get current => _currentIndex >= 0 ? _history[_currentIndex] : null;
  
  void setState(T state) {
    // Remove any future states if we're not at the end
    if (_currentIndex < _history.length - 1) {
      _history.removeRange(_currentIndex + 1, _history.length);
    }
    
    _history.add(state);
    _currentIndex = _history.length - 1;
  }
  
  void undo() {
    if (_currentIndex > 0) {
      _currentIndex--;
    }
  }
  
  void redo() {
    if (_currentIndex < _history.length - 1) {
      _currentIndex++;
    }
  }
  
  bool get canUndo => _currentIndex > 0;
  bool get canRedo => _currentIndex < _history.length - 1;
}

void main() {
  StateManager<String> manager = StateManager();
  
  manager.setState('First');
  manager.setState('Second');
  manager.setState('Third');
  
  print(manager.current); // Third
  
  manager.undo();
  print(manager.current); // Second
  
  manager.undo();
  print(manager.current); // First
  
  manager.redo();
  print(manager.current); // Second
}
```

### Challenge Exercise 10.5

Create a comprehensive generic `Repository` class with CRUD operations, filtering, and pagination.

```dart
class Repository<T> {
  final List<T> _items = [];
  
  // Create
  void add(T item) => _items.add(item);
  
  // Read
  T? getById(int id) => id >= 0 && id < _items.length ? _items[id] : null;
  List<T> getAll() => List.from(_items);
  
  // Update
  void update(int id, T item) {
    if (id >= 0 && id < _items.length) {
      _items[id] = item;
    }
  }
  
  // Delete
  void delete(int id) {
    if (id >= 0 && id < _items.length) {
      _items.removeAt(id);
    }
  }
  
  // Filter
  List<T> where(bool Function(T) predicate) => _items.where(predicate).toList();
  
  // Pagination
  List<T> paginate({int page = 1, int pageSize = 10}) {
    int start = (page - 1) * pageSize;
    int end = start + pageSize;
    return _items.sublist(start, end.clamp(0, _items.length));
  }
  
  // Count
  int count([bool Function(T)? predicate]) {
    return predicate == null ? _items.length : _items.where(predicate).length;
  }
  
  // Clear
  void clear() => _items.clear();
}

void main() {
  Repository<String> repo = Repository();
  
  // Add items
  repo.add('Item 1');
  repo.add('Item 2');
  repo.add('Item 3');
  
  // Get all
  print(repo.getAll()); // [Item 1, Item 2, Item 3]
  
  // Get by ID
  print(repo.getById(1)); // Item 2
  
  // Filter
  print(repo.where((item) => item.contains('2'))); // [Item 2]
  
  // Pagination
  for (int i = 0; i < 10; i++) {
    repo.add('Item ${i + 4}');
  }
  print(repo.paginate(page: 1, pageSize: 5)); // [Item 1, Item 2, Item 3, Item 4, Item 5]
  print(repo.paginate(page: 2, pageSize: 5)); // [Item 6, Item 7, Item 8, Item 9, Item 10]
}
```

---

# 🎯 Final Project: Complete Dart Application

Let's combine everything we've learned into a complete application: a **Task Management System**.

```dart
import 'dart:collection';

// Enums
enum TaskStatus { todo, inProgress, done }
enum TaskPriority { low, medium, high }

// Custom Exceptions
class TaskNotFoundException implements Exception {
  final String id;
  TaskNotFoundException(this.id);
  
  @override
  String toString() => 'Task not found: $id';
}

class InvalidTaskException implements Exception {
  final String message;
  InvalidTaskException(this.message);
  
  @override
  String toString() => 'Invalid task: $message';
}

// Models
class Task {
  final String id;
  String title;
  String? description;
  TaskStatus status;
  TaskPriority priority;
  DateTime createdAt;
  DateTime? completedAt;
  
  Task({
    required this.id,
    required this.title,
    this.description,
    this.status = TaskStatus.todo,
    this.priority = TaskPriority.medium,
    DateTime? createdAt,
    this.completedAt,
  }) : createdAt = createdAt ?? DateTime.now();
  
  void markAsDone() {
    status = TaskStatus.done;
    completedAt = DateTime.now();
  }
  
  @override
  String toString() => 'Task(id: $id, title: $title, status: $status)';
}

// Repository with Generics
class TaskRepository {
  final Map<String, Task> _tasks = {};
  
  void add(Task task) {
    if (_tasks.containsKey(task.id)) {
      throw InvalidTaskException('Task with ID ${task.id} already exists');
    }
    _tasks[task.id] = task;
  }
  
  Task getById(String id) {
    if (!_tasks.containsKey(id)) {
      throw TaskNotFoundException(id);
    }
    return _tasks[id]!;
  }
  
  List<Task> getAll() => _tasks.values.toList();
  
  void update(Task task) {
    if (!_tasks.containsKey(task.id)) {
      throw TaskNotFoundException(task.id);
    }
    _tasks[task.id] = task;
  }
  
  void delete(String id) {
    if (!_tasks.containsKey(id)) {
      throw TaskNotFoundException(id);
    }
    _tasks.remove(id);
  }
  
  List<Task> filterByStatus(TaskStatus status) =>
    _tasks.values.where((task) => task.status == status).toList();
  
  List<Task> filterByPriority(TaskPriority priority) =>
    _tasks.values.where((task) => task.priority == priority).toList();
}

// Service Layer with Async
class TaskService {
  final TaskRepository _repository;
  
  TaskService(this._repository);
  
  Future<void> addTask(Task task) async {
    await Future.delayed(Duration(milliseconds: 100)); // Simulate DB delay
    _repository.add(task);
  }
  
  Future<Task> getTask(String id) async {
    await Future.delayed(Duration(milliseconds: 100));
    return _repository.getById(id);
  }
  
  Future<List<Task>> getAllTasks() async {
    await Future.delayed(Duration(milliseconds: 100));
    return _repository.getAll();
  }
  
  Future<void> completeTask(String id) async {
    Task task = await getTask(id);
    task.markAsDone();
    _repository.update(task);
  }
  
  // Stream for real-time updates
  Stream<Task> getTaskUpdates() async* {
    while (true) {
      yield* Stream.fromIterable(_repository.getAll());
      await Future.delayed(Duration(seconds: 5));
    }
  }
}

// Observer Pattern for Task Changes
class TaskObserver {
  final List<void Function(Task)> _listeners = [];
  
  void subscribe(void Function(Task) listener) => _listeners.add(listener);
  void unsubscribe(void Function(Task) listener) => _listeners.remove(listener);
  void notify(Task task) {
    for (var listener in _listeners) {
      listener(task);
    }
  }
}

// Extension Methods
extension TaskExtensions on Task {
  bool get isOverdue {
    if (status != TaskStatus.todo) return false;
    // Assume task is overdue if created more than 7 days ago
    return DateTime.now().difference(createdAt).inDays > 7;
  }
  
  String get formattedCreatedAt => createdAt.toString().split('.')[0];
}

// Main Application
void main() async {
  // Setup
  TaskRepository repository = TaskRepository();
  TaskService service = TaskService(repository);
  TaskObserver observer = TaskObserver();
  
  // Subscribe to task changes
  observer.subscribe((task) => print('Task updated: $task'));
  
  // Add tasks
  try {
    await service.addTask(Task(
      id: '1',
      title: 'Complete Dart Course',
      description: 'Finish all 10 files',
      priority: TaskPriority.high,
    ));
    
    await service.addTask(Task(
      id: '2',
      title: 'Review Code',
      priority: TaskPriority.medium,
    ));
    
    await service.addTask(Task(
      id: '3',
      title: 'Write Documentation',
      priority: TaskPriority.low,
    ));
    
    // Get all tasks
    List<Task> tasks = await service.getAllTasks();
    print('\nAll Tasks:');
    tasks.forEach(print);
    
    // Complete a task
    await service.completeTask('1');
    print('\nAfter completing task 1:');
    (await service.getAllTasks()).forEach(print);
    
    // Filter tasks
    List<Task> highPriority = repository.filterByPriority(TaskPriority.high);
    print('\nHigh Priority Tasks:');
    highPriority.forEach(print);
    
    // Listen for updates
    print('\nListening for task updates...');
    await for (Task task in service.getTaskUpdates().take(3)) {
      print('Update: $task');
    }
    
    // Error handling
    try {
      await service.getTask('999');
    } on TaskNotFoundException catch (e) {
      print('\nError: $e');
    }
    
  } catch (e) {
    print('Unexpected error: $e');
  }
}
```

---

# 📖 Summary of Files 9 &amp; 10

## File 9: Generics and Collections

✅ **Generics** - Writing type-safe, reusable code  
✅ **Generic classes, functions, and methods**  
✅ **Bounded generics** with `extends`  
✅ **Generic interfaces and extensions**  
✅ **Lists, Sets, Maps** - In-depth collection operations  
✅ **Iterables** and functional programming with collections  
✅ **Collection if and spread operator**

## File 10: Advanced Topics

✅ **Isolates** - Dart's concurrency model  
✅ **Callable classes** with `call()` method  
✅ **Extension methods** - Adding functionality to existing classes  
✅ **Metadata (Annotations)** - Adding metadata to code  
✅ **Functional programming** - First-class functions, pure functions, composition  
✅ **Typedefs** - Creating type aliases  
✅ **Enums** - Basic and enhanced enums  
✅ **Records** - Combining multiple objects (Dart 3.0+)  
✅ **Patterns** - Destructuring and pattern matching (Dart 3.0+)

## 🎉 Course Completion

You've now completed a **comprehensive Dart course** covering:

- Dart fundamentals and syntax
- Variables, data types, and operators
- Control flow statements
- Functions and functional programming
- Object-oriented programming
- Inheritance, polymorphism, and abstraction
- Exception handling
- Asynchronous programming
- Generics and collections
- Advanced Dart features

You're now ready to:

- Build Flutter applications
- Write backend services in Dart
- Create command-line tools
- Contribute to Dart open-source projects
- Understand and use advanced Dart features

## 📚 Next Steps

**Recommended Learning Path:**

1. **Flutter Development** - Use your Dart knowledge to build mobile apps
2. **Dart FFI** - Learn to interoperate with C code
3. **Dart Web** - Build web applications with Dart
4. **Server-side Dart** - Explore frameworks like Shelf and Aqueduct
5. **Contribute to Open Source** - Check out Dart projects on GitHub

**Resources:**

- [Dart Official Documentation](https://dart.dev/docs)
- [Flutter Documentation](https://flutter.dev/docs)
- [Dart Packages](https://pub.dev/)
- [Dart GitHub](https://github.com/dart-lang)

## 🏆 Final Challenge

Build a complete **Todo Application** with:

- Task CRUD operations
- Task prioritization
- Filtering and sorting
- Local storage (using `shared_preferences` or `hive`)
- User interface (Flutter) or command-line interface
- Error handling
- State management

This will test your understanding of all the concepts covered in this course!

---

## 🎓 Congratulations!

You've completed the **Complete Dart Programming Course**! 🎉

From learning basic syntax to mastering advanced concepts like generics, async programming, and isolates, you now have a solid foundation in Dart. Keep practicing, building projects, and exploring the Dart ecosystem to continue your journey as a Dart developer!

Happy coding! 🚀