# Dart Course: File 5 - Object-Oriented Programming

## 📚 Chapter 5: Classes and Objects

Object-Oriented Programming (OOP) is a programming paradigm that organizes code into objects that contain both data (attributes) and behavior (methods). Dart is a fully object-oriented language.

---

## 5.1 Introduction to OOP

### Four Pillars of OOP:

1. **Encapsulation**: Bundling data and methods that work on that data within one unit (class)
2. **Inheritance**: Creating new classes from existing ones
3. **Polymorphism**: Using a unified interface for different data types
4. **Abstraction**: Hiding complex implementation details

### OOP in Dart:

- Everything in Dart is an object (except primitive types like `int`, `bool`)
- Every object is an instance of a class
- Classes define the blueprint for objects

---

## 5.2 Classes and Objects

### 5.2.1 Class Definition

A class is a blueprint for creating objects.

```dart
class Person {
  // Fields (instance variables)
  String name;
  int age;
  
  // Constructor
  Person(this.name, this.age);
  
  // Methods
  void introduce() {
    print('Hello, I am $name and I am $age years old.');
  }
}
```

### 5.2.2 Creating Objects

```dart
void main() {
  // Create objects (instances of Person class)
  Person person1 = Person('Ahmed', 25);
  Person person2 = Person('Badr', 30);
  
  // Access fields
  print(person1.name); // Ahmed
  print(person2.age);  // 30
  
  // Call methods
  person1.introduce(); // Hello, I am Ahmed and I am 25 years old.
  person2.introduce(); // Hello, I am Badr and I am 30 years old.
  
  // Modify fields
  person1.age = 26;
  print(person1.age); // 26
}
```

### 5.2.3 The `new` Keyword (Optional)

In Dart, the `new` keyword is optional when creating objects:

```dart
// These are equivalent
Person person1 = Person('Ahmed', 25);
Person person2 = new Person('Badr', 30);
```

## 5.3 Constructors

### 5.3.1 Default Constructor

If you don't define any constructor, Dart provides a default constructor.

```dart
class Person {
  String name = 'Unknown';
  int age = 0;
  
  // Dart provides a default constructor
}

void main() {
  Person person = Person();
  print(person.name); // Unknown
}
```

### 5.3.2 Parameterized Constructor

```dart
class Person {
  String name;
  int age;
  
  // Parameterized constructor
  Person(String name, int age) {
    this.name = name;
    this.age = age;
  }
}
```

### 5.3.3 Constructor with `this` Shorthand

```dart
class Person {
  String name;
  int age;
  
  // Using this. parameter shorthand
  Person(this.name, this.age);
}
```

### 5.3.4 Named Constructors

You can define multiple constructors with different names.

```dart
class Person {
  String name;
  int age;
  
  // Default constructor
  Person(this.name, this.age);
  
  // Named constructor
  Person.fromBirthYear(String name, int birthYear) {
    this.name = name;
    this.age = DateTime.now().year - birthYear;
  }
  
  // Another named constructor
  Person.unknown() {
    name = 'Unknown';
    age = 0;
  }
}

void main() {
  Person person1 = Person('Ahmed', 25);
  Person person2 = Person.fromBirthYear('Badr', 1990);
  Person person3 = Person.unknown();
}
```

### 5.3.5 Initializer Lists

Used to initialize fields before the constructor body runs.

```dart
class Person {
  final String name;
  final int age;
  final String email;
  
  Person(this.name, this.age, String emailDomain)
    : email = '$name@$emailDomain';
}

void main() {
  Person person = Person('Ahmed', 25, 'example.com');
  print(person.email); // Ahmed@example.com
}
```

### 5.3.6 Redirecting Constructors

One constructor can redirect to another.

```dart
class Person {
  String name;
  int age;
  
  Person(this.name, this.age);
  
  // Redirects to the main constructor
  Person.fromBirthYear(String name, int birthYear)
    : this(name, DateTime.now().year - birthYear);
}

void main() {
  Person person = Person.fromBirthYear('Ahmed', 1995);
  print(person.age); // 2026 - 1995 = 31 (assuming current year is 2026)
}
```

### 5.3.7 Constant Constructors

Used to create compile-time constant objects.

```dart
class Point {
  final int x;
  final int y;
  
  const Point(this.x, this.y);
}

void main() {
  const point1 = Point(10, 20);
  const point2 = Point(10, 20);
  
  print(point1 == point2); // true - same instance
}
```

### 5.3.8 Factory Constructors

Used when you need to control the creation of objects.

```dart
class Logger {
  final String name;
  static final Map<String, Logger> _cache = {};
  
  // Factory constructor
  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name]!;
    } else {
      final logger = Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }
  
  // Private constructor
  Logger._internal(this.name);
}

void main() {
  Logger logger1 = Logger('myLogger');
  Logger logger2 = Logger('myLogger');
  
  print(identical(logger1, logger2)); // true - same instance
}
```

## 5.4 Instance Variables (Fields)

### 5.4.1 Declaring Instance Variables

```dart
class Person {
  // Instance variables (fields)
  String name;
  int age;
  String? email; // Nullable
  final String id; // Final (immutable)
  late String address; // Late initialization
  
  Person(this.name, this.age, this.id);
}
```

### 5.4.2 Late Variables

The `late` keyword allows you to declare non-nullable variables that are initialized after their declaration.

```dart
class Person {
  late String fullName;
  final String firstName;
  final String lastName;
  
  Person(this.firstName, this.lastName) {
    fullName = '$firstName $lastName';
  }
}
```

### 5.4.3 Final Variables

`final` variables can only be set once and cannot be changed.

```dart
class Person {
  final String id;
  final String name;
  
  Person(this.id, this.name);
}
```

## 5.5 Methods

### 5.5.1 Instance Methods

Methods that operate on an instance of the class.

```dart
class Person {
  String name;
  int age;
  
  Person(this.name, this.age);
  
  // Instance method
  void introduce() {
    print('Hello, I am $name.');
  }
  
  // Method with parameters
  void celebrateBirthday() {
    age++;
    print('Happy Birthday, $name! You are now $age years old.');
  }
  
  // Method with return value
  int getAgeInMonths() {
    return age * 12;
  }
}
```

### 5.5.2 Static Methods

Methods that belong to the class rather than instances.

```dart
class MathUtils {
  // Static method
  static int add(int a, int b) {
    return a + b;
  }
  
  static int multiply(int a, int b) {
    return a * b;
  }
}

void main() {
  // Call static method without creating an instance
  int sum = MathUtils.add(5, 3);
  int product = MathUtils.multiply(5, 3);
}
```

### 5.5.3 Getters and Setters

Special methods to get and set the values of instance variables.

```dart
class Person {
  String _name; // Private variable (convention)
  int _age;
  
  Person(this._name, this._age);
  
  // Getter
  String get name => _name;
  
  // Setter
  set name(String newName) {
    if (newName.isNotEmpty) {
      _name = newName;
    }
  }
  
  // Getter with computation
  int get ageInMonths => _age * 12;
  
  // Setter with validation
  set age(int newAge) {
    if (newAge >= 0) {
      _age = newAge;
    }
  }
}

void main() {
  Person person = Person('Ahmed', 25);
  
  print(person.name); // Ahmed
  person.name = 'Badr';
  print(person.name); // Badr
  
  print(person.ageInMonths); // 300
  person.age = 30;
  print(person.ageInMonths); // 360
}

```

## 5.6 Access Modifiers

Dart doesn't have traditional access modifiers like `public`, `private`, `protected`. Instead:

- **Public**: All identifiers are public by default
- **Private**: Prefix with underscore (`_`)

```dart
class Person {
  String name; // Public
  int _age;   // Private (convention)
  
  Person(this.name, this._age);
  
  // Public method
  void introduce() {
    print('Hello, I am $name and I am $_age years old.');
  }
  
  // Private method
  int _calculateAgeInMonths() {
    return _age * 12;
  }
}

void main() {
  Person person = Person('Ahmed', 25);
  
  print(person.name); // OK
  // print(person._age); // ERROR: Private member
  person.introduce(); // OK
  // person._calculateAgeInMonths(); // ERROR: Private member
}
```

## 5.7 The `this` Keyword

The `this` keyword refers to the current instance of the class.

```dart
class Person {
  String name;
  int age;
  
  Person(String name, int age) {
    this.name = name; // this refers to the instance variable
    this.age = age;
  }
  
  void introduce() {
    print('Hello, I am ${this.name}.'); // this is optional here
  }
}
```

## 5.8 Cascades (`..` operator)

The cascade operator allows you to make a sequence of operations on the same object.

```dart
class Person {
  String name;
  int age;
  String city;
  
  Person(this.name, this.age, this.city);
  
  void introduce() {
    print('Hello, I am $name from $city.');
  }
  
  void celebrateBirthday() {
    age++;
  }
}

void main() {
  // Without cascade
  Person person1 = Person('Ahmed', 25, 'Cairo');
  person1.celebrateBirthday();
  person1.introduce();
  
  // With cascade
  Person person2 = Person('Badr', 30, 'Alexandria')
    ..celebrateBirthday()
    ..introduce();
}
```

## 5.9 Object Initialization

### 5.9.1 Initializer Lists

```dart
class Rectangle {
  final int width;
  final int height;
  final int area;
  
  Rectangle(this.width, this.height) : area = width * height;
}
```

### 5.9.2 Assertions

Used to validate inputs during development.

```dart
class Person {
  String name;
  int age;
  
  Person(this.name, this.age) : assert(age >= 0, 'Age cannot be negative');
}
```

## 5.10 Exercises

### Practice Exercise 5.1

Create a `Book` class with fields for title, author, and pages. Add a method to display book information.

```dart
class Book {
  String title;
  String author;
  int pages;
  
  Book(this.title, this.author, this.pages);
  
  void displayInfo() {
    print('Title: $title, Author: $author, Pages: $pages');
  }
}
```

### Practice Exercise 5.2

Create a `Calculator` class with static methods for addition, subtraction, multiplication, and division.

```dart
class Calculator {
  static double add(double a, double b) => a + b;
  static double subtract(double a, double b) => a - b;
  static double multiply(double a, double b) => a * b;
  static double divide(double a, double b) => a / b;
}
```

### Practice Exercise 5.3

Create a `Student` class with private fields for name and grade. Add getter and setter methods for the grade with validation (0-100).

```dart
class Student {
  String _name;
  int _grade;
  
  Student(this._name, this._grade);
  
  String get name => _name;
  
  int get grade => _grade;
  set grade(int newGrade) {
    if (newGrade >= 0 && newGrade <= 100) {
      _grade = newGrade;
    }
  }
}
```

### Practice Exercise 5.4

Create a `BankAccount` class with fields for account number, account holder name, and balance. Add methods to deposit and withdraw money.

```dart
class BankAccount {
  String accountNumber;
  String accountHolder;
  double balance;
  
  BankAccount(this.accountNumber, this.accountHolder, this.balance);
  
  void deposit(double amount) {
    if (amount > 0) {
      balance += amount;
      print('Deposited: $amount. New balance: $balance');
    }
  }
  
  void withdraw(double amount) {
    if (amount > 0 && amount <= balance) {
      balance -= amount;
      print('Withdrew: $amount. New balance: $balance');
    } else {
      print('Insufficient funds or invalid amount.');
    }
  }
}
```

### Practice Exercise 5.5

Create a `Rectangle` class with width and height. Add a constructor that takes these values and a method to calculate the area.

```dart
class Rectangle {
  double width;
  double height;
  
  Rectangle(this.width, this.height);
  
  double getArea() => width * height;
}
```

### Challenge Exercise 5.6

Create a `Product` class with fields for name, price, and quantity. Add named constructors to create products with and without quantity (default to 1).

```dart
class Product {
  String name;
  double price;
  int quantity;
  
  Product(this.name, this.price, [this.quantity = 1]);
  
  Product.withoutQuantity(this.name, this.price) : quantity = 1;
  
  double getTotalPrice() => price * quantity;
}
```

### Challenge Exercise 5.7

Create a `Car` class with fields for make, model, and year. Add a factory constructor that creates a Car from a string in the format "make,model,year".

```dart
class Car {
  String make;
  String model;
  int year;
  
  Car(this.make, this.model, this.year);
  
  factory Car.fromString(String carString) {
    List<String> parts = carString.split(',');
    return Car(parts[0], parts[1], int.parse(parts[2]));
  }
}
```

### Challenge Exercise 5.8

Create a `Temperature` class that can store temperature in Celsius and Fahrenheit. Add getters and setters to convert between them automatically.

```dart
class Temperature {
  double _celsius;
  
  Temperature(this._celsius);
  
  double get celsius => _celsius;
  set celsius(double c) => _celsius = c;
  
  double get fahrenheit => (_celsius * 9/5) + 32;
  set fahrenheit(double f) => _celsius = (f - 32) * 5/9;
}
```

### Challenge Exercise 5.9

Create a `ShoppingCart` class that can add products, remove products, and calculate the total price. Use a list to store products.

```dart
class Product {
  String name;
  double price;
  
  Product(this.name, this.price);
}

class ShoppingCart {
  List<Product> products = [];
  
  void addProduct(Product product) {
    products.add(product);
  }
  
  void removeProduct(Product product) {
    products.remove(product);
  }
  
  double getTotalPrice() {
    return products.fold(0, (total, product) => total + product.price);
  }
}
```

### Challenge Exercise 5.10

Create a `Person` class with a nested `Address` class. The Person class should have a field of type Address.

```dart
class Person {
  String name;
  int age;
  Address address;
  
  Person(this.name, this.age, this.address);
}

class Address {
  String street;
  String city;
  String country;
  
  Address(this.street, this.city, this.country);
}

void main() {
  Address address = Address('123 Main St', 'Cairo', 'Egypt');
  Person person = Person('Ahmed', 25, address);
  print('${person.name} lives at ${person.address.street}, ${person.address.city}');
}
```

---

## 📖 Summary

In this file, you learned:

✅ **OOP fundamentals** in Dart  
✅ **Classes and objects** - the building blocks of OOP  
✅ **Constructors** - various types and their uses  
✅ **Instance variables** (fields) and methods  
✅ **Access modifiers** (public and private)  
✅ **Getters and setters** for controlled access  
✅ **Static members** for class-level functionality  
✅ **Cascade operator** for method chaining  
✅ **Factory constructors** for controlled object creation  
✅ **Initializer lists** for complex initialization

## 🔗 Next Steps

**File 6: Inheritance, Polymorphism, and Abstraction** will cover:

- Inheritance (extending classes)
- Method overriding
- The `super` keyword
- Polymorphism
- Abstract classes and methods
- Interfaces
- Mixins

## 📚 Additional Resources

- [Dart Classes](https://dart.dev/guides/language/language-tour#classes)
- [Dart Constructors](https://dart.dev/guides/language/constructors)
- [Dart OOP Concepts](https://dart.dev/guides/language/language-tour#important-concepts)

## ✅ Checklist

Before moving to File 6:

- [ ] Understand class definition and object creation
- [ ] Can use different types of constructors
- [ ] Know how to use instance variables and methods
- [ ] Understand access modifiers and encapsulation
- [ ] Can use getters and setters
- [ ] Understand static members
- [ ] Can use cascade operator
- [ ] Have completed all exercises in this file