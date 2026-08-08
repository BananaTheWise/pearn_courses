# Dart Course: File 8 - Asynchronous Programming

## 📚 Chapter 8: Async Programming in Dart

Asynchronous programming allows your code to perform time-consuming operations (like network requests, file I/O, or database queries) without blocking the execution of other code. Dart provides several mechanisms for writing async code.

---

## 8.1 Understanding Asynchrony

### Synchronous vs Asynchronous

**Synchronous (Blocking):**

```dart
void main() {
  print('Start');
  // This blocks the thread
  sleep(Duration(seconds: 2));
  print('End');
}
// Output: Start (2 second pause) End
```

**Asynchronous (Non-blocking):**

```dart
void main() async {
  print('Start');
  // This doesn't block
  await Future.delayed(Duration(seconds: 2));
  print('End');
}
// Output: Start (immediate) End (after 2 seconds)
```

### Why Async?

- Prevent UI freezing in Flutter apps
- Handle network requests efficiently
- Perform file operations without blocking
- Process large datasets in chunks
- Improve app responsiveness

## 8.2 Futures

A `Future` represents a potential value or error that will be available at some time in the future.

### 8.2.1 Creating Futures

```dart
// Create a future that completes immediately
Future<int> immediateFuture = Future.value(42);

// Create a future that completes with an error
Future<int> errorFuture = Future.error('Something went wrong');

// Create a future that completes after a delay
Future<int> delayedFuture = Future.delayed(
  Duration(seconds: 1),
  () => 42
);
```

### 8.2.2 Using Futures with `then`

```dart
void main() {
  Future<int> future = Future.delayed(
    Duration(seconds: 1),
    () => 42
  );
  
  future.then((value) {
    print('Received value: $value');
  });
  
  print('Future created');
}
// Output: Future created (immediate)
//         Received value: 42 (after 1 second)
```

### 8.2.3 Chaining Futures

```dart
void main() {
  Future.delayed(Duration(seconds: 1), () => 10)
    .then((value) => value * 2)
    .then((value) => value + 5)
    .then((value) => print('Final result: $value'));
}
// Output: Final result: 25 (after 1 second)
```

### 8.2.4 Handling Errors with `catchError`

```dart
void main() {
  Future.delayed(Duration(seconds: 1), () => throw 'Error!')
    .then((value) => print('Success: $value'))
    .catchError((error) => print('Caught error: $error'));
}
// Output: Caught error: Error!
```

### 8.2.5 Completing with `whenComplete`

```dart
void main() {
  Future.delayed(Duration(seconds: 1), () => 42)
    .then((value) => print('Value: $value'))
    .catchError((error) => print('Error: $error'))
    .whenComplete(() => print('Future completed'));
}
// Output: Value: 42
//         Future completed
```

## 8.3 Async/Await

The `async` and `await` keywords provide a more readable way to work with futures.

### 8.3.1 Basic Async/Await

```dart
Future<void> fetchData() async {
  print('Fetching data...');
  await Future.delayed(Duration(seconds: 2));
  print('Data fetched!');
}

void main() async {
  print('Starting...');
  await fetchData();
  print('Done!');
}
// Output: Starting...
//         Fetching data...
//         Data fetched!
//         Done!
```

### 8.3.2 Awaiting Multiple Futures

```dart
Future<int> fetchFirst() async {
  await Future.delayed(Duration(seconds: 1));
  return 1;
}

Future<int> fetchSecond() async {
  await Future.delayed(Duration(seconds: 2));
  return 2;
}

void main() async {
  // Sequential (total: 3 seconds)
  int result1 = await fetchFirst();
  int result2 = await fetchSecond();
  print('Results: $result1, $result2');
  
  // Parallel (total: 2 seconds)
  Future<int> future1 = fetchFirst();
  Future<int> future2 = fetchSecond();
  int r1 = await future1;
  int r2 = await future2;
  print('Results: $r1, $r2');
}
```

### 8.3.3 Using `await` in Expressions

```dart
Future<int> calculate() async {
  int a = await Future.value(10);
  int b = await Future.value(20);
  return a + b;
}

// Or more concisely:
Future<int> calculate2() async => 
  (await Future.value(10)) + (await Future.value(20));
```

### 8.3.4 Error Handling with Try-Catch

```dart
Future<void> fetchWithError() async {
  try {
    await Future.delayed(Duration(seconds: 1), () => throw 'Error!');
  } catch (e) {
    print('Caught error: $e');
  }
}

void main() async {
  await fetchWithError();
}
```

## 8.4 Future API

### 8.4.1 `Future.wait`

Waits for multiple futures to complete.

```dart
void main() async {
  Future<int> future1 = Future.delayed(Duration(seconds: 1), () => 1);
  Future<int> future2 = Future.delayed(Duration(seconds: 2), () => 2);
  Future<int> future3 = Future.delayed(Duration(seconds: 3), () => 3);
  
  // Wait for all futures to complete
  List<int> results = await Future.wait([future1, future2, future3]);
  print(results); // [1, 2, 3] (after 3 seconds)
}
```

### 8.4.2 `Future.any`

Waits for any of the futures to complete.

```dart
void main() async {
  Future<int> future1 = Future.delayed(Duration(seconds: 3), () => 1);
  Future<int> future2 = Future.delayed(Duration(seconds: 1), () => 2);
  Future<int> future3 = Future.delayed(Duration(seconds: 2), () => 3);
  
  // Returns the first completed future's value
  int result = await Future.any([future1, future2, future3]);
  print(result); // 2 (after 1 second)
}
```

### 8.4.3 `Future.value` and `Future.error`

```dart
void main() async {
  // Immediately completed future
  int value = await Future.value(42);
  print(value); // 42
  
  try {
    await Future.error('Something went wrong');
  } catch (e) {
    print(e); // Something went wrong
  }
}
```

### 8.4.4 `Future.delayed`

```dart
void main() async {
  // Delay execution
  await Future.delayed(Duration(seconds: 1));
  print('1 second passed');
  
  // With a value
  int result = await Future.delayed(
    Duration(seconds: 1),
    () => 42
  );
  print(result); // 42
}
```

## 8.5 Completers

A `Completer` is used to manually complete a future.

### 8.5.1 Basic Completer

```dart
void main() async {
  Completer<int> completer = Completer();
  
  // Complete the future after a delay
  Future.delayed(Duration(seconds: 1), () {
    completer.complete(42);
  });
  
  int result = await completer.future;
  print(result); // 42
}
```

### 8.5.2 Completing with Error

```dart
void main() async {
  Completer<int> completer = Completer();
  
  Future.delayed(Duration(seconds: 1), () {
    completer.completeError('Something went wrong');
  });
  
  try {
    await completer.future;
  } catch (e) {
    print(e); // Something went wrong
  }
}
```

### 8.5.3 Practical Completer Example

```dart
class TaskRunner {
  final Completer<void> _completer = Completer();
  
  Future<void> get onComplete => _completer.future;
  
  void startTask() {
    print('Task started...');
    Future.delayed(Duration(seconds: 2), () {
      print('Task completed!');
      _completer.complete();
    });
  }
  
  void failTask() {
    _completer.completeError('Task failed');
  }
}

void main() async {
  TaskRunner runner = TaskRunner();
  runner.startTask();
  await runner.onComplete;
  print('All done!');
}
```

## 8.6 Streams

A `Stream` represents a sequence of asynchronous events. Unlike futures (which provide a single value), streams can provide multiple values over time.

### 8.6.1 Creating Streams

```dart
// Create a stream from a list
Stream<int> numberStream = Stream.fromIterable([1, 2, 3, 4, 5]);

// Create a stream that emits values periodically
Stream<int> periodicStream = Stream.periodic(
  Duration(seconds: 1),
  (count) => count
);

// Create a stream from a future
Stream<int> singleValueStream = Stream.fromFuture(Future.value(42));

// Create an empty stream
Stream<int> emptyStream = Stream.empty();

// Create a stream that errors
Stream<int> errorStream = Stream.error('Error occurred');
```

### 8.6.2 Listening to Streams

```dart
void main() {
  Stream<int> stream = Stream.fromIterable([1, 2, 3, 4, 5]);
  
  StreamSubscription<int> subscription = stream.listen(
    (data) => print('Data: $data'),
    onError: (error) => print('Error: $error'),
    onDone: () => print('Stream completed'),
  );
}
// Output:
// Data: 1
// Data: 2
// Data: 3
// Data: 4
// Data: 5
// Stream completed
```

### 8.6.3 Stream Subscription Methods

```dart
void main() {
  Stream<int> stream = Stream.fromIterable([1, 2, 3, 4, 5]);
  
  StreamSubscription<int> subscription = stream.listen(
    (data) => print('Received: $data'),
  );
  
  // Pause the subscription
  Future.delayed(Duration(milliseconds: 50), () {
    subscription.pause();
    print('Subscription paused');
  });
  
  // Resume the subscription
  Future.delayed(Duration(milliseconds: 150), () {
    subscription.resume();
    print('Subscription resumed');
  });
  
  // Cancel the subscription
  Future.delayed(Duration(milliseconds: 250), () {
    subscription.cancel();
    print('Subscription cancelled');
  });
}
```

### 8.6.4 Transforming Streams

```dart
void main() {
  Stream<int> stream = Stream.fromIterable([1, 2, 3, 4, 5]);
  
  // Map: transform each element
  Stream<int> doubled = stream.map((x) => x * 2);
  
  // Where: filter elements
  Stream<int> evens = doubled.where((x) => x % 2 == 0);
  
  // Take: get first N elements
  Stream<int> firstThree = evens.take(3);
  
  // Listen to the transformed stream
  firstThree.listen((data) => print(data));
  // Output: 4, 8 (from [2, 4, 6, 8, 10] -> [4, 8])
}
```

### 8.6.5 Stream Transformers

```dart
void main() {
  Stream<int> stream = Stream.fromIterable([1, 2, 3, 4, 5]);
  
  // Create a transformer
  var transformer = StreamTransformer<int, int>.fromHandlers(
    handleData: (data, sink) {
      if (data % 2 == 0) {
        sink.add(data * 10);
      }
    },
  );
  
  // Apply the transformer
  Stream<int> transformed = stream.transform(transformer);
  
  transformed.listen((data) => print(data));
  // Output: 20, 40 (from [1,2,3,4,5] -> [2,4] -> [20,40])
}
```

### 8.6.6 Broadcasting Streams

Broadcast streams allow multiple listeners.

```dart
void main() {
  // Create a broadcast stream
  Stream<int> broadcastStream = Stream.fromIterable([1, 2, 3]).asBroadcastStream();
  
  // First listener
  broadcastStream.listen((data) => print('Listener 1: $data'));
  
  // Second listener
  broadcastStream.listen((data) => print('Listener 2: $data'));
}
// Output:
// Listener 1: 1
// Listener 2: 1
// Listener 1: 2
// Listener 2: 2
// Listener 1: 3
// Listener 2: 3
```

### 8.6.7 Stream Controllers

Create custom streams with `StreamController`.

```dart
void main() async {
  // Create a stream controller
  StreamController<int> controller = StreamController();
  
  // Get the stream
  Stream<int> stream = controller.stream;
  
  // Listen to the stream
  stream.listen((data) => print('Received: $data'));
  
  // Add data to the stream
  controller.add(1);
  controller.add(2);
  controller.add(3);
  
  // Close the stream
  await controller.close();
}
// Output:
// Received: 1
// Received: 2
// Received: 3
```

### 8.6.8 Bidirectional Streams

```dart
void main() async {
  // Create a stream controller for bidirectional communication
  StreamController<String> controller = StreamController();
  
  // Get the stream and sink
  Stream<String> stream = controller.stream;
  StreamSink<String> sink = controller.sink;
  
  // Listen to the stream
  stream.listen((data) => print('Received: $data'));
  
  // Add data through the sink
  sink.add('Hello');
  sink.add('World');
  
  // Close the controller
  await controller.close();
}
```

## 8.7 Async Generators

Async generators are functions that produce a sequence of values asynchronously.

### 8.7.1 Basic Async Generator

```dart
Stream<int> countStream(int to) async* {
  for (int i = 1; i <= to; i++) {
    await Future.delayed(Duration(milliseconds: 100));
    yield i;
  }
}

void main() async {
  await for (int number in countStream(5)) {
    print(number);
  }
}
// Output: 1, 2, 3, 4, 5 (each after 100ms)
```

### 8.7.2 Async Generator with Error

```dart
Stream<int> countWithError(int to) async* {
  for (int i = 1; i <= to; i++) {
    if (i == 3) {
      throw 'Error at 3';
    }
    await Future.delayed(Duration(milliseconds: 100));
    yield i;
  }
}

void main() async {
  try {
    await for (int number in countWithError(5)) {
      print(number);
    }
  } catch (e) {
    print('Caught: $e');
  }
}
```

### 8.7.3 Synchronous Generators

For comparison, synchronous generators use `sync*`.

```dart
Iterable<int> countSync(int to) sync* {
  for (int i = 1; i <= to; i++) {
    yield i;
  }
}

void main() {
  for (int number in countSync(5)) {
    print(number);
  }
}
```

## 8.8 Working with APIs

### 8.8.1 HTTP GET Request

First, add the `http` package to your `pubspec.yaml`:

```yaml
dependencies:
  http: ^1.1.0
```

Then:

```dart
import 'package:http/http.dart' as http;

Future<void> fetchData() async {
  try {
    final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts/1'));
    
    if (response.statusCode == 200) {
      print('Response: ${response.body}');
    } else {
      print('Request failed with status: ${response.statusCode}');
    }
  } catch (e) {
    print('Error: $e');
  }
}

void main() async {
  await fetchData();
}
```

### 8.8.2 HTTP POST Request

```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<void> postData() async {
  try {
    final response = await http.post(
      Uri.parse('https://jsonplaceholder.typicode.com/posts'),
      headers: {'Content-Type': 'application/json'},
      body: jsonEncode({
        'title': 'foo',
        'body': 'bar',
        'userId': 1,
      }),
    );
    
    if (response.statusCode == 201) {
      print('Post created: ${response.body}');
    } else {
      print('Request failed with status: ${response.statusCode}');
    }
  } catch (e) {
    print('Error: $e');
  }
}
```

### 8.8.3 Using Dio (Alternative HTTP Client)

Add to `pubspec.yaml`:

```yaml
dependencies:
  dio: ^5.3.0
```

Then:

```dart
import 'package:dio/dio.dart';

Future<void> fetchWithDio() async {
  final dio = Dio();
  
  try {
    final response = await dio.get('https://jsonplaceholder.typicode.com/posts/1');
    print(response.data);
  } catch (e) {
    print('Error: $e');
  }
}
```

## 8.9 Practical Examples

### Example 1: Fetching Multiple API Endpoints

```dart
import 'package:http/http.dart' as http;

Future<List<dynamic>> fetchMultipleEndpoints() async {
  List<Future<http.Response>> futures = [
    http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts/1')),
    http.get(Uri.parse('https://jsonplaceholder.typicode.com/comments/1')),
    http.get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1')),
  ];
  
  List<http.Response> responses = await Future.wait(futures);
  
  return responses.map((response) => {
    'status': response.statusCode,
    'data': response.body,
  }).toList();
}

void main() async {
  try {
    List<dynamic> results = await fetchMultipleEndpoints();
    results.forEach(print);
  } catch (e) {
    print('Error: $e');
  }
}
```

### Example 2: Real-time Data Streaming

```dart
Stream<int> generateSensorData() async* {
  int value = 0;
  while (true) {
    await Future.delayed(Duration(milliseconds: 500));
    value = (value + 1) % 100;
    yield value;
  }
}

void main() {
  StreamSubscription<int>? subscription;
  
  subscription = generateSensorData().listen(
    (data) => print('Sensor value: $data'),
    onError: (error) => print('Error: $error'),
    onDone: () => print('Stream completed'),
  );
  
  // Stop after 5 seconds
  Future.delayed(Duration(seconds: 5), () {
    subscription?.cancel();
    print('Stopped listening');
  });
}
```

### Example 3: File Operations

```dart
import 'dart:io';

Future<void> readWriteFile() async {
  try {
    // Write to file
    File file = File('example.txt');
    await file.writeAsString('Hello, Dart!');
    print('File written');
    
    // Read from file
    String content = await file.readAsString();
    print('File content: $content');
    
    // Check if file exists
    bool exists = await file.exists();
    print('File exists: $exists');
    
    // Get file size
    int size = await file.length();
    print('File size: $size bytes');
    
    // Delete file
    await file.delete();
    print('File deleted');
  } catch (e) {
    print('File error: $e');
  }
}

void main() async {
  await readWriteFile();
}
```

### Example 4: Concurrent Operations

```dart
Future<void> processItems(List<String> items) async {
  // Process items concurrently
  List<Future<void>> futures = items.map((item) async {
    await Future.delayed(Duration(milliseconds: 100));
    print('Processed: $item');
  }).toList();
  
  await Future.wait(futures);
  print('All items processed');
}

void main() async {
  List<String> items = List.generate(10, (i) => 'Item ${i + 1}');
  await processItems(items);
}
```

### Example 5: Retry Mechanism

```dart
Future<String> fetchWithRetry(String url, {int retries = 3}) async {
  for (int attempt = 1; attempt <= retries; attempt++) {
    try {
      // Simulate network request
      await Future.delayed(Duration(milliseconds: 100));
      
      // Simulate random failure
      if (url == 'fail.com' && attempt < retries) {
        throw Exception('Request failed');
      }
      
      return 'Data from $url';
    } catch (e) {
      if (attempt == retries) rethrow;
      print('Attempt $attempt failed: $e');
      await Future.delayed(Duration(milliseconds: 100 * attempt));
    }
  }
  throw Exception('All retries failed');
}

void main() async {
  try {
    String data = await fetchWithRetry('fail.com');
    print(data);
  } catch (e) {
    print('Final error: $e');
  }
}
```

## 8.10 Exercises

### Practice Exercise 8.1

Write a function that waits for 2 seconds and then returns the string "Hello, Dart!".

```dart
Future<String> greetAfterDelay() async {
  await Future.delayed(Duration(seconds: 2));
  return 'Hello, Dart!';
}
```

### Practice Exercise 8.2

Write a function that takes a list of futures and returns a future that completes when all of them complete.

```dart
Future<List<T>> waitForAll<T>(List<Future<T>> futures) async {
  return Future.wait(futures);
}
```

### Practice Exercise 8.3

Write a function that takes a list of numbers and returns a stream that emits each number multiplied by 2.

```dart
Stream<int> doubleNumbers(List<int> numbers) async* {
  for (int number in numbers) {
    await Future.delayed(Duration(milliseconds: 100));
    yield number * 2;
  }
}
```

### Practice Exercise 8.4

Write a function that fetches data from a mock API and handles errors.

```dart
Future<Map<String, dynamic>> fetchUserData(int userId) async {
  await Future.delayed(Duration(milliseconds: 500));
  
  if (userId < 1) {
    throw ArgumentError('User ID must be positive');
  }
  
  if (userId == 999) {
    throw Exception('User not found');
  }
  
  return {
    'id': userId,
    'name': 'User $userId',
    'email': 'user$userId@example.com'
  };
}
```

### Practice Exercise 8.5

Write a function that creates a stream of numbers from 1 to N, emitting one number per second.

```dart
Stream<int> countToN(int n) async* {
  for (int i = 1; i <= n; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}
```

### Challenge Exercise 8.6

Create a weather app simulation with:

- A function to fetch weather data for a city
- A stream that provides weather updates every 5 seconds
- Error handling for invalid cities

```dart
class WeatherService {
  final Map<String, dynamic> _weatherData = {
    'Cairo': {'temp': 25, 'condition': 'Sunny'},
    'London': {'temp': 15, 'condition': 'Cloudy'},
    'New York': {'temp': 20, 'condition': 'Rainy'},
  };
  
  Future<Map<String, dynamic>> fetchWeather(String city) async {
    await Future.delayed(Duration(milliseconds: 500));
    
    if (!_weatherData.containsKey(city)) {
      throw ArgumentError('City not found: $city');
    }
    
    return _weatherData[city]!;
  }
  
  Stream<Map<String, dynamic>> getWeatherUpdates(String city) async* {
    while (true) {
      try {
        yield await fetchWeather(city);
        await Future.delayed(Duration(seconds: 5));
      } catch (e) {
        yield {'error': e.toString()};
        await Future.delayed(Duration(seconds: 5));
      }
    }
  }
}

void main() async {
  WeatherService weather = WeatherService();
  
  await for (var weatherData in weather.getWeatherUpdates('Cairo')) {
    print('Current weather: $weatherData');
  }
}
```

### Challenge Exercise 8.7

Create a task queue system that:

- Accepts tasks as functions
- Processes them one at a time
- Provides a stream of task completion events

```dart
class TaskQueue {
  final Queue<Future<void> Function()> _tasks = Queue();
  bool _isProcessing = false;
  final StreamController<String> _controller = StreamController.broadcast();
  
  Stream<String> get onTaskCompleted => _controller.stream;
  
  void addTask(String name, Future<void> Function() task) {
    _tasks.add(task);
    _processNext();
  }
  
  Future<void> _processNext() async {
    if (_isProcessing || _tasks.isEmpty) return;
    
    _isProcessing = true;
    var task = _tasks.removeFirst();
    
    try {
      await task();
      _controller.add('Task completed');
    } catch (e) {
      _controller.addError('Task failed: $e');
    } finally {
      _isProcessing = false;
      _processNext();
    }
  }
  
  Future<void> close() async {
    await _controller.close();
  }
}

void main() async {
  TaskQueue queue = TaskQueue();
  
  queue.onTaskCompleted.listen(
    (event) => print(event),
    onError: (error) => print(error),
  );
  
  // Add tasks
  queue.addTask('Task 1', () async {
    await Future.delayed(Duration(milliseconds: 500));
    print('Task 1 executed');
  });
  
  queue.addTask('Task 2', () async {
    await Future.delayed(Duration(milliseconds: 300));
    print('Task 2 executed');
  });
  
  queue.addTask('Task 3', () async {
    throw Exception('Task 3 failed');
  });
  
  await Future.delayed(Duration(seconds: 2));
  await queue.close();
}
```

### Challenge Exercise 8.8

Create a chat application simulation with:

- A stream for incoming messages
- A function to send messages
- A function to receive messages

```dart
class ChatService {
  final StreamController<String> _messageController = StreamController.broadcast();
  final StreamController<String> _sentController = StreamController.broadcast();
  
  Stream<String> get onMessageReceived => _messageController.stream;
  Stream<String> get onMessageSent => _sentController.stream;
  
  void sendMessage(String message) {
    _sentController.add(message);
    // Simulate network delay
    Future.delayed(Duration(milliseconds: 200), () {
      // Simulate receiving the message back (echo)
      _messageController.add('Echo: $message');
    });
  }
  
  void simulateIncomingMessage(String message) {
    _messageController.add(message);
  }
  
  Future<void> close() async {
    await _messageController.close();
    await _sentController.close();
  }
}

void main() async {
  ChatService chat = ChatService();
  
  // Listen for received messages
  chat.onMessageReceived.listen((message) {
    print('Received: $message');
  });
  
  // Listen for sent messages
  chat.onMessageSent.listen((message) {
    print('Sent: $message');
  });
  
  // Send messages
  chat.sendMessage('Hello!');
  chat.sendMessage('How are you?');
  
  // Simulate incoming messages
  chat.simulateIncomingMessage('Hi there!');
  chat.simulateIncomingMessage('I am good!');
  
  await Future.delayed(Duration(seconds: 1));
  await chat.close();
}
```

### Challenge Exercise 8.9

Create a data synchronization service with:

- A function to sync data from a remote server
- A stream that provides sync status updates
- Retry logic for failed syncs

```dart
class DataSyncService {
  final StreamController<String> _statusController = StreamController.broadcast();
  bool _isSyncing = false;
  
  Stream<String> get onStatusChanged => _statusController.stream;
  
  Future<void> syncData({int maxRetries = 3}) async {
    if (_isSyncing) {
      _statusController.add('Sync already in progress');
      return;
    }
    
    _isSyncing = true;
    _statusController.add('Starting sync...');
    
    for (int attempt = 1; attempt <= maxRetries; attempt++) {
      try {
        _statusController.add('Sync attempt $attempt/$maxRetries');
        
        // Simulate sync operation
        await Future.delayed(Duration(milliseconds: 500));
        
        // Simulate random failure
        if (attempt < maxRetries) {
          throw Exception('Sync failed');
        }
        
        _statusController.add('Sync completed successfully!');
        _isSyncing = false;
        return;
      } catch (e) {
        _statusController.add('Sync failed: $e');
        if (attempt < maxRetries) {
          await Future.delayed(Duration(milliseconds: 500 * attempt));
        }
      }
    }
    
    _statusController.add('Sync failed after $maxRetries attempts');
    _isSyncing = false;
  }
  
  Future<void> close() async {
    await _statusController.close();
  }
}

void main() async {
  DataSyncService syncService = DataSyncService();
  
  syncService.onStatusChanged.listen((status) {
    print(status);
  });
  
  await syncService.syncData(maxRetries: 3);
  
  await Future.delayed(Duration(milliseconds: 100));
  await syncService.close();
}
```

### Challenge Exercise 8.10

Create a comprehensive e-commerce product service with:

- A function to fetch product list
- A function to fetch product details
- A function to search products
- A stream for real-time price updates
- Error handling for all operations

```dart
class Product {
  final int id;
  final String name;
  final double price;
  
  Product(this.id, this.name, this.price);
}

class ProductService {
  final List<Product> _products = [
    Product(1, 'Laptop', 999.99),
    Product(2, 'Phone', 699.99),
    Product(3, 'Tablet', 399.99),
    Product(4, 'Headphones', 149.99),
  ];
  
  final StreamController<Map<int, double>> _priceUpdateController = StreamController.broadcast();
  
  Stream<Map<int, double>> get onPriceUpdate => _priceUpdateController.stream;
  
  Future<List<Product>> getProducts() async {
    await Future.delayed(Duration(milliseconds: 200));
    return List.from(_products);
  }
  
  Future<Product> getProductDetails(int id) async {
    await Future.delayed(Duration(milliseconds: 100));
    
    Product? product = _products.firstWhere((p) => p.id == id, orElse: () => null);
    
    if (product == null) {
      throw ArgumentError('Product not found: $id');
    }
    
    return product;
  }
  
  Future<List<Product>> searchProducts(String query) async {
    await Future.delayed(Duration(milliseconds: 150));
    
    if (query.isEmpty) {
      throw ArgumentError('Query cannot be empty');
    }
    
    return _products.where((p) => 
      p.name.toLowerCase().contains(query.toLowerCase())
    ).toList();
  }
  
  void updatePrice(int productId, double newPrice) {
    int index = _products.indexWhere((p) => p.id == productId);
    if (index != -1) {
      _products[index] = Product(productId, _products[index].name, newPrice);
      _priceUpdateController.add({productId: newPrice});
    }
  }
  
  Future<void> close() async {
    await _priceUpdateController.close();
  }
}

void main() async {
  ProductService service = ProductService();
  
  // Listen for price updates
  service.onPriceUpdate.listen((updates) {
    print('Price updates: $updates');
  });
  
  try {
    // Get all products
    List<Product> products = await service.getProducts();
    print('All products: $products');
    
    // Get product details
    Product product = await service.getProductDetails(1);
    print('Product details: $product');
    
    // Search products
    List<Product> results = await service.searchProducts('phone');
    print('Search results: $results');
    
    // Update price
    service.updatePrice(1, 899.99);
    
    // Get updated product
    Product updatedProduct = await service.getProductDetails(1);
    print('Updated product: $updatedProduct');
    
  } catch (e) {
    print('Error: $e');
  } finally {
    await service.close();
  }
}
```

---

## 📖 Summary

In this file, you learned:

✅ **Asynchronous programming** fundamentals  
✅ **Futures** - representing future values  
✅ **Async/await** - writing async code synchronously  
✅ **Future API** - `Future.wait`, `Future.any`, etc.  
✅ **Completers** - manually completing futures  
✅ **Streams** - sequences of asynchronous events  
✅ **Stream operations** - listen, transform, filter  
✅ **Stream controllers** - creating custom streams  
✅ **Async generators** - `async*` and `yield`  
✅ **Working with APIs** - making HTTP requests  
✅ **Practical examples** - file operations, concurrent tasks, retry mechanisms

## 🔗 Next Steps

**File 9: Generics and Collections** will cover:

- Generic classes and functions
- Generic methods
- Bounded generics
- Collection methods in depth
- Iterables and iterators
- Working with maps and sets

## 📚 Additional Resources

- [Dart Asynchrony Support](https://dart.dev/guides/language/language-tour#asynchrony-support)
- [Dart Futures](https://dart.dev/guides/libraries/library-tour#futures)
- [Dart Streams](https://dart.dev/guides/libraries/library-tour#streams)
- [Dart Async/Await](https://dart.dev/codelabs/async-await)
- [HTTP Package](https://pub.dev/packages/http)
- [Dio Package](https://pub.dev/packages/dio)

## ✅ Checklist

Before moving to File 9:

- [ ] Understand the difference between synchronous and asynchronous code
- [ ] Can create and use Futures
- [ ] Know how to use async/await syntax
- [ ] Understand Future API methods
- [ ] Can create and use Streams
- [ ] Know how to transform and filter streams
- [ ] Can create custom streams with StreamController
- [ ] Understand async generators
- [ ] Can make HTTP requests with Dart
- [ ] Have completed all exercises in this file