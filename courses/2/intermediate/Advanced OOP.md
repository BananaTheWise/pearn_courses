# Dart Course: File 6 - Inheritance, Polymorphism, and Abstraction

## 📚 Chapter 6: Advanced OOP Concepts

This file builds on the OOP fundamentals from File 5, covering inheritance, polymorphism, abstract classes, interfaces, and mixins.

---

## 6.1 Inheritance

Inheritance allows a class to inherit properties and methods from another class. The class that inherits is called the **subclass** or **child class**, and the class being inherited from is called the **superclass** or **parent class**.

### 6.1.1 Basic Inheritance

#### Syntax:

```dart
class ChildClass extends ParentClass {
  // Additional fields and methods
}
```

#### Example:

```dart
class Animal {
  String name;
  int age;
  
  Animal(this.name, this.age);
  
  void eat() {
    print('$name is eating.');
  }
  
  void sleep() {
    print('$name is sleeping.');
  }
}

class Dog extends Animal {
  String breed;
  
  Dog(String name, int age, this.breed) : super(name, age);
  
  void bark() {
    print('$name says Woof!');
  }
}

void main() {
  Dog myDog = Dog('Buddy', 3, 'Golden Retriever');
  myDog.eat();   // Inherited from Animal
  myDog.sleep(); // Inherited from Animal
  myDog.bark();  // Defined in Dog
  print('Breed: ${myDog.breed}'); // Golden Retriever
}
```

### 6.1.2 The `super` Keyword

The `super` keyword is used to access members of the parent class.

```dart
class Animal {
  String name;
  
  Animal(this.name);
  
  void makeSound() {
    print('$name makes a sound.');
  }
}

class Cat extends Animal {
  Cat(String name) : super(name);
  
  @override
  void makeSound() {
    super.makeSound(); // Call parent method
    print('$name says Meow!');
  }
}

void main() {
  Cat myCat = Cat('Whiskers');
  myCat.makeSound();
  // Output:
  // Whiskers makes a sound.
  // Whiskers says Meow!
}
```

### 6.1.3 Constructor Inheritance

When a subclass is created, it must call one of the superclass's constructors.

```dart
class Vehicle {
  String manufacturer;
  int year;
  
  Vehicle(this.manufacturer, this.year);
  
  Vehicle.unknown() : manufacturer = 'Unknown', year = 0;
}

class Car extends Vehicle {
  String model;
  
  // Must call superclass constructor
  Car(String manufacturer, int year, this.model) : super(manufacturer, year);
  
  // Can also call named constructor
  Car.unknownModel(this.model) : super.unknown();
}

void main() {
  Car myCar = Car('Toyota', 2020, 'Camry');
  Car unknownCar = Car.unknownModel('Model X');
}
```

### 6.1.4 Method Overriding

A subclass can override methods from its superclass.

```dart
class Animal {
  void makeSound() {
    print('Some generic animal sound');
  }
}

class Dog extends Animal {
  @override
  void makeSound() {
    print('Woof! Woof!');
  }
}

class Cat extends Animal {
  @override
  void makeSound() {
    print('Meow!');
  }
}

void main() {
  Animal myDog = Dog();
  Animal myCat = Cat();
  
  myDog.makeSound(); // Woof! Woof!
  myCat.makeSound(); // Meow!
}
```

### 6.1.5 The `@override` Annotation

The `@override` annotation indicates that a member is overriding a member from a superclass. It's optional but recommended.

```dart
class Animal {
  void makeSound() {}
}

class Dog extends Animal {
  @override
  void makeSound() {
    print('Woof!');
  }
}
```

## 6.2 Polymorphism

Polymorphism allows objects of different classes to be treated as objects of a common superclass. It enables one interface to represent different underlying forms (data types).

### 6.2.1 Runtime Polymorphism (Method Overriding)

```dart
class Shape {
  void draw() {
    print('Drawing a shape');
  }
}

class Circle extends Shape {
  @override
  void draw() {
    print('Drawing a circle');
  }
}

class Square extends Shape {
  @override
  void draw() {
    print('Drawing a square');
  }
}

void main() {
  Shape shape1 = Circle();
  Shape shape2 = Square();
  
  shape1.draw(); // Drawing a circle
  shape2.draw(); // Drawing a square
}
```

### 6.2.2 Polymorphism with Lists

```dart
class Animal {
  void makeSound();
}

class Dog extends Animal {
  @override
  void makeSound() => print('Woof!');
}

class Cat extends Animal {
  @override
  void makeSound() => print('Meow!');
}

class Cow extends Animal {
  @override
  void makeSound() => print('Moo!');
}

void main() {
  List<Animal> animals = [Dog(), Cat(), Cow()];
  
  for (Animal animal in animals) {
    animal.makeSound();
  }
  // Output:
  // Woof!
  // Meow!
  // Moo!
}
```

### 6.2.3 Polymorphism with Functions

```dart
class Animal {
  void makeSound();
}

class Dog extends Animal {
  @override
  void makeSound() => print('Woof!');
}

class Cat extends Animal {
  @override
  void makeSound() => print('Meow!');
}

void makeAnimalSounds(List<Animal> animals) {
  for (Animal animal in animals) {
    animal.makeSound();
  }
}

void main() {
  List<Animal> animals = [Dog(), Cat(), Dog(), Cat()];
  makeAnimalSounds(animals);
}
```

## 6.3 Abstract Classes

An abstract class cannot be instantiated directly. It's used to define a common interface for its subclasses.

### 6.3.1 Defining Abstract Classes

```dart
abstract class Shape {
  // Abstract method (no implementation)
  void draw();
  
  // Concrete method
  void introduce() {
    print('This is a shape.');
  }
}

class Circle extends Shape {
  @override
  void draw() {
    print('Drawing a circle');
  }
}

void main() {
  // Shape shape = Shape(); // ERROR: Cannot instantiate abstract class
  Shape circle = Circle();
  circle.draw();
  circle.introduce();
}
```

### 6.3.2 Abstract Methods

Abstract methods are declared but don't have an implementation. Subclasses must override them.

```dart
abstract class Animal {
  String name;
  
  Animal(this.name);
  
  // Abstract method
  void makeSound();
  
  // Abstract method with parameters
  void eat(String food);
}

class Dog extends Animal {
  Dog(String name) : super(name);
  
  @override
  void makeSound() {
    print('$name says Woof!');
  }
  
  @override
  void eat(String food) {
    print('$name is eating $food.');
  }
}
```

### 6.3.3 Abstract Classes vs Interfaces


| Feature                   | Abstract Class | Interface                  |
| ------------------------- | -------------- | -------------------------- |
| Can be instantiated       | ❌ No           | ❌ No                       |
| Can have concrete methods | ✅ Yes          | ✅ Yes (since Dart 2.7)     |
| Can have abstract methods | ✅ Yes          | ✅ Yes                      |
| Can have fields           | ✅ Yes          | ❌ No (only since Dart 2.7) |
| Can have constructors     | ✅ Yes          | ❌ No                       |
| Supports inheritance      | ✅ Single       | ❌ Multiple                 |
| Supports implementation   | ✅ Yes          | ✅ Yes                      |


## 6.4 Interfaces

An interface defines a contract that classes must follow. In Dart, every class implicitly defines an interface.

### 6.4.1 Defining and Implementing Interfaces

```dart
// Define an interface (just a class with abstract methods)
abstract class Flyable {
  void fly();
}

abstract class Swimmable {
  void swim();
}

// Implement multiple interfaces
class Duck implements Flyable, Swimmable {
  @override
  void fly() {
    print('Duck is flying');
  }
  
  @override
  void swim() {
    print('Duck is swimming');
  }
}
```

### 6.4.2 Class as Interface

In Dart, you can use any class as an interface.

```dart
class Logger {
  void log(String message);
}

class FileLogger implements Logger {
  @override
  void log(String message) {
    print('[FILE] $message');
  }
}

class ConsoleLogger implements Logger {
  @override
  void log(String message) {
    print('[CONSOLE] $message');
  }
}
```

### 6.4.3 Interface with Default Implementation

Since Dart 2.7, interfaces can have default implementations.

```dart
abstract class Logger {
  void log(String message);
  
  // Default implementation
  void logError(String message) {
    log('[ERROR] $message');
  }
}

class ConsoleLogger implements Logger {
  @override
  void log(String message) {
    print(message);
  }
  
  // logError is inherited
}
```

## 6.5 Mixins

Mixins are a way to reuse code in multiple class hierarchies. They provide a way to share methods among unrelated classes.

### 6.5.1 Basic Mixins

```dart
// Define a mixin
mixin Loggable {
  void log(String message) {
    print('[LOG] $message');
  }
}

// Use mixin in a class
class User with Loggable {
  String name;
  
  User(this.name);
  
  void introduce() {
    log('User $name is introducing themselves');
    print('Hello, I am $name.');
  }
}

void main() {
  User user = User('Ahmed');
  user.introduce();
  // Output:
  // [LOG] User Ahmed is introducing themselves
  // Hello, I am Ahmed.
}
```

### 6.5.2 Mixin Restrictions

Mixins cannot:

- Have constructors
- Extend other classes (except `Object`)
- Be used with classes that have the same superclass (unless that superclass is `Object`)

### 6.5.3 Multiple Mixins

```dart
mixin Loggable {
  void log(String message) => print('[LOG] $message');
}

mixin Timestamped {
  DateTime get timestamp => DateTime.now();
}

class AuditEntry with Loggable, Timestamped {
  String action;
  
  AuditEntry(this.action) {
    log('Action: $action at $timestamp');
  }
}
```

### 6.5.4 Mixin on Clause

You can restrict which classes can use a mixin.

```dart
class Animal {
  String name;
  Animal(this.name);
}

mixin Flyable on Animal {
  void fly() {
    print('$name is flying');
  }
}

class Bird extends Animal with Flyable {
  Bird(String name) : super(name);
}

void main() {
  Bird bird = Bird('Eagle');
  bird.fly(); // Eagle is flying
}
```

## 6.6 The `is` and `as` Operators

### 6.6.1 Type Checking with `is`

```dart
class Animal {}
class Dog extends Animal {}
class Cat extends Animal {}

void main() {
  Animal animal = Dog();
  
  if (animal is Dog) {
    print('It is a dog');
  } else if (animal is Cat) {
    print('It is a cat');
  } else {
    print('It is some other animal');
  }
}
```

### 6.6.2 Type Casting with `as`

```dart
class Animal {}
class Dog extends Animal {
  void bark() => print('Woof!');
}

void main() {
  Animal animal = Dog();
  
  if (animal is Dog) {
    // Safe to cast
    Dog dog = animal as Dog;
    dog.bark();
  }
  
  // Or directly (throws if cast fails)
  // Dog dog = animal as Dog; // Use only if sure
}
```

## 6.7 Method Overriding Rules

1. The overriding method must have the same name as the superclass method
2. The overriding method must have compatible parameter types
3. The overriding method must have a compatible return type (covariant return types are allowed)
4. The overriding method cannot have a more restrictive access modifier

### 6.7.1 Covariant Return Types

Dart allows covariant return types, meaning the overriding method can return a more specific type than the superclass method.

```dart
class Animal {
  Animal reproduce() {
    return Animal();
  }
}

class Dog extends Animal {
  @override
  Dog reproduce() {
    return Dog();
  }
}
```

## 6.8 The `dynamic` Type and Polymorphism

```dart
class Animal {
  void makeSound();
}

class Dog extends Animal {
  @override
  void makeSound() => print('Woof!');
}

void main() {
  // Using dynamic type
  dynamic animal = Dog();
  animal.makeSound(); // Woof!
  
  // But dynamic disables type checking
  animal.nonExistentMethod(); // No error at compile time, but runtime error
}
```

## 6.9 Practical Examples

### Example 1: Shape Hierarchy

```dart
abstract class Shape {
  void draw();
  double get area;
}

class Circle extends Shape {
  double radius;
  
  Circle(this.radius);
  
  @override
  void draw() => print('Drawing a circle with radius $radius');
  
  @override
  double get area => 3.14159 * radius * radius;
}

class Rectangle extends Shape {
  double width;
  double height;
  
  Rectangle(this.width, this.height);
  
  @override
  void draw() => print('Drawing a rectangle $width x $height');
  
  @override
  double get area => width * height;
}

void printShapeInfo(Shape shape) {
  shape.draw();
  print('Area: ${shape.area}');
}

void main() {
  printShapeInfo(Circle(5));
  printShapeInfo(Rectangle(4, 6));
}
```

### Example 2: Employee Hierarchy

```dart
abstract class Employee {
  String name;
  double salary;
  
  Employee(this.name, this.salary);
  
  void work();
  double getBonus();
}

class Developer extends Employee {
  String programmingLanguage;
  
  Developer(String name, double salary, this.programmingLanguage)
    : super(name, salary);
  
  @override
  void work() => print('$name is coding in $programmingLanguage');
  
  @override
  double getBonus() => salary * 0.2;
}

class Manager extends Employee {
  int teamSize;
  
  Manager(String name, double salary, this.teamSize)
    : super(name, salary);
  
  @override
  void work() => print('$name is managing a team of $teamSize');
  
  @override
  double getBonus() => salary * 0.3;
}

void main() {
  Employee dev = Developer('Ahmed', 50000, 'Dart');
  Employee manager = Manager('Badr', 80000, 10);
  
  dev.work();
  print('Bonus: ${dev.getBonus()}');
  
  manager.work();
  print('Bonus: ${manager.getBonus()}');
}
```

### Example 3: Multiple Inheritance with Mixins

```dart
mixin CanFly {
  void fly() => print('Flying');
}

mixin CanSwim {
  void swim() => print('Swimming');
}

mixin CanRun {
  void run() => print('Running');
}

class Animal {
  String name;
  Animal(this.name);
}

class Duck extends Animal with CanFly, CanSwim, CanRun {
  Duck(String name) : super(name);
}

class Fish extends Animal with CanSwim {
  Fish(String name) : super(name);
}

class Bird extends Animal with CanFly, CanRun {
  Bird(String name) : super(name);
}

void main() {
  Duck duck = Duck('Donald');
  duck.fly();
  duck.swim();
  duck.run();
}
```

## 6.10 Exercises

### Practice Exercise 6.1

Create a `Bird` class that extends an `Animal` class. Override the `makeSound` method to return "Chirp".

```dart
class Animal {
  void makeSound() => print('Some sound');
}

class Bird extends Animal {
  @override
  void makeSound() => print('Chirp');
}
```

### Practice Exercise 6.2

Create an abstract class `Vehicle` with abstract methods `start()` and `stop()`. Create a `Car` class that implements these methods.

```dart
abstract class Vehicle {
  void start();
  void stop();
}

class Car extends Vehicle {
  @override
  void start() => print('Car started');
  
  @override
  void stop() => print('Car stopped');
}
```

### Practice Exercise 6.3

Create a `Rectangle` class and a `Square` class that both extend a `Shape` class. Override a `draw()` method in both.

```dart
class Shape {
  void draw() => print('Drawing a shape');
}

class Rectangle extends Shape {
  @override
  void draw() => print('Drawing a rectangle');
}

class Square extends Shape {
  @override
  void draw() => print('Drawing a square');
}
```

### Practice Exercise 6.4

Create an interface `Logger` with a `log()` method. Create two classes `FileLogger` and `ConsoleLogger` that implement this interface.

```dart
abstract class Logger {
  void log(String message);
}

class FileLogger implements Logger {
  @override
  void log(String message) => print('[FILE] $message');
}

class ConsoleLogger implements Logger {
  @override
  void log(String message) => print('[CONSOLE] $message');
}
```

### Practice Exercise 6.5

Create a mixin `Serializable` with a `toJson()` method. Create a `User` class that uses this mixin.

```dart
mixin Serializable {
  Map<String, dynamic> toJson() => {'id': 1, 'name': 'Default'}; // Default implementation
}

class User with Serializable {
  int id;
  String name;
  
  User(this.id, this.name);
  
  @override
  Map<String, dynamic> toJson() => {'id': id, 'name': name};
}
```

### Challenge Exercise 6.6

Create a class hierarchy for a zoo management system with the following requirements:

- Abstract `Animal` class with `name`, `age`, and abstract methods `makeSound()` and `feed()`
- `Mammal` class extending `Animal` with a `furColor` field
- `Bird` class extending `Animal` with a `canFly` field
- `Lion` class extending `Mammal` that implements all abstract methods
- `Eagle` class extending `Bird` that implements all abstract methods

```dart
abstract class Animal {
  String name;
  int age;
  
  Animal(this.name, this.age);
  
  void makeSound();
  void feed();
}

class Mammal extends Animal {
  String furColor;
  
  Mammal(String name, int age, this.furColor) : super(name, age);
}

class Bird extends Animal {
  bool canFly;
  
  Bird(String name, int age, this.canFly) : super(name, age);
}

class Lion extends Mammal {
  Lion(String name, int age, String furColor) : super(name, age, furColor);
  
  @override
  void makeSound() => print('$name roars!');
  
  @override
  void feed() => print('Feeding $name with meat');
}

class Eagle extends Bird {
  Eagle(String name, int age) : super(name, age, true);
  
  @override
  void makeSound() => print('$name screeches!');
  
  @override
  void feed() => print('Feeding $name with fish');
}
```

### Challenge Exercise 6.7

Create a mixin `Comparable` that adds comparison functionality to classes. Implement the `compareTo` method that returns -1, 0, or 1 based on comparison.

```dart
mixin Comparable<T> {
  int compareTo(T other);
}

class Person with Comparable<Person> {
  String name;
  int age;
  
  Person(this.name, this.age);
  
  @override
  int compareTo(Person other) {
    if (age < other.age) return -1;
    if (age > other.age) return 1;
    return 0;
  }
}
```

### Challenge Exercise 6.8

Create an abstract class `Database` with abstract methods for CRUD operations. Create a `MySqlDatabase` class that implements these methods.

```dart
abstract class Database {
  void connect();
  void disconnect();
  void create(String table, Map<String, dynamic> data);
  Map<String, dynamic>? read(String table, int id);
  void update(String table, int id, Map<String, dynamic> data);
  void delete(String table, int id);
}

class MySqlDatabase extends Database {
  @override
  void connect() => print('Connected to MySQL database');
  
  @override
  void disconnect() => print('Disconnected from MySQL database');
  
  @override
  void create(String table, Map<String, dynamic> data) {
    print('Created record in $table: $data');
  }
  
  @override
  Map<String, dynamic>? read(String table, int id) {
    print('Reading record $id from $table');
    return {'id': id, 'name': 'Sample'}; // Simulated data
  }
  
  @override
  void update(String table, int id, Map<String, dynamic> data) {
    print('Updated record $id in $table with $data');
  }
  
  @override
  void delete(String table, int id) {
    print('Deleted record $id from $table');
  }
}
```

### Challenge Exercise 6.9

Create a payment processing system with:

- Abstract `PaymentMethod` class with `processPayment(double amount)` method
- `CreditCardPayment` class extending `PaymentMethod`
- `PayPalPayment` class extending `PaymentMethod`
- `BankTransferPayment` class extending `PaymentMethod`
- A function that processes payment using polymorphism

```dart
abstract class PaymentMethod {
  void processPayment(double amount);
}

class CreditCardPayment extends PaymentMethod {
  String cardNumber;
  
  CreditCardPayment(this.cardNumber);
  
  @override
  void processPayment(double amount) {
    print('Processing $amount using credit card $cardNumber');
  }
}

class PayPalPayment extends PaymentMethod {
  String email;
  
  PayPalPayment(this.email);
  
  @override
  void processPayment(double amount) {
    print('Processing $amount using PayPal account $email');
  }
}

class BankTransferPayment extends PaymentMethod {
  String accountNumber;
  
  BankTransferPayment(this.accountNumber);
  
  @override
  void processPayment(double amount) {
    print('Processing $amount via bank transfer to $accountNumber');
  }
}

void processPayment(PaymentMethod method, double amount) {
  method.processPayment(amount);
}

void main() {
  processPayment(CreditCardPayment('1234-5678'), 100.0);
  processPayment(PayPalPayment('user@example.com'), 50.0);
  processPayment(BankTransferPayment('987654321'), 200.0);
}
```

### Challenge Exercise 6.10

Create a game character system with:

- Abstract `Character` class with `attack()`, `defend()`, and `specialAbility()` methods
- Mixin `CanFly` with `fly()` method
- Mixin `CanSwim` with `swim()` method
- `Hero` class extending `Character` with `CanFly`
- `Villain` class extending `Character` with `CanSwim`
- `Aquaman` class extending `Character` with both `CanFly` and `CanSwim`

```dart
abstract class Character {
  String name;
  int health;
  
  Character(this.name, this.health);
  
  void attack();
  void defend();
  void specialAbility();
}

mixin CanFly {
  void fly() => print('Flying through the air');
}

mixin CanSwim {
  void swim() => print('Swimming through water');
}

class Hero extends Character with CanFly {
  Hero(String name, int health) : super(name, health);
  
  @override
  void attack() => print('$name attacks with sword');
  
  @override
  void defend() => print('$name raises shield');
  
  @override
  void specialAbility() {
    fly();
    print('$name uses healing power');
  }
}

class Villain extends Character with CanSwim {
  Villain(String name, int health) : super(name, health);
  
  @override
  void attack() => print('$name attacks with dark magic');
  
  @override
  void defend() => print('$name creates dark barrier');
  
  @override
  void specialAbility() {
    swim();
    print('$name summons sea creatures');
  }
}

class Aquaman extends Character with CanFly, CanSwim {
  Aquaman(String name, int health) : super(name, health);
  
  @override
  void attack() => print('$name attacks with trident');
  
  @override
  void defend() => print('$name uses water shield');
  
  @override
  void specialAbility() {
    fly();
    swim();
    print('$name commands the ocean');
  }
}
```

---

## 📖 Summary

In this file, you learned:

✅ **Inheritance** - extending classes to create specialized types  
✅ **Polymorphism** - treating different objects uniformly through a common interface  
✅ **Method overriding** - providing specific implementations in subclasses  
✅ **Abstract classes** - defining common interfaces that cannot be instantiated  
✅ **Interfaces** - defining contracts that classes must implement  
✅ **Mixins** - reusing code across different class hierarchies  
✅ **Type checking** with `is` and type casting with `as`  
✅ **Covariant return types** in method overriding  
✅ **Practical applications** of OOP principles

## 🔗 Next Steps

**File 7: Exception Handling and Error Management** will cover:

- Try, catch, and finally blocks
- Throwing exceptions
- Custom exceptions
- Stack traces and error handling
- Assertions
- Null safety in error handling

## 📚 Additional Resources

- [Dart Inheritance](https://dart.dev/guides/language/language-tour#extending-a-class)
- [Dart Abstract Classes](https://dart.dev/guides/language/language-tour#abstract-classes)
- [Dart Interfaces and Mixins](https://dart.dev/guides/language/language-tour#interfaces-and-mixins)
- [Dart Polymorphism](https://dart.dev/guides/language/language-tour#implicit-interfaces)

## ✅ Checklist

Before moving to File 7:

- [ ] Understand inheritance and how to extend classes
- [ ] Know how to override methods in subclasses
- [ ] Can use the `super` keyword properly
- [ ] Understand polymorphism and its benefits
- [ ] Can create and use abstract classes
- [ ] Can define and implement interfaces
- [ ] Can create and use mixins
- [ ] Understand type checking and casting
- [ ] Have completed all exercises in this file