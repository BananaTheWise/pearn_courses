# Dart Course: File 3 - Control Flow

## 📚 Chapter 3: Control Flow Statements

Control flow statements allow your program to make decisions, repeat actions, and control the flow of execution.

---

## 3.1 Conditional Statements

### 3.1.1 If Statement

The `if` statement executes code only when a condition is true.

```dart
void main() {
  int age = 18;
  
  if (age >= 18) {
    print('You are an adult.');
  }
}
```

### 3.1.2 If-Else Statement

The `else` clause executes when the condition is false.

```dart
void main() {
  int age = 16;
  
  if (age >= 18) {
    print('You are an adult.');
  } else {
    print('You are a minor.');
  }
}
```

### 3.1.3 Else-If Ladder

Multiple conditions can be checked using `else if`.

```dart
void main() {
  int score = 85;
  
  if (score >= 90) {
    print('Grade: A');
  } else if (score >= 80) {
    print('Grade: B');
  } else if (score >= 70) {
    print('Grade: C');
  } else if (score >= 60) {
    print('Grade: D');
  } else {
    print('Grade: F');
  }
}
```

### 3.1.4 Nested If Statements

You can nest `if` statements inside other `if` statements.

```dart
void main() {
  bool isLoggedIn = true;
  bool hasSubscription = true;
  
  if (isLoggedIn) {
    print('User is logged in.');
    
    if (hasSubscription) {
      print('User has premium access.');
    } else {
      print('User has free access.');
    }
  } else {
    print('Please log in.');
  }
}
```

## 3.2 Switch Statement

The `switch` statement is used for multiple conditions on the same variable.

### Basic Syntax:

```dart
switch (variable) {
  case value1:
    // code to execute
    break;
  case value2:
    // code to execute
    break;
  default:
    // default code
}
```

### Example:

```dart
void main() {
  String day = 'Monday';
  
  switch (day) {
    case 'Monday':
      print('Start of the work week!');
      break;
    case 'Friday':
      print('Weekend is coming!');
      break;
    case 'Saturday':
    case 'Sunday':
      print('Weekend!');
      break;
    default:
      print('Midweek day.');
  }
}
```

### Important Notes:

- The `break` statement is required to exit each case (Dart doesn't have fall-through like C/Java)
- You can group multiple cases together (like Saturday and Sunday above)
- The `default` case is optional but recommended
- Switch works with: `int`, `String`, `enum`, and compile-time constants

### Modern Dart: Switch Expressions (Dart 3+)

Dart 3 introduced switch expressions that can return values:

```dart
void main() {
  String day = 'Monday';
  
  String message = switch (day) {
    'Monday' => 'Start of the work week!',
    'Friday' => 'Weekend is coming!',
    'Saturday' || 'Sunday' => 'Weekend!',
    _ => 'Midweek day.'
  };
  
  print(message);
}
```

## 3.3 Loops

### 3.3.1 For Loop

The `for` loop repeats a block of code a specific number of times.

#### Basic Syntax:

```dart
for (initialization; condition; increment) {
  // code to repeat
}
```

#### Examples:

```dart
void main() {
  // Print numbers 1 to 5
  for (int i = 1; i <= 5; i++) {
    print(i);
  }
  
  // Print even numbers 2 to 10
  for (int i = 2; i <= 10; i += 2) {
    print(i);
  }
  
  // Count down from 5 to 1
  for (int i = 5; i >= 1; i--) {
    print(i);
  }
}
```

#### For-In Loop (Iterating over collections):

```dart
void main() {
  List<String> names = ['Ahmed', 'Badr', 'Mohamed', 'Ali'];
  
  for (String name in names) {
    print('Hello, $name!');
  }
  
  // With Map
  Map<String, int> ages = {'Ahmed': 25, 'Badr': 30, 'Mohamed': 22};
  for (var entry in ages.entries) {
    print('${entry.key} is ${entry.value} years old');
  }
}
```

### 3.3.2 While Loop

The `while` loop repeats a block of code while a condition is true.

#### Basic Syntax:

```dart
while (condition) {
  // code to repeat
}
```

#### Examples:

```dart
void main() {
  // Count from 1 to 5
  int i = 1;
  while (i <= 5) {
    print(i);
    i++;
  }
  
  // Read input until user enters 'quit'
  // (This would require input, but shows the concept)
  String input = '';
  while (input != 'quit') {
    // get input...
    // process input...
  }
}
```

### 3.3.3 Do-While Loop

The `do-while` loop executes the code block once before checking the condition.

#### Basic Syntax:

```dart
do {
  // code to repeat
} while (condition);
```

#### Example:

```dart
void main() {
  // This will always execute at least once
  int i = 6;
  
  do {
    print(i);
    i++;
  } while (i <= 5);
  // Output: 6 (only prints once because condition is false after)
}
```

### 3.3.4 For-Each with Collection Methods

Dart collections have built-in methods for iteration:

```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  // forEach method
  numbers.forEach((number) {
    print(number * 2);
  });
  
  // With arrow syntax
  numbers.forEach((number) => print(number * 2));
  
  // With Map
  Map<String, int> ages = {'Ahmed': 25, 'Badr': 30};
  ages.forEach((name, age) {
    print('$name: $age');
  });
}
```

## 3.4 Loop Control Statements

### 3.4.1 Break Statement

The `break` statement exits the loop immediately.

```dart
void main() {
  // Find first even number
  for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
      print('First even number: $i');
      break;
    }
  }
  
  // Exit while loop when condition met
  int i = 1;
  while (true) {
    print(i);
    i++;
    if (i > 5) break;
  }
}
```

### 3.4.2 Continue Statement

The `continue` statement skips the current iteration and continues with the next.

```dart
void main() {
  // Print odd numbers only
  for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
      continue;  // Skip even numbers
    }
    print(i);
  }
}
```

### 3.4.3 Labeled Statements

You can label loops and use them with `break` and `continue`:

```dart
void main() {
  outerLoop: for (int i = 1; i <= 3; i++) {
    innerLoop: for (int j = 1; j <= 3; j++) {
      if (i == 2 && j == 2) {
        break outerLoop;  // Break out of outer loop
      }
      print('i=$i, j=$j');
    }
  }
}
```

## 3.5 Practical Examples

### Example 1: Check if a Number is Prime

```dart
bool isPrime(int number) {
  if (number <= 1) return false;
  if (number == 2) return true;
  if (number % 2 == 0) return false;
  
  for (int i = 3; i * i <= number; i += 2) {
    if (number % i == 0) {
      return false;
    }
  }
  return true;
}

void main() {
  int num = 17;
  if (isPrime(num)) {
    print('$num is a prime number.');
  } else {
    print('$num is not a prime number.');
  }
}
```

### Example 2: Calculate Factorial

```dart
int factorial(int n) {
  int result = 1;
  for (int i = 1; i <= n; i++) {
    result *= i;
  }
  return result;
}

void main() {
  int number = 5;
  print('Factorial of $number is ${factorial(number)}');
}
```

### Example 3: Find Maximum Number in a List

```dart
void main() {
  List<int> numbers = [3, 7, 2, 9, 5, 1, 8];
  
  int max = numbers[0];
  for (int number in numbers) {
    if (number > max) {
      max = number;
    }
  }
  print('Maximum number: $max');
}
```

### Example 4: FizzBuzz

```dart
void main() {
  for (int i = 1; i <= 100; i++) {
    if (i % 15 == 0) {
      print('FizzBuzz');
    } else if (i % 3 == 0) {
      print('Fizz');
    } else if (i % 5 == 0) {
      print('Buzz');
    } else {
      print(i);
    }
  }
}
```

## 3.6 Exercises

### Practice Exercise 3.1

Write a program that checks if a number is positive, negative, or zero.

```dart
void main() {
  int number = -5;
  // Your code here
  if (number > 0) {
    print('Positive');
  } else if (number < 0) {
    print('Negative');
  } else {
    print('Zero');
  }
}
```

### Practice Exercise 3.2

Write a program that prints the multiplication table of 5 (from 1 to 10).

```dart
void main() {
  // Your code here
  for (int i = 1; i <= 10; i++) {
    print('5 x $i = ${5 * i}');
  }
}
```

### Practice Exercise 3.3

Write a program that calculates the sum of the first 10 natural numbers.

```dart
void main() {
  // Your code here
  int sum = 0;
  for (int i = 1; i <= 10; i++) {
    sum += i;
  }
  print('Sum: $sum');
}
```

### Practice Exercise 3.4

Write a program that finds the largest among three numbers.

```dart
void main() {
  int a = 10, b = 20, c = 15;
  // Your code here
  int largest = a;
  if (b > largest) largest = b;
  if (c > largest) largest = c;
  print('Largest: $largest');
}
```

### Practice Exercise 3.5

Write a program that prints numbers from 1 to 100, but for multiples of 3 print "Dart", for multiples of 5 print "Flutter", and for multiples of both print "DartFlutter".

```dart
void main() {
  // Your code here (similar to FizzBuzz)
  for (int i = 1; i <= 100; i++) {
    if (i % 15 == 0) {
      print('DartFlutter');
    } else if (i % 3 == 0) {
      print('Dart');
    } else if (i % 5 == 0) {
      print('Flutter');
    } else {
      print(i);
    }
  }
}
```

### Practice Exercise 3.6

Write a program that checks if a year is a leap year. A leap year is divisible by 4, but not by 100 unless it's also divisible by 400.

```dart
void main() {
  int year = 2024;
  // Your code here
  bool isLeapYear = (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
  print('$year is ${isLeapYear ? '' : 'not '}a leap year');
}
```

### Challenge Exercise 3.7

Write a program that generates a right-angled triangle pattern using asterisks (\*).

Example output for 5 rows:

```
*
**
***
****
*****
```

```dart
void main() {
  int rows = 5;
  // Your code here
  for (int i = 1; i <= rows; i++) {
    print('*' * i);
  }
}
```

### Challenge Exercise 3.8

Write a program that checks if a string is a palindrome (reads the same forwards and backwards).

```dart
void main() {
  String text = 'madam';
  // Your code here
  String reversed = text.split('').reversed.join('');
  bool isPalindrome = text == reversed;
  print('$text is ${isPalindrome ? '' : 'not '}a palindrome');
}
```

### Challenge Exercise 3.9

Write a program that calculates the factorial of a number using a while loop.

```dart
void main() {
  int number = 5;
  // Your code here
  int factorial = 1;
  int i = 1;
  while (i <= number) {
    factorial *= i;
    i++;
  }
  print('Factorial of $number is $factorial');
}
```

---

## 📖 Summary

In this file, you learned:

✅ **Conditional statements** (`if`, `else if`, `else`)  
✅ **Nested if statements** for complex conditions  
✅ **Switch statements** for multiple conditions  
✅ **For loops** for known iterations  
✅ **While and do-while loops** for condition-based iterations  
✅ **For-in loops** for iterating over collections  
✅ **Loop control** (`break`, `continue`, labeled statements)  
✅ **Practical examples** of control flow in action  
✅ **Switch expressions** (Dart 3+)

## 🔗 Next Steps

**File 4: Functions in Dart** will cover:

- Function declaration and parameters
- Return values and types
- Optional, named, and positional parameters
- Arrow functions and lambda expressions
- Function scope and closures
- Recursive functions

## 📚 Additional Resources

- [Dart Control Flow](https://dart.dev/guides/language/language-tour#control-flow-statements)
- [Dart Loops](https://dart.dev/guides/language/language-tour#loops)
- [Dart Switch Expressions](https://dart.dev/language/branches-and-loops#switch-expressions)

## ✅ Checklist

Before moving to File 4:

- [ ] Understand if/else and switch statements
- [ ] Can use for, while, and do-while loops
- [ ] Know how to use break and continue
- [ ] Can write nested control structures
- [ ] Understand the difference between for and while loops
- [ ] Have completed all exercises in this file