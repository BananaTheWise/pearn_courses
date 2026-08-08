# Dart Course: File 4 - Functions in Dart

## 📚 Chapter 4: Functions

Functions are reusable blocks of code that perform a specific task. They help organize code, reduce repetition, and make programs more modular.

---

## 4.1 Function Basics

### 4.1.1 Function Declaration

#### Basic Syntax:

```dart
returnType functionName(parameters) {
  // function body
  return value; // optional, depending on return type
}
```

#### Example:

```dart
void greet() {
  print('Hello, World!');
}

void main() {
  greet(); // Function call
}
```

### 4.1.2 Function with Parameters

```dart
void greetUser(String name) {
  print('Hello, $name!');
}

void main() {
  greetUser('Ahmed');
  greetUser('Badr');
}
```

### 4.1.3 Function with Return Value

```dart
int add(int a, int b) {
  return a + b;
}

void main() {
  int result = add(5, 3);
  print('Result: $result'); // Output: Result: 8
}
```

### 4.1.4 Function with Multiple Parameters

```dart
String formatUserInfo(String name, int age, String city) {
  return 'Name: $name, Age: $age, City: $city';
}

void main() {
  String info = formatUserInfo('Ahmed', 25, 'Cairo');
  print(info);
}
```

## 4.2 Function Parameters

### 4.2.1 Positional Parameters

Parameters are matched by their position (order matters).

```dart
void printInfo(String name, int age) {
  print('Name: $name, Age: $age');
}

void main() {
  printInfo('Ahmed', 25); // Correct
  printInfo(25, 'Ahmed'); // ERROR: Wrong types
}
```

### 4.2.2 Named Parameters

Parameters can be referenced by name, making the order irrelevant.

#### Syntax:

```dart
void functionName({param1, param2, param3}) {
  // function body
}
```

#### Example:

```dart
void printUserInfo({String? name, int? age, String? city}) {
  print('Name: $name, Age: $age, City: $city');
}

void main() {
  printUserInfo(name: 'Ahmed', age: 25, city: 'Cairo');
  printUserInfo(age: 30, name: 'Badr'); // Order doesn't matter
  printUserInfo(city: 'Alexandria', name: 'Mohamed');
}
```

### 4.2.3 Optional Parameters

Parameters can be made optional using `[]` for positional or `{}` for named parameters.

#### Optional Positional Parameters:

```dart
void greet(String name, [int? age, String? city]) {
  String message = 'Hello, $name';
  if (age != null) message += ', Age: $age';
  if (city != null) message += ', City: $city';
  print(message);
}

void main() {
  greet('Ahmed');
  greet('Badr', 30);
  greet('Mohamed', 25, 'Cairo');
}
```

#### Optional Named Parameters:

```dart
void greet({required String name, int? age, String? city}) {
  String message = 'Hello, $name';
  if (age != null) message += ', Age: $age';
  if (city != null) message += ', City: $city';
  print(message);
}

void main() {
  greet(name: 'Ahmed');
  greet(name: 'Badr', age: 30);
  greet(name: 'Mohamed', age: 25, city: 'Cairo');
}
```

### 4.2.4 Required Named Parameters

You can mark named parameters as required:

```dart
void createUser({
  required String username,
  required String email,
  String? bio
}) {
  print('User: $username, Email: $email, Bio: ${bio ?? 'None'}');
}

void main() {
  createUser(username: 'ahmed', email: 'ahmed@example.com');
  // createUser(email: 'test@example.com'); // ERROR: Missing required parameter 'username'
}
```

### 4.2.5 Default Parameter Values

You can provide default values for optional parameters.

```dart
void greet(String name, [String greeting = 'Hello', String punctuation = '!']) {
  print('$greeting, $name$punctuation');
}

void main() {
  greet('Ahmed'); // Hello, Ahmed!
  greet('Badr', 'Hi'); // Hi, Badr!
  greet('Mohamed', 'Welcome', '!!!'); // Welcome, Mohamed!!!
}
```

## 4.3 Arrow Functions (Short Syntax)

For functions with a single expression, you can use the arrow syntax (`=>`).

```dart
// Regular function
int add(int a, int b) {
  return a + b;
}

// Arrow function
int add(int a, int b) => a + b;

void main() {
  // More examples
  bool isEven(int n) => n % 2 == 0;
  String greet(String name) => 'Hello, $name!';
  
  print(add(5, 3)); // 8
  print(isEven(4)); // true
  print(greet('Ahmed')); // Hello, Ahmed!
}
```

## 4.4 Lambda Functions (Anonymous Functions)

Lambda functions are functions without a name. They are often used as arguments to other functions.

### Basic Syntax:

```dart
(parameters) {
  // function body
}
```

### Examples:

```dart
void main() {
  // Assign lambda to a variable
  var greet = (String name) => 'Hello, $name!';
  print(greet('Ahmed'));
  
  // Pass lambda as argument
  List<int> numbers = [1, 2, 3, 4, 5];
  numbers.forEach((number) => print(number * 2));
  
  // Without arrow syntax
  numbers.forEach((number) {
    print(number * 2);
  });
}
```

### Use with Collection Methods:

```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  // Map: transform each element
  var doubled = numbers.map((n) => n * 2).toList();
  print(doubled); // [2, 4, 6, 8, 10]
  
  // Where: filter elements
  var evens = numbers.where((n) => n % 2 == 0).toList();
  print(evens); // [2, 4]
  
  // Reduce: aggregate elements
  var sum = numbers.reduce((a, b) => a + b);
  print(sum); // 15
  
  // Sort
  numbers.sort((a, b) => b.compareTo(a)); // Descending
  print(numbers); // [5, 4, 3, 2, 1]
}
```

## 4.5 Function Scope and Closures

### 4.5.1 Function Scope

Variables declared inside a function are only accessible within that function.

```dart
void outerFunction() {
  String message = 'Hello from outer';
  
  void innerFunction() {
    print(message); // Can access outer function's variable
  }
  
  innerFunction();
}

void main() {
  outerFunction();
  // print(message); // ERROR: 'message' is not defined
}
```

### 4.5.2 Closures

A closure is a function that has access to variables in its lexical scope, even after the outer function has finished executing.

```dart
Function createGreeter(String greeting) {
  return (String name) => '$greeting, $name!';
}

void main() {
  var greetHello = createGreeter('Hello');
  var greetHi = createGreeter('Hi');
  
  print(greetHello('Ahmed')); // Hello, Ahmed!
  print(greetHi('Badr')); // Hi, Badr!
}
```

## 4.6 Recursive Functions

A recursive function is a function that calls itself.

### Example 1: Factorial

```dart
int factorial(int n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}

void main() {
  print(factorial(5)); // 120 (5! = 5 × 4 × 3 × 2 × 1)
}
```

### Example 2: Fibonacci Sequence

```dart
int fibonacci(int n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

void main() {
  for (int i = 0; i < 10; i++) {
    print(fibonacci(i));
  }
}
```

### Example 3: Sum of Natural Numbers

```dart
int sum(int n) {
  if (n <= 0) return 0;
  return n + sum(n - 1);
}

void main() {
  print(sum(5)); // 15 (5 + 4 + 3 + 2 + 1)
}
```

## 4.7 Higher-Order Functions

Higher-order functions are functions that take other functions as arguments or return them.

### Example 1: Function as Parameter

```dart
void processNumbers(List<int> numbers, Function(int) processor) {
  for (int number in numbers) {
    processor(number);
  }
}

void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  // Pass a lambda function
  processNumbers(numbers, (n) => print(n * 2));
  
  // Pass a named function
  processNumbers(numbers, printNumber);
}

void printNumber(int n) {
  print(n);
}
```

### Example 2: Function as Return Value

```dart
Function createMultiplier(int factor) {
  return (int number) => number * factor;
}

void main() {
  var double = createMultiplier(2);
  var triple = createMultiplier(3);
  
  print(double(5)); // 10
  print(triple(5)); // 15
}
```

### Example 3: Common Higher-Order Functions

Dart provides several built-in higher-order functions for collections:

```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  // forEach: execute function for each element
  numbers.forEach((n) => print(n));
  
  // map: transform each element
  var squared = numbers.map((n) => n * n).toList();
  print(squared); // [1, 4, 9, 16, 25]
  
  // where: filter elements
  var evens = numbers.where((n) => n % 2 == 0).toList();
  print(evens); // [2, 4]
  
  // reduce: aggregate elements
  var total = numbers.reduce((a, b) => a + b);
  print(total); // 15
  
  // fold: aggregate with initial value
  var sum = numbers.fold(0, (a, b) => a + b);
  print(sum); // 15
  
  // any: check if any element satisfies condition
  bool hasEven = numbers.any((n) => n % 2 == 0);
  print(hasEven); // true
  
  // every: check if all elements satisfy condition
  bool allEven = numbers.every((n) => n % 2 == 0);
  print(allEven); // false
}
```

## 4.8 Function Types

### 4.8.1 Typing Functions

You can specify the function type for parameters and return values.

```dart
// Function type: int Function(int, int)
int Function(int, int) operation;

int add(int a, int b) => a + b;
int subtract(int a, int b) => a - b;

void main() {
  operation = add;
  print(operation(5, 3)); // 8
  
  operation = subtract;
  print(operation(5, 3)); // 2
}
```

### 4.8.2 Generic Function Types

```dart
typedef Operation<T> = T Function(T, T);

T add<T extends num>(T a, T b) => a + b as T;

void main() {
  Operation<int> intOperation = add;
  print(intOperation(5, 3)); // 8
}
```

## 4.9 Main Function with Parameters

The `main()` function can accept parameters from the command line.

```dart
void main(List<String> arguments) {
  print('Arguments: $arguments');
  
  if (arguments.isNotEmpty) {
    print('First argument: ${arguments[0]}');
  }
}
```

Run it from the command line:

```bash
dart run main.dart hello world
# Output: Arguments: [hello, world]
#         First argument: hello
```

## 4.10 Exercises

### Practice Exercise 4.1

Write a function that takes two numbers and returns their sum.

```dart
int add(int a, int b) {
  return a + b;
}
```

### Practice Exercise 4.2

Write a function that takes a person's name and age, and returns a greeting message.

```dart
String greet(String name, int age) {
  return 'Hello $name! You are $age years old.';
}
```

### Practice Exercise 4.3

Write a function that checks if a number is even and returns a boolean.

```dart
bool isEven(int number) {
  return number % 2 == 0;
}
```

### Practice Exercise 4.4

Write a function that takes a list of numbers and returns the largest number.

```dart
int findMax(List<int> numbers) {
  if (numbers.isEmpty) return 0;
  int max = numbers[0];
  for (int number in numbers) {
    if (number > max) max = number;
  }
  return max;
}
```

### Practice Exercise 4.5

Write a function that uses named parameters to calculate the area of a rectangle.

```dart
double calculateArea({required double width, required double height}) {
  return width * height;
}
```

### Practice Exercise 4.6

Write a function that takes a number and returns its factorial using recursion.

```dart
int factorial(int n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}
```

### Challenge Exercise 4.7

Write a function that takes a list of strings and returns a new list with only the strings that start with a specific prefix.

```dart
List<String> filterByPrefix(List<String> strings, String prefix) {
  return strings.where((s) => s.startsWith(prefix)).toList();
}
```

### Challenge Exercise 4.8

Write a higher-order function that takes a list of numbers and a function, and applies the function to each element.

```dart
List<T> applyToAll<T>(List<T> list, T Function(T) func) {
  return list.map(func).toList();
}
```

### Challenge Exercise 4.9

Write a function that returns a closure. The closure should keep track of how many times it has been called.

```dart
Function createCounter() {
  int count = 0;
  return () => ++count;
}

void main() {
  var counter = createCounter();
  print(counter()); // 1
  print(counter()); // 2
  print(counter()); // 3
}
```

### Challenge Exercise 4.10

Write a function that takes a list of numbers and returns a map with statistics: count, sum, average, min, and max.

```dart
Map<String, dynamic> calculateStats(List<double> numbers) {
  if (numbers.isEmpty) {
    return {'count': 0, 'sum': 0, 'average': 0, 'min': 0, 'max': 0};
  }
  
  double sum = numbers.reduce((a, b) => a + b);
  double avg = sum / numbers.length;
  double min = numbers.reduce((a, b) => a < b ? a : b);
  double max = numbers.reduce((a, b) => a > b ? a : b);
  
  return {
    'count': numbers.length,
    'sum': sum,
    'average': avg,
    'min': min,
    'max': max
  };
}
```

---

## 📖 Summary

In this file, you learned:

✅ **Function basics** (declaration, parameters, return values)  
✅ **Parameter types** (positional, named, optional, default)  
✅ **Arrow functions** for concise syntax  
✅ **Lambda functions** (anonymous functions)  
✅ **Closures** and function scope  
✅ **Recursive functions** for problems that can be divided  
✅ **Higher-order functions** (functions as parameters and return values)  
✅ **Function types** and typing functions  
✅ **Main function with command-line arguments**

## 🔗 Next Steps

**File 5: Object-Oriented Programming - Classes and Objects** will cover:

- Classes and objects
- Constructors
- Instance variables and methods
- Inheritance
- Polymorphism
- Encapsulation

## 📚 Additional Resources

- [Dart Functions](https://dart.dev/guides/language/language-tour#functions)
- [Dart Function Parameters](https://dart.dev/guides/language/language-tour#parameters)
- [Dart Higher-Order Functions](https://dart.dev/guides/libraries/library-tour#collections)

## ✅ Checklist

Before moving to File 5:

- [ ] Understand function declaration and calling
- [ ] Can use different parameter types (positional, named, optional)
- [ ] Know how to use arrow and lambda functions
- [ ] Understand closures and function scope
- [ ] Can write recursive functions
- [ ] Understand higher-order functions
- [ ] Can type functions properly
- [ ] Have completed all exercises in this file