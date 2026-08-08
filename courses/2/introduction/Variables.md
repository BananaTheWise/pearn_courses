# Dart Course: File 2 - Variables, Data Types, and Operators

## 📚 Chapter 2: Variables, Data Types, and Operators

---

## 2.1 Variables in Dart

### What are Variables?

Variables are containers that store data values. In Dart, every variable has a **type** that determines what kind of data it can hold.

### Variable Declaration

#### Basic Syntax:

```dart
// Syntax: type variableName = value;
String name = 'Ahmed';
int age = 25;
double height = 1.75;
bool isStudent = true;
```

#### Variable Declaration Examples:

```dart
// Declare and initialize
String city = 'Cairo';

// Declare first, then assign
int population;
population = 1000000;

// Multiple variables of same type
String firstName = 'Ahmed', lastName = 'Badr';
```

### 2.2 The `var` Keyword (Type Inference)

Dart can infer the type automatically using `var`:

```dart
var name = 'Ahmed';      // Inferred as String
var age = 25;           // Inferred as int
var height = 1.75;     // Inferred as double
var isActive = true;   // Inferred as bool
```

**Important:** Once inferred, the type cannot change:

```dart
var name = 'Ahmed';
name = 25;  // ERROR: A value of type 'int' can't be assigned to a variable of type 'String'
```

### 2.3 The `dynamic` Type

If you want a variable to hold any type of value, use `dynamic`:

```dart
dynamic value = 'Hello';
value = 42;        // OK
value = true;      // OK
value = 3.14;      // OK
```

**⚠️ Warning:** Using `dynamic` disables type checking, so use it sparingly.

### 2.4 The `final` and `const` Keywords

#### `final` - Runtime Constant

```dart
final name = 'Ahmed';
// name = 'Badr';  // ERROR: Can't assign to final variable

final currentTime = DateTime.now();  // Can be computed at runtime
```

#### `const` - Compile-time Constant

```dart
const pi = 3.14159;
// pi = 3.14;  // ERROR: Can't assign to const variable

const maxItems = 100;
// const currentTime = DateTime.now();  // ERROR: Not a compile-time constant
```

**Key Differences:**


| Feature                             | `final` | `const` |
| ----------------------------------- | ------- | ------- |
| Value can be computed at runtime    | ✅ Yes   | ❌ No    |
| Value must be known at compile time | ❌ No    | ✅ Yes   |
| Can be used with instance variables | ✅ Yes   | ❌ No    |


### 2.5 Dart Data Types

#### Built-in Data Types:

1. **`int`** - Integer numbers (whole numbers)
  ```dart
   int age = 25;
   int negative = -10;
   int hex = 0xFF;  // Hexadecimal
   int binary = 0b1010;  // Binary
  ```
2. **`double`** - Floating-point numbers (decimal numbers)
  ```dart
   double price = 19.99;
   double temperature = 25.5;
   double scientific = 1.5e3;  // 1500.0
  ```
3. **`String`** - Text data
  ```dart
   String name = 'Ahmed';
   String message = "Hello, World!";
   String multiLine = '''This is a
   multi-line string''';
   String template = 'Hello, $name!';  // String interpolation
  ```
4. **`bool`** - Boolean values (true/false)
  ```dart
   bool isLoggedIn = true;
   bool hasPermission = false;
  ```
5. **`List`** - Ordered collection of items
  ```dart
   List<String> names = ['Ahmed', 'Badr', 'Mohamed'];
   var numbers = [1, 2, 3, 4, 5];
   List<dynamic> mixed = [1, 'text', true];
  ```
6. **`Map`** - Key-value pairs
  ```dart
   Map<String, int> ages = {'Ahmed': 25, 'Badr': 30};
   var user = {'name': 'Ahmed', 'age': 25};
  ```
7. **`Set`** - Unordered collection of unique items
  ```dart
   Set<String> uniqueNames = {'Ahmed', 'Badr', 'Mohamed'};
   var uniqueNumbers = {1, 2, 3, 3, 4};  // {1, 2, 3, 4}
  ```
8. **`Runes`** - Unicode character sequences
  ```dart
   var heart = '❤️';
   var emoji = '😊';
  ```
9. **`Symbol`** - Used for reflection (advanced)
  ```dart
   var symbol = #mySymbol;
  ```
10. **`Null`** - Represents the absence of a value
  ```dart
   String? name = null;  // Nullable type
  ```

### 2.6 Null Safety in Dart

Dart has **sound null safety**, which means variables cannot be `null` by default.

#### Non-nullable Types (Default)

```dart
String name = 'Ahmed';
// name = null;  // ERROR: A value of type 'Null' can't be assigned to a variable of type 'String'
```

#### Nullable Types

```dart
String? name = 'Ahmed';
name = null;  // OK

// Check if nullable variable is null
if (name != null) {
  print(name.length);  // Safe to access
}
```

#### Null-aware Operators

1. **`?.`** - Safe access
  ```dart
   int? length = name?.length;  // Returns null if name is null
  ```
2. **`??`** - If-null operator
  ```dart
   String result = name ?? 'Guest';  // 'Guest' if name is null
  ```
3. **`??=`** - Assign if null
  ```dart
   name ??= 'Guest';  // Assigns 'Guest' only if name is null
  ```
4. **`!`** - Bang operator (force non-null)
  ```dart
   String nonNullable = name!;  // Throws error if name is null
  ```

### 2.7 Operators in Dart

#### Arithmetic Operators


| Operator | Name                | Example          | Result     |
| -------- | ------------------- | ---------------- | ---------- |
| `+`      | Addition            | `10 + 5`         | `15`       |
| `-`      | Subtraction         | `10 - 5`         | `5`        |
| `*`      | Multiplication      | `10 * 5`         | `50`       |
| `/`      | Division            | `10 / 3`         | `3.333...` |
| `~/`     | Integer Division    | `10 ~/ 3`        | `3`        |
| `%`      | Modulus (Remainder) | `10 % 3`         | `1`        |
| `-`      | Unary Minus         | `-5`             | `-5`       |
| `++`     | Increment           | `var x = 5; x++` | `6`        |
| `--`     | Decrement           | `var x = 5; x--` | `4`        |


#### Assignment Operators


| Operator | Example   | Equivalent   |
| -------- | --------- | ------------ |
| `=`      | `x = 5`   | `x = 5`      |
| `+=`     | `x += 3`  | `x = x + 3`  |
| `-=`     | `x -= 3`  | `x = x - 3`  |
| `*=`     | `x *= 3`  | `x = x * 3`  |
| `/=`     | `x /= 3`  | `x = x / 3`  |
| `~/=`    | `x ~/= 3` | `x = x ~/ 3` |
| `%=`     | `x %= 3`  | `x = x % 3`  |


#### Relational Operators


| Operator | Name                  | Example            |
| -------- | --------------------- | ------------------ |
| `==`     | Equal to              | `5 == 5` → `true`  |
| `!=`     | Not equal             | `5 != 3` → `true`  |
| `>`      | Greater than          | `5 > 3` → `true`   |
| `<`      | Less than             | `5 < 3` → `false`  |
| `>=`     | Greater than or equal | `5 >= 5` → `true`  |
| `<=`     | Less than or equal    | `5 <= 3` → `false` |


#### Logical Operators


| Operator | Name        | Example                   |
| -------- | ----------- | ------------------------- |
| `&&`     | Logical AND | `true && false` → `false` |
| \`       |             | \`                        |
| `!`      | Logical NOT | `!true` → `false`         |


#### Bitwise Operators


| Operator | Name        | Example         |
| -------- | ----------- | --------------- |
| `&`      | AND         | `5 & 3` → `1`   |
| \`       | \`          | OR              |
| `^`      | XOR         | `5 ^ 3` → `6`   |
| `~`      | NOT         | `~5` → `-6`     |
| `<<`     | Left Shift  | `5 << 1` → `10` |
| `>>`     | Right Shift | `5 >> 1` → `2`  |


#### Type Test Operators


| Operator | Purpose                                     | Example                 |
| -------- | ------------------------------------------- | ----------------------- |
| `is`     | True if object has specified type           | `5 is int` → `true`     |
| `is!`    | True if object does NOT have specified type | `5 is! String` → `true` |
| `as`     | Type cast                                   | `var x = 5 as dynamic;` |


#### Conditional Expressions

1. **Ternary Operator**
  ```dart
   var result = condition ? expr1 : expr2;
   var isEven = number % 2 == 0 ? true : false;
  ```
2. **If-null Operator (`??`)**
  ```dart
   var name = nullableName ?? 'Guest';
  ```

### 2.8 Type Conversion

#### String to Number

```dart
var number = int.parse('123');      // String to int
var decimal = double.parse('123.45'); // String to double
```

#### Number to String

```dart
var text = 123.toString();          // int to String
var text2 = 123.45.toString();      // double to String
```

#### String to Boolean

```dart
var isTrue = bool.parse('true');    // String to bool
```

#### Number Conversion

```dart
var intValue = 123.45.toInt();      // double to int (truncates)
var doubleValue = 123.toDouble();   // int to double
```

### 2.9 Exercises

#### Practice Exercise 2.1

Create variables to store your name, age, height (in meters), and whether you're a student. Print them in a formatted sentence.

```dart
void main() {
  String name = 'Ahmed';
  int age = 25;
  double height = 1.75;
  bool isStudent = false;
  
  print('Name: $name, Age: $age, Height: $height, Student: $isStudent');
}
```

#### Practice Exercise 2.2

Calculate the area of a rectangle. Create variables for length and width, then calculate and print the area.

```dart
void main() {
  double length = 5.5;
  double width = 3.2;
  double area = length * width;
  print('Area: $area');
}
```

#### Practice Exercise 2.3

Use arithmetic operators to calculate:

1. The sum of 15 and 25
2. The product of 12 and 8
3. The remainder when 17 is divided by 5
4. 10 squared (10^2)

```dart
void main() {
  int sum = 15 + 25;
  int product = 12 * 8;
  int remainder = 17 % 5;
  int square = 10 * 10;
  
  print('Sum: $sum');
  print('Product: $product');
  print('Remainder: $remainder');
  print('Square: $square');
}
```

#### Practice Exercise 2.4

Create a program that checks if a number is even or odd using the modulus operator.

```dart
void main() {
  int number = 7;
  bool isEven = number % 2 == 0;
  print('$number is ${isEven ? 'even' : 'odd'}');
}
```

#### Practice Exercise 2.5

Convert temperature from Celsius to Fahrenheit. Formula: F = (C × 9/5) + 32

```dart
void main() {
  double celsius = 25.0;
  double fahrenheit = (celsius * 9/5) + 32;
  print('$celsius°C = $fahrenheit°F');
}
```

#### Challenge Exercise 2.6

Create a program that:

1. Declares a nullable String variable
2. Assigns it a value
3. Uses the null-aware operator to print its length
4. Sets it to null and handles the case gracefully

```dart
void main() {
  String? name = 'Dart';
  print('Length: ${name?.length}');
  
  name = null;
  print('Length: ${name?.length ?? 'null'}');
}
```

#### Challenge Exercise 2.7

Create a program that uses the ternary operator to check if a person is eligible to vote (age &gt;= 18).

```dart
void main() {
  int age = 20;
  String result = age >= 18 ? 'Eligible to vote' : 'Not eligible to vote';
  print(result);
}
```

---

## 📖 Summary

In this file, you learned:

✅ How to declare variables in Dart  
✅ The difference between `var`, `dynamic`, `final`, and `const`  
✅ All built-in data types in Dart  
✅ Null safety and nullable types  
✅ Various operators: arithmetic, assignment, relational, logical, bitwise, type test  
✅ Type conversion between different data types  
✅ Conditional expressions

## 🔗 Next Steps

**File 3: Control Flow - If/Else, Loops, and Switch** will cover:

- If/else statements
- For loops and while loops
- Break and continue statements
- Switch statements
- Loop control

## 📚 Additional Resources

- [Dart Language Tour - Built-in Types](https://dart.dev/guides/language/language-tour#built-in-types)
- [Dart Null Safety](https://dart.dev/null-safety)
- [Dart Operators](https://dart.dev/guides/language/language-tour#operators)

## ✅ Checklist

Before moving to File 3:

- [ ] Understand variable declaration and initialization
- [ ] Know the difference between mutable and immutable variables
- [ ] Can use all basic data types
- [ ] Understand null safety concepts
- [ ] Can use various operators
- [ ] Can convert between different data types
- [ ] Have completed all exercises in this file