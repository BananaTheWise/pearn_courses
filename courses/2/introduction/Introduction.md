# Dart Course: File 1 - Introduction to Dart

## 🎯 Course Overview

Welcome to the **Complete Dart Programming Course**! This is **File 1 of 10** that will take you from absolute beginner to proficient Dart developer.

---

## 📚 Chapter 1: Introduction to Dart

### 1.1 What is Dart?

**Dart** is a client-optimized programming language developed by Google for building fast apps on any platform. It's the primary language used for **Flutter** development.

#### Key Characteristics:

- **Compiled Language**: Dart code can be compiled to native code (AOT) or JavaScript (for web)
- **Object-Oriented**: Everything is an object (except primitive types like `int`, `bool`)
- **Strongly Typed**: Variable types are checked at compile time
- **Garbage Collected**: Automatic memory management
- **Single-threaded**: Uses an event loop for concurrency (like JavaScript)

### 1.2 Why Learn Dart?


| Feature                | Benefit                                                   |
| ---------------------- | --------------------------------------------------------- |
| 🚀 Fast Performance    | Compiles to native ARM code for mobile and desktop        |
| 🔄 Hot Reload          | See changes instantly without restarting your app         |
| 🌐 Cross-Platform      | Write once, run on mobile, web, desktop, and server       |
| 🛠️ Great Tooling      | Excellent IDE support (VS Code, Android Studio, IntelliJ) |
| 📦 Rich Ecosystem      | Access to thousands of packages via pub.dev               |
| 🎨 Flutter Integration | The language behind Flutter for beautiful UIs             |


### 1.3 Dart vs Other Languages


| Language   | Typing  | Compilation | Platform | Special Features        |
| ---------- | ------- | ----------- | -------- | ----------------------- |
| Dart       | Strong  | AOT/JIT     | Multi    | Hot Reload, Null Safety |
| JavaScript | Weak    | JIT         | Browser  | Event Loop, Dynamic     |
| Java       | Strong  | AOT/JIT     | JVM      | Threads, OOP            |
| Python     | Dynamic | Interpreted | Multi    | Easy Syntax, Scripting  |


### 1.4 Dart Use Cases

#### Primary Use Cases:

1. **Mobile App Development** (with Flutter)
2. **Web Applications** (compiled to JavaScript)
3. **Desktop Applications** (Flutter for desktop)
4. **Server-Side Applications** (Dart VM)
5. **Command-Line Tools**

#### Popular Companies Using Dart:

- Google (Flutter, Ads, internal tools)
- Alibaba (mobile apps)
- BMW (My BMW app)
- eBay (Seller app)

### 1.5 Setting Up Your Environment

#### Installation

**Option 1: Install Dart SDK (Standalone)**

```bash
# macOS
brew tap dart-lang/dart
brew install dart

# Linux (Ubuntu/Debian)
sudo apt-get install apt-transport-https
sudo sh -c 'wget -qO- https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -'
sudo sh -c 'wget -qO- https://storage.googleapis.com/download.dartlang.org/linux/debian/dart_stable.list > /etc/apt/sources.list.d/dart_stable.list'
sudo apt-get update
sudo apt-get install dart

# Windows: Download from https://dart.dev/get-dart
```

**Option 2: Install with Flutter (Recommended for mobile development)**

```bash
git clone https://github.com/flutter/flutter.git -b stable
```

#### Verify Installation

```bash
dart --version
flutter --version
```

#### IDE Setup

**Recommended IDEs:**

- **Visual Studio Code** (with Dart &amp; Flutter extensions)
- **Android Studio** (with Dart &amp; Flutter plugins)
- **IntelliJ IDEA** (with Dart plugin)

**VS Code Extensions to Install:**

- Dart
- Flutter
- Code Runner

### 1.6 Your First Dart Program

Create a file named `main.dart`:

```dart
// main.dart
void main() {
  // This is a single-line comment
  print('Hello, World!');
  
  /* This is a 
     multi-line comment */
  print('Welcome to Dart!');
}
```

#### Running the Program

```bash
# Run directly
dart run main.dart

# Or compile and run
dart compile exe main.dart
./main.exe
```

#### Expected Output:

```
Hello, World!
Welcome to Dart!
```

### 1.7 Dart Program Structure

Every Dart program has:

- Optional imports at the top
- A `main()` function as the entry point
- Your code inside the main function
- Other functions, classes, etc. can be defined

### 1.8 Dart Syntax Basics

#### Identifiers

- Can contain letters, digits, and underscores (`_`)
- Must start with a letter or underscore
- Case-sensitive (`myVar` ≠ `MyVar`)
- Cannot be Dart keywords

**Valid identifiers:**

```dart
name, age, user_name, _private, count1
```

#### Dart Keywords (Reserved Words)

These cannot be used as identifiers:

```
abstract, as, async, await, break, case, catch, class, const, continue,
default, deferred, do, dynamic, else, enum, export, external, extends,
false, final, finally, for, Function, get, hide, if, implements, import,
in, interface, is, late, library, like, main, new, null, on, operator,
part, rethrow, return, set, show, static, super, switch, this, throw,
true, try, typedef, var, void, while, with, yield
```

### 1.9 Dart Coding Conventions

1. **Naming:**
  - `lowerCamelCase` for variables and functions
  - `UpperCamelCase` for classes
  - `UPPER_CASE` for constants
2. **Indentation:** 2 spaces (no tabs)
3. **Braces:** Opening braces on the same line
4. **Semicolons:** Required at the end of statements
5. **Line Length:** Maximum 80 characters

### 1.10 Exercises

#### Practice Exercise 1.1

Create a Dart program that prints your name and age on separate lines.

```dart
void main() {
  print('Your Name');
  print('Your Age');
}
```

#### Practice Exercise 1.2

Create a Dart program that prints the following pattern:

```
*
**
***
****
```

#### Challenge Exercise 1.3

Create a Dart program that prints a simple ASCII art of your choice.

---

## Summary

In this first file, you learned:

✅ What Dart is and its key features  
✅ Why Dart is a great choice for modern development  
✅ How Dart compares to other languages  
✅ Various use cases for Dart  
✅ How to set up your development environment  
✅ Basic Dart program structure and syntax  
✅ Dart coding conventions

## Next Steps

**File 2: Dart Basics - Variables, Data Types, and Operators** will cover:

- Variable declaration and types
- Data types in Dart
- Operators and expressions
- Type inference and type conversion

## Additional Resources

- [Official Dart Documentation](https://dart.dev/guides)
- [Dart Language Tour](https://dart.dev/guides/language/language-tour)
- [DartPad - Online Dart Editor](https://dartpad.dev/)
- [Pub.dev - Dart Packages](https://pub.dev/)

## Checklist

Before moving to File 2:

- [ ] Have Dart SDK installed
- [ ] Have a code editor set up with Dart extensions
- [ ] Can run a simple Dart program
- [ ] Understand basic Dart syntax and structure
- [ ] Have completed all exercises in this file