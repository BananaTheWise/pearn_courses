# Dart Course: File 7 - Exception Handling and Error Management

## 📚 Chapter 7: Exception Handling

Errors are inevitable in programming. Dart provides robust mechanisms to handle errors gracefully and prevent crashes.

---

## 7.1 Understanding Errors and Exceptions

### Types of Errors:

1. **Compile-time Errors**: Detected by the Dart compiler before execution
  ```dart
   int x = 'hello'; // ERROR: A value of type 'String' can't be assigned to a variable of type 'int'
  ```
2. **Runtime Errors**: Occur during program execution
  - **Exceptions**: Errors that can be caught (e.g., `IntegerDivisionByZeroException`)
  - **Errors**: Serious problems that shouldn't be caught (e.g., `StackOverflowError`, `OutOfMemoryError`)

### Built-in Exception Types:


| Exception                        | Description                                   |
| -------------------------------- | --------------------------------------------- |
| `Exception`                      | Base class for all exceptions                 |
| `FormatException`                | Invalid format (e.g., parsing invalid number) |
| `IntegerDivisionByZeroException` | Division by zero for integers                 |
| `IsolateSpawnException`          | Error in isolate spawning                     |
| `TimeoutException`               | Operation timed out                           |
| `ArgumentError`                  | Invalid argument passed to function           |
| `RangeError`                     | Value outside valid range                     |
| `NullThrownError`                | Threw null                                    |
| `LateInitializationError`        | Late variable not initialized                 |
| `NoSuchMethodError`              | Called non-existent method                    |


## 7.2 Try, Catch, and Finally

### 7.2.1 Basic Try-Catch

```dart
void main() {
  try {
    // Code that might throw an exception
    int result = 10 ~/ 0; // This will throw IntegerDivisionByZeroException
    print('Result: $result');
  } catch (e) {
    // Handle the exception
    print('An error occurred: $e');
  }
}
```

### 7.2.2 Catching Specific Exceptions

```dart
void main() {
  try {
    int number = int.parse('not_a_number');
  } on FormatException {
    print('Invalid number format!');
  } on IntegerDivisionByZeroException {
    print('Cannot divide by zero!');
  } catch (e) {
    print('Unknown error: $e');
  }
}
```

### 7.2.3 Catch with Exception Object

```dart
void main() {
  try {
    int number = int.parse('abc');
  } on FormatException catch (e) {
    print('Format Exception: ${e.message}');
  } catch (e, s) {
    // e: exception object
    // s: stack trace
    print('Exception: $e');
    print('Stack Trace: $s');
  }
}
```

### 7.2.4 The `finally` Block

The `finally` block always executes, whether an exception was thrown or not.

```dart
void main() {
  try {
    int result = 10 ~/ 2;
    print('Result: $result');
    return; // Even with return, finally will execute
  } catch (e) {
    print('Error: $e');
  } finally {
    print('This always executes');
  }
}
```

### 7.2.5 Try-Catch as Expression

In Dart, `try-catch` can be used as an expression that returns a value.

```dart
void main() {
  int result = try {
    10 ~/ 2
  } catch (e) {
    0
  };
  
  print(result); // 5
}
```

## 7.3 Throwing Exceptions

### 7.3.1 The `throw` Keyword

```dart
void divide(int a, int b) {
  if (b == 0) {
    throw IntegerDivisionByZeroException();
  }
  print('Result: ${a / b}');
}

void main() {
  try {
    divide(10, 0);
  } catch (e) {
    print('Caught: $e');
  }
}
```

### 7.3.2 Throwing with Custom Message

```dart
void setAge(int age) {
  if (age < 0) {
    throw ArgumentError('Age cannot be negative');
  }
  print('Age set to: $age');
}

void main() {
  try {
    setAge(-5);
  } catch (e) {
    print('Error: $e');
  }
}
```

### 7.3.3 Throwing Any Object

In Dart, you can throw any object, not just exceptions.

```dart
void main() {
  try {
    throw 'This is a custom error message';
  } catch (e) {
    print('Caught: $e');
  }
  
  try {
    throw 404;
  } catch (e) {
    print('Caught: $e');
  }
}
```

### 7.3.4 Re-throwing Exceptions

```dart
void processData(String data) {
  try {
    // Process data
    if (data.isEmpty) {
      throw ArgumentError('Data cannot be empty');
    }
  } catch (e) {
    print('Error processing data: $e');
    rethrow; // Re-throw the caught exception
  }
}

void main() {
  try {
    processData('');
  } catch (e) {
    print('Main caught: $e');
  }
}
```

## 7.4 Custom Exceptions

### 7.4.1 Creating Custom Exception Classes

```dart
class InvalidEmailException implements Exception {
  final String email;
  
  InvalidEmailException(this.email);
  
  @override
  String toString() => 'Invalid email format: $email';
}

void validateEmail(String email) {
  if (!email.contains('@')) {
    throw InvalidEmailException(email);
  }
  print('Valid email: $email');
}

void main() {
  try {
    validateEmail('invalid.email');
  } on InvalidEmailException catch (e) {
    print(e);
  }
}
```

### 7.4.2 Custom Exception with Details

```dart
class InsufficientFundsException implements Exception {
  final double balance;
  final double amount;
  
  InsufficientFundsException(this.balance, this.amount);
  
  double get deficit => amount - balance;
  
  @override
  String toString() => 'Insufficient funds: need $deficit more (balance: $balance, requested: $amount)';
}

class BankAccount {
  double balance;
  
  BankAccount(this.balance);
  
  void withdraw(double amount) {
    if (amount > balance) {
      throw InsufficientFundsException(balance, amount);
    }
    balance -= amount;
    print('Withdrew: $amount. New balance: $balance');
  }
}

void main() {
  try {
    BankAccount account = BankAccount(100);
    account.withdraw(150);
  } on InsufficientFundsException catch (e) {
    print('Error: $e');
    print('You need: ${e.deficit}');
  }
}
```

### 7.4.3 Extending Existing Exceptions

```dart
class InvalidAgeException extends ArgumentError {
  final int age;
  
  InvalidAgeException(this.age) : super('Invalid age: $age. Must be between 0 and 120.');
}

void setUserAge(int age) {
  if (age < 0 || age > 120) {
    throw InvalidAgeException(age);
  }
  print('Age set to: $age');
}

void main() {
  try {
    setUserAge(150);
  } on InvalidAgeException catch (e) {
    print(e.message);
  }
}
```

## 7.5 Stack Traces

### 7.5.1 Understanding Stack Traces

```dart
void functionA() {
  functionB();
}

void functionB() {
  functionC();
}

void functionC() {
  throw Exception('Something went wrong!');
}

void main() {
  try {
    functionA();
  } catch (e, s) {
    print('Exception: $e');
    print('Stack Trace:');
    print(s);
  }
}
```

### 7.5.2 Creating Custom Stack Traces

```dart
void main() {
  try {
    throw Exception('Custom error');
  } catch (e, s) {
    print('Exception: $e');
    print('Stack Trace:');
    
    // Print each frame
    for (var frame in s.toString().split('\n')) {
      print('  $frame');
    }
  }
}
```

## 7.6 Assertions

Assertions are used for debugging and testing. They are removed in production code when compiled with `--release` flag.

### 7.6.1 Basic Assertions

```dart
void setAge(int age) {
  assert(age >= 0, 'Age cannot be negative');
  assert(age <= 120, 'Age cannot be greater than 120');
  print('Age set to: $age');
}

void main() {
  setAge(25); // OK
  setAge(-5); // Throws AssertionError in debug mode
}
```

### 7.6.2 Assert with Conditions

```dart
void main() {
  List<int> numbers = [1, 2, 3];
  
  // Check if list is not empty
  assert(numbers.isNotEmpty, 'List cannot be empty');
  
  // Check if index is valid
  int index = 0;
  assert(index >= 0 && index < numbers.length, 'Index out of range');
}
```

### 7.6.3 Assert with Functions

```dart
bool isValidEmail(String email) {
  return email.contains('@') && email.contains('.');
}

void main() {
  String email = 'test@example.com';
  assert(isValidEmail(email), 'Invalid email format');
}
```

## 7.7 Null Safety and Exception Handling

### 7.7.1 Handling Null with Null-Aware Operators

```dart
void main() {
  String? name = null;
  
  // Safe access
  int? length = name?.length;
  print(length); // null
  
  // If-null operator
  String displayName = name ?? 'Unknown';
  print(displayName); // Unknown
  
  // Null assertion operator (use carefully)
  try {
    String nonNullable = name!; // Throws LateInitializationError if null
  } catch (e) {
    print('Caught: $e');
  }
}
```

### 7.7.2 Throwing on Null

```dart
void processString(String? input) {
  String text = input ?? (throw ArgumentError('Input cannot be null'));
  print('Processing: $text');
}

void main() {
  try {
    processString(null);
  } catch (e) {
    print('Error: $e');
  }
}
```

### 7.7.3 Null Safety in Collections

```dart
void main() {
  List<String?>? names = ['Ahmed', null, 'Badr'];
  
  try {
    // This might throw if list is null
    for (String? name in names!) {
      print(name?.toUpperCase() ?? 'NULL');
    }
  } catch (e) {
    print('Error: $e');
  }
}
```

## 7.8 Asynchronous Exception Handling

### 7.8.1 Try-Catch with Futures

```dart
Future<void> fetchData() async {
  try {
    // Simulate async operation that might fail
    await Future.delayed(Duration(seconds: 1));
    throw Exception('Failed to fetch data');
  } catch (e) {
    print('Async error: $e');
  }
}

void main() async {
  await fetchData();
}
```

### 7.8.2 CatchError with Futures

```dart
Future<void> fetchData() {
  return Future.error('Network error')
    .then((_) => print('Success'))
    .catchError((e) => print('Error: $e'));
}

void main() async {
  await fetchData();
}
```

### 7.8.3 Try-Catch with Streams

```dart
void main() {
  // Create a stream that might emit an error
  Stream<int> stream = Stream.fromIterable([1, 2, 3])
    .map((x) => x * 2)
    .handleError((e) => print('Stream error: $e'));
  
  stream.listen(
    (data) => print('Data: $data'),
    onError: (e) => print('Error in stream: $e'),
    onDone: () => print('Stream completed'),
  );
}
```

## 7.9 Best Practices for Exception Handling

### ✅ Do:

1. **Be specific** - Catch specific exceptions when possible
2. **Provide meaningful messages** - Include context in exception messages
3. **Clean up resources** - Use `finally` to release resources
4. **Log exceptions** - Record errors for debugging
5. **Handle gracefully** - Provide user-friendly error messages
6. **Use custom exceptions** - For domain-specific errors
7. **Validate inputs** - Use assertions and validation

### ❌ Don't:

1. **Catch all exceptions blindly** - Avoid empty catch blocks
2. **Ignore exceptions** - Don't catch and do nothing
3. **Catch Errors** - Don't catch `Error` types (like `StackOverflowError`)
4. **Use exceptions for control flow** - Exceptions should be exceptional
5. **Expose sensitive information** - Don't include secrets in error messages
6. **Swallow exceptions** - At least log them

### Example: Good Exception Handling

```dart
Future<User> fetchUser(int id) async {
  try {
    // Validate input
    if (id <= 0) {
      throw ArgumentError('User ID must be positive');
    }
    
    // Simulate API call
    await Future.delayed(Duration(milliseconds: 100));
    
    // Simulate error
    if (id == 999) {
      throw UserNotFoundException(id);
    }
    
    return User(id, 'Ahmed');
  } on ArgumentError catch (e) {
    print('Invalid argument: ${e.message}');
    rethrow; // Re-throw for caller to handle
  } on UserNotFoundException {
    print('User not found, returning default');
    return User(0, 'Guest');
  } catch (e, s) {
    print('Unexpected error fetching user $id: $e');
    print('Stack: $s');
    throw DatabaseException('Failed to fetch user', e);
  }
}

class UserNotFoundException implements Exception {
  final int userId;
  UserNotFoundException(this.userId);
  
  @override
  String toString() => 'User with ID $userId not found';
}

class DatabaseException implements Exception {
  final String message;
  final dynamic cause;
  DatabaseException(this.message, [this.cause]);
  
  @override
  String toString() => '$message${cause != null ? ': $cause' : ''}';
}
```

## 7.10 Exercises

### Practice Exercise 7.1

Write a function that divides two numbers and handles division by zero.

```dart
double safeDivide(double a, double b) {
  try {
    return a / b;
  } on IntegerDivisionByZeroException {
    return double.infinity;
  }
}
```

### Practice Exercise 7.2

Write a function that parses a string to an integer and handles invalid format.

```dart
int? safeParse(String input) {
  try {
    return int.parse(input);
  } on FormatException {
    return null;
  }
}
```

### Practice Exercise 7.3

Write a function that accesses an element in a list by index and handles out-of-range errors.

```dart
T? safeGet<T>(List<T> list, int index) {
  try {
    return list[index];
  } on RangeError {
    return null;
  }
}
```

### Practice Exercise 7.4

Create a custom exception for invalid email format and use it in an email validation function.

```dart
class InvalidEmailException implements Exception {
  final String email;
  InvalidEmailException(this.email);
  
  @override
  String toString() => 'Invalid email: $email';
}

void validateEmail(String email) {
  if (!email.contains('@') || !email.contains('.')) {
    throw InvalidEmailException(email);
  }
}
```

### Practice Exercise 7.5

Write a function that reads a file (simulated) and handles various file-related exceptions.

```dart
Future<String> readFile(String path) async {
  try {
    // Simulate file reading
    await Future.delayed(Duration(milliseconds: 100));
    
    if (path.isEmpty) {
      throw ArgumentError('Path cannot be empty');
    }
    
    if (!path.endsWith('.txt')) {
      throw FormatException('Only .txt files are supported');
    }
    
    if (path == 'forbidden.txt') {
      throw UnauthorizedException('Access denied');
    }
    
    return 'File content of $path';
  } catch (e) {
    print('Error reading file: $e');
    rethrow;
  }
}

class UnauthorizedException implements Exception {
  final String message;
  UnauthorizedException(this.message);
  
  @override
  String toString() => message;
}
```

### Challenge Exercise 7.6

Create a bank account system with:

- Custom exceptions for insufficient funds and invalid amounts
- Methods to deposit, withdraw, and transfer money
- Proper exception handling in all methods

```dart
class InsufficientFundsException implements Exception {
  final double balance;
  final double amount;
  InsufficientFundsException(this.balance, this.amount);
  
  @override
  String toString() => 'Insufficient funds. Balance: $balance, Requested: $amount';
}

class InvalidAmountException implements Exception {
  final double amount;
  InvalidAmountException(this.amount);
  
  @override
  String toString() => 'Invalid amount: $amount. Must be positive.';
}

class BankAccount {
  String accountNumber;
  double balance;
  
  BankAccount(this.accountNumber, this.balance);
  
  void deposit(double amount) {
    if (amount <= 0) {
      throw InvalidAmountException(amount);
    }
    balance += amount;
  }
  
  void withdraw(double amount) {
    if (amount <= 0) {
      throw InvalidAmountException(amount);
    }
    if (amount > balance) {
      throw InsufficientFundsException(balance, amount);
    }
    balance -= amount;
  }
  
  void transfer(BankAccount recipient, double amount) {
    this.withdraw(amount);
    try {
      recipient.deposit(amount);
    } catch (e) {
      // Rollback if deposit fails
      this.deposit(amount);
      rethrow;
    }
  }
}
```

### Challenge Exercise 7.7

Create a user registration system with:

- Validation for username, email, and password
- Custom exceptions for each validation failure
- A register function that validates all inputs

```dart
class InvalidUsernameException implements Exception {
  final String username;
  InvalidUsernameException(this.username);
  
  @override
  String toString() => 'Invalid username: $username. Must be 3-20 characters.';
}

class InvalidEmailException implements Exception {
  final String email;
  InvalidEmailException(this.email);
  
  @override
  String toString() => 'Invalid email: $email';
}

class InvalidPasswordException implements Exception {
  final String password;
  InvalidPasswordException(this.password);
  
  @override
  String toString() => 'Invalid password. Must be at least 8 characters.';
}

class User {
  String username;
  String email;
  String password;
  
  User(this.username, this.email, this.password);
}

class UserRegistration {
  static User register(String username, String email, String password) {
    if (username.length < 3 || username.length > 20) {
      throw InvalidUsernameException(username);
    }
    
    if (!email.contains('@') || !email.contains('.')) {
      throw InvalidEmailException(email);
    }
    
    if (password.length < 8) {
      throw InvalidPasswordException(password);
    }
    
    return User(username, email, password);
  }
}
```

### Challenge Exercise 7.8

Create a calculator that handles various arithmetic operations with proper error handling.

```dart
class CalculatorException implements Exception {
  final String message;
  CalculatorException(this.message);
  
  @override
  String toString() => 'Calculator Error: $message';
}

class Calculator {
  double add(double a, double b) => a + b;
  double subtract(double a, double b) => a - b;
  double multiply(double a, double b) => a * b;
  
  double divide(double a, double b) {
    if (b == 0) {
      throw CalculatorException('Division by zero');
    }
    return a / b;
  }
  
  double calculate(String operation, double a, double b) {
    try {
      switch (operation) {
        case '+': return add(a, b);
        case '-': return subtract(a, b);
        case '*': return multiply(a, b);
        case '/': return divide(a, b);
        default: throw CalculatorException('Invalid operation: $operation');
      }
    } catch (e) {
      if (e is CalculatorException) rethrow;
      throw CalculatorException('Failed to calculate: $e');
    }
  }
}
```

### Challenge Exercise 7.9

Create an async data fetcher with:

- Simulated network requests
- Custom exceptions for network errors and timeouts
- Retry logic for failed requests

```dart
class NetworkException implements Exception {
  final String url;
  NetworkException(this.url);
  
  @override
  String toString() => 'Network error for $url';
}

class TimeoutException implements Exception {
  final String url;
  final Duration timeout;
  TimeoutException(this.url, this.timeout);
  
  @override
  String toString() => 'Request to $url timed out after $timeout';
}

class DataFetcher {
  final Duration timeout;
  
  DataFetcher(this.timeout);
  
  Future<String> fetch(String url, {int retries = 3}) async {
    for (int attempt = 1; attempt <= retries; attempt++) {
      try {
        // Simulate network request
        await Future.delayed(Duration(milliseconds: 100));
        
        if (url == 'error.com') {
          throw NetworkException(url);
        }
        
        if (url == 'slow.com') {
          await Future.delayed(timeout + Duration(milliseconds: 100));
          throw TimeoutException(url, timeout);
        }
        
        return 'Data from $url';
      } catch (e) {
        if (attempt == retries) rethrow;
        print('Attempt $attempt failed for $url: $e');
        await Future.delayed(Duration(milliseconds: 100 * attempt));
      }
    }
    throw NetworkException(url);
  }
}
```

### Challenge Exercise 7.10

Create a comprehensive error handling system for a shopping cart with:

- Product validation
- Inventory checking
- Payment processing
- Order processing with rollback on failure

```dart
// Custom Exceptions
class ProductNotFoundException implements Exception {
  final String productId;
  ProductNotFoundException(this.productId);
  
  @override
  String toString() => 'Product not found: $productId';
}

class OutOfStockException implements Exception {
  final String productId;
  final int requested;
  final int available;
  OutOfStockException(this.productId, this.requested, this.available);
  
  @override
  String toString() => 'Out of stock for $productId. Requested: $requested, Available: $available';
}

class PaymentFailedException implements Exception {
  final double amount;
  PaymentFailedException(this.amount);
  
  @override
  String toString() => 'Payment failed for amount: $amount';
}

// Classes
class Product {
  String id;
  String name;
  double price;
  int stock;
  
  Product(this.id, this.name, this.price, this.stock);
}

class ShoppingCart {
  Map<String, int> items = {}; // productId -> quantity
  
  void addItem(String productId, int quantity) {
    items[productId] = (items[productId] ?? 0) + quantity;
  }
}

class OrderProcessor {
  final Map<String, Product> products;
  
  OrderProcessor(this.products);
  
  Future<void> processOrder(ShoppingCart cart, double balance) async {
    // Validate products
    for (var entry in cart.items.entries) {
      String productId = entry.key;
      int quantity = entry.value;
      
      if (!products.containsKey(productId)) {
        throw ProductNotFoundException(productId);
      }
      
      Product product = products[productId]!;
      if (product.stock < quantity) {
        throw OutOfStockException(productId, quantity, product.stock);
      }
    }
    
    // Calculate total
    double total = 0;
    for (var entry in cart.items.entries) {
      Product product = products[entry.key]!;
      total += product.price * entry.value;
    }
    
    // Process payment
    if (total > balance) {
      throw PaymentFailedException(total);
    }
    
    // Update inventory
    for (var entry in cart.items.entries) {
      String productId = entry.key;
      int quantity = entry.value;
      products[productId]!.stock -= quantity;
    }
    
    print('Order processed successfully! Total: $total');
  }
}
```

---

## 📖 Summary

In this file, you learned:

✅ **Types of errors** in Dart (compile-time vs runtime)  
✅ **Try-catch-finally** blocks for exception handling  
✅ **Catching specific exceptions** with `on` and `catch`  
✅ **Throwing exceptions** with `throw` and `rethrow`  
✅ **Custom exceptions** for domain-specific errors  
✅ **Stack traces** for debugging  
✅ **Assertions** for debugging and testing  
✅ **Null safety** in exception handling  
✅ **Asynchronous exception handling** with Futures and Streams  
✅ **Best practices** for robust error handling

## 🔗 Next Steps

**File 8: Asynchronous Programming** will cover:

- Futures and async/await
- Streams and event-based programming
- Completers
- Async generators
- Error handling in async code
- Working with APIs

## 📚 Additional Resources

- [Dart Exceptions](https://dart.dev/guides/language/language-tour#exceptions)
- [Dart Error Handling](https://dart.dev/guides/libraries/library-tour#exceptions)
- [Dart Asynchronous Programming](https://dart.dev/codelabs/async-await)

## ✅ Checklist

Before moving to File 8:

- [ ] Understand the difference between exceptions and errors
- [ ] Can use try-catch-finally blocks effectively
- [ ] Know how to catch specific exceptions
- [ ] Can throw and re-throw exceptions
- [ ] Can create custom exception classes
- [ ] Understand stack traces and how to use them
- [ ] Know how to use assertions for debugging
- [ ] Understand null safety in exception handling
- [ ] Can handle exceptions in asynchronous code
- [ ] Have completed all exercises in this file