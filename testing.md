# Flutter Testing - 25 Essential Examples

This comprehensive guide covers Flutter testing with 25 practical examples,  
from basic unit tests to advanced integration testing. Each example  
demonstrates different testing techniques to ensure your Flutter apps are  
reliable, maintainable, and bug-free.

## Basic Unit Test Setup

Setting up unit testing environment with the test package and writing  
your first unit test.  

```dart
import 'package:flutter_test/flutter_test.dart';

// Simple calculator class to test
class Calculator {
  int add(int a, int b) => a + b;
  int subtract(int a, int b) => a - b;
  int multiply(int a, int b) => a * b;
  double divide(int a, int b) {
    if (b == 0) throw ArgumentError('Cannot divide by zero');
    return a / b;
  }
}

void main() {
  group('Calculator Tests', () {
    late Calculator calculator;

    setUp(() {
      calculator = Calculator();
    });

    test('should add two numbers correctly', () {
      // Arrange
      const a = 5;
      const b = 3;

      // Act
      final result = calculator.add(a, b);

      // Assert
      expect(result, equals(8));
    });

    test('should subtract two numbers correctly', () {
      expect(calculator.subtract(10, 4), equals(6));
    });

    test('should multiply two numbers correctly', () {
      expect(calculator.multiply(3, 4), equals(12));
    });

    test('should divide two numbers correctly', () {
      expect(calculator.divide(10, 2), equals(5.0));
    });

    test('should throw error when dividing by zero', () {
      expect(() => calculator.divide(10, 0), throwsArgumentError);
    });
  });
}
```

This example demonstrates the basic structure of unit tests using the  
test package. The setUp method initializes test objects before each test,  
while group organizes related tests together. Each test follows the  
Arrange-Act-Assert pattern for clarity.

## Testing Functions and Methods

Testing standalone functions and static methods with various input types  
and edge cases.  

```dart
import 'package:flutter_test/flutter_test.dart';

// Utility functions to test
class StringUtils {
  static bool isPalindrome(String text) {
    if (text.isEmpty) return true;
    final cleaned = text.toLowerCase().replaceAll(RegExp(r'[^a-z0-9]'), '');
    return cleaned == cleaned.split('').reversed.join('');
  }

  static String capitalize(String text) {
    if (text.isEmpty) return text;
    return text[0].toUpperCase() + text.substring(1).toLowerCase();
  }

  static List<String> extractEmails(String text) {
    final emailRegex = RegExp(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b');
    return emailRegex.allMatches(text).map((match) => match.group(0)!).toList();
  }
}

void main() {
  group('StringUtils Tests', () {
    group('isPalindrome', () {
      test('should return true for palindromes', () {
        expect(StringUtils.isPalindrome('racecar'), isTrue);
        expect(StringUtils.isPalindrome('A man a plan a canal Panama'), isTrue);
        expect(StringUtils.isPalindrome('Was it a rat I saw?'), isTrue);
      });

      test('should return false for non-palindromes', () {
        expect(StringUtils.isPalindrome('hello'), isFalse);
        expect(StringUtils.isPalindrome('Flutter'), isFalse);
      });

      test('should handle edge cases', () {
        expect(StringUtils.isPalindrome(''), isTrue);
        expect(StringUtils.isPalindrome('a'), isTrue);
        expect(StringUtils.isPalindrome('  '), isTrue);
      });
    });

    group('capitalize', () {
      test('should capitalize first letter', () {
        expect(StringUtils.capitalize('hello'), equals('Hello'));
        expect(StringUtils.capitalize('WORLD'), equals('World'));
        expect(StringUtils.capitalize('tEST'), equals('Test'));
      });

      test('should handle edge cases', () {
        expect(StringUtils.capitalize(''), equals(''));
        expect(StringUtils.capitalize('a'), equals('A'));
      });
    });

    group('extractEmails', () {
      test('should extract valid email addresses', () {
        const text = 'Contact us at info@example.com or support@test.org';
        final emails = StringUtils.extractEmails(text);
        expect(emails, hasLength(2));
        expect(emails, contains('info@example.com'));
        expect(emails, contains('support@test.org'));
      });

      test('should return empty list for no emails', () {
        const text = 'No emails here';
        expect(StringUtils.extractEmails(text), isEmpty);
      });
    });
  });
}
```

This example shows testing of utility functions with various data types  
and edge cases. The tests are grouped by function for better organization,  
and each group tests different scenarios including edge cases.

## Testing Classes with Dependencies

Testing classes that depend on other services using dependency injection  
and proper test isolation.  

```dart
import 'package:flutter_test/flutter_test.dart';

// Dependencies
abstract class UserRepository {
  Future<User?> findById(String id);
  Future<void> save(User user);
}

class User {
  final String id;
  final String name;
  final String email;

  const User({required this.id, required this.name, required this.email});

  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
      other is User &&
          runtimeType == other.runtimeType &&
          id == other.id &&
          name == other.name &&
          email == other.email;

  @override
  int get hashCode => id.hashCode ^ name.hashCode ^ email.hashCode;
}

// Service to test
class UserService {
  final UserRepository _repository;

  UserService(this._repository);

  Future<User?> getUser(String id) async {
    if (id.isEmpty) {
      throw ArgumentError('User ID cannot be empty');
    }
    return await _repository.findById(id);
  }

  Future<bool> updateUserName(String userId, String newName) async {
    final user = await _repository.findById(userId);
    if (user == null) return false;

    final updatedUser = User(
      id: user.id,
      name: newName,
      email: user.email,
    );
    
    await _repository.save(updatedUser);
    return true;
  }
}

// Mock implementation
class MockUserRepository implements UserRepository {
  final Map<String, User> _users = {};

  @override
  Future<User?> findById(String id) async {
    return _users[id];
  }

  @override
  Future<void> save(User user) async {
    _users[user.id] = user;
  }

  void addUser(User user) {
    _users[user.id] = user;
  }

  void clear() {
    _users.clear();
  }
}

void main() {
  group('UserService Tests', () {
    late UserService userService;
    late MockUserRepository mockRepository;

    setUp(() {
      mockRepository = MockUserRepository();
      userService = UserService(mockRepository);
    });

    tearDown(() {
      mockRepository.clear();
    });

    group('getUser', () {
      test('should return user when found', () async {
        // Arrange
        const user = User(id: '1', name: 'John Doe', email: 'john@example.com');
        mockRepository.addUser(user);

        // Act
        final result = await userService.getUser('1');

        // Assert
        expect(result, equals(user));
      });

      test('should return null when user not found', () async {
        final result = await userService.getUser('999');
        expect(result, isNull);
      });

      test('should throw error for empty user ID', () async {
        expect(() => userService.getUser(''), throwsArgumentError);
      });
    });

    group('updateUserName', () {
      test('should update existing user name', () async {
        // Arrange
        const originalUser = User(id: '1', name: 'John Doe', email: 'john@example.com');
        mockRepository.addUser(originalUser);

        // Act
        final result = await userService.updateUserName('1', 'Jane Smith');

        // Assert
        expect(result, isTrue);
        final updatedUser = await mockRepository.findById('1');
        expect(updatedUser?.name, equals('Jane Smith'));
        expect(updatedUser?.email, equals('john@example.com'));
      });

      test('should return false for non-existent user', () async {
        final result = await userService.updateUserName('999', 'New Name');
        expect(result, isFalse);
      });
    });
  });
}
```

This example demonstrates testing classes with dependencies using mock  
objects. The MockUserRepository provides controlled test data, while  
setUp and tearDown ensure test isolation.

## Mocking External Dependencies

Using the mockito package to create sophisticated mocks and verify  
interactions with external dependencies.  

```dart
import 'package:flutter_test/flutter_test.dart';
import 'package:mockito/mockito.dart';
import 'package:mockito/annotations.dart';

// External services
abstract class NetworkService {
  Future<String> fetchData(String url);
}

abstract class CacheService {
  Future<String?> get(String key);
  Future<void> put(String key, String value);
}

// Service to test
class DataService {
  final NetworkService _networkService;
  final CacheService _cacheService;

  DataService(this._networkService, this._cacheService);

  Future<String> getData(String endpoint) async {
    final cacheKey = 'data_$endpoint';
    
    // Try cache first
    final cached = await _cacheService.get(cacheKey);
    if (cached != null) {
      return cached;
    }

    // Fetch from network
    final data = await _networkService.fetchData('https://api.example.com/$endpoint');
    
    // Cache the result
    await _cacheService.put(cacheKey, data);
    
    return data;
  }
}

// Generate mocks
@GenerateMocks([NetworkService, CacheService])
class MockNetworkService extends Mock implements NetworkService {}
class MockCacheService extends Mock implements CacheService {}

void main() {
  group('DataService Tests', () {
    late DataService dataService;
    late MockNetworkService mockNetworkService;
    late MockCacheService mockCacheService;

    setUp(() {
      mockNetworkService = MockNetworkService();
      mockCacheService = MockCacheService();
      dataService = DataService(mockNetworkService, mockCacheService);
    });

    test('should return cached data when available', () async {
      // Arrange
      const endpoint = 'users';
      const cachedData = 'cached user data';
      when(mockCacheService.get('data_users'))
          .thenAnswer((_) async => cachedData);

      // Act
      final result = await dataService.getData(endpoint);

      // Assert
      expect(result, equals(cachedData));
      verify(mockCacheService.get('data_users')).called(1);
      verifyNever(mockNetworkService.fetchData(any));
      verifyNever(mockCacheService.put(any, any));
    });

    test('should fetch from network when cache miss', () async {
      // Arrange
      const endpoint = 'users';
      const networkData = 'network user data';
      when(mockCacheService.get('data_users'))
          .thenAnswer((_) async => null);
      when(mockNetworkService.fetchData('https://api.example.com/users'))
          .thenAnswer((_) async => networkData);
      when(mockCacheService.put('data_users', networkData))
          .thenAnswer((_) async {});

      // Act
      final result = await dataService.getData(endpoint);

      // Assert
      expect(result, equals(networkData));
      verify(mockCacheService.get('data_users')).called(1);
      verify(mockNetworkService.fetchData('https://api.example.com/users')).called(1);
      verify(mockCacheService.put('data_users', networkData)).called(1);
    });

    test('should handle network errors', () async {
      // Arrange
      when(mockCacheService.get('data_users'))
          .thenAnswer((_) async => null);
      when(mockNetworkService.fetchData('https://api.example.com/users'))
          .thenThrow(Exception('Network error'));

      // Act & Assert
      expect(() => dataService.getData('users'), throwsException);
      verify(mockCacheService.get('data_users')).called(1);
      verify(mockNetworkService.fetchData('https://api.example.com/users')).called(1);
      verifyNever(mockCacheService.put(any, any));
    });
  });
}
```

This example shows advanced mocking techniques using the mockito package.  
It demonstrates stubbing method calls with when(), verifying interactions  
with verify(), and testing error scenarios.

## Testing Asynchronous Code

Testing Future-based operations, Stream handling, and async/await patterns  
with proper error handling.  

```dart
import 'package:flutter_test/flutter_test.dart';
import 'dart:async';

// Async service to test
class AsyncDataProcessor {
  Future<String> processData(String input) async {
    await Future.delayed(const Duration(milliseconds: 100));
    if (input.isEmpty) {
      throw ArgumentError('Input cannot be empty');
    }
    return input.toUpperCase();
  }

  Future<List<String>> processBatch(List<String> inputs) async {
    final results = <String>[];
    for (final input in inputs) {
      final result = await processData(input);
      results.add(result);
    }
    return results;
  }

  Stream<String> processStream(List<String> inputs) async* {
    for (final input in inputs) {
      await Future.delayed(const Duration(milliseconds: 50));
      yield input.toUpperCase();
    }
  }

  Future<String> processWithTimeout(String input, Duration timeout) async {
    return await processData(input).timeout(
      timeout,
      onTimeout: () => throw TimeoutException('Processing timeout', timeout),
    );
  }
}

void main() {
  group('AsyncDataProcessor Tests', () {
    late AsyncDataProcessor processor;

    setUp(() {
      processor = AsyncDataProcessor();
    });

    group('processData', () {
      test('should process data correctly', () async {
        final result = await processor.processData('hello');
        expect(result, equals('HELLO'));
      });

      test('should throw error for empty input', () async {
        expect(() => processor.processData(''), throwsArgumentError);
      });

      test('should handle concurrent processing', () async {
        final futures = [
          processor.processData('first'),
          processor.processData('second'),
          processor.processData('third'),
        ];

        final results = await Future.wait(futures);
        expect(results, equals(['FIRST', 'SECOND', 'THIRD']));
      });
    });

    group('processBatch', () {
      test('should process batch of inputs', () async {
        final inputs = ['hello', 'world', 'flutter'];
        final results = await processor.processBatch(inputs);
        expect(results, equals(['HELLO', 'WORLD', 'FLUTTER']));
      });

      test('should handle empty batch', () async {
        final results = await processor.processBatch([]);
        expect(results, isEmpty);
      });
    });

    group('processStream', () {
      test('should emit processed values', () async {
        final inputs = ['a', 'b', 'c'];
        final stream = processor.processStream(inputs);
        
        final results = <String>[];
        await for (final value in stream) {
          results.add(value);
        }

        expect(results, equals(['A', 'B', 'C']));
      });

      test('should complete stream properly', () async {
        final stream = processor.processStream(['test']);
        expect(stream, emitsInOrder(['TEST', emitsDone]));
      });

      test('should handle stream transformation', () async {
        final stream = processor.processStream(['hello', 'world']);
        
        final transformed = stream
            .where((value) => value.length > 4)
            .map((value) => 'Processed: $value');

        expect(transformed, emitsInOrder([
          'Processed: HELLO',
          'Processed: WORLD',
          emitsDone,
        ]));
      });
    });

    group('processWithTimeout', () {
      test('should complete within timeout', () async {
        final result = await processor.processWithTimeout(
          'test', 
          const Duration(seconds: 1),
        );
        expect(result, equals('TEST'));
      });

      test('should throw timeout exception', () async {
        expect(
          () => processor.processWithTimeout(
            'test', 
            const Duration(milliseconds: 10),
          ),
          throwsA(isA<TimeoutException>()),
        );
      });
    });
  });
}
```

This example demonstrates comprehensive async testing including Future  
operations, Stream handling, concurrent execution, and timeout scenarios.  
The tests use async/await properly and handle various edge cases.

## Testing with Fixtures and Test Data

Using test fixtures, data builders, and test utilities to create  
maintainable and reusable test data.  

```dart
import 'package:flutter_test/flutter_test.dart';
import 'dart:convert';

// Domain models
class Product {
  final String id;
  final String name;
  final double price;
  final List<String> tags;
  final DateTime createdAt;

  const Product({
    required this.id,
    required this.name,
    required this.price,
    required this.tags,
    required this.createdAt,
  });

  factory Product.fromJson(Map<String, dynamic> json) {
    return Product(
      id: json['id'],
      name: json['name'],
      price: json['price'].toDouble(),
      tags: List<String>.from(json['tags']),
      createdAt: DateTime.parse(json['createdAt']),
    );
  }

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'price': price,
      'tags': tags,
      'createdAt': createdAt.toIso8601String(),
    };
  }

  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
      other is Product && id == other.id && name == other.name && price == other.price;

  @override
  int get hashCode => id.hashCode ^ name.hashCode ^ price.hashCode;
}

// Service to test
class ProductService {
  List<Product> filterByPrice(List<Product> products, double minPrice, double maxPrice) {
    return products.where((product) => 
        product.price >= minPrice && product.price <= maxPrice).toList();
  }

  List<Product> searchByTags(List<Product> products, List<String> tags) {
    return products.where((product) => 
        tags.any((tag) => product.tags.contains(tag))).toList();
  }

  double calculateTotalValue(List<Product> products) {
    return products.fold(0.0, (sum, product) => sum + product.price);
  }
}

// Test data builders
class ProductBuilder {
  String _id = 'default-id';
  String _name = 'Test Product';
  double _price = 10.0;
  List<String> _tags = ['test'];
  DateTime _createdAt = DateTime(2023, 1, 1);

  ProductBuilder withId(String id) {
    _id = id;
    return this;
  }

  ProductBuilder withName(String name) {
    _name = name;
    return this;
  }

  ProductBuilder withPrice(double price) {
    _price = price;
    return this;
  }

  ProductBuilder withTags(List<String> tags) {
    _tags = tags;
    return this;
  }

  ProductBuilder withCreatedAt(DateTime createdAt) {
    _createdAt = createdAt;
    return this;
  }

  Product build() {
    return Product(
      id: _id,
      name: _name,
      price: _price,
      tags: _tags,
      createdAt: _createdAt,
    );
  }
}

// Test fixtures
class TestFixtures {
  static Product get sampleProduct => ProductBuilder()
      .withId('1')
      .withName('Laptop')
      .withPrice(999.99)
      .withTags(['electronics', 'computers'])
      .build();

  static List<Product> get sampleProducts => [
    ProductBuilder().withId('1').withName('Laptop').withPrice(999.99).withTags(['electronics', 'computers']).build(),
    ProductBuilder().withId('2').withName('Phone').withPrice(699.99).withTags(['electronics', 'mobile']).build(),
    ProductBuilder().withId('3').withName('Book').withPrice(29.99).withTags(['education', 'reading']).build(),
    ProductBuilder().withId('4').withName('Headphones').withPrice(199.99).withTags(['electronics', 'audio']).build(),
  ];

  static List<Product> expensiveProducts() {
    return [
      ProductBuilder().withPrice(1500.0).build(),
      ProductBuilder().withPrice(2000.0).build(),
    ];
  }

  static String loadTestData(String filename) {
    // In real app, load from assets or test resources
    final testData = {
      'products.json': '''[
        {
          "id": "test-1",
          "name": "Test Product 1",
          "price": 50.0,
          "tags": ["test", "sample"],
          "createdAt": "2023-01-01T00:00:00.000Z"
        },
        {
          "id": "test-2",
          "name": "Test Product 2",
          "price": 75.0,
          "tags": ["test", "example"],
          "createdAt": "2023-01-02T00:00:00.000Z"
        }
      ]'''
    };
    return testData[filename] ?? '[]';
  }
}

void main() {
  group('ProductService Tests', () {
    late ProductService productService;
    late List<Product> testProducts;

    setUp(() {
      productService = ProductService();
      testProducts = TestFixtures.sampleProducts;
    });

    group('filterByPrice', () {
      test('should filter products within price range', () {
        final result = productService.filterByPrice(testProducts, 100.0, 800.0);
        expect(result, hasLength(2)); // Phone and Headphones
        expect(result.every((p) => p.price >= 100.0 && p.price <= 800.0), isTrue);
      });

      test('should return empty list for impossible price range', () {
        final result = productService.filterByPrice(testProducts, 2000.0, 3000.0);
        expect(result, isEmpty);
      });

      test('should handle edge cases', () {
        final result = productService.filterByPrice(testProducts, 29.99, 29.99);
        expect(result, hasLength(1));
        expect(result.first.name, equals('Book'));
      });
    });

    group('searchByTags', () {
      test('should find products with matching tags', () {
        final result = productService.searchByTags(testProducts, ['electronics']);
        expect(result, hasLength(3)); // Laptop, Phone, Headphones
        expect(result.every((p) => p.tags.contains('electronics')), isTrue);
      });

      test('should find products with any matching tag', () {
        final result = productService.searchByTags(testProducts, ['mobile', 'reading']);
        expect(result, hasLength(2)); // Phone and Book
      });
    });

    group('calculateTotalValue', () {
      test('should calculate total value correctly', () {
        final total = productService.calculateTotalValue(testProducts);
        expect(total, equals(1929.96)); // Sum of all prices
      });

      test('should handle empty list', () {
        final total = productService.calculateTotalValue([]);
        expect(total, equals(0.0));
      });

      test('should work with expensive products fixture', () {
        final expensiveProducts = TestFixtures.expensiveProducts();
        final total = productService.calculateTotalValue(expensiveProducts);
        expect(total, equals(3500.0));
      });
    });

    group('with JSON test data', () {
      test('should load and process test data from JSON', () {
        final jsonData = TestFixtures.loadTestData('products.json');
        final productsJson = jsonDecode(jsonData) as List;
        final products = productsJson.map((json) => Product.fromJson(json)).toList();

        expect(products, hasLength(2));
        expect(products.first.name, equals('Test Product 1'));
        
        final filtered = productService.filterByPrice(products, 60.0, 80.0);
        expect(filtered, hasLength(1));
        expect(filtered.first.name, equals('Test Product 2'));
      });
    });
  });
}
```

This example demonstrates creating reusable test data with builders,  
fixtures, and JSON loading. The TestFixtures class provides consistent  
test data, while ProductBuilder allows flexible object creation.

## Testing Error Handling

Comprehensive testing of error scenarios, exception handling, and  
error recovery mechanisms.  

```dart
import 'package:flutter_test/flutter_test.dart';
import 'dart:io';

// Custom exceptions
class ValidationError extends Error {
  final String message;
  final String field;
  
  ValidationError(this.message, this.field);
  
  @override
  String toString() => 'ValidationError: $message (field: $field)';
}

class NetworkError extends Error {
  final int statusCode;
  final String message;
  
  NetworkError(this.statusCode, this.message);
  
  @override
  String toString() => 'NetworkError: $message (code: $statusCode)';
}

// Service with error handling
class UserRegistrationService {
  bool validateEmail(String email) {
    if (email.isEmpty) {
      throw ValidationError('Email is required', 'email');
    }
    if (!email.contains('@')) {
      throw ValidationError('Invalid email format', 'email');
    }
    return true;
  }

  bool validatePassword(String password) {
    if (password.length < 8) {
      throw ValidationError('Password must be at least 8 characters', 'password');
    }
    if (!password.contains(RegExp(r'[A-Z]'))) {
      throw ValidationError('Password must contain uppercase letter', 'password');
    }
    if (!password.contains(RegExp(r'[0-9]'))) {
      throw ValidationError('Password must contain number', 'password');
    }
    return true;
  }

  Future<String> registerUser(String email, String password) async {
    try {
      // Validate inputs
      validateEmail(email);
      validatePassword(password);

      // Simulate network call
      await Future.delayed(const Duration(milliseconds: 100));
      
      // Simulate various network responses
      if (email == 'taken@example.com') {
        throw NetworkError(409, 'Email already exists');
      }
      
      if (email == 'server@error.com') {
        throw NetworkError(500, 'Internal server error');
      }
      
      if (email == 'timeout@example.com') {
        throw TimeoutException('Request timeout', const Duration(seconds: 30));
      }

      return 'user-${DateTime.now().millisecondsSinceEpoch}';
    } on ValidationError {
      rethrow;
    } on NetworkError {
      rethrow;
    } catch (e) {
      throw Exception('Unexpected error during registration: $e');
    }
  }

  Future<String?> registerUserSafe(String email, String password) async {
    try {
      return await registerUser(email, password);
    } on ValidationError catch (e) {
      print('Validation error: $e');
      return null;
    } on NetworkError catch (e) {
      print('Network error: $e');
      return null;
    } catch (e) {
      print('Unknown error: $e');
      return null;
    }
  }

  Future<List<String>> registerMultipleUsers(
    List<Map<String, String>> users
  ) async {
    final results = <String>[];
    final errors = <String>[];

    for (final userData in users) {
      try {
        final userId = await registerUser(
          userData['email']!,
          userData['password']!,
        );
        results.add(userId);
      } catch (e) {
        errors.add('Failed to register ${userData['email']}: $e');
        continue;
      }
    }

    if (errors.isNotEmpty && results.isEmpty) {
      throw Exception('All registrations failed: ${errors.join(', ')}');
    }

    return results;
  }
}

void main() {
  group('UserRegistrationService Error Handling Tests', () {
    late UserRegistrationService service;

    setUp(() {
      service = UserRegistrationService();
    });

    group('Validation Errors', () {
      group('validateEmail', () {
        test('should throw ValidationError for empty email', () {
          expect(
            () => service.validateEmail(''),
            throwsA(isA<ValidationError>()
                .having((e) => e.field, 'field', 'email')
                .having((e) => e.message, 'message', contains('required'))),
          );
        });

        test('should throw ValidationError for invalid email format', () {
          expect(
            () => service.validateEmail('invalid-email'),
            throwsA(isA<ValidationError>()
                .having((e) => e.field, 'field', 'email')
                .having((e) => e.message, 'message', contains('Invalid'))),
          );
        });

        test('should pass for valid email', () {
          expect(() => service.validateEmail('test@example.com'), returnsNormally);
        });
      });

      group('validatePassword', () {
        test('should throw ValidationError for short password', () {
          expect(
            () => service.validatePassword('short'),
            throwsA(isA<ValidationError>()
                .having((e) => e.field, 'field', 'password')
                .having((e) => e.message, 'message', contains('8 characters'))),
          );
        });

        test('should throw ValidationError for password without uppercase', () {
          expect(
            () => service.validatePassword('lowercase123'),
            throwsA(isA<ValidationError>()
                .having((e) => e.message, 'message', contains('uppercase'))),
          );
        });

        test('should throw ValidationError for password without number', () {
          expect(
            () => service.validatePassword('NoNumbers'),
            throwsA(isA<ValidationError>()
                .having((e) => e.message, 'message', contains('number'))),
          );
        });
      });
    });

    group('Network Errors', () {
      test('should throw NetworkError for taken email', () async {
        expect(
          () => service.registerUser('taken@example.com', 'ValidPass123'),
          throwsA(isA<NetworkError>()
              .having((e) => e.statusCode, 'statusCode', 409)
              .having((e) => e.message, 'message', contains('already exists'))),
        );
      });

      test('should throw NetworkError for server error', () async {
        expect(
          () => service.registerUser('server@error.com', 'ValidPass123'),
          throwsA(isA<NetworkError>()
              .having((e) => e.statusCode, 'statusCode', 500)),
        );
      });

      test('should throw TimeoutException for timeout', () async {
        expect(
          () => service.registerUser('timeout@example.com', 'ValidPass123'),
          throwsA(isA<TimeoutException>()),
        );
      });
    });

    group('Error Recovery', () {
      test('registerUserSafe should return null on validation error', () async {
        final result = await service.registerUserSafe('invalid', 'ValidPass123');
        expect(result, isNull);
      });

      test('registerUserSafe should return null on network error', () async {
        final result = await service.registerUserSafe('taken@example.com', 'ValidPass123');
        expect(result, isNull);
      });

      test('registerUserSafe should return userId on success', () async {
        final result = await service.registerUserSafe('valid@example.com', 'ValidPass123');
        expect(result, isNotNull);
        expect(result, startsWith('user-'));
      });
    });

    group('Batch Processing with Errors', () {
      test('should continue processing after individual failures', () async {
        final users = [
          {'email': 'valid1@example.com', 'password': 'ValidPass123'},
          {'email': 'taken@example.com', 'password': 'ValidPass123'}, // Will fail
          {'email': 'valid2@example.com', 'password': 'ValidPass123'},
        ];

        final results = await service.registerMultipleUsers(users);
        expect(results, hasLength(2)); // Only successful registrations
      });

      test('should throw exception when all registrations fail', () async {
        final users = [
          {'email': 'taken@example.com', 'password': 'ValidPass123'},
          {'email': 'server@error.com', 'password': 'ValidPass123'},
        ];

        expect(
          () => service.registerMultipleUsers(users),
          throwsA(isA<Exception>()
              .having((e) => e.toString(), 'message', contains('All registrations failed'))),
        );
      });
    });

    group('Exception Type Matching', () {
      test('should distinguish between different exception types', () async {
        // Test that we can catch specific exception types
        Object? caughtException;
        
        try {
          await service.registerUser('', 'ValidPass123');
        } catch (e) {
          caughtException = e;
        }
        
        expect(caughtException, isA<ValidationError>());
        expect((caughtException as ValidationError).field, equals('email'));
      });

      test('should handle nested exception scenarios', () async {
        // Test exception handling in complex scenarios
        final futures = [
          service.registerUserSafe('valid@example.com', 'ValidPass123'),
          service.registerUserSafe('invalid', 'ValidPass123'),
          service.registerUserSafe('taken@example.com', 'ValidPass123'),
        ];

        final results = await Future.wait(futures);
        expect(results.where((r) => r != null), hasLength(1));
        expect(results.where((r) => r == null), hasLength(2));
      });
    });
  });
}
```

This example provides comprehensive error testing scenarios including  
custom exceptions, error recovery, batch processing with partial failures,  
and proper exception type verification.

## Parameterized Tests

Using parameterized tests to run the same test with different data sets  
and reduce code duplication.  

```dart
import 'package:flutter_test/flutter_test.dart';

// Service to test
class MathUtils {
  static int factorial(int n) {
    if (n < 0) throw ArgumentError('Factorial not defined for negative numbers');
    if (n == 0 || n == 1) return 1;
    return n * factorial(n - 1);
  }

  static bool isPrime(int n) {
    if (n < 2) return false;
    for (int i = 2; i <= n ~/ 2; i++) {
      if (n % i == 0) return false;
    }
    return true;
  }

  static String formatCurrency(double amount, String currency) {
    switch (currency.toUpperCase()) {
      case 'USD':
        return '\$${amount.toStringAsFixed(2)}';
      case 'EUR':
        return '€${amount.toStringAsFixed(2)}';
      case 'GBP':
        return '£${amount.toStringAsFixed(2)}';
      case 'JPY':
        return '¥${amount.toStringAsFixed(0)}';
      default:
        return '${amount.toStringAsFixed(2)} $currency';
    }
  }

  static String validatePassword(String password) {
    final errors = <String>[];
    
    if (password.length < 8) {
      errors.add('Password must be at least 8 characters long');
    }
    if (!password.contains(RegExp(r'[A-Z]'))) {
      errors.add('Password must contain at least one uppercase letter');
    }
    if (!password.contains(RegExp(r'[a-z]'))) {
      errors.add('Password must contain at least one lowercase letter');
    }
    if (!password.contains(RegExp(r'[0-9]'))) {
      errors.add('Password must contain at least one number');
    }
    if (!password.contains(RegExp(r'[!@#$%^&*(),.?":{}|<>]'))) {
      errors.add('Password must contain at least one special character');
    }
    
    return errors.isEmpty ? 'Valid' : errors.join(', ');
  }
}

// Test data classes
class FactorialTestCase {
  final int input;
  final int expected;
  final String description;

  const FactorialTestCase(this.input, this.expected, this.description);
}

class PrimeTestCase {
  final int input;
  final bool expected;
  final String description;

  const PrimeTestCase(this.input, this.expected, this.description);
}

class CurrencyTestCase {
  final double amount;
  final String currency;
  final String expected;
  final String description;

  const CurrencyTestCase(this.amount, this.currency, this.expected, this.description);
}

class PasswordTestCase {
  final String password;
  final bool shouldBeValid;
  final String description;
  final List<String> expectedErrors;

  const PasswordTestCase(
    this.password, 
    this.shouldBeValid, 
    this.description,
    [this.expectedErrors = const []]
  );
}

void main() {
  group('MathUtils Parameterized Tests', () {
    group('factorial', () {
      final testCases = [
        const FactorialTestCase(0, 1, 'factorial of 0'),
        const FactorialTestCase(1, 1, 'factorial of 1'),
        const FactorialTestCase(2, 2, 'factorial of 2'),
        const FactorialTestCase(3, 6, 'factorial of 3'),
        const FactorialTestCase(4, 24, 'factorial of 4'),
        const FactorialTestCase(5, 120, 'factorial of 5'),
        const FactorialTestCase(10, 3628800, 'factorial of 10'),
      ];

      for (final testCase in testCases) {
        test('should calculate ${testCase.description}', () {
          final result = MathUtils.factorial(testCase.input);
          expect(result, equals(testCase.expected),
              reason: 'factorial(${testCase.input}) should be ${testCase.expected}');
        });
      }

      test('should throw error for negative numbers', () {
        final negativeInputs = [-1, -5, -10];
        for (final input in negativeInputs) {
          expect(() => MathUtils.factorial(input), throwsArgumentError,
              reason: 'factorial($input) should throw ArgumentError');
        }
      });
    });

    group('isPrime', () {
      final primeTestCases = [
        const PrimeTestCase(-1, false, 'negative number'),
        const PrimeTestCase(0, false, 'zero'),
        const PrimeTestCase(1, false, 'one'),
        const PrimeTestCase(2, true, 'smallest prime'),
        const PrimeTestCase(3, true, 'small prime'),
        const PrimeTestCase(4, false, 'composite number'),
        const PrimeTestCase(17, true, 'medium prime'),
        const PrimeTestCase(25, false, 'perfect square'),
        const PrimeTestCase(97, true, 'large prime'),
        const PrimeTestCase(100, false, 'large composite'),
      ];

      for (final testCase in primeTestCases) {
        test('should identify ${testCase.description} (${testCase.input})', () {
          final result = MathUtils.isPrime(testCase.input);
          expect(result, equals(testCase.expected),
              reason: '${testCase.input} prime check failed');
        });
      }
    });

    group('formatCurrency', () {
      final currencyTestCases = [
        const CurrencyTestCase(100.50, 'USD', '\$100.50', 'USD formatting'),
        const CurrencyTestCase(75.25, 'EUR', '€75.25', 'EUR formatting'),
        const CurrencyTestCase(50.00, 'GBP', '£50.00', 'GBP formatting'),
        const CurrencyTestCase(1000.0, 'JPY', '¥1000', 'JPY formatting (no decimals)'),
        const CurrencyTestCase(25.99, 'CAD', '25.99 CAD', 'unknown currency formatting'),
        const CurrencyTestCase(0.00, 'USD', '\$0.00', 'zero amount'),
        const CurrencyTestCase(1234.567, 'EUR', '€1234.57', 'rounding test'),
      ];

      for (final testCase in currencyTestCases) {
        test('should format ${testCase.description}', () {
          final result = MathUtils.formatCurrency(testCase.amount, testCase.currency);
          expect(result, equals(testCase.expected),
              reason: 'Currency formatting failed for ${testCase.amount} ${testCase.currency}');
        });
      }
    });

    group('validatePassword', () {
      final passwordTestCases = [
        const PasswordTestCase(
          'ValidPass123!',
          true,
          'valid password with all requirements',
        ),
        const PasswordTestCase(
          'short',
          false,
          'password too short',
          ['Password must be at least 8 characters long'],
        ),
        const PasswordTestCase(
          'nouppercase123!',
          false,
          'password without uppercase',
          ['Password must contain at least one uppercase letter'],
        ),
        const PasswordTestCase(
          'NOLOWERCASE123!',
          false,
          'password without lowercase',
          ['Password must contain at least one lowercase letter'],
        ),
        const PasswordTestCase(
          'NoNumbers!',
          false,
          'password without numbers',
          ['Password must contain at least one number'],
        ),
        const PasswordTestCase(
          'NoSpecialChars123',
          false,
          'password without special characters',
          ['Password must contain at least one special character'],
        ),
        const PasswordTestCase(
          'weak',
          false,
          'password with multiple violations',
        ),
      ];

      for (final testCase in passwordTestCases) {
        test('should validate ${testCase.description}', () {
          final result = MathUtils.validatePassword(testCase.password);
          
          if (testCase.shouldBeValid) {
            expect(result, equals('Valid'),
                reason: 'Password "${testCase.password}" should be valid');
          } else {
            expect(result, isNot('Valid'),
                reason: 'Password "${testCase.password}" should be invalid');
            
            // Check specific error messages if provided
            for (final expectedError in testCase.expectedErrors) {
              expect(result, contains(expectedError),
                  reason: 'Password validation should contain error: $expectedError');
            }
          }
        });
      }

      // Additional parameterized test using test data from external source
      group('boundary value testing', () {
        final boundaryTests = [
          {'length': 7, 'shouldFail': true, 'description': 'just under minimum length'},
          {'length': 8, 'shouldFail': false, 'description': 'exactly minimum length'},
          {'length': 50, 'shouldFail': false, 'description': 'very long password'},
        ];

        for (final testData in boundaryTests) {
          test('should handle ${testData['description']}', () {
            final password = 'A1!' + 'a' * (testData['length'] as int - 3);
            final result = MathUtils.validatePassword(password);
            
            if (testData['shouldFail'] as bool) {
              expect(result, isNot('Valid'));
            } else {
              expect(result, equals('Valid'));
            }
          });
        }
      });
    });

    // Cross-parameter testing
    group('cross-parameter validation', () {
      final combinations = [
        {'primeInput': 2, 'factorialInput': 2, 'description': 'prime and factorial of 2'},
        {'primeInput': 3, 'factorialInput': 3, 'description': 'prime and factorial of 3'},
        {'primeInput': 5, 'factorialInput': 5, 'description': 'prime and factorial of 5'},
      ];

      for (final combo in combinations) {
        test('should handle ${combo['description']}', () {
          final primeResult = MathUtils.isPrime(combo['primeInput'] as int);
          final factorialResult = MathUtils.factorial(combo['factorialInput'] as int);
          
          expect(primeResult, isTrue, reason: '${combo['primeInput']} should be prime');
          expect(factorialResult, greaterThan(1), 
              reason: 'factorial of ${combo['factorialInput']} should be > 1');
        });
      }
    });
  });
}
```

This example demonstrates comprehensive parameterized testing with test  
data classes, boundary value testing, and cross-parameter validation.  
Each test case includes descriptive names and detailed assertions.

## Basic Widget Testing Setup

Setting up widget testing environment and testing basic widget rendering  
and properties.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

// Widget to test
class CounterWidget extends StatefulWidget {
  final int initialValue;
  final String title;

  const CounterWidget({
    super.key,
    this.initialValue = 0,
    this.title = 'Counter',
  });

  @override
  State<CounterWidget> createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  late int _counter;

  @override
  void initState() {
    super.initState();
    _counter = widget.initialValue;
  }

  void _increment() {
    setState(() {
      _counter++;
    });
  }

  void _decrement() {
    setState(() {
      _counter--;
    });
  }

  void _reset() {
    setState(() {
      _counter = widget.initialValue;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text(widget.title),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const Text('Counter Value:'),
              Text(
                '$_counter',
                style: const TextStyle(fontSize: 48, fontWeight: FontWeight.bold),
                key: const Key('counter-text'),
              ),
              const SizedBox(height: 20),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  ElevatedButton(
                    onPressed: _decrement,
                    key: const Key('decrement-button'),
                    child: const Icon(Icons.remove),
                  ),
                  ElevatedButton(
                    onPressed: _reset,
                    key: const Key('reset-button'),
                    child: const Icon(Icons.refresh),
                  ),
                  ElevatedButton(
                    onPressed: _increment,
                    key: const Key('increment-button'),
                    child: const Icon(Icons.add),
                  ),
                ],
              ),
            ],
          ),
        ),
      ),
    );
  }
}

void main() {
  group('CounterWidget Tests', () {
    testWidgets('should display initial counter value', (WidgetTester tester) async {
      // Build the widget
      await tester.pumpWidget(const CounterWidget(initialValue: 5));

      // Verify initial state
      expect(find.text('5'), findsOneWidget);
      expect(find.text('Counter Value:'), findsOneWidget);
      expect(find.text('Counter'), findsOneWidget);
    });

    testWidgets('should increment counter when increment button is tapped', 
        (WidgetTester tester) async {
      await tester.pumpWidget(const CounterWidget());

      // Verify initial state
      expect(find.text('0'), findsOneWidget);

      // Tap increment button
      await tester.tap(find.byKey(const Key('increment-button')));
      await tester.pump(); // Rebuild the widget

      // Verify state after increment
      expect(find.text('1'), findsOneWidget);
      expect(find.text('0'), findsNothing);
    });

    testWidgets('should decrement counter when decrement button is tapped',
        (WidgetTester tester) async {
      await tester.pumpWidget(const CounterWidget(initialValue: 5));

      // Tap decrement button
      await tester.tap(find.byKey(const Key('decrement-button')));
      await tester.pump();

      // Verify state after decrement
      expect(find.text('4'), findsOneWidget);
      expect(find.text('5'), findsNothing);
    });

    testWidgets('should reset counter when reset button is tapped',
        (WidgetTester tester) async {
      await tester.pumpWidget(const CounterWidget(initialValue: 10));

      // Modify the counter first
      await tester.tap(find.byKey(const Key('increment-button')));
      await tester.pump();
      expect(find.text('11'), findsOneWidget);

      // Reset the counter
      await tester.tap(find.byKey(const Key('reset-button')));
      await tester.pump();

      // Verify reset to initial value
      expect(find.text('10'), findsOneWidget);
      expect(find.text('11'), findsNothing);
    });

    testWidgets('should display custom title', (WidgetTester tester) async {
      await tester.pumpWidget(const CounterWidget(title: 'My Custom Counter'));

      expect(find.text('My Custom Counter'), findsOneWidget);
      expect(find.text('Counter'), findsNothing);
    });

    testWidgets('should find widgets by key', (WidgetTester tester) async {
      await tester.pumpWidget(const CounterWidget());

      // Find widgets by their keys
      expect(find.byKey(const Key('counter-text')), findsOneWidget);
      expect(find.byKey(const Key('increment-button')), findsOneWidget);
      expect(find.byKey(const Key('decrement-button')), findsOneWidget);
      expect(find.byKey(const Key('reset-button')), findsOneWidget);
    });

    testWidgets('should find widgets by type', (WidgetTester tester) async {
      await tester.pumpWidget(const CounterWidget());

      // Find widgets by their type
      expect(find.byType(ElevatedButton), findsNWidgets(3));
      expect(find.byType(Text), findsNWidgets(3)); // 'Counter Value:', counter value, and title
      expect(find.byType(Icon), findsNWidgets(3));
      expect(find.byType(AppBar), findsOneWidget);
      expect(find.byType(Scaffold), findsOneWidget);
    });

    testWidgets('should handle multiple taps correctly', (WidgetTester tester) async {
      await tester.pumpWidget(const CounterWidget());

      // Multiple increments
      for (int i = 1; i <= 5; i++) {
        await tester.tap(find.byKey(const Key('increment-button')));
        await tester.pump();
        expect(find.text('$i'), findsOneWidget);
      }

      // Multiple decrements
      for (int i = 4; i >= 0; i--) {
        await tester.tap(find.byKey(const Key('decrement-button')));
        await tester.pump();
        expect(find.text('$i'), findsOneWidget);
      }
    });

    group('Counter Widget Edge Cases', () {
      testWidgets('should handle negative numbers', (WidgetTester tester) async {
        await tester.pumpWidget(const CounterWidget());

        // Decrement from 0 to negative
        await tester.tap(find.byKey(const Key('decrement-button')));
        await tester.pump();

        expect(find.text('-1'), findsOneWidget);
      });

      testWidgets('should handle large numbers', (WidgetTester tester) async {
        await tester.pumpWidget(const CounterWidget(initialValue: 999999));

        await tester.tap(find.byKey(const Key('increment-button')));
        await tester.pump();

        expect(find.text('1000000'), findsOneWidget);
      });
    });
  });
}
```

This example demonstrates basic widget testing fundamentals including  
widget setup, finding widgets by different methods (text, key, type),  
testing user interactions, and verifying state changes after interactions.

## Testing Widget Rendering

Advanced widget rendering tests including layout verification,  
style testing, and conditional rendering.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

// Widget with complex rendering logic
class UserProfileWidget extends StatelessWidget {
  final String name;
  final String email;
  final String? avatarUrl;
  final bool isOnline;
  final int messageCount;
  final bool isVerified;
  final Color themeColor;

  const UserProfileWidget({
    super.key,
    required this.name,
    required this.email,
    this.avatarUrl,
    this.isOnline = false,
    this.messageCount = 0,
    this.isVerified = false,
    this.themeColor = Colors.blue,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.all(8.0),
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                CircleAvatar(
                  radius: 30,
                  backgroundColor: themeColor,
                  backgroundImage: avatarUrl != null ? NetworkImage(avatarUrl!) : null,
                  child: avatarUrl == null 
                      ? Text(
                          name.isNotEmpty ? name[0].toUpperCase() : '?',
                          style: const TextStyle(fontSize: 24, color: Colors.white),
                        )
                      : null,
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Row(
                        children: [
                          Flexible(
                            child: Text(
                              name,
                              style: const TextStyle(
                                fontSize: 18,
                                fontWeight: FontWeight.bold,
                              ),
                              overflow: TextOverflow.ellipsis,
                            ),
                          ),
                          if (isVerified) ...[
                            const SizedBox(width: 4),
                            Icon(
                              Icons.verified,
                              color: themeColor,
                              size: 16,
                            ),
                          ],
                        ],
                      ),
                      Text(
                        email,
                        style: TextStyle(
                          fontSize: 14,
                          color: Colors.grey[600],
                        ),
                      ),
                    ],
                  ),
                ),
                Container(
                  width: 12,
                  height: 12,
                  decoration: BoxDecoration(
                    shape: BoxShape.circle,
                    color: isOnline ? Colors.green : Colors.grey,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            if (messageCount > 0)
              Container(
                padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
                decoration: BoxDecoration(
                  color: themeColor.withOpacity(0.1),
                  borderRadius: BorderRadius.circular(12),
                ),
                child: Text(
                  messageCount == 1 ? '1 new message' : '$messageCount new messages',
                  style: TextStyle(
                    color: themeColor,
                    fontSize: 12,
                    fontWeight: FontWeight.w500,
                  ),
                ),
              ),
          ],
        ),
      ),
    );
  }
}

void main() {
  group('UserProfileWidget Rendering Tests', () {
    testWidgets('should render basic user information', (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: 'John Doe',
              email: 'john.doe@example.com',
            ),
          ),
        ),
      );

      // Verify basic information is displayed
      expect(find.text('John Doe'), findsOneWidget);
      expect(find.text('john.doe@example.com'), findsOneWidget);
      expect(find.byType(CircleAvatar), findsOneWidget);
      expect(find.byType(Card), findsOneWidget);
    });

    testWidgets('should show avatar placeholder when no URL provided', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: 'Jane Smith',
              email: 'jane@example.com',
            ),
          ),
        ),
      );

      // Should show first letter of name in avatar
      expect(find.text('J'), findsOneWidget);
      
      // CircleAvatar should not have backgroundImage
      final CircleAvatar avatar = tester.widget(find.byType(CircleAvatar));
      expect(avatar.backgroundImage, isNull);
      expect(avatar.child, isNotNull);
    });

    testWidgets('should display verified icon when user is verified', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: 'Verified User',
              email: 'verified@example.com',
              isVerified: true,
            ),
          ),
        ),
      );

      expect(find.byIcon(Icons.verified), findsOneWidget);
    });

    testWidgets('should not display verified icon when user is not verified', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: 'Regular User',
              email: 'regular@example.com',
              isVerified: false,
            ),
          ),
        ),
      );

      expect(find.byIcon(Icons.verified), findsNothing);
    });

    testWidgets('should show online status indicator correctly', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: 'Online User',
              email: 'online@example.com',
              isOnline: true,
            ),
          ),
        ),
      );

      // Find the status indicator container
      final Finder containerFinder = find.byWidgetPredicate(
        (Widget widget) =>
            widget is Container &&
            widget.decoration is BoxDecoration &&
            (widget.decoration as BoxDecoration).color == Colors.green,
      );

      expect(containerFinder, findsOneWidget);
    });

    testWidgets('should display message count when messages exist', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: 'User with Messages',
              email: 'messages@example.com',
              messageCount: 5,
            ),
          ),
        ),
      );

      expect(find.text('5 new messages'), findsOneWidget);
    });

    testWidgets('should display singular message text for one message', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: 'User with One Message',
              email: 'onemessage@example.com',
              messageCount: 1,
            ),
          ),
        ),
      );

      expect(find.text('1 new message'), findsOneWidget);
    });

    testWidgets('should not display message count when no messages', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: 'No Messages User',
              email: 'nomessages@example.com',
              messageCount: 0,
            ),
          ),
        ),
      );

      expect(find.textContaining('message'), findsNothing);
    });

    testWidgets('should apply custom theme color', (WidgetTester tester) async {
      const customColor = Colors.purple;
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: 'Themed User',
              email: 'themed@example.com',
              isVerified: true,
              messageCount: 3,
              themeColor: customColor,
            ),
          ),
        ),
      );

      // Check CircleAvatar background color
      final CircleAvatar avatar = tester.widget(find.byType(CircleAvatar));
      expect(avatar.backgroundColor, equals(customColor));

      // Check verified icon color
      final Icon verifiedIcon = tester.widget(find.byIcon(Icons.verified));
      expect(verifiedIcon.color, equals(customColor));
    });

    testWidgets('should handle empty name gracefully', (WidgetTester tester) async {
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserProfileWidget(
              name: '',
              email: 'empty@example.com',
            ),
          ),
        ),
      );

      // Should show '?' as placeholder in avatar
      expect(find.text('?'), findsOneWidget);
      expect(find.text(''), findsOneWidget); // Empty name text
    });

    testWidgets('should handle very long names', (WidgetTester tester) async {
      const longName = 'This is a very long name that should be truncated';
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: SizedBox(
              width: 300, // Constrain width to test ellipsis
              child: UserProfileWidget(
                name: longName,
                email: 'long@example.com',
              ),
            ),
          ),
        ),
      );

      // The text should still be found (even if visually truncated)
      expect(find.text(longName), findsOneWidget);
      
      // Verify text has ellipsis overflow
      final Text nameText = tester.widget(find.text(longName));
      expect(nameText.overflow, equals(TextOverflow.ellipsis));
    });

    group('Layout Structure Tests', () {
      testWidgets('should have correct widget hierarchy', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: UserProfileWidget(
                name: 'Test User',
                email: 'test@example.com',
              ),
            ),
          ),
        );

        // Verify widget hierarchy
        expect(find.byType(Card), findsOneWidget);
        expect(find.byType(Padding), findsWidgets);
        expect(find.byType(Column), findsWidgets);
        expect(find.byType(Row), findsWidgets);
        expect(find.byType(CircleAvatar), findsOneWidget);
        expect(find.byType(Text), findsWidgets);
      });

      testWidgets('should have proper spacing', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: UserProfileWidget(
                name: 'Spaced User',
                email: 'spaced@example.com',
              ),
            ),
          ),
        );

        // Check for SizedBox widgets that provide spacing
        expect(find.byWidgetPredicate((widget) => 
            widget is SizedBox && widget.width == 12), findsOneWidget);
        expect(find.byWidgetPredicate((widget) => 
            widget is SizedBox && widget.height == 12), findsOneWidget);
      });
    });

    group('Style Verification Tests', () {
      testWidgets('should apply correct text styles', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: UserProfileWidget(
                name: 'Styled User',
                email: 'styled@example.com',
              ),
            ),
          ),
        );

        // Find name text and verify style
        final Finder nameTextFinder = find.text('Styled User');
        final Text nameText = tester.widget(nameTextFinder);
        expect(nameText.style?.fontSize, equals(18));
        expect(nameText.style?.fontWeight, equals(FontWeight.bold));

        // Find email text and verify style
        final Finder emailTextFinder = find.text('styled@example.com');
        final Text emailText = tester.widget(emailTextFinder);
        expect(emailText.style?.fontSize, equals(14));
      });
    });
  });
}
```

This example demonstrates advanced widget rendering tests including  
conditional rendering, style verification, layout structure testing,  
and handling of edge cases like empty values and long text content.

## Testing User Interactions

Testing complex user interactions including taps, gestures, scrolling,  
and multi-step interaction flows.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

// Interactive widget to test
class InteractiveListWidget extends StatefulWidget {
  final List<String> initialItems;

  const InteractiveListWidget({
    super.key,
    this.initialItems = const [],
  });

  @override
  State<InteractiveListWidget> createState() => _InteractiveListWidgetState();
}

class _InteractiveListWidgetState extends State<InteractiveListWidget> {
  late List<String> _items;
  final TextEditingController _textController = TextEditingController();
  final FocusNode _focusNode = FocusNode();
  bool _isAddingItem = false;

  @override
  void initState() {
    super.initState();
    _items = List.from(widget.initialItems);
  }

  @override
  void dispose() {
    _textController.dispose();
    _focusNode.dispose();
    super.dispose();
  }

  void _addItem() {
    final text = _textController.text.trim();
    if (text.isNotEmpty) {
      setState(() {
        _items.add(text);
        _textController.clear();
        _isAddingItem = false;
      });
    }
  }

  void _removeItem(int index) {
    setState(() {
      _items.removeAt(index);
    });
  }

  void _toggleAddMode() {
    setState(() {
      _isAddingItem = !_isAddingItem;
      if (_isAddingItem) {
        _focusNode.requestFocus();
      }
    });
  }

  void _editItem(int index, String newText) {
    if (newText.trim().isNotEmpty) {
      setState(() {
        _items[index] = newText.trim();
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Interactive List'),
          actions: [
            IconButton(
              key: const Key('add-toggle-button'),
              icon: Icon(_isAddingItem ? Icons.close : Icons.add),
              onPressed: _toggleAddMode,
            ),
          ],
        ),
        body: Column(
          children: [
            if (_isAddingItem)
              Padding(
                padding: const EdgeInsets.all(16.0),
                child: Row(
                  children: [
                    Expanded(
                      child: TextField(
                        key: const Key('add-item-field'),
                        controller: _textController,
                        focusNode: _focusNode,
                        decoration: const InputDecoration(
                          hintText: 'Enter new item',
                          border: OutlineInputBorder(),
                        ),
                        onSubmitted: (_) => _addItem(),
                      ),
                    ),
                    const SizedBox(width: 8),
                    ElevatedButton(
                      key: const Key('confirm-add-button'),
                      onPressed: _addItem,
                      child: const Text('Add'),
                    ),
                  ],
                ),
              ),
            Expanded(
              child: _items.isEmpty
                  ? const Center(
                      child: Text(
                        'No items yet. Tap + to add some!',
                        key: Key('empty-message'),
                      ),
                    )
                  : ListView.builder(
                      key: const Key('items-list'),
                      itemCount: _items.length,
                      itemBuilder: (context, index) {
                        return ListTile(
                          key: Key('item-$index'),
                          title: GestureDetector(
                            key: Key('item-text-$index'),
                            onDoubleTap: () => _showEditDialog(context, index),
                            child: Text(_items[index]),
                          ),
                          trailing: IconButton(
                            key: Key('delete-button-$index'),
                            icon: const Icon(Icons.delete, color: Colors.red),
                            onPressed: () => _removeItem(index),
                          ),
                        );
                      },
                    ),
            ),
          ],
        ),
      ),
    );
  }

  void _showEditDialog(BuildContext context, int index) {
    final controller = TextEditingController(text: _items[index]);
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        key: const Key('edit-dialog'),
        title: const Text('Edit Item'),
        content: TextField(
          key: const Key('edit-field'),
          controller: controller,
          autofocus: true,
          decoration: const InputDecoration(
            border: OutlineInputBorder(),
          ),
        ),
        actions: [
          TextButton(
            key: const Key('cancel-edit-button'),
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            key: const Key('save-edit-button'),
            onPressed: () {
              _editItem(index, controller.text);
              Navigator.of(context).pop();
            },
            child: const Text('Save'),
          ),
        ],
      ),
    );
  }
}

void main() {
  group('InteractiveListWidget Interaction Tests', () {
    testWidgets('should toggle add mode when add button is tapped', 
        (WidgetTester tester) async {
      await tester.pumpWidget(const InteractiveListWidget());

      // Initially not in add mode
      expect(find.byKey(const Key('add-item-field')), findsNothing);
      expect(find.byIcon(Icons.add), findsOneWidget);

      // Tap add button
      await tester.tap(find.byKey(const Key('add-toggle-button')));
      await tester.pump();

      // Should now be in add mode
      expect(find.byKey(const Key('add-item-field')), findsOneWidget);
      expect(find.byIcon(Icons.close), findsOneWidget);
    });

    testWidgets('should add new item when text is entered and button tapped', 
        (WidgetTester tester) async {
      await tester.pumpWidget(const InteractiveListWidget());

      // Enter add mode
      await tester.tap(find.byKey(const Key('add-toggle-button')));
      await tester.pump();

      // Enter text
      await tester.enterText(find.byKey(const Key('add-item-field')), 'New Item');
      
      // Tap add button
      await tester.tap(find.byKey(const Key('confirm-add-button')));
      await tester.pump();

      // Verify item was added
      expect(find.text('New Item'), findsOneWidget);
      expect(find.byKey(const Key('item-0')), findsOneWidget);
      expect(find.byKey(const Key('empty-message')), findsNothing);
    });

    testWidgets('should add new item when Enter is pressed in text field', 
        (WidgetTester tester) async {
      await tester.pumpWidget(const InteractiveListWidget());

      // Enter add mode
      await tester.tap(find.byKey(const Key('add-toggle-button')));
      await tester.pump();

      // Enter text and press Enter
      await tester.enterText(find.byKey(const Key('add-item-field')), 'Enter Item');
      await tester.testTextInput.receiveAction(TextInputAction.done);
      await tester.pump();

      // Verify item was added
      expect(find.text('Enter Item'), findsOneWidget);
    });

    testWidgets('should not add empty items', (WidgetTester tester) async {
      await tester.pumpWidget(const InteractiveListWidget());

      // Enter add mode
      await tester.tap(find.byKey(const Key('add-toggle-button')));
      await tester.pump();

      // Try to add empty item
      await tester.tap(find.byKey(const Key('confirm-add-button')));
      await tester.pump();

      // Should still show empty message
      expect(find.byKey(const Key('empty-message')), findsOneWidget);
      expect(find.byKey(const Key('items-list')), findsNothing);
    });

    testWidgets('should remove item when delete button is tapped', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        const InteractiveListWidget(initialItems: ['Item 1', 'Item 2', 'Item 3']),
      );

      // Verify initial items
      expect(find.text('Item 1'), findsOneWidget);
      expect(find.text('Item 2'), findsOneWidget);
      expect(find.text('Item 3'), findsOneWidget);

      // Delete middle item
      await tester.tap(find.byKey(const Key('delete-button-1')));
      await tester.pump();

      // Verify item was removed
      expect(find.text('Item 1'), findsOneWidget);
      expect(find.text('Item 2'), findsNothing);
      expect(find.text('Item 3'), findsOneWidget);
    });

    testWidgets('should show edit dialog when item is double-tapped', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        const InteractiveListWidget(initialItems: ['Editable Item']),
      );

      // Double tap on item text
      await tester.tap(find.byKey(const Key('item-text-0')), warnIfMissed: false);
      await tester.pump(const Duration(milliseconds: 100));
      await tester.tap(find.byKey(const Key('item-text-0')), warnIfMissed: false);
      await tester.pump();

      // Verify edit dialog appears
      expect(find.byKey(const Key('edit-dialog')), findsOneWidget);
      expect(find.text('Edit Item'), findsOneWidget);
      expect(find.byKey(const Key('edit-field')), findsOneWidget);
    });

    testWidgets('should edit item when save is tapped in dialog', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        const InteractiveListWidget(initialItems: ['Original Item']),
      );

      // Open edit dialog
      await tester.tap(find.byKey(const Key('item-text-0')), warnIfMissed: false);
      await tester.pump(const Duration(milliseconds: 100));
      await tester.tap(find.byKey(const Key('item-text-0')), warnIfMissed: false);
      await tester.pump();

      // Edit the text
      await tester.enterText(find.byKey(const Key('edit-field')), 'Modified Item');
      
      // Save changes
      await tester.tap(find.byKey(const Key('save-edit-button')));
      await tester.pump();

      // Verify item was updated
      expect(find.text('Modified Item'), findsOneWidget);
      expect(find.text('Original Item'), findsNothing);
      expect(find.byKey(const Key('edit-dialog')), findsNothing);
    });

    testWidgets('should cancel edit when cancel is tapped in dialog', 
        (WidgetTester tester) async {
      await tester.pumpWidget(
        const InteractiveListWidget(initialItems: ['Unchanged Item']),
      );

      // Open edit dialog
      await tester.tap(find.byKey(const Key('item-text-0')), warnIfMissed: false);
      await tester.pump(const Duration(milliseconds: 100));
      await tester.tap(find.byKey(const Key('item-text-0')), warnIfMissed: false);
      await tester.pump();

      // Modify text but cancel
      await tester.enterText(find.byKey(const Key('edit-field')), 'Should Not Change');
      await tester.tap(find.byKey(const Key('cancel-edit-button')));
      await tester.pump();

      // Verify item was not changed
      expect(find.text('Unchanged Item'), findsOneWidget);
      expect(find.text('Should Not Change'), findsNothing);
      expect(find.byKey(const Key('edit-dialog')), findsNothing);
    });

    testWidgets('should handle multiple rapid interactions', 
        (WidgetTester tester) async {
      await tester.pumpWidget(const InteractiveListWidget());

      // Rapid add mode toggling
      for (int i = 0; i < 5; i++) {
        await tester.tap(find.byKey(const Key('add-toggle-button')));
        await tester.pump();
      }

      // Should be in add mode (odd number of taps)
      expect(find.byKey(const Key('add-item-field')), findsOneWidget);

      // Add multiple items rapidly
      final items = ['Item A', 'Item B', 'Item C'];
      for (int i = 0; i < items.length; i++) {
        await tester.enterText(find.byKey(const Key('add-item-field')), items[i]);
        await tester.tap(find.byKey(const Key('confirm-add-button')));
        await tester.pump();
      }

      // Verify all items were added
      for (final item in items) {
        expect(find.text(item), findsOneWidget);
      }
    });

    testWidgets('should handle scrolling with many items', 
        (WidgetTester tester) async {
      // Create a list with many items
      final manyItems = List.generate(50, (index) => 'Item $index');
      
      await tester.pumpWidget(
        InteractiveListWidget(initialItems: manyItems),
      );

      // Verify first few items are visible
      expect(find.text('Item 0'), findsOneWidget);
      expect(find.text('Item 1'), findsOneWidget);

      // Scroll down to find items that are initially off-screen
      await tester.scrollUntilVisible(
        find.text('Item 49'),
        500.0,
        scrollable: find.byKey(const Key('items-list')),
      );

      // Verify last item is now visible
      expect(find.text('Item 49'), findsOneWidget);
    });

    group('Focus and Keyboard Interaction Tests', () {
      testWidgets('should focus text field when entering add mode', 
          (WidgetTester tester) async {
        await tester.pumpWidget(const InteractiveListWidget());

        // Enter add mode
        await tester.tap(find.byKey(const Key('add-toggle-button')));
        await tester.pump();

        // Verify text field has focus
        final TextField textField = tester.widget(find.byKey(const Key('add-item-field')));
        expect(textField.focusNode?.hasFocus, isTrue);
      });

      testWidgets('should handle keyboard navigation', (WidgetTester tester) async {
        await tester.pumpWidget(const InteractiveListWidget());

        // Enter add mode
        await tester.tap(find.byKey(const Key('add-toggle-button')));
        await tester.pump();

        // Test Tab key navigation (would move focus to next widget)
        await tester.sendKeyEvent(LogicalKeyboardKey.tab);
        await tester.pump();

        // This test verifies that keyboard events are handled properly
        // In a real app, you might check focus changes between widgets
      });
    });

    group('Edge Case Interaction Tests', () {
      testWidgets('should handle whitespace-only input', (WidgetTester tester) async {
        await tester.pumpWidget(const InteractiveListWidget());

        // Enter add mode
        await tester.tap(find.byKey(const Key('add-toggle-button')));
        await tester.pump();

        // Enter only whitespace
        await tester.enterText(find.byKey(const Key('add-item-field')), '   ');
        await tester.tap(find.byKey(const Key('confirm-add-button')));
        await tester.pump();

        // Should not add the item
        expect(find.byKey(const Key('empty-message')), findsOneWidget);
      });

      testWidgets('should handle very long item names', (WidgetTester tester) async {
        const longText = 'This is a very long item name that should be handled gracefully';
        
        await tester.pumpWidget(const InteractiveListWidget());

        // Add long item
        await tester.tap(find.byKey(const Key('add-toggle-button')));
        await tester.pump();
        await tester.enterText(find.byKey(const Key('add-item-field')), longText);
        await tester.tap(find.byKey(const Key('confirm-add-button')));
        await tester.pump();

        // Verify long text is displayed
        expect(find.text(longText), findsOneWidget);
      });
    });
  });
}
```

This example demonstrates comprehensive user interaction testing including  
taps, double-taps, text input, dialog interactions, scrolling, focus  
management, and handling of edge cases like rapid interactions and  
long content.

## Testing Form Inputs

Testing complex forms with validation, different input types, and  
form submission workflows.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

// Form widget to test
class UserRegistrationForm extends StatefulWidget {
  final Function(Map<String, String> userData)? onSubmit;

  const UserRegistrationForm({super.key, this.onSubmit});

  @override
  State<UserRegistrationForm> createState() => _UserRegistrationFormState();
}

class _UserRegistrationFormState extends State<UserRegistrationForm> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  final _confirmPasswordController = TextEditingController();
  
  bool _acceptTerms = false;
  String _selectedCountry = '';
  bool _isSubmitting = false;
  
  final List<String> _countries = [
    'United States', 'Canada', 'United Kingdom', 'Germany', 'France'
  ];

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _passwordController.dispose();
    _confirmPasswordController.dispose();
    super.dispose();
  }

  String? _validateName(String? value) {
    if (value == null || value.trim().isEmpty) {
      return 'Name is required';
    }
    if (value.trim().length < 2) {
      return 'Name must be at least 2 characters';
    }
    return null;
  }

  String? _validateEmail(String? value) {
    if (value == null || value.trim().isEmpty) {
      return 'Email is required';
    }
    if (!RegExp(r'^[^@]+@[^@]+\.[^@]+').hasMatch(value)) {
      return 'Please enter a valid email';
    }
    return null;
  }

  String? _validatePassword(String? value) {
    if (value == null || value.isEmpty) {
      return 'Password is required';
    }
    if (value.length < 8) {
      return 'Password must be at least 8 characters';
    }
    if (!RegExp(r'[A-Z]').hasMatch(value)) {
      return 'Password must contain an uppercase letter';
    }
    if (!RegExp(r'[0-9]').hasMatch(value)) {
      return 'Password must contain a number';
    }
    return null;
  }

  String? _validateConfirmPassword(String? value) {
    if (value != _passwordController.text) {
      return 'Passwords do not match';
    }
    return null;
  }

  Future<void> _submitForm() async {
    if (!_formKey.currentState!.validate()) {
      return;
    }

    if (!_acceptTerms) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Please accept the terms and conditions'),
          key: Key('terms-error-snackbar'),
        ),
      );
      return;
    }

    if (_selectedCountry.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Please select a country'),
          key: Key('country-error-snackbar'),
        ),
      );
      return;
    }

    setState(() {
      _isSubmitting = true;
    });

    // Simulate API call
    await Future.delayed(const Duration(milliseconds: 500));

    final userData = {
      'name': _nameController.text.trim(),
      'email': _emailController.text.trim(),
      'password': _passwordController.text,
      'country': _selectedCountry,
    };

    widget.onSubmit?.call(userData);

    setState(() {
      _isSubmitting = false;
    });

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Registration successful!'),
          key: Key('success-snackbar'),
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text('User Registration')),
        body: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Form(
            key: _formKey,
            child: Column(
              children: [
                TextFormField(
                  key: const Key('name-field'),
                  controller: _nameController,
                  decoration: const InputDecoration(
                    labelText: 'Full Name',
                    border: OutlineInputBorder(),
                  ),
                  validator: _validateName,
                ),
                const SizedBox(height: 16),
                TextFormField(
                  key: const Key('email-field'),
                  controller: _emailController,
                  keyboardType: TextInputType.emailAddress,
                  decoration: const InputDecoration(
                    labelText: 'Email',
                    border: OutlineInputBorder(),
                  ),
                  validator: _validateEmail,
                ),
                const SizedBox(height: 16),
                TextFormField(
                  key: const Key('password-field'),
                  controller: _passwordController,
                  obscureText: true,
                  decoration: const InputDecoration(
                    labelText: 'Password',
                    border: OutlineInputBorder(),
                  ),
                  validator: _validatePassword,
                ),
                const SizedBox(height: 16),
                TextFormField(
                  key: const Key('confirm-password-field'),
                  controller: _confirmPasswordController,
                  obscureText: true,
                  decoration: const InputDecoration(
                    labelText: 'Confirm Password',
                    border: OutlineInputBorder(),
                  ),
                  validator: _validateConfirmPassword,
                ),
                const SizedBox(height: 16),
                DropdownButtonFormField<String>(
                  key: const Key('country-dropdown'),
                  value: _selectedCountry.isEmpty ? null : _selectedCountry,
                  decoration: const InputDecoration(
                    labelText: 'Country',
                    border: OutlineInputBorder(),
                  ),
                  items: _countries.map((country) {
                    return DropdownMenuItem(
                      value: country,
                      child: Text(country),
                    );
                  }).toList(),
                  onChanged: (value) {
                    setState(() {
                      _selectedCountry = value ?? '';
                    });
                  },
                ),
                const SizedBox(height: 16),
                CheckboxListTile(
                  key: const Key('terms-checkbox'),
                  value: _acceptTerms,
                  onChanged: (value) {
                    setState(() {
                      _acceptTerms = value ?? false;
                    });
                  },
                  title: const Text('I accept the terms and conditions'),
                  controlAffinity: ListTileControlAffinity.leading,
                ),
                const SizedBox(height: 24),
                SizedBox(
                  width: double.infinity,
                  height: 50,
                  child: ElevatedButton(
                    key: const Key('submit-button'),
                    onPressed: _isSubmitting ? null : _submitForm,
                    child: _isSubmitting 
                        ? const CircularProgressIndicator()
                        : const Text('Register'),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

void main() {
  group('UserRegistrationForm Tests', () {
    testWidgets('should display all form fields', (WidgetTester tester) async {
      await tester.pumpWidget(const UserRegistrationForm());

      // Verify all form fields are present
      expect(find.byKey(const Key('name-field')), findsOneWidget);
      expect(find.byKey(const Key('email-field')), findsOneWidget);
      expect(find.byKey(const Key('password-field')), findsOneWidget);
      expect(find.byKey(const Key('confirm-password-field')), findsOneWidget);
      expect(find.byKey(const Key('country-dropdown')), findsOneWidget);
      expect(find.byKey(const Key('terms-checkbox')), findsOneWidget);
      expect(find.byKey(const Key('submit-button')), findsOneWidget);
    });

    group('Form Validation Tests', () {
      testWidgets('should show validation errors for empty required fields',
          (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Try to submit empty form
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        // Verify validation error messages
        expect(find.text('Name is required'), findsOneWidget);
        expect(find.text('Email is required'), findsOneWidget);
        expect(find.text('Password is required'), findsOneWidget);
      });

      testWidgets('should validate name field correctly', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Test too short name
        await tester.enterText(find.byKey(const Key('name-field')), 'A');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Name must be at least 2 characters'), findsOneWidget);

        // Test valid name
        await tester.enterText(find.byKey(const Key('name-field')), 'John Doe');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Name must be at least 2 characters'), findsNothing);
      });

      testWidgets('should validate email field correctly', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Test invalid email
        await tester.enterText(find.byKey(const Key('email-field')), 'invalid-email');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Please enter a valid email'), findsOneWidget);

        // Test valid email
        await tester.enterText(find.byKey(const Key('email-field')), 'test@example.com');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Please enter a valid email'), findsNothing);
      });

      testWidgets('should validate password requirements', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Test short password
        await tester.enterText(find.byKey(const Key('password-field')), 'short');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Password must be at least 8 characters'), findsOneWidget);

        // Test password without uppercase
        await tester.enterText(find.byKey(const Key('password-field')), 'lowercase123');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Password must contain an uppercase letter'), findsOneWidget);

        // Test password without number
        await tester.enterText(find.byKey(const Key('password-field')), 'NoNumbers');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Password must contain a number'), findsOneWidget);

        // Test valid password
        await tester.enterText(find.byKey(const Key('password-field')), 'ValidPass123');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Password must be at least 8 characters'), findsNothing);
      });

      testWidgets('should validate password confirmation', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Enter password
        await tester.enterText(find.byKey(const Key('password-field')), 'ValidPass123');
        
        // Enter different confirmation
        await tester.enterText(find.byKey(const Key('confirm-password-field')), 'DifferentPass123');
        
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Passwords do not match'), findsOneWidget);

        // Enter matching confirmation
        await tester.enterText(find.byKey(const Key('confirm-password-field')), 'ValidPass123');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Passwords do not match'), findsNothing);
      });
    });

    group('Form Input Tests', () {
      testWidgets('should accept text input in all fields', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Enter text in all fields
        await tester.enterText(find.byKey(const Key('name-field')), 'Test User');
        await tester.enterText(find.byKey(const Key('email-field')), 'test@example.com');
        await tester.enterText(find.byKey(const Key('password-field')), 'TestPass123');
        await tester.enterText(find.byKey(const Key('confirm-password-field')), 'TestPass123');

        // Verify text was entered
        expect(find.text('Test User'), findsOneWidget);
        expect(find.text('test@example.com'), findsOneWidget);
        // Password fields show asterisks, so we can't test displayed text
      });

      testWidgets('should handle dropdown selection', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Tap dropdown to open
        await tester.tap(find.byKey(const Key('country-dropdown')));
        await tester.pump();

        // Select a country
        await tester.tap(find.text('Canada').last);
        await tester.pump();

        // Verify selection (dropdown should show selected value)
        final DropdownButtonFormField<String> dropdown = 
            tester.widget(find.byKey(const Key('country-dropdown')));
        expect(dropdown.value, equals('Canada'));
      });

      testWidgets('should handle checkbox interaction', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Initially unchecked
        CheckboxListTile checkbox = tester.widget(find.byKey(const Key('terms-checkbox')));
        expect(checkbox.value, isFalse);

        // Tap to check
        await tester.tap(find.byKey(const Key('terms-checkbox')));
        await tester.pump();

        // Verify checked
        checkbox = tester.widget(find.byKey(const Key('terms-checkbox')));
        expect(checkbox.value, isTrue);

        // Tap to uncheck
        await tester.tap(find.byKey(const Key('terms-checkbox')));
        await tester.pump();

        // Verify unchecked
        checkbox = tester.widget(find.byKey(const Key('terms-checkbox')));
        expect(checkbox.value, isFalse);
      });
    });

    group('Form Submission Tests', () {
      testWidgets('should show error when terms not accepted', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Fill valid form data but don't accept terms
        await tester.enterText(find.byKey(const Key('name-field')), 'Test User');
        await tester.enterText(find.byKey(const Key('email-field')), 'test@example.com');
        await tester.enterText(find.byKey(const Key('password-field')), 'ValidPass123');
        await tester.enterText(find.byKey(const Key('confirm-password-field')), 'ValidPass123');

        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.byKey(const Key('terms-error-snackbar')), findsOneWidget);
      });

      testWidgets('should show error when country not selected', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Fill valid form data and accept terms but don't select country
        await tester.enterText(find.byKey(const Key('name-field')), 'Test User');
        await tester.enterText(find.byKey(const Key('email-field')), 'test@example.com');
        await tester.enterText(find.byKey(const Key('password-field')), 'ValidPass123');
        await tester.enterText(find.byKey(const Key('confirm-password-field')), 'ValidPass123');
        await tester.tap(find.byKey(const Key('terms-checkbox')));
        await tester.pump();

        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.byKey(const Key('country-error-snackbar')), findsOneWidget);
      });

      testWidgets('should submit form successfully with valid data', (WidgetTester tester) async {
        Map<String, String>? submittedData;
        
        await tester.pumpWidget(
          UserRegistrationForm(
            onSubmit: (data) => submittedData = data,
          ),
        );

        // Fill all form fields with valid data
        await tester.enterText(find.byKey(const Key('name-field')), 'Test User');
        await tester.enterText(find.byKey(const Key('email-field')), 'test@example.com');
        await tester.enterText(find.byKey(const Key('password-field')), 'ValidPass123');
        await tester.enterText(find.byKey(const Key('confirm-password-field')), 'ValidPass123');
        
        // Select country
        await tester.tap(find.byKey(const Key('country-dropdown')));
        await tester.pump();
        await tester.tap(find.text('Canada').last);
        await tester.pump();
        
        // Accept terms
        await tester.tap(find.byKey(const Key('terms-checkbox')));
        await tester.pump();

        // Submit form
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        // Wait for submission to complete
        await tester.pump(const Duration(milliseconds: 600));

        // Verify success message
        expect(find.byKey(const Key('success-snackbar')), findsOneWidget);
        
        // Verify callback was called with correct data
        expect(submittedData, isNotNull);
        expect(submittedData!['name'], equals('Test User'));
        expect(submittedData!['email'], equals('test@example.com'));
        expect(submittedData!['password'], equals('ValidPass123'));
        expect(submittedData!['country'], equals('Canada'));
      });

      testWidgets('should show loading state during submission', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Fill valid form
        await tester.enterText(find.byKey(const Key('name-field')), 'Test User');
        await tester.enterText(find.byKey(const Key('email-field')), 'test@example.com');
        await tester.enterText(find.byKey(const Key('password-field')), 'ValidPass123');
        await tester.enterText(find.byKey(const Key('confirm-password-field')), 'ValidPass123');
        
        await tester.tap(find.byKey(const Key('country-dropdown')));
        await tester.pump();
        await tester.tap(find.text('Canada').last);
        await tester.pump();
        
        await tester.tap(find.byKey(const Key('terms-checkbox')));
        await tester.pump();

        // Submit form
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        // Verify loading state
        expect(find.byType(CircularProgressIndicator), findsOneWidget);
        
        // Verify button is disabled during loading
        final ElevatedButton submitButton = tester.widget(find.byKey(const Key('submit-button')));
        expect(submitButton.onPressed, isNull);
      });
    });

    group('Form Edge Cases', () {
      testWidgets('should handle whitespace in text fields', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Enter whitespace-only name
        await tester.enterText(find.byKey(const Key('name-field')), '   ');
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        expect(find.text('Name is required'), findsOneWidget);

        // Enter name with surrounding whitespace (should be trimmed)
        Map<String, String>? submittedData;
        await tester.pumpWidget(
          UserRegistrationForm(onSubmit: (data) => submittedData = data),
        );

        await tester.enterText(find.byKey(const Key('name-field')), '  Test User  ');
        await tester.enterText(find.byKey(const Key('email-field')), 'test@example.com');
        await tester.enterText(find.byKey(const Key('password-field')), 'ValidPass123');
        await tester.enterText(find.byKey(const Key('confirm-password-field')), 'ValidPass123');
        
        await tester.tap(find.byKey(const Key('country-dropdown')));
        await tester.pump();
        await tester.tap(find.text('Canada').last);
        await tester.pump();
        
        await tester.tap(find.byKey(const Key('terms-checkbox')));
        await tester.pump();

        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump(const Duration(milliseconds: 600));

        expect(submittedData!['name'], equals('Test User')); // Trimmed
      });

      testWidgets('should handle special characters in fields', (WidgetTester tester) async {
        await tester.pumpWidget(const UserRegistrationForm());

        // Test special characters in name
        await tester.enterText(find.byKey(const Key('name-field')), "O'Connor-Smith");
        await tester.enterText(find.byKey(const Key('email-field')), 'test+tag@example-site.com');
        
        await tester.tap(find.byKey(const Key('submit-button')));
        await tester.pump();

        // Should not show validation errors for valid special characters
        expect(find.text('Name is required'), findsNothing);
        expect(find.text('Please enter a valid email'), findsNothing);
      });
    });
  });
}
```

This example demonstrates comprehensive form testing including field  
validation, input handling, dropdown and checkbox interactions, form  
submission workflows, loading states, and edge case handling.

## Testing Navigation Between Screens

Testing navigation flows, route transitions, passing data between screens,  
and handling navigation edge cases.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

// Screen widgets for navigation testing
class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text('Welcome to Home Screen'),
            const SizedBox(height: 20),
            ElevatedButton(
              key: const Key('navigate-to-profile'),
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const ProfileScreen(userId: 'user123'),
                  ),
                );
              },
              child: const Text('Go to Profile'),
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              key: const Key('navigate-to-settings'),
              onPressed: () {
                Navigator.pushNamed(context, '/settings');
              },
              child: const Text('Go to Settings'),
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              key: const Key('navigate-with-result'),
              onPressed: () async {
                final result = await Navigator.push<String>(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const SelectionScreen(),
                  ),
                );
                if (result != null && context.mounted) {
                  ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(
                      key: const Key('result-snackbar'),
                      content: Text('Selected: $result'),
                    ),
                  );
                }
              },
              child: const Text('Select Item'),
            ),
          ],
        ),
      ),
    );
  }
}

class ProfileScreen extends StatelessWidget {
  final String userId;

  const ProfileScreen({super.key, required this.userId});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Profile'),
        actions: [
          IconButton(
            key: const Key('edit-profile'),
            icon: const Icon(Icons.edit),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => EditProfileScreen(userId: userId),
                ),
              );
            },
          ),
        ],
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Profile Screen for user: $userId'),
            const SizedBox(height: 20),
            ElevatedButton(
              key: const Key('back-to-home'),
              onPressed: () {
                Navigator.pop(context);
              },
              child: const Text('Back to Home'),
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              key: const Key('navigate-to-nested'),
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const NestedScreen(),
                  ),
                );
              },
              child: const Text('Go to Nested Screen'),
            ),
          ],
        ),
      ),
    );
  }
}

class EditProfileScreen extends StatefulWidget {
  final String userId;

  const EditProfileScreen({super.key, required this.userId});

  @override
  State<EditProfileScreen> createState() => _EditProfileScreenState();
}

class _EditProfileScreenState extends State<EditProfileScreen> {
  final _nameController = TextEditingController(text: 'Default Name');
  bool _hasUnsavedChanges = false;

  @override
  void initState() {
    super.initState();
    _nameController.addListener(() {
      setState(() {
        _hasUnsavedChanges = true;
      });
    });
  }

  @override
  void dispose() {
    _nameController.dispose();
    super.dispose();
  }

  Future<bool> _onWillPop() async {
    if (!_hasUnsavedChanges) return true;

    return await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        key: const Key('unsaved-changes-dialog'),
        title: const Text('Unsaved Changes'),
        content: const Text('You have unsaved changes. Are you sure you want to leave?'),
        actions: [
          TextButton(
            key: const Key('stay-button'),
            onPressed: () => Navigator.pop(context, false),
            child: const Text('Stay'),
          ),
          TextButton(
            key: const Key('leave-button'),
            onPressed: () => Navigator.pop(context, true),
            child: const Text('Leave'),
          ),
        ],
      ),
    ) ?? false;
  }

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: _onWillPop,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('Edit Profile'),
          actions: [
            TextButton(
              key: const Key('save-button'),
              onPressed: () {
                setState(() {
                  _hasUnsavedChanges = false;
                });
                Navigator.pop(context, _nameController.text);
              },
              child: const Text('Save', style: TextStyle(color: Colors.white)),
            ),
          ],
        ),
        body: Padding(
          padding: const EdgeInsets.all(16),
          child: Column(
            children: [
              Text('Editing profile for: ${widget.userId}'),
              const SizedBox(height: 20),
              TextField(
                key: const Key('name-field'),
                controller: _nameController,
                decoration: const InputDecoration(
                  labelText: 'Name',
                  border: OutlineInputBorder(),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class SettingsScreen extends StatelessWidget {
  const SettingsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Settings'),
      ),
      body: const Center(
        child: Text('Settings Screen'),
      ),
    );
  }
}

class SelectionScreen extends StatelessWidget {
  const SelectionScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final items = ['Option A', 'Option B', 'Option C'];

    return Scaffold(
      appBar: AppBar(
        title: const Text('Select Item'),
      ),
      body: ListView.builder(
        itemCount: items.length,
        itemBuilder: (context, index) {
          return ListTile(
            key: Key('option-$index'),
            title: Text(items[index]),
            onTap: () {
              Navigator.pop(context, items[index]);
            },
          );
        },
      ),
    );
  }
}

class NestedScreen extends StatelessWidget {
  const NestedScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Nested Screen'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text('This is a nested screen'),
            const SizedBox(height: 20),
            ElevatedButton(
              key: const Key('pop-until-home'),
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/'));
              },
              child: const Text('Back to Home'),
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              key: const Key('replace-with-settings'),
              onPressed: () {
                Navigator.pushReplacementNamed(context, '/settings');
              },
              child: const Text('Replace with Settings'),
            ),
          ],
        ),
      ),
    );
  }
}

// Main app with routing
class NavigationTestApp extends StatelessWidget {
  const NavigationTestApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: '/',
      routes: {
        '/': (context) => const HomeScreen(),
        '/settings': (context) => const SettingsScreen(),
      },
    );
  }
}

void main() {
  group('Navigation Tests', () {
    testWidgets('should navigate to profile screen', (WidgetTester tester) async {
      await tester.pumpWidget(const NavigationTestApp());

      // Verify we're on home screen
      expect(find.text('Welcome to Home Screen'), findsOneWidget);

      // Navigate to profile
      await tester.tap(find.byKey(const Key('navigate-to-profile')));
      await tester.pumpAndSettle();

      // Verify we're on profile screen
      expect(find.text('Profile Screen for user: user123'), findsOneWidget);
      expect(find.text('Welcome to Home Screen'), findsNothing);
    });

    testWidgets('should navigate using named routes', (WidgetTester tester) async {
      await tester.pumpWidget(const NavigationTestApp());

      // Navigate to settings using named route
      await tester.tap(find.byKey(const Key('navigate-to-settings')));
      await tester.pumpAndSettle();

      // Verify we're on settings screen
      expect(find.text('Settings Screen'), findsOneWidget);
      expect(find.text('Welcome to Home Screen'), findsNothing);
    });

    testWidgets('should navigate back using pop', (WidgetTester tester) async {
      await tester.pumpWidget(const NavigationTestApp());

      // Navigate to profile
      await tester.tap(find.byKey(const Key('navigate-to-profile')));
      await tester.pumpAndSettle();

      // Navigate back
      await tester.tap(find.byKey(const Key('back-to-home')));
      await tester.pumpAndSettle();

      // Verify we're back on home screen
      expect(find.text('Welcome to Home Screen'), findsOneWidget);
      expect(find.text('Profile Screen'), findsNothing);
    });

    testWidgets('should use system back button', (WidgetTester tester) async {
      await tester.pumpWidget(const NavigationTestApp());

      // Navigate to profile
      await tester.tap(find.byKey(const Key('navigate-to-profile')));
      await tester.pumpAndSettle();

      // Use system back button
      await tester.pageBack();
      await tester.pumpAndSettle();

      // Verify we're back on home screen
      expect(find.text('Welcome to Home Screen'), findsOneWidget);
    });

    testWidgets('should handle navigation with result', (WidgetTester tester) async {
      await tester.pumpWidget(const NavigationTestApp());

      // Navigate to selection screen
      await tester.tap(find.byKey(const Key('navigate-with-result')));
      await tester.pumpAndSettle();

      // Select an option
      await tester.tap(find.byKey(const Key('option-1')));
      await tester.pumpAndSettle();

      // Verify result is shown in snackbar
      expect(find.byKey(const Key('result-snackbar')), findsOneWidget);
      expect(find.text('Selected: Option B'), findsOneWidget);
    });

    testWidgets('should navigate to nested screens', (WidgetTester tester) async {
      await tester.pumpWidget(const NavigationTestApp());

      // Navigate to profile
      await tester.tap(find.byKey(const Key('navigate-to-profile')));
      await tester.pumpAndSettle();

      // Navigate to nested screen
      await tester.tap(find.byKey(const Key('navigate-to-nested')));
      await tester.pumpAndSettle();

      // Verify we're on nested screen
      expect(find.text('This is a nested screen'), findsOneWidget);
    });

    testWidgets('should pop until specific route', (WidgetTester tester) async {
      await tester.pumpWidget(const NavigationTestApp());

      // Navigate to profile then nested screen
      await tester.tap(find.byKey(const Key('navigate-to-profile')));
      await tester.pumpAndSettle();

      await tester.tap(find.byKey(const Key('navigate-to-nested')));
      await tester.pumpAndSettle();

      // Pop until home
      await tester.tap(find.byKey(const Key('pop-until-home')));
      await tester.pumpAndSettle();

      // Verify we're back on home screen
      expect(find.text('Welcome to Home Screen'), findsOneWidget);
    });

    testWidgets('should replace current route', (WidgetTester tester) async {
      await tester.pumpWidget(const NavigationTestApp());

      // Navigate to profile then nested screen
      await tester.tap(find.byKey(const Key('navigate-to-profile')));
      await tester.pumpAndSettle();

      await tester.tap(find.byKey(const Key('navigate-to-nested')));
      await tester.pumpAndSettle();

      // Replace with settings
      await tester.tap(find.byKey(const Key('replace-with-settings')));
      await tester.pumpAndSettle();

      // Verify we're on settings screen
      expect(find.text('Settings Screen'), findsOneWidget);

      // Try to go back - should go to profile (not nested)
      await tester.pageBack();
      await tester.pumpAndSettle();

      expect(find.text('Profile Screen'), findsOneWidget);
    });

    group('Navigation with Data Passing', () {
      testWidgets('should pass data to next screen', (WidgetTester tester) async {
        await tester.pumpWidget(const NavigationTestApp());

        // Navigate to profile
        await tester.tap(find.byKey(const Key('navigate-to-profile')));
        await tester.pumpAndSettle();

        // Verify user ID was passed
        expect(find.text('Profile Screen for user: user123'), findsOneWidget);

        // Navigate to edit profile
        await tester.tap(find.byKey(const Key('edit-profile')));
        await tester.pumpAndSettle();

        // Verify user ID was passed to edit screen
        expect(find.text('Editing profile for: user123'), findsOneWidget);
      });

      testWidgets('should handle unsaved changes dialog', (WidgetTester tester) async {
        await tester.pumpWidget(const NavigationTestApp());

        // Navigate to profile then edit profile
        await tester.tap(find.byKey(const Key('navigate-to-profile')));
        await tester.pumpAndSettle();

        await tester.tap(find.byKey(const Key('edit-profile')));
        await tester.pumpAndSettle();

        // Make changes
        await tester.enterText(find.byKey(const Key('name-field')), 'Changed Name');

        // Try to navigate back
        await tester.pageBack();
        await tester.pumpAndSettle();

        // Verify unsaved changes dialog appears
        expect(find.byKey(const Key('unsaved-changes-dialog')), findsOneWidget);

        // Choose to stay
        await tester.tap(find.byKey(const Key('stay-button')));
        await tester.pumpAndSettle();

        // Verify we're still on edit screen
        expect(find.text('Edit Profile'), findsOneWidget);

        // Try to leave again
        await tester.pageBack();
        await tester.pumpAndSettle();

        // Choose to leave
        await tester.tap(find.byKey(const Key('leave-button')));
        await tester.pumpAndSettle();

        // Verify we're back on profile screen
        expect(find.text('Profile Screen'), findsOneWidget);
      });

      testWidgets('should not show unsaved changes dialog when saved', 
          (WidgetTester tester) async {
        await tester.pumpWidget(const NavigationTestApp());

        // Navigate to profile then edit profile
        await tester.tap(find.byKey(const Key('navigate-to-profile')));
        await tester.pumpAndSettle();

        await tester.tap(find.byKey(const Key('edit-profile')));
        await tester.pumpAndSettle();

        // Make changes and save
        await tester.enterText(find.byKey(const Key('name-field')), 'Saved Name');
        await tester.tap(find.byKey(const Key('save-button')));
        await tester.pumpAndSettle();

        // Verify we're back on profile screen without dialog
        expect(find.text('Profile Screen'), findsOneWidget);
        expect(find.byKey(const Key('unsaved-changes-dialog')), findsNothing);
      });
    });

    group('Navigation Edge Cases', () {
      testWidgets('should handle rapid navigation taps', (WidgetTester tester) async {
        await tester.pumpWidget(const NavigationTestApp());

        // Rapidly tap navigation button
        for (int i = 0; i < 3; i++) {
          await tester.tap(find.byKey(const Key('navigate-to-profile')));
        }
        await tester.pumpAndSettle();

        // Should only navigate once
        expect(find.text('Profile Screen'), findsOneWidget);

        // Should be able to go back once
        await tester.pageBack();
        await tester.pumpAndSettle();

        expect(find.text('Welcome to Home Screen'), findsOneWidget);
      });

      testWidgets('should handle navigation with no result', (WidgetTester tester) async {
        await tester.pumpWidget(const NavigationTestApp());

        // Navigate to selection screen
        await tester.tap(find.byKey(const Key('navigate-with-result')));
        await tester.pumpAndSettle();

        // Go back without selecting
        await tester.pageBack();
        await tester.pumpAndSettle();

        // No snackbar should appear
        expect(find.byKey(const Key('result-snackbar')), findsNothing);
      });
    });
  });
}
```

This example demonstrates comprehensive navigation testing including basic  
navigation, named routes, data passing, navigation with results, nested  
navigation, unsaved changes handling, and edge cases like rapid tapping.

## Testing Stateful Widgets

Testing stateful widgets with complex state management, lifecycle methods,  
and state-dependent rendering.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'dart:async';

// Stateful widget to test
class TimerWidget extends StatefulWidget {
  final int initialSeconds;
  final VoidCallback? onTimerComplete;

  const TimerWidget({
    super.key,
    this.initialSeconds = 60,
    this.onTimerComplete,
  });

  @override
  State<TimerWidget> createState() => _TimerWidgetState();
}

class _TimerWidgetState extends State<TimerWidget> with TickerProviderStateMixin {
  late int _remainingSeconds;
  Timer? _timer;
  bool _isRunning = false;
  bool _isPaused = false;
  late AnimationController _pulseController;
  late Animation<double> _pulseAnimation;

  @override
  void initState() {
    super.initState();
    _remainingSeconds = widget.initialSeconds;
    
    _pulseController = AnimationController(
      duration: const Duration(seconds: 1),
      vsync: this,
    );
    _pulseAnimation = Tween<double>(begin: 1.0, end: 1.2).animate(
      CurvedAnimation(parent: _pulseController, curve: Curves.easeInOut),
    );
  }

  @override
  void dispose() {
    _timer?.cancel();
    _pulseController.dispose();
    super.dispose();
  }

  void _startTimer() {
    if (_remainingSeconds <= 0) return;

    setState(() {
      _isRunning = true;
      _isPaused = false;
    });

    _pulseController.repeat(reverse: true);

    _timer = Timer.periodic(const Duration(seconds: 1), (timer) {
      setState(() {
        _remainingSeconds--;
      });

      if (_remainingSeconds <= 0) {
        _stopTimer();
        widget.onTimerComplete?.call();
      }
    });
  }

  void _pauseTimer() {
    _timer?.cancel();
    _pulseController.stop();
    
    setState(() {
      _isRunning = false;
      _isPaused = true;
    });
  }

  void _stopTimer() {
    _timer?.cancel();
    _pulseController.stop();
    
    setState(() {
      _isRunning = false;
      _isPaused = false;
    });
  }

  void _resetTimer() {
    _stopTimer();
    setState(() {
      _remainingSeconds = widget.initialSeconds;
    });
  }

  String _formatTime(int seconds) {
    final minutes = seconds ~/ 60;
    final remainingSeconds = seconds % 60;
    return '${minutes.toString().padLeft(2, '0')}:${remainingSeconds.toString().padLeft(2, '0')}';
  }

  Color _getTimerColor() {
    if (_remainingSeconds <= 10) return Colors.red;
    if (_remainingSeconds <= 30) return Colors.orange;
    return Colors.blue;
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Timer Widget'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              AnimatedBuilder(
                animation: _pulseAnimation,
                builder: (context, child) {
                  return Transform.scale(
                    scale: _isRunning ? _pulseAnimation.value : 1.0,
                    child: Container(
                      key: const Key('timer-display'),
                      width: 200,
                      height: 200,
                      decoration: BoxDecoration(
                        shape: BoxShape.circle,
                        color: _getTimerColor().withOpacity(0.2),
                        border: Border.all(
                          color: _getTimerColor(),
                          width: 4,
                        ),
                      ),
                      child: Center(
                        child: Text(
                          _formatTime(_remainingSeconds),
                          style: TextStyle(
                            fontSize: 32,
                            fontWeight: FontWeight.bold,
                            color: _getTimerColor(),
                          ),
                        ),
                      ),
                    ),
                  );
                },
              ),
              const SizedBox(height: 40),
              Row(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  ElevatedButton(
                    key: const Key('start-button'),
                    onPressed: _remainingSeconds > 0 && !_isRunning ? _startTimer : null,
                    child: const Text('Start'),
                  ),
                  const SizedBox(width: 12),
                  ElevatedButton(
                    key: const Key('pause-button'),
                    onPressed: _isRunning ? _pauseTimer : null,
                    child: const Text('Pause'),
                  ),
                  const SizedBox(width: 12),
                  ElevatedButton(
                    key: const Key('reset-button'),
                    onPressed: _resetTimer,
                    child: const Text('Reset'),
                  ),
                ],
              ),
              const SizedBox(height: 20),
              Text(
                _isRunning 
                    ? 'Timer Running' 
                    : _isPaused 
                        ? 'Timer Paused' 
                        : _remainingSeconds <= 0 
                            ? 'Timer Completed' 
                            : 'Timer Ready',
                key: const Key('status-text'),
                style: const TextStyle(fontSize: 16),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// Shopping cart stateful widget
class ShoppingCartWidget extends StatefulWidget {
  const ShoppingCartWidget({super.key});

  @override
  State<ShoppingCartWidget> createState() => _ShoppingCartWidgetState();
}

class CartItem {
  final String id;
  final String name;
  final double price;
  int quantity;

  CartItem({
    required this.id,
    required this.name,
    required this.price,
    this.quantity = 1,
  });

  double get total => price * quantity;
}

class _ShoppingCartWidgetState extends State<ShoppingCartWidget> {
  final List<CartItem> _items = [];
  bool _isLoading = false;

  void _addItem(String id, String name, double price) {
    setState(() {
      final existingIndex = _items.indexWhere((item) => item.id == id);
      if (existingIndex >= 0) {
        _items[existingIndex].quantity++;
      } else {
        _items.add(CartItem(id: id, name: name, price: price));
      }
    });
  }

  void _removeItem(String id) {
    setState(() {
      _items.removeWhere((item) => item.id == id);
    });
  }

  void _updateQuantity(String id, int quantity) {
    setState(() {
      final index = _items.indexWhere((item) => item.id == id);
      if (index >= 0) {
        if (quantity <= 0) {
          _items.removeAt(index);
        } else {
          _items[index].quantity = quantity;
        }
      }
    });
  }

  double get _totalPrice {
    return _items.fold(0.0, (sum, item) => sum + item.total);
  }

  int get _totalItems {
    return _items.fold(0, (sum, item) => sum + item.quantity);
  }

  Future<void> _checkout() async {
    if (_items.isEmpty) return;

    setState(() {
      _isLoading = true;
    });

    // Simulate checkout process
    await Future.delayed(const Duration(seconds: 2));

    setState(() {
      _items.clear();
      _isLoading = false;
    });

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          key: Key('checkout-success'),
          content: Text('Checkout successful!'),
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Shopping Cart (${_totalItems} items)'),
        ),
        body: Column(
          children: [
            // Sample products to add
            Container(
              padding: const EdgeInsets.all(16),
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  ElevatedButton(
                    key: const Key('add-apple'),
                    onPressed: () => _addItem('apple', 'Apple', 1.99),
                    child: const Text('Add Apple \$1.99'),
                  ),
                  ElevatedButton(
                    key: const Key('add-banana'),
                    onPressed: () => _addItem('banana', 'Banana', 0.99),
                    child: const Text('Add Banana \$0.99'),
                  ),
                ],
              ),
            ),
            const Divider(),
            // Cart items list
            Expanded(
              child: _items.isEmpty
                  ? const Center(
                      child: Text(
                        'Your cart is empty',
                        key: Key('empty-cart'),
                        style: TextStyle(fontSize: 18),
                      ),
                    )
                  : ListView.builder(
                      key: const Key('cart-list'),
                      itemCount: _items.length,
                      itemBuilder: (context, index) {
                        final item = _items[index];
                        return ListTile(
                          key: Key('item-${item.id}'),
                          title: Text(item.name),
                          subtitle: Text('\$${item.price.toStringAsFixed(2)} each'),
                          trailing: SizedBox(
                            width: 120,
                            child: Row(
                              mainAxisSize: MainAxisSize.min,
                              children: [
                                IconButton(
                                  key: Key('decrease-${item.id}'),
                                  icon: const Icon(Icons.remove),
                                  onPressed: () => _updateQuantity(item.id, item.quantity - 1),
                                ),
                                Text('${item.quantity}'),
                                IconButton(
                                  key: Key('increase-${item.id}'),
                                  icon: const Icon(Icons.add),
                                  onPressed: () => _updateQuantity(item.id, item.quantity + 1),
                                ),
                                IconButton(
                                  key: Key('remove-${item.id}'),
                                  icon: const Icon(Icons.delete, color: Colors.red),
                                  onPressed: () => _removeItem(item.id),
                                ),
                              ],
                            ),
                          ),
                        );
                      },
                    ),
            ),
            // Cart summary
            if (_items.isNotEmpty)
              Container(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    Row(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: [
                        const Text('Total:', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                        Text(
                          '\$${_totalPrice.toStringAsFixed(2)}',
                          key: const Key('total-price'),
                          style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                        ),
                      ],
                    ),
                    const SizedBox(height: 16),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        key: const Key('checkout-button'),
                        onPressed: _isLoading ? null : _checkout,
                        child: _isLoading
                            ? const CircularProgressIndicator()
                            : const Text('Checkout'),
                      ),
                    ),
                  ],
                ),
              ),
          ],
        ),
      ),
    );
  }
}

void main() {
  group('Stateful Widget Tests', () {
    group('TimerWidget Tests', () {
      testWidgets('should display initial timer value', (WidgetTester tester) async {
        await tester.pumpWidget(const TimerWidget(initialSeconds: 120));

        expect(find.text('02:00'), findsOneWidget);
        expect(find.text('Timer Ready'), findsOneWidget);
      });

      testWidgets('should start timer when start button is pressed', (WidgetTester tester) async {
        await tester.pumpWidget(const TimerWidget(initialSeconds: 5));

        // Start the timer
        await tester.tap(find.byKey(const Key('start-button')));
        await tester.pump();

        // Verify timer is running
        expect(find.text('Timer Running'), findsOneWidget);
        
        // Wait for timer to tick
        await tester.pump(const Duration(seconds: 1));
        expect(find.text('00:04'), findsOneWidget);
      });

      testWidgets('should pause and resume timer', (WidgetTester tester) async {
        await tester.pumpWidget(const TimerWidget(initialSeconds: 10));

        // Start timer
        await tester.tap(find.byKey(const Key('start-button')));
        await tester.pump();
        await tester.pump(const Duration(seconds: 1));

        // Pause timer
        await tester.tap(find.byKey(const Key('pause-button')));
        await tester.pump();

        expect(find.text('Timer Paused'), findsOneWidget);

        // Resume timer
        await tester.tap(find.byKey(const Key('start-button')));
        await tester.pump();

        expect(find.text('Timer Running'), findsOneWidget);
      });

      testWidgets('should reset timer', (WidgetTester tester) async {
        await tester.pumpWidget(const TimerWidget(initialSeconds: 10));

        // Start and run timer for a bit
        await tester.tap(find.byKey(const Key('start-button')));
        await tester.pump();
        await tester.pump(const Duration(seconds: 3));

        // Reset timer
        await tester.tap(find.byKey(const Key('reset-button')));
        await tester.pump();

        expect(find.text('00:10'), findsOneWidget);
        expect(find.text('Timer Ready'), findsOneWidget);
      });

      testWidgets('should call completion callback when timer reaches zero', (WidgetTester tester) async {
        bool callbackCalled = false;

        await tester.pumpWidget(
          TimerWidget(
            initialSeconds: 1,
            onTimerComplete: () => callbackCalled = true,
          ),
        );

        // Start timer
        await tester.tap(find.byKey(const Key('start-button')));
        await tester.pump();

        // Wait for timer to complete
        await tester.pump(const Duration(seconds: 1));

        expect(find.text('00:00'), findsOneWidget);
        expect(find.text('Timer Completed'), findsOneWidget);
        expect(callbackCalled, isTrue);
      });

      testWidgets('should change color based on remaining time', (WidgetTester tester) async {
        await tester.pumpWidget(const TimerWidget(initialSeconds: 5));

        // Start timer
        await tester.tap(find.byKey(const Key('start-button')));
        await tester.pump();

        // Initially should be orange (5 seconds remaining)
        Container timerContainer = tester.widget(find.byKey(const Key('timer-display')));
        BoxDecoration decoration = timerContainer.decoration as BoxDecoration;
        expect(decoration.border?.top.color, equals(Colors.orange));

        // Wait until final seconds (should be red)
        await tester.pump(const Duration(seconds: 5));
        
        timerContainer = tester.widget(find.byKey(const Key('timer-display')));
        decoration = timerContainer.decoration as BoxDecoration;
        expect(decoration.border?.top.color, equals(Colors.red));
      });

      testWidgets('should disable start button when timer is running', (WidgetTester tester) async {
        await tester.pumpWidget(const TimerWidget(initialSeconds: 10));

        // Initially start button should be enabled
        ElevatedButton startButton = tester.widget(find.byKey(const Key('start-button')));
        expect(startButton.onPressed, isNotNull);

        // Start timer
        await tester.tap(find.byKey(const Key('start-button')));
        await tester.pump();

        // Start button should now be disabled
        startButton = tester.widget(find.byKey(const Key('start-button')));
        expect(startButton.onPressed, isNull);
      });
    });

    group('ShoppingCartWidget Tests', () {
      testWidgets('should show empty cart initially', (WidgetTester tester) async {
        await tester.pumpWidget(const ShoppingCartWidget());

        expect(find.byKey(const Key('empty-cart')), findsOneWidget);
        expect(find.text('Shopping Cart (0 items)'), findsOneWidget);
      });

      testWidgets('should add items to cart', (WidgetTester tester) async {
        await tester.pumpWidget(const ShoppingCartWidget());

        // Add apple
        await tester.tap(find.byKey(const Key('add-apple')));
        await tester.pump();

        // Verify item was added
        expect(find.text('Apple'), findsOneWidget);
        expect(find.text('Shopping Cart (1 items)'), findsOneWidget);
        expect(find.byKey(const Key('empty-cart')), findsNothing);

        // Add banana
        await tester.tap(find.byKey(const Key('add-banana')));
        await tester.pump();

        expect(find.text('Banana'), findsOneWidget);
        expect(find.text('Shopping Cart (2 items)'), findsOneWidget);
      });

      testWidgets('should increase quantity when adding same item', (WidgetTester tester) async {
        await tester.pumpWidget(const ShoppingCartWidget());

        // Add apple twice
        await tester.tap(find.byKey(const Key('add-apple')));
        await tester.pump();
        await tester.tap(find.byKey(const Key('add-apple')));
        await tester.pump();

        // Should show quantity 2
        expect(find.text('2'), findsOneWidget);
        expect(find.text('Shopping Cart (2 items)'), findsOneWidget);
      });

      testWidgets('should update item quantity', (WidgetTester tester) async {
        await tester.pumpWidget(const ShoppingCartWidget());

        // Add apple
        await tester.tap(find.byKey(const Key('add-apple')));
        await tester.pump();

        // Increase quantity
        await tester.tap(find.byKey(const Key('increase-apple')));
        await tester.pump();

        expect(find.text('2'), findsOneWidget);
        expect(find.text('Shopping Cart (2 items)'), findsOneWidget);

        // Decrease quantity
        await tester.tap(find.byKey(const Key('decrease-apple')));
        await tester.pump();

        expect(find.text('1'), findsOneWidget);
        expect(find.text('Shopping Cart (1 items)'), findsOneWidget);
      });

      testWidgets('should remove item when quantity reaches zero', (WidgetTester tester) async {
        await tester.pumpWidget(const ShoppingCartWidget());

        // Add apple
        await tester.tap(find.byKey(const Key('add-apple')));
        await tester.pump();

        // Decrease quantity to zero
        await tester.tap(find.byKey(const Key('decrease-apple')));
        await tester.pump();

        // Item should be removed
        expect(find.text('Apple'), findsNothing);
        expect(find.byKey(const Key('empty-cart')), findsOneWidget);
        expect(find.text('Shopping Cart (0 items)'), findsOneWidget);
      });

      testWidgets('should remove item with delete button', (WidgetTester tester) async {
        await tester.pumpWidget(const ShoppingCartWidget());

        // Add apple
        await tester.tap(find.byKey(const Key('add-apple')));
        await tester.pump();

        // Remove with delete button
        await tester.tap(find.byKey(const Key('remove-apple')));
        await tester.pump();

        expect(find.text('Apple'), findsNothing);
        expect(find.byKey(const Key('empty-cart')), findsOneWidget);
      });

      testWidgets('should calculate total price correctly', (WidgetTester tester) async {
        await tester.pumpWidget(const ShoppingCartWidget());

        // Add apple ($1.99) and banana ($0.99)
        await tester.tap(find.byKey(const Key('add-apple')));
        await tester.pump();
        await tester.tap(find.byKey(const Key('add-banana')));
        await tester.pump();

        // Increase apple quantity to 2
        await tester.tap(find.byKey(const Key('increase-apple')));
        await tester.pump();

        // Total should be $4.97 (2 * $1.99 + 1 * $0.99)
        expect(find.text('\$4.97'), findsOneWidget);
      });

      testWidgets('should handle checkout process', (WidgetTester tester) async {
        await tester.pumpWidget(const ShoppingCartWidget());

        // Add items
        await tester.tap(find.byKey(const Key('add-apple')));
        await tester.pump();

        // Start checkout
        await tester.tap(find.byKey(const Key('checkout-button')));
        await tester.pump();

        // Should show loading indicator
        expect(find.byType(CircularProgressIndicator), findsOneWidget);

        // Wait for checkout to complete
        await tester.pump(const Duration(seconds: 2));

        // Cart should be empty and success message shown
        expect(find.byKey(const Key('empty-cart')), findsOneWidget);
        expect(find.byKey(const Key('checkout-success')), findsOneWidget);
      });

      testWidgets('should maintain state during widget rebuilds', (WidgetTester tester) async {
        await tester.pumpWidget(const ShoppingCartWidget());

        // Add items
        await tester.tap(find.byKey(const Key('add-apple')));
        await tester.pump();
        await tester.tap(find.byKey(const Key('add-banana')));
        await tester.pump();

        // Force widget rebuild (simulate parent widget rebuild)
        await tester.pumpWidget(const ShoppingCartWidget());

        // State should be maintained
        expect(find.text('Apple'), findsOneWidget);
        expect(find.text('Banana'), findsOneWidget);
        expect(find.text('Shopping Cart (2 items)'), findsOneWidget);
      });
    });

    group('State Lifecycle Tests', () {
      testWidgets('should properly dispose resources', (WidgetTester tester) async {
        // This test verifies that dispose is called properly
        await tester.pumpWidget(const TimerWidget(initialSeconds: 10));

        // Start timer to create resources
        await tester.tap(find.byKey(const Key('start-button')));
        await tester.pump();

        // Remove widget from tree
        await tester.pumpWidget(const SizedBox());

        // No exceptions should be thrown during disposal
        // This test mainly ensures dispose methods are called correctly
      });

      testWidgets('should handle state changes after disposal', (WidgetTester tester) async {
        bool callbackCalled = false;
        
        await tester.pumpWidget(
          TimerWidget(
            initialSeconds: 1,
            onTimerComplete: () => callbackCalled = true,
          ),
        );

        // Start timer
        await tester.tap(find.byKey(const Key('start-button')));
        await tester.pump();

        // Remove widget before timer completes
        await tester.pumpWidget(const SizedBox());

        // Wait for where timer would have completed
        await tester.pump(const Duration(seconds: 2));

        // Callback should not be called after disposal
        expect(callbackCalled, isFalse);
      });
    });
  });
}
```

This example demonstrates comprehensive stateful widget testing including  
state management, lifecycle methods, timers, animations, complex interactions,  
resource disposal, and maintaining state consistency during rebuilds.

## Testing Custom Widgets

Testing custom widgets with complex logic, custom painters, and  
specialized functionality.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'dart:math' as math;

// Custom progress indicator widget
class CircularProgressIndicator extends StatefulWidget {
  final double progress;
  final Color color;
  final double strokeWidth;
  final String? label;
  final Duration animationDuration;

  const CircularProgressIndicator({
    super.key,
    required this.progress,
    this.color = Colors.blue,
    this.strokeWidth = 8.0,
    this.label,
    this.animationDuration = const Duration(milliseconds: 300),
  });

  @override
  State<CircularProgressIndicator> createState() => _CircularProgressIndicatorState();
}

class _CircularProgressIndicatorState extends State<CircularProgressIndicator>
    with SingleTickerProviderStateMixin {
  late AnimationController _animationController;
  late Animation<double> _animation;
  double _currentProgress = 0.0;

  @override
  void initState() {
    super.initState();
    _animationController = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );
    _animation = Tween<double>(
      begin: 0.0,
      end: widget.progress.clamp(0.0, 1.0),
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: Curves.easeInOut,
    ));
    
    _animation.addListener(() {
      setState(() {
        _currentProgress = _animation.value;
      });
    });
    
    _animationController.forward();
  }

  @override
  void didUpdateWidget(CircularProgressIndicator oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (oldWidget.progress != widget.progress) {
      _animation = Tween<double>(
        begin: _currentProgress,
        end: widget.progress.clamp(0.0, 1.0),
      ).animate(CurvedAnimation(
        parent: _animationController,
        curve: Curves.easeInOut,
      ));
      _animationController.reset();
      _animationController.forward();
    }
  }

  @override
  void dispose() {
    _animationController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return CustomPaint(
      painter: _CircularProgressPainter(
        progress: _currentProgress,
        color: widget.color,
        strokeWidth: widget.strokeWidth,
      ),
      child: Container(
        width: 100,
        height: 100,
        alignment: Alignment.center,
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              '${(_currentProgress * 100).toInt()}%',
              style: const TextStyle(
                fontSize: 16,
                fontWeight: FontWeight.bold,
              ),
            ),
            if (widget.label != null)
              Text(
                widget.label!,
                style: const TextStyle(fontSize: 12),
              ),
          ],
        ),
      ),
    );
  }
}

class _CircularProgressPainter extends CustomPainter {
  final double progress;
  final Color color;
  final double strokeWidth;

  _CircularProgressPainter({
    required this.progress,
    required this.color,
    required this.strokeWidth,
  });

  @override
  void paint(Canvas canvas, Size size) {
    final center = Offset(size.width / 2, size.height / 2);
    final radius = (size.width - strokeWidth) / 2;

    // Draw background circle
    final backgroundPaint = Paint()
      ..color = color.withOpacity(0.2)
      ..strokeWidth = strokeWidth
      ..style = PaintingStyle.stroke;

    canvas.drawCircle(center, radius, backgroundPaint);

    // Draw progress arc
    final progressPaint = Paint()
      ..color = color
      ..strokeWidth = strokeWidth
      ..style = PaintingStyle.stroke
      ..strokeCap = StrokeCap.round;

    final startAngle = -math.pi / 2; // Start from top
    final sweepAngle = 2 * math.pi * progress;

    canvas.drawArc(
      Rect.fromCircle(center: center, radius: radius),
      startAngle,
      sweepAngle,
      false,
      progressPaint,
    );
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) {
    return oldDelegate is _CircularProgressPainter &&
        (oldDelegate.progress != progress ||
         oldDelegate.color != color ||
         oldDelegate.strokeWidth != strokeWidth);
  }
}

// Custom rating widget
class StarRating extends StatefulWidget {
  final int maxRating;
  final double currentRating;
  final Function(double rating)? onRatingChanged;
  final double starSize;
  final Color activeColor;
  final Color inactiveColor;
  final bool allowHalfRating;

  const StarRating({
    super.key,
    this.maxRating = 5,
    this.currentRating = 0.0,
    this.onRatingChanged,
    this.starSize = 30.0,
    this.activeColor = Colors.amber,
    this.inactiveColor = Colors.grey,
    this.allowHalfRating = true,
  });

  @override
  State<StarRating> createState() => _StarRatingState();
}

class _StarRatingState extends State<StarRating> {
  late double _currentRating;

  @override
  void initState() {
    super.initState();
    _currentRating = widget.currentRating;
  }

  @override
  void didUpdateWidget(StarRating oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (oldWidget.currentRating != widget.currentRating) {
      _currentRating = widget.currentRating;
    }
  }

  void _handleTap(int starIndex, Offset localPosition, double starWidth) {
    if (widget.onRatingChanged == null) return;

    double rating;
    if (widget.allowHalfRating) {
      final isFirstHalf = localPosition.dx < starWidth / 2;
      rating = starIndex + (isFirstHalf ? 0.5 : 1.0);
    } else {
      rating = starIndex + 1.0;
    }

    setState(() {
      _currentRating = rating;
    });
    widget.onRatingChanged!(rating);
  }

  Widget _buildStar(int index) {
    final starValue = index + 1;
    final fullStars = _currentRating.floor();
    final hasHalfStar = _currentRating - fullStars >= 0.5;

    IconData iconData;
    Color color;

    if (starValue <= fullStars) {
      iconData = Icons.star;
      color = widget.activeColor;
    } else if (starValue == fullStars + 1 && hasHalfStar) {
      iconData = Icons.star_half;
      color = widget.activeColor;
    } else {
      iconData = Icons.star_border;
      color = widget.inactiveColor;
    }

    return GestureDetector(
      key: Key('star-$index'),
      onTapDown: (details) {
        final box = context.findRenderObject() as RenderBox;
        final localPosition = box.globalToLocal(details.globalPosition);
        final starWidth = widget.starSize;
        final adjustedPosition = Offset(
          localPosition.dx - (index * starWidth),
          localPosition.dy,
        );
        _handleTap(index, adjustedPosition, starWidth);
      },
      child: Icon(
        iconData,
        size: widget.starSize,
        color: color,
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisSize: MainAxisSize.min,
      children: List.generate(
        widget.maxRating,
        (index) => _buildStar(index),
      ),
    );
  }
}

// Custom expandable widget
class ExpandableCard extends StatefulWidget {
  final String title;
  final Widget content;
  final bool initiallyExpanded;
  final Duration animationDuration;
  final Function(bool isExpanded)? onExpansionChanged;

  const ExpandableCard({
    super.key,
    required this.title,
    required this.content,
    this.initiallyExpanded = false,
    this.animationDuration = const Duration(milliseconds: 300),
    this.onExpansionChanged,
  });

  @override
  State<ExpandableCard> createState() => _ExpandableCardState();
}

class _ExpandableCardState extends State<ExpandableCard>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _expansionAnimation;
  late Animation<double> _iconAnimation;
  late bool _isExpanded;

  @override
  void initState() {
    super.initState();
    _isExpanded = widget.initiallyExpanded;
    _controller = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );
    _expansionAnimation = Tween<double>(begin: 0.0, end: 1.0).animate(
      CurvedAnimation(parent: _controller, curve: Curves.easeInOut),
    );
    _iconAnimation = Tween<double>(begin: 0.0, end: 0.5).animate(
      CurvedAnimation(parent: _controller, curve: Curves.easeInOut),
    );

    if (_isExpanded) {
      _controller.value = 1.0;
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  void _toggleExpansion() {
    setState(() {
      _isExpanded = !_isExpanded;
    });

    if (_isExpanded) {
      _controller.forward();
    } else {
      _controller.reverse();
    }

    widget.onExpansionChanged?.call(_isExpanded);
  }

  @override
  Widget build(BuildContext context) {
    return Card(
      key: const Key('expandable-card'),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          InkWell(
            key: const Key('card-header'),
            onTap: _toggleExpansion,
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Row(
                children: [
                  Expanded(
                    child: Text(
                      widget.title,
                      style: const TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                      ),
                    ),
                  ),
                  AnimatedBuilder(
                    animation: _iconAnimation,
                    builder: (context, child) {
                      return Transform.rotate(
                        angle: _iconAnimation.value * 2 * math.pi,
                        child: const Icon(Icons.expand_more),
                      );
                    },
                  ),
                ],
              ),
            ),
          ),
          AnimatedBuilder(
            animation: _expansionAnimation,
            builder: (context, child) {
              return ClipRect(
                child: Align(
                  alignment: Alignment.topCenter,
                  heightFactor: _expansionAnimation.value,
                  child: Padding(
                    padding: const EdgeInsets.only(
                      left: 16.0,
                      right: 16.0,
                      bottom: 16.0,
                    ),
                    child: widget.content,
                  ),
                ),
              );
            },
          ),
        ],
      ),
    );
  }
}

void main() {
  group('Custom Widget Tests', () {
    group('CircularProgressIndicator Tests', () {
      testWidgets('should display progress percentage', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: CircularProgressIndicator(
                progress: 0.75,
              ),
            ),
          ),
        );

        // Should show 75% after animation completes
        await tester.pumpAndSettle();
        expect(find.text('75%'), findsOneWidget);
      });

      testWidgets('should display custom label', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: CircularProgressIndicator(
                progress: 0.5,
                label: 'Loading...',
              ),
            ),
          ),
        );

        await tester.pumpAndSettle();
        expect(find.text('Loading...'), findsOneWidget);
      });

      testWidgets('should animate progress changes', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: CircularProgressIndicator(
                progress: 0.0,
                animationDuration: Duration(milliseconds: 100),
              ),
            ),
          ),
        );

        // Initial state
        expect(find.text('0%'), findsOneWidget);

        // Update progress
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: CircularProgressIndicator(
                progress: 1.0,
                animationDuration: Duration(milliseconds: 100),
              ),
            ),
          ),
        );

        // Should animate to new value
        await tester.pump(const Duration(milliseconds: 50));
        // During animation, should be somewhere between 0% and 100%
        
        await tester.pumpAndSettle();
        expect(find.text('100%'), findsOneWidget);
      });

      testWidgets('should clamp progress values', (WidgetTester tester) async {
        // Test progress > 1.0
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: CircularProgressIndicator(
                progress: 1.5,
              ),
            ),
          ),
        );

        await tester.pumpAndSettle();
        expect(find.text('100%'), findsOneWidget);

        // Test progress < 0.0
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: CircularProgressIndicator(
                progress: -0.5,
              ),
            ),
          ),
        );

        await tester.pumpAndSettle();
        expect(find.text('0%'), findsOneWidget);
      });

      testWidgets('should use custom colors and stroke width', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: CircularProgressIndicator(
                progress: 0.5,
                color: Colors.red,
                strokeWidth: 12.0,
              ),
            ),
          ),
        );

        // The CustomPaint widget should be present
        expect(find.byType(CustomPaint), findsOneWidget);
      });
    });

    group('StarRating Tests', () {
      testWidgets('should display correct number of stars', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: StarRating(
                maxRating: 5,
                currentRating: 3.0,
              ),
            ),
          ),
        );

        // Should have 5 star widgets
        for (int i = 0; i < 5; i++) {
          expect(find.byKey(Key('star-$i')), findsOneWidget);
        }
      });

      testWidgets('should display full stars correctly', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: StarRating(
                maxRating: 5,
                currentRating: 3.0,
              ),
            ),
          ),
        );

        // First 3 stars should be filled
        expect(find.byIcon(Icons.star), findsNWidgets(3));
        expect(find.byIcon(Icons.star_border), findsNWidgets(2));
      });

      testWidgets('should display half stars when enabled', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: StarRating(
                maxRating: 5,
                currentRating: 3.5,
                allowHalfRating: true,
              ),
            ),
          ),
        );

        expect(find.byIcon(Icons.star), findsNWidgets(3));
        expect(find.byIcon(Icons.star_half), findsOneWidget);
        expect(find.byIcon(Icons.star_border), findsOneWidget);
      });

      testWidgets('should handle star taps when interactive', (WidgetTester tester) async {
        double? tappedRating;

        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: StarRating(
                maxRating: 5,
                currentRating: 2.0,
                onRatingChanged: (rating) => tappedRating = rating,
              ),
            ),
          ),
        );

        // Tap on the 4th star
        await tester.tap(find.byKey(const Key('star-3')));
        await tester.pump();

        expect(tappedRating, equals(4.0));
        expect(find.byIcon(Icons.star), findsNWidgets(4));
      });

      testWidgets('should not respond to taps when read-only', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: StarRating(
                maxRating: 5,
                currentRating: 2.0,
                // No onRatingChanged callback
              ),
            ),
          ),
        );

        // Tap on a star
        await tester.tap(find.byKey(const Key('star-3')));
        await tester.pump();

        // Rating should remain unchanged
        expect(find.byIcon(Icons.star), findsNWidgets(2));
      });

      testWidgets('should update when currentRating changes', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: StarRating(
                maxRating: 5,
                currentRating: 2.0,
              ),
            ),
          ),
        );

        expect(find.byIcon(Icons.star), findsNWidgets(2));

        // Update rating
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: StarRating(
                maxRating: 5,
                currentRating: 4.0,
              ),
            ),
          ),
        );

        expect(find.byIcon(Icons.star), findsNWidgets(4));
      });

      testWidgets('should use custom colors', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: StarRating(
                maxRating: 5,
                currentRating: 3.0,
                activeColor: Colors.red,
                inactiveColor: Colors.blue,
              ),
            ),
          ),
        );

        // Get the first star (should be active/red)
        final activeIcon = tester.widget<Icon>(find.byIcon(Icons.star).first);
        expect(activeIcon.color, equals(Colors.red));

        // Get the last star (should be inactive/blue)  
        final inactiveIcon = tester.widget<Icon>(find.byIcon(Icons.star_border).first);
        expect(inactiveIcon.color, equals(Colors.blue));
      });
    });

    group('ExpandableCard Tests', () {
      testWidgets('should display title and initially collapsed content', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: ExpandableCard(
                title: 'Test Title',
                content: const Text('Test Content'),
                initiallyExpanded: false,
              ),
            ),
          ),
        );

        expect(find.text('Test Title'), findsOneWidget);
        // Content should be present but not visible (height factor = 0)
        expect(find.text('Test Content'), findsOneWidget);
      });

      testWidgets('should expand when header is tapped', (WidgetTester tester) async {
        bool? expansionChanged;

        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: ExpandableCard(
                title: 'Test Title',
                content: const Text('Test Content'),
                onExpansionChanged: (expanded) => expansionChanged = expanded,
              ),
            ),
          ),
        );

        // Tap header to expand
        await tester.tap(find.byKey(const Key('card-header')));
        await tester.pumpAndSettle();

        expect(expansionChanged, isTrue);
      });

      testWidgets('should start expanded when initiallyExpanded is true', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: ExpandableCard(
                title: 'Test Title',
                content: const Text('Test Content'),
                initiallyExpanded: true,
              ),
            ),
          ),
        );

        // Content should be fully visible initially
        expect(find.text('Test Content'), findsOneWidget);
      });

      testWidgets('should animate expansion and collapse', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: ExpandableCard(
                title: 'Test Title',
                content: const Text('Test Content'),
                animationDuration: Duration(milliseconds: 200),
              ),
            ),
          ),
        );

        // Tap to expand
        await tester.tap(find.byKey(const Key('card-header')));
        await tester.pump();

        // During animation
        await tester.pump(const Duration(milliseconds: 100));
        
        // Complete animation
        await tester.pumpAndSettle();

        // Now tap to collapse
        await tester.tap(find.byKey(const Key('card-header')));
        await tester.pump();
        await tester.pump(const Duration(milliseconds: 100));
        await tester.pumpAndSettle();
      });

      testWidgets('should rotate expand icon during animation', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: ExpandableCard(
                title: 'Test Title',
                content: const Text('Test Content'),
              ),
            ),
          ),
        );

        expect(find.byIcon(Icons.expand_more), findsOneWidget);

        // Tap to expand
        await tester.tap(find.byKey(const Key('card-header')));
        await tester.pumpAndSettle();

        // Icon should still be there (rotated)
        expect(find.byIcon(Icons.expand_more), findsOneWidget);
      });

      testWidgets('should handle rapid expansion toggles', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: ExpandableCard(
                title: 'Test Title',
                content: const Text('Test Content'),
              ),
            ),
          ),
        );

        // Rapidly tap header multiple times
        for (int i = 0; i < 5; i++) {
          await tester.tap(find.byKey(const Key('card-header')));
          await tester.pump(const Duration(milliseconds: 10));
        }

        // Let animation settle
        await tester.pumpAndSettle();

        // Should handle this gracefully without errors
      });
    });

    group('Custom Widget Integration Tests', () {
      testWidgets('should work together in complex layouts', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: Column(
                children: [
                  CircularProgressIndicator(
                    progress: 0.8,
                    label: 'Progress',
                  ),
                  const SizedBox(height: 20),
                  StarRating(
                    currentRating: 4.5,
                    onRatingChanged: (rating) {},
                  ),
                  const SizedBox(height: 20),
                  ExpandableCard(
                    title: 'Details',
                    content: const Text('Content here'),
                  ),
                ],
              ),
            ),
          ),
        );

        await tester.pumpAndSettle();

        // All widgets should be present
        expect(find.text('80%'), findsOneWidget);
        expect(find.byIcon(Icons.star), findsNWidgets(4));
        expect(find.byIcon(Icons.star_half), findsOneWidget);
        expect(find.text('Details'), findsOneWidget);
      });
    });
  });
}
```

This example demonstrates testing custom widgets including custom painters,  
interactive widgets with gestures, animated widgets, property validation,  
and integration testing of multiple custom widgets together.

## Testing Animations

Testing animated widgets, animation controllers, and complex animation  
sequences with proper timing verification.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

// Animated bounce button widget
class BounceButton extends StatefulWidget {
  final String text;
  final VoidCallback? onPressed;
  final Duration animationDuration;

  const BounceButton({
    super.key,
    required this.text,
    this.onPressed,
    this.animationDuration = const Duration(milliseconds: 150),
  });

  @override
  State<BounceButton> createState() => _BounceButtonState();
}

class _BounceButtonState extends State<BounceButton>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _scaleAnimation;
  bool _isPressed = false;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );
    _scaleAnimation = Tween<double>(
      begin: 1.0,
      end: 0.9,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  void _handleTapDown(TapDownDetails details) {
    setState(() {
      _isPressed = true;
    });
    _controller.forward();
  }

  void _handleTapUp(TapUpDetails details) {
    _handleTapEnd();
  }

  void _handleTapCancel() {
    _handleTapEnd();
  }

  void _handleTapEnd() {
    setState(() {
      _isPressed = false;
    });
    _controller.reverse();
    widget.onPressed?.call();
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTapDown: _handleTapDown,
      onTapUp: _handleTapUp,
      onTapCancel: _handleTapCancel,
      child: AnimatedBuilder(
        animation: _scaleAnimation,
        builder: (context, child) {
          return Transform.scale(
            scale: _scaleAnimation.value,
            child: Container(
              key: const Key('bounce-container'),
              padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
              decoration: BoxDecoration(
                color: _isPressed ? Colors.blue[700] : Colors.blue,
                borderRadius: BorderRadius.circular(8),
                boxShadow: [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.2),
                    offset: Offset(0, _isPressed ? 2 : 4),
                    blurRadius: _isPressed ? 4 : 8,
                  ),
                ],
              ),
              child: Text(
                widget.text,
                style: const TextStyle(
                  color: Colors.white,
                  fontSize: 16,
                  fontWeight: FontWeight.w600,
                ),
              ),
            ),
          );
        },
      ),
    );
  }
}

// Fade transition widget
class FadeTransitionWidget extends StatefulWidget {
  final Widget child;
  final bool visible;
  final Duration duration;

  const FadeTransitionWidget({
    super.key,
    required this.child,
    required this.visible,
    this.duration = const Duration(milliseconds: 300),
  });

  @override
  State<FadeTransitionWidget> createState() => _FadeTransitionWidgetState();
}

class _FadeTransitionWidgetState extends State<FadeTransitionWidget>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _fadeAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: widget.duration,
      vsync: this,
    );
    _fadeAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));

    if (widget.visible) {
      _controller.forward();
    }
  }

  @override
  void didUpdateWidget(FadeTransitionWidget oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (oldWidget.visible != widget.visible) {
      if (widget.visible) {
        _controller.forward();
      } else {
        _controller.reverse();
      }
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return FadeTransition(
      opacity: _fadeAnimation,
      child: widget.child,
    );
  }
}

// Loading animation widget
class LoadingSpinner extends StatefulWidget {
  final double size;
  final Color color;
  final Duration duration;

  const LoadingSpinner({
    super.key,
    this.size = 40.0,
    this.color = Colors.blue,
    this.duration = const Duration(seconds: 1),
  });

  @override
  State<LoadingSpinner> createState() => _LoadingSpinnerState();
}

class _LoadingSpinnerState extends State<LoadingSpinner>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _rotationAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: widget.duration,
      vsync: this,
    );
    _rotationAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(_controller);

    _controller.repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _rotationAnimation,
      builder: (context, child) {
        return Transform.rotate(
          angle: _rotationAnimation.value * 2 * 3.14159,
          child: Container(
            width: widget.size,
            height: widget.size,
            decoration: BoxDecoration(
              border: Border.all(
                color: widget.color.withOpacity(0.3),
                width: 3,
              ),
              borderRadius: BorderRadius.circular(widget.size / 2),
            ),
            child: CustomPaint(
              painter: _LoadingPainter(color: widget.color),
            ),
          ),
        );
      },
    );
  }
}

class _LoadingPainter extends CustomPainter {
  final Color color;

  _LoadingPainter({required this.color});

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..color = color
      ..strokeWidth = 3
      ..style = PaintingStyle.stroke
      ..strokeCap = StrokeCap.round;

    final center = Offset(size.width / 2, size.height / 2);
    final radius = size.width / 2 - 1.5;

    canvas.drawArc(
      Rect.fromCircle(center: center, radius: radius),
      0,
      3.14159, // Half circle
      false,
      paint,
    );
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => false;
}

// Slide animation widget
class SlideInWidget extends StatefulWidget {
  final Widget child;
  final bool visible;
  final SlideDirection direction;
  final Duration duration;

  const SlideInWidget({
    super.key,
    required this.child,
    required this.visible,
    this.direction = SlideDirection.fromBottom,
    this.duration = const Duration(milliseconds: 300),
  });

  @override
  State<SlideInWidget> createState() => _SlideInWidgetState();
}

enum SlideDirection { fromTop, fromBottom, fromLeft, fromRight }

class _SlideInWidgetState extends State<SlideInWidget>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<Offset> _slideAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: widget.duration,
      vsync: this,
    );

    _slideAnimation = Tween<Offset>(
      begin: _getBeginOffset(),
      end: Offset.zero,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeOutCubic,
    ));

    if (widget.visible) {
      _controller.forward();
    }
  }

  Offset _getBeginOffset() {
    switch (widget.direction) {
      case SlideDirection.fromTop:
        return const Offset(0, -1);
      case SlideDirection.fromBottom:
        return const Offset(0, 1);
      case SlideDirection.fromLeft:
        return const Offset(-1, 0);
      case SlideDirection.fromRight:
        return const Offset(1, 0);
    }
  }

  @override
  void didUpdateWidget(SlideInWidget oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (oldWidget.visible != widget.visible) {
      if (widget.visible) {
        _controller.forward();
      } else {
        _controller.reverse();
      }
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return SlideTransition(
      position: _slideAnimation,
      child: widget.child,
    );
  }
}

void main() {
  group('Animation Tests', () {
    group('BounceButton Tests', () {
      testWidgets('should render with correct text', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: BounceButton(text: 'Click Me'),
            ),
          ),
        );

        expect(find.text('Click Me'), findsOneWidget);
        expect(find.byKey(const Key('bounce-container')), findsOneWidget);
      });

      testWidgets('should animate scale on tap', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: BounceButton(
                text: 'Test Button',
                animationDuration: Duration(milliseconds: 100),
              ),
            ),
          ),
        );

        // Get initial transform
        Transform initialTransform = tester.widget(
          find.descendant(
            of: find.byKey(const Key('bounce-container')),
            matching: find.byType(Transform),
          ),
        );

        // Tap down to start animation
        await tester.startGesture(tester.getCenter(find.text('Test Button')));
        await tester.pump(const Duration(milliseconds: 50));

        // Get transform during animation
        Transform animatedTransform = tester.widget(
          find.descendant(
            of: find.byKey(const Key('bounce-container')),
            matching: find.byType(Transform),
          ),
        );

        // Transform should have changed (scale should be different)
        expect(animatedTransform.transform, isNot(equals(initialTransform.transform)));
      });

      testWidgets('should call onPressed callback', (WidgetTester tester) async {
        bool pressed = false;

        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: BounceButton(
                text: 'Press Me',
                onPressed: () => pressed = true,
              ),
            ),
          ),
        );

        await tester.tap(find.text('Press Me'));
        await tester.pumpAndSettle();

        expect(pressed, isTrue);
      });

      testWidgets('should handle tap cancel', (WidgetTester tester) async {
        bool pressed = false;

        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: BounceButton(
                text: 'Cancel Test',
                onPressed: () => pressed = true,
              ),
            ),
          ),
        );

        // Start gesture but don't complete it
        final gesture = await tester.startGesture(tester.getCenter(find.text('Cancel Test')));
        await tester.pump(const Duration(milliseconds: 50));
        
        // Cancel the gesture
        await gesture.cancel();
        await tester.pumpAndSettle();

        expect(pressed, isFalse);
      });

      testWidgets('should use custom animation duration', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: BounceButton(
                text: 'Custom Duration',
                animationDuration: Duration(milliseconds: 500),
              ),
            ),
          ),
        );

        // Start tap
        await tester.tap(find.text('Custom Duration'));
        
        // Animation should still be running after 250ms (half of 500ms)
        await tester.pump(const Duration(milliseconds: 250));
        
        // Should complete after full duration
        await tester.pump(const Duration(milliseconds: 300));
        await tester.pumpAndSettle();
      });
    });

    group('FadeTransitionWidget Tests', () {
      testWidgets('should start invisible when visible is false', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: FadeTransitionWidget(
                visible: false,
                child: const Text('Fade Me'),
              ),
            ),
          ),
        );

        // Widget should be present but with 0 opacity
        final FadeTransition fadeTransition = tester.widget(find.byType(FadeTransition));
        expect(fadeTransition.opacity.value, equals(0.0));
      });

      testWidgets('should start visible when visible is true', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: FadeTransitionWidget(
                visible: true,
                child: const Text('Visible'),
                duration: Duration(milliseconds: 100),
              ),
            ),
          ),
        );

        await tester.pumpAndSettle();

        final FadeTransition fadeTransition = tester.widget(find.byType(FadeTransition));
        expect(fadeTransition.opacity.value, equals(1.0));
      });

      testWidgets('should fade in when visibility changes to true', (WidgetTester tester) async {
        bool isVisible = false;

        await tester.pumpWidget(
          MaterialApp(
            home: StatefulBuilder(
              builder: (context, setState) {
                return Scaffold(
                  body: Column(
                    children: [
                      ElevatedButton(
                        onPressed: () => setState(() => isVisible = !isVisible),
                        child: const Text('Toggle'),
                      ),
                      FadeTransitionWidget(
                        visible: isVisible,
                        duration: Duration(milliseconds: 100),
                        child: const Text('Fade Content'),
                      ),
                    ],
                  ),
                );
              },
            ),
          ),
        );

        // Initially invisible
        FadeTransition fadeTransition = tester.widget(find.byType(FadeTransition));
        expect(fadeTransition.opacity.value, equals(0.0));

        // Toggle visibility
        await tester.tap(find.text('Toggle'));
        await tester.pump();

        // Should start fading in
        await tester.pump(const Duration(milliseconds: 50));
        fadeTransition = tester.widget(find.byType(FadeTransition));
        expect(fadeTransition.opacity.value, greaterThan(0.0));
        expect(fadeTransition.opacity.value, lessThan(1.0));

        // Should be fully visible after animation
        await tester.pumpAndSettle();
        fadeTransition = tester.widget(find.byType(FadeTransition));
        expect(fadeTransition.opacity.value, equals(1.0));
      });

      testWidgets('should fade out when visibility changes to false', (WidgetTester tester) async {
        bool isVisible = true;

        await tester.pumpWidget(
          MaterialApp(
            home: StatefulBuilder(
              builder: (context, setState) {
                return Scaffold(
                  body: Column(
                    children: [
                      ElevatedButton(
                        onPressed: () => setState(() => isVisible = !isVisible),
                        child: const Text('Hide'),
                      ),
                      FadeTransitionWidget(
                        visible: isVisible,
                        duration: Duration(milliseconds: 100),
                        child: const Text('Hide Me'),
                      ),
                    ],
                  ),
                );
              },
            ),
          ),
        );

        await tester.pumpAndSettle();

        // Initially visible
        FadeTransition fadeTransition = tester.widget(find.byType(FadeTransition));
        expect(fadeTransition.opacity.value, equals(1.0));

        // Toggle to hide
        await tester.tap(find.text('Hide'));
        await tester.pump(const Duration(milliseconds: 50));

        // Should be fading out
        fadeTransition = tester.widget(find.byType(FadeTransition));
        expect(fadeTransition.opacity.value, lessThan(1.0));
        expect(fadeTransition.opacity.value, greaterThan(0.0));
      });
    });

    group('LoadingSpinner Tests', () {
      testWidgets('should render with correct size and color', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: LoadingSpinner(
                size: 50.0,
                color: Colors.red,
              ),
            ),
          ),
        );

        // Check container size
        final Container container = tester.widget(find.byType(Container));
        expect(container.constraints?.maxWidth, equals(50.0));
        expect(container.constraints?.maxHeight, equals(50.0));
      });

      testWidgets('should continuously rotate', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: LoadingSpinner(
                duration: Duration(milliseconds: 200),
              ),
            ),
          ),
        );

        // Get initial rotation
        Transform initialTransform = tester.widget(find.byType(Transform));
        final initialMatrix = initialTransform.transform;

        // Wait for rotation
        await tester.pump(const Duration(milliseconds: 100));

        Transform rotatedTransform = tester.widget(find.byType(Transform));
        final rotatedMatrix = rotatedTransform.transform;

        // Transform should have changed
        expect(rotatedMatrix, isNot(equals(initialMatrix)));

        // Wait for full rotation cycle
        await tester.pump(const Duration(milliseconds: 150));

        Transform nextTransform = tester.widget(find.byType(Transform));
        final nextMatrix = nextTransform.transform;

        // Should continue rotating
        expect(nextMatrix, isNot(equals(rotatedMatrix)));
      });

      testWidgets('should use custom duration', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: LoadingSpinner(
                duration: Duration(milliseconds: 1000), // 1 second
              ),
            ),
          ),
        );

        Transform initialTransform = tester.widget(find.byType(Transform));
        final initialMatrix = initialTransform.transform;

        // After half duration, should be at different rotation
        await tester.pump(const Duration(milliseconds: 500));

        Transform halfwayTransform = tester.widget(find.byType(Transform));
        final halfwayMatrix = halfwayTransform.transform;

        expect(halfwayMatrix, isNot(equals(initialMatrix)));
      });
    });

    group('SlideInWidget Tests', () {
      testWidgets('should slide in from bottom by default', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: SlideInWidget(
                visible: true,
                duration: Duration(milliseconds: 100),
                child: const Text('Slide Content'),
              ),
            ),
          ),
        );

        // Should animate to position
        await tester.pumpAndSettle();

        final SlideTransition slideTransition = tester.widget(find.byType(SlideTransition));
        expect(slideTransition.position.value, equals(Offset.zero));
      });

      testWidgets('should slide from different directions', (WidgetTester tester) async {
        for (final direction in SlideDirection.values) {
          await tester.pumpWidget(
            MaterialApp(
              home: Scaffold(
                body: SlideInWidget(
                  visible: true,
                  direction: direction,
                  duration: Duration(milliseconds: 50),
                  child: Text('Slide ${direction.name}'),
                ),
              ),
            ),
          );

          await tester.pumpAndSettle();

          final SlideTransition slideTransition = tester.widget(find.byType(SlideTransition));
          expect(slideTransition.position.value, equals(Offset.zero));

          // Clean up for next iteration
          await tester.pumpWidget(Container());
        }
      });

      testWidgets('should slide in when visibility changes', (WidgetTester tester) async {
        bool isVisible = false;

        await tester.pumpWidget(
          MaterialApp(
            home: StatefulBuilder(
              builder: (context, setState) {
                return Scaffold(
                  body: Column(
                    children: [
                      ElevatedButton(
                        onPressed: () => setState(() => isVisible = true),
                        child: const Text('Show'),
                      ),
                      SlideInWidget(
                        visible: isVisible,
                        duration: Duration(milliseconds: 100),
                        child: const Text('Slide In'),
                      ),
                    ],
                  ),
                );
              },
            ),
          ),
        );

        // Trigger slide in
        await tester.tap(find.text('Show'));
        await tester.pump();

        // Should be animating
        await tester.pump(const Duration(milliseconds: 50));
        
        // Should complete
        await tester.pumpAndSettle();
        
        final SlideTransition slideTransition = tester.widget(find.byType(SlideTransition));
        expect(slideTransition.position.value, equals(Offset.zero));
      });
    });

    group('Animation Performance Tests', () {
      testWidgets('should handle multiple simultaneous animations', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: Column(
                children: [
                  BounceButton(text: 'Bounce 1'),
                  BounceButton(text: 'Bounce 2'),
                  FadeTransitionWidget(
                    visible: true,
                    child: const Text('Fade'),
                  ),
                  LoadingSpinner(),
                  SlideInWidget(
                    visible: true,
                    child: const Text('Slide'),
                  ),
                ],
              ),
            ),
          ),
        );

        // All animations should work together without issues
        await tester.tap(find.text('Bounce 1'));
        await tester.tap(find.text('Bounce 2'));
        
        await tester.pump(const Duration(milliseconds: 50));
        await tester.pumpAndSettle();

        // All widgets should still be present
        expect(find.text('Bounce 1'), findsOneWidget);
        expect(find.text('Bounce 2'), findsOneWidget);
        expect(find.text('Fade'), findsOneWidget);
        expect(find.byType(LoadingSpinner), findsOneWidget);
        expect(find.text('Slide'), findsOneWidget);
      });

      testWidgets('should properly dispose animation controllers', (WidgetTester tester) async {
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: Column(
                children: [
                  BounceButton(text: 'Test'),
                  FadeTransitionWidget(visible: true, child: Text('Test')),
                  LoadingSpinner(),
                ],
              ),
            ),
          ),
        );

        // Remove all animated widgets
        await tester.pumpWidget(
          MaterialApp(
            home: Scaffold(
              body: const Text('Empty'),
            ),
          ),
        );

        // Should not throw any errors during disposal
        await tester.pumpAndSettle();
      });
    });
  });
}
```

This example demonstrates comprehensive animation testing including scale  
animations, fade transitions, rotation animations, slide animations,  
timing verification, and performance testing with multiple simultaneous  
animations.

## Basic Integration Test Setup

Setting up integration testing environment and writing end-to-end tests  
that simulate real user interactions across the entire app.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';

// Main app for integration testing
class TestApp extends StatelessWidget {
  const TestApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Integration Test App',
      theme: ThemeData.dark(),
      initialRoute: '/',
      routes: {
        '/': (context) => const HomeScreen(),
        '/login': (context) => const LoginScreen(),
        '/profile': (context) => const ProfileScreen(),
        '/settings': (context) => const SettingsScreen(),
      },
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home'),
        actions: [
          IconButton(
            key: const Key('settings-icon'),
            icon: const Icon(Icons.settings),
            onPressed: () => Navigator.pushNamed(context, '/settings'),
          ),
        ],
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Welcome to the App!',
              key: Key('welcome-text'),
              style: TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              key: const Key('login-button'),
              onPressed: () => Navigator.pushNamed(context, '/login'),
              child: const Text('Go to Login'),
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              key: const Key('profile-button'),
              onPressed: () => Navigator.pushNamed(context, '/profile'),
              child: const Text('View Profile'),
            ),
          ],
        ),
      ),
    );
  }
}

class LoginScreen extends StatefulWidget {
  const LoginScreen({super.key});

  @override
  State<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final _usernameController = TextEditingController();
  final _passwordController = TextEditingController();
  bool _isLoading = false;

  @override
  void dispose() {
    _usernameController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  Future<void> _login() async {
    if (_usernameController.text.isEmpty || _passwordController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          key: Key('error-snackbar'),
          content: Text('Please fill in all fields'),
        ),
      );
      return;
    }

    setState(() {
      _isLoading = true;
    });

    // Simulate API call
    await Future.delayed(const Duration(seconds: 2));

    if (_usernameController.text == 'admin' && _passwordController.text == 'password') {
      Navigator.pushReplacementNamed(context, '/profile');
    } else {
      if (mounted) {
        setState(() {
          _isLoading = false;
        });
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            key: Key('login-error-snackbar'),
            content: Text('Invalid credentials'),
          ),
        );
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Login'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              key: const Key('username-field'),
              controller: _usernameController,
              decoration: const InputDecoration(
                labelText: 'Username',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              key: const Key('password-field'),
              controller: _passwordController,
              obscureText: true,
              decoration: const InputDecoration(
                labelText: 'Password',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 24),
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                key: const Key('submit-login'),
                onPressed: _isLoading ? null : _login,
                child: _isLoading
                    ? const CircularProgressIndicator()
                    : const Text('Login'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class ProfileScreen extends StatelessWidget {
  const ProfileScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Profile'),
        leading: IconButton(
          key: const Key('back-to-home'),
          icon: const Icon(Icons.home),
          onPressed: () => Navigator.pushNamedAndRemoveUntil(
            context,
            '/',
            (route) => false,
          ),
        ),
      ),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            CircleAvatar(
              key: Key('profile-avatar'),
              radius: 50,
              child: Icon(Icons.person, size: 50),
            ),
            SizedBox(height: 20),
            Text(
              'Welcome, Admin!',
              key: Key('profile-welcome'),
              style: TextStyle(fontSize: 24),
            ),
            SizedBox(height: 10),
            Text(
              'This is your profile page.',
              key: Key('profile-description'),
            ),
          ],
        ),
      ),
    );
  }
}

class SettingsScreen extends StatefulWidget {
  const SettingsScreen({super.key});

  @override
  State<SettingsScreen> createState() => _SettingsScreenState();
}

class _SettingsScreenState extends State<SettingsScreen> {
  bool _darkMode = true;
  bool _notifications = false;
  double _volume = 0.5;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Settings'),
      ),
      body: ListView(
        padding: const EdgeInsets.all(16.0),
        children: [
          SwitchListTile(
            key: const Key('dark-mode-switch'),
            title: const Text('Dark Mode'),
            value: _darkMode,
            onChanged: (value) => setState(() => _darkMode = value),
          ),
          SwitchListTile(
            key: const Key('notifications-switch'),
            title: const Text('Enable Notifications'),
            value: _notifications,
            onChanged: (value) => setState(() => _notifications = value),
          ),
          const SizedBox(height: 20),
          Text('Volume: ${(_volume * 100).round()}%'),
          Slider(
            key: const Key('volume-slider'),
            value: _volume,
            onChanged: (value) => setState(() => _volume = value),
          ),
          const SizedBox(height: 20),
          ElevatedButton(
            key: const Key('save-settings'),
            onPressed: () {
              ScaffoldMessenger.of(context).showSnackBar(
                const SnackBar(
                  key: Key('settings-saved'),
                  content: Text('Settings saved!'),
                ),
              );
            },
            child: const Text('Save Settings'),
          ),
        ],
      ),
    );
  }
}

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  group('Integration Tests', () {
    testWidgets('basic app flow test', (WidgetTester tester) async {
      await tester.pumpWidget(const TestApp());
      await tester.pumpAndSettle();

      // Verify home screen loads
      expect(find.byKey(const Key('welcome-text')), findsOneWidget);
      expect(find.text('Welcome to the App!'), findsOneWidget);

      // Navigate to settings
      await tester.tap(find.byKey(const Key('settings-icon')));
      await tester.pumpAndSettle();

      // Verify settings screen
      expect(find.text('Settings'), findsOneWidget);
      expect(find.byKey(const Key('dark-mode-switch')), findsOneWidget);

      // Go back to home
      await tester.pageBack();
      await tester.pumpAndSettle();

      // Should be back on home screen
      expect(find.byKey(const Key('welcome-text')), findsOneWidget);
    });

    testWidgets('login flow test', (WidgetTester tester) async {
      await tester.pumpWidget(const TestApp());
      await tester.pumpAndSettle();

      // Navigate to login
      await tester.tap(find.byKey(const Key('login-button')));
      await tester.pumpAndSettle();

      // Verify login screen
      expect(find.text('Login'), findsOneWidget);
      expect(find.byKey(const Key('username-field')), findsOneWidget);
      expect(find.byKey(const Key('password-field')), findsOneWidget);

      // Test empty field validation
      await tester.tap(find.byKey(const Key('submit-login')));
      await tester.pumpAndSettle();

      expect(find.byKey(const Key('error-snackbar')), findsOneWidget);
      expect(find.text('Please fill in all fields'), findsOneWidget);

      // Wait for snackbar to disappear
      await tester.pump(const Duration(seconds: 3));

      // Test invalid credentials
      await tester.enterText(find.byKey(const Key('username-field')), 'wrong');
      await tester.enterText(find.byKey(const Key('password-field')), 'wrong');
      await tester.tap(find.byKey(const Key('submit-login')));

      // Wait for loading state
      await tester.pump();
      expect(find.byType(CircularProgressIndicator), findsOneWidget);

      // Wait for response
      await tester.pump(const Duration(seconds: 2));
      await tester.pumpAndSettle();

      expect(find.byKey(const Key('login-error-snackbar')), findsOneWidget);

      // Wait for snackbar to disappear
      await tester.pump(const Duration(seconds: 3));

      // Test valid credentials
      await tester.enterText(find.byKey(const Key('username-field')), 'admin');
      await tester.enterText(find.byKey(const Key('password-field')), 'password');
      await tester.tap(find.byKey(const Key('submit-login')));

      // Wait for loading and navigation
      await tester.pump();
      await tester.pump(const Duration(seconds: 2));
      await tester.pumpAndSettle();

      // Should be on profile screen
      expect(find.text('Welcome, Admin!'), findsOneWidget);
      expect(find.byKey(const Key('profile-avatar')), findsOneWidget);
    });

    testWidgets('settings interaction test', (WidgetTester tester) async {
      await tester.pumpWidget(const TestApp());
      await tester.pumpAndSettle();

      // Navigate to settings
      await tester.tap(find.byKey(const Key('settings-icon')));
      await tester.pumpAndSettle();

      // Test dark mode switch
      final darkModeSwitch = find.byKey(const Key('dark-mode-switch'));
      expect(darkModeSwitch, findsOneWidget);
      
      // Switch should be on initially
      Switch switchWidget = tester.widget(
        find.descendant(
          of: darkModeSwitch,
          matching: find.byType(Switch),
        ),
      );
      expect(switchWidget.value, isTrue);

      // Toggle switch
      await tester.tap(darkModeSwitch);
      await tester.pump();

      switchWidget = tester.widget(
        find.descendant(
          of: darkModeSwitch,
          matching: find.byType(Switch),
        ),
      );
      expect(switchWidget.value, isFalse);

      // Test notifications switch
      await tester.tap(find.byKey(const Key('notifications-switch')));
      await tester.pump();

      final notificationSwitch = find.byKey(const Key('notifications-switch'));
      Switch notificationSwitchWidget = tester.widget(
        find.descendant(
          of: notificationSwitch,
          matching: find.byType(Switch),
        ),
      );
      expect(notificationSwitchWidget.value, isTrue);

      // Test volume slider
      final volumeSlider = find.byKey(const Key('volume-slider'));
      await tester.drag(volumeSlider, const Offset(100, 0));
      await tester.pump();

      // Save settings
      await tester.tap(find.byKey(const Key('save-settings')));
      await tester.pumpAndSettle();

      expect(find.byKey(const Key('settings-saved')), findsOneWidget);
    });

    testWidgets('complete user journey test', (WidgetTester tester) async {
      await tester.pumpWidget(const TestApp());
      await tester.pumpAndSettle();

      // Start from home
      expect(find.text('Welcome to the App!'), findsOneWidget);

      // Go to login
      await tester.tap(find.byKey(const Key('login-button')));
      await tester.pumpAndSettle();

      // Login successfully
      await tester.enterText(find.byKey(const Key('username-field')), 'admin');
      await tester.enterText(find.byKey(const Key('password-field')), 'password');
      await tester.tap(find.byKey(const Key('submit-login')));
      await tester.pump(const Duration(seconds: 2));
      await tester.pumpAndSettle();

      // Should be on profile
      expect(find.text('Welcome, Admin!'), findsOneWidget);

      // Go back to home
      await tester.tap(find.byKey(const Key('back-to-home')));
      await tester.pumpAndSettle();

      // Should be back on home screen
      expect(find.text('Welcome to the App!'), findsOneWidget);

      // Go to settings
      await tester.tap(find.byKey(const Key('settings-icon')));
      await tester.pumpAndSettle();

      // Change some settings
      await tester.tap(find.byKey(const Key('notifications-switch')));
      await tester.drag(find.byKey(const Key('volume-slider')), const Offset(50, 0));
      await tester.tap(find.byKey(const Key('save-settings')));
      await tester.pumpAndSettle();

      // Verify save message
      expect(find.text('Settings saved!'), findsOneWidget);
    });

    testWidgets('error handling and recovery test', (WidgetTester tester) async {
      await tester.pumpWidget(const TestApp());
      await tester.pumpAndSettle();

      // Navigate to login
      await tester.tap(find.byKey(const Key('login-button')));
      await tester.pumpAndSettle();

      // Try multiple failed login attempts
      for (int i = 0; i < 3; i++) {
        await tester.enterText(find.byKey(const Key('username-field')), 'user$i');
        await tester.enterText(find.byKey(const Key('password-field')), 'wrong$i');
        await tester.tap(find.byKey(const Key('submit-login')));
        
        // Wait for response
        await tester.pump(const Duration(seconds: 2));
        await tester.pumpAndSettle();
        
        // Should show error
        expect(find.text('Invalid credentials'), findsOneWidget);
        
        // Wait for snackbar to disappear
        await tester.pump(const Duration(seconds: 3));
        await tester.pump();
      }

      // Finally login successfully
      await tester.enterText(find.byKey(const Key('username-field')), 'admin');
      await tester.enterText(find.byKey(const Key('password-field')), 'password');
      await tester.tap(find.byKey(const Key('submit-login')));
      await tester.pump(const Duration(seconds: 2));
      await tester.pumpAndSettle();

      // Should eventually succeed
      expect(find.text('Welcome, Admin!'), findsOneWidget);
    });
  });
}
```

This example demonstrates comprehensive integration testing including full  
user workflows, navigation testing, form interactions, error handling,  
and complex multi-screen user journeys that test the entire app flow.

## End-to-End User Flows

Testing complete user scenarios that span multiple screens and features,  
simulating real-world usage patterns and user behavior.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';

// E-commerce app for end-to-end testing
class ShoppingApp extends StatelessWidget {
  const ShoppingApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Shopping App E2E Test',
      theme: ThemeData.dark(),
      home: const ProductListScreen(),
    );
  }
}

class Product {
  final String id;
  final String name;
  final double price;
  final String description;

  Product({
    required this.id,
    required this.name,
    required this.price,
    required this.description,
  });
}

class CartItem {
  final Product product;
  int quantity;

  CartItem({required this.product, this.quantity = 1});
}

class Cart {
  static final Cart _instance = Cart._internal();
  factory Cart() => _instance;
  Cart._internal();

  final List<CartItem> _items = [];

  List<CartItem> get items => List.unmodifiable(_items);

  void addItem(Product product) {
    final existingIndex = _items.indexWhere((item) => item.product.id == product.id);
    if (existingIndex >= 0) {
      _items[existingIndex].quantity++;
    } else {
      _items.add(CartItem(product: product));
    }
  }

  void removeItem(String productId) {
    _items.removeWhere((item) => item.product.id == productId);
  }

  void updateQuantity(String productId, int quantity) {
    final index = _items.indexWhere((item) => item.product.id == productId);
    if (index >= 0) {
      if (quantity <= 0) {
        _items.removeAt(index);
      } else {
        _items[index].quantity = quantity;
      }
    }
  }

  double get total => _items.fold(0.0, (sum, item) => sum + (item.product.price * item.quantity));

  int get itemCount => _items.fold(0, (sum, item) => sum + item.quantity);

  void clear() => _items.clear();
}

class ProductListScreen extends StatelessWidget {
  const ProductListScreen({super.key});

  static final List<Product> _products = [
    Product(id: '1', name: 'Laptop', price: 999.99, description: 'High-performance laptop'),
    Product(id: '2', name: 'Phone', price: 699.99, description: 'Latest smartphone'),
    Product(id: '3', name: 'Headphones', price: 199.99, description: 'Noise-cancelling headphones'),
    Product(id: '4', name: 'Tablet', price: 449.99, description: '10-inch tablet'),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Products'),
        actions: [
          IconButton(
            key: const Key('search-icon'),
            icon: const Icon(Icons.search),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(builder: (context) => const SearchScreen()),
              );
            },
          ),
          IconButton(
            key: const Key('cart-icon'),
            icon: Badge(
              label: Text('${Cart().itemCount}'),
              child: const Icon(Icons.shopping_cart),
            ),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(builder: (context) => const CartScreen()),
              );
            },
          ),
        ],
      ),
      body: ListView.builder(
        key: const Key('product-list'),
        itemCount: _products.length,
        itemBuilder: (context, index) {
          final product = _products[index];
          return ListTile(
            key: Key('product-${product.id}'),
            leading: const Icon(Icons.phone_android, size: 40),
            title: Text(product.name),
            subtitle: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(product.description),
                Text('\$${product.price.toStringAsFixed(2)}'),
              ],
            ),
            trailing: ElevatedButton(
              key: Key('add-to-cart-${product.id}'),
              onPressed: () {
                Cart().addItem(product);
                ScaffoldMessenger.of(context).showSnackBar(
                  SnackBar(
                    key: Key('added-snackbar-${product.id}'),
                    content: Text('${product.name} added to cart'),
                    duration: const Duration(seconds: 1),
                  ),
                );
              },
              child: const Text('Add to Cart'),
            ),
            onTap: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => ProductDetailScreen(product: product),
                ),
              );
            },
          );
        },
      ),
    );
  }
}

class ProductDetailScreen extends StatelessWidget {
  final Product product;

  const ProductDetailScreen({super.key, required this.product});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(product.name),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Container(
              key: const Key('product-image'),
              width: double.infinity,
              height: 200,
              color: Colors.grey[800],
              child: const Icon(Icons.image, size: 80),
            ),
            const SizedBox(height: 20),
            Text(
              product.name,
              style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            Text(
              '\$${product.price.toStringAsFixed(2)}',
              style: const TextStyle(fontSize: 20, color: Colors.green),
            ),
            const SizedBox(height: 20),
            Text(
              product.description,
              style: const TextStyle(fontSize: 16),
            ),
            const Spacer(),
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                key: const Key('add-to-cart-detail'),
                onPressed: () {
                  Cart().addItem(product);
                  ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(
                      key: const Key('detail-added-snackbar'),
                      content: Text('${product.name} added to cart'),
                    ),
                  );
                },
                child: const Text('Add to Cart'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class SearchScreen extends StatefulWidget {
  const SearchScreen({super.key});

  @override
  State<SearchScreen> createState() => _SearchScreenState();
}

class _SearchScreenState extends State<SearchScreen> {
  final _searchController = TextEditingController();
  List<Product> _searchResults = [];

  static final List<Product> _allProducts = [
    Product(id: '1', name: 'Laptop', price: 999.99, description: 'High-performance laptop'),
    Product(id: '2', name: 'Phone', price: 699.99, description: 'Latest smartphone'),
    Product(id: '3', name: 'Headphones', price: 199.99, description: 'Noise-cancelling headphones'),
    Product(id: '4', name: 'Tablet', price: 449.99, description: '10-inch tablet'),
    Product(id: '5', name: 'Mouse', price: 29.99, description: 'Wireless mouse'),
    Product(id: '6', name: 'Keyboard', price: 79.99, description: 'Mechanical keyboard'),
  ];

  @override
  void dispose() {
    _searchController.dispose();
    super.dispose();
  }

  void _performSearch(String query) {
    if (query.isEmpty) {
      setState(() {
        _searchResults = [];
      });
      return;
    }

    setState(() {
      _searchResults = _allProducts
          .where((product) =>
              product.name.toLowerCase().contains(query.toLowerCase()) ||
              product.description.toLowerCase().contains(query.toLowerCase()))
          .toList();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: TextField(
          key: const Key('search-field'),
          controller: _searchController,
          autofocus: true,
          decoration: const InputDecoration(
            hintText: 'Search products...',
            border: InputBorder.none,
          ),
          onChanged: _performSearch,
        ),
      ),
      body: _searchResults.isEmpty
          ? const Center(
              child: Text(
                'No results found',
                key: Key('no-results'),
              ),
            )
          : ListView.builder(
              key: const Key('search-results'),
              itemCount: _searchResults.length,
              itemBuilder: (context, index) {
                final product = _searchResults[index];
                return ListTile(
                  key: Key('search-result-${product.id}'),
                  title: Text(product.name),
                  subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
                  onTap: () {
                    Navigator.push(
                      context,
                      MaterialPageRoute(
                        builder: (context) => ProductDetailScreen(product: product),
                      ),
                    );
                  },
                );
              },
            ),
    );
  }
}

class CartScreen extends StatefulWidget {
  const CartScreen({super.key});

  @override
  State<CartScreen> createState() => _CartScreenState();
}

class _CartScreenState extends State<CartScreen> {
  @override
  Widget build(BuildContext context) {
    final cart = Cart();
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Shopping Cart'),
      ),
      body: cart.items.isEmpty
          ? const Center(
              child: Text(
                'Your cart is empty',
                key: Key('empty-cart'),
                style: TextStyle(fontSize: 18),
              ),
            )
          : Column(
              children: [
                Expanded(
                  child: ListView.builder(
                    key: const Key('cart-items'),
                    itemCount: cart.items.length,
                    itemBuilder: (context, index) {
                      final cartItem = cart.items[index];
                      return ListTile(
                        key: Key('cart-item-${cartItem.product.id}'),
                        title: Text(cartItem.product.name),
                        subtitle: Text('\$${cartItem.product.price.toStringAsFixed(2)}'),
                        trailing: Row(
                          mainAxisSize: MainAxisSize.min,
                          children: [
                            IconButton(
                              key: Key('decrease-${cartItem.product.id}'),
                              icon: const Icon(Icons.remove),
                              onPressed: () {
                                setState(() {
                                  cart.updateQuantity(cartItem.product.id, cartItem.quantity - 1);
                                });
                              },
                            ),
                            Text('${cartItem.quantity}'),
                            IconButton(
                              key: Key('increase-${cartItem.product.id}'),
                              icon: const Icon(Icons.add),
                              onPressed: () {
                                setState(() {
                                  cart.updateQuantity(cartItem.product.id, cartItem.quantity + 1);
                                });
                              },
                            ),
                            IconButton(
                              key: Key('remove-${cartItem.product.id}'),
                              icon: const Icon(Icons.delete, color: Colors.red),
                              onPressed: () {
                                setState(() {
                                  cart.removeItem(cartItem.product.id);
                                });
                              },
                            ),
                          ],
                        ),
                      );
                    },
                  ),
                ),
                Container(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    children: [
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          const Text('Total:', style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
                          Text(
                            '\$${cart.total.toStringAsFixed(2)}',
                            key: const Key('cart-total'),
                            style: const TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
                          ),
                        ],
                      ),
                      const SizedBox(height: 16),
                      SizedBox(
                        width: double.infinity,
                        child: ElevatedButton(
                          key: const Key('checkout-button'),
                          onPressed: cart.items.isNotEmpty
                              ? () {
                                  Navigator.push(
                                    context,
                                    MaterialPageRoute(
                                      builder: (context) => const CheckoutScreen(),
                                    ),
                                  );
                                }
                              : null,
                          child: const Text('Proceed to Checkout'),
                        ),
                      ),
                    ],
                  ),
                ),
              ],
            ),
    );
  }
}

class CheckoutScreen extends StatefulWidget {
  const CheckoutScreen({super.key});

  @override
  State<CheckoutScreen> createState() => _CheckoutScreenState();
}

class _CheckoutScreenState extends State<CheckoutScreen> {
  final _nameController = TextEditingController();
  final _addressController = TextEditingController();
  final _phoneController = TextEditingController();
  bool _isProcessing = false;

  @override
  void dispose() {
    _nameController.dispose();
    _addressController.dispose();
    _phoneController.dispose();
    super.dispose();
  }

  Future<void> _processOrder() async {
    if (_nameController.text.isEmpty ||
        _addressController.text.isEmpty ||
        _phoneController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          key: Key('checkout-error'),
          content: Text('Please fill in all fields'),
        ),
      );
      return;
    }

    setState(() {
      _isProcessing = true;
    });

    // Simulate payment processing
    await Future.delayed(const Duration(seconds: 3));

    Cart().clear();

    if (mounted) {
      Navigator.pushAndRemoveUntil(
        context,
        MaterialPageRoute(builder: (context) => const OrderSuccessScreen()),
        (route) => false,
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    final cart = Cart();
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Checkout'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Order Summary',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            ...cart.items.map((item) => Padding(
              padding: const EdgeInsets.symmetric(vertical: 4),
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  Text('${item.product.name} x${item.quantity}'),
                  Text('\$${(item.product.price * item.quantity).toStringAsFixed(2)}'),
                ],
              ),
            )),
            const Divider(),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                const Text('Total:', style: TextStyle(fontWeight: FontWeight.bold)),
                Text(
                  '\$${cart.total.toStringAsFixed(2)}',
                  key: const Key('checkout-total'),
                  style: const TextStyle(fontWeight: FontWeight.bold),
                ),
              ],
            ),
            const SizedBox(height: 30),
            const Text(
              'Shipping Information',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            TextField(
              key: const Key('checkout-name'),
              controller: _nameController,
              decoration: const InputDecoration(
                labelText: 'Full Name',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              key: const Key('checkout-address'),
              controller: _addressController,
              maxLines: 3,
              decoration: const InputDecoration(
                labelText: 'Shipping Address',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              key: const Key('checkout-phone'),
              controller: _phoneController,
              keyboardType: TextInputType.phone,
              decoration: const InputDecoration(
                labelText: 'Phone Number',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 30),
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                key: const Key('place-order'),
                onPressed: _isProcessing ? null : _processOrder,
                child: _isProcessing
                    ? const CircularProgressIndicator()
                    : const Text('Place Order'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class OrderSuccessScreen extends StatelessWidget {
  const OrderSuccessScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Icon(
              Icons.check_circle,
              key: Key('success-icon'),
              color: Colors.green,
              size: 100,
            ),
            const SizedBox(height: 20),
            const Text(
              'Order Placed Successfully!',
              key: Key('success-message'),
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            const Text(
              'Thank you for your purchase.',
              style: TextStyle(fontSize: 16),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              key: const Key('back-to-products'),
              onPressed: () {
                Navigator.pushAndRemoveUntil(
                  context,
                  MaterialPageRoute(builder: (context) => const ProductListScreen()),
                  (route) => false,
                );
              },
              child: const Text('Continue Shopping'),
            ),
          ],
        ),
      ),
    );
  }
}

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  group('E2E Shopping Flow Tests', () {
    setUp(() {
      // Clear cart before each test
      Cart().clear();
    });

    testWidgets('complete shopping journey - single product', (WidgetTester tester) async {
      await tester.pumpWidget(const ShoppingApp());
      await tester.pumpAndSettle();

      // Step 1: Browse products
      expect(find.text('Products'), findsOneWidget);
      expect(find.byKey(const Key('product-list')), findsOneWidget);

      // Step 2: Add laptop to cart from main list
      await tester.tap(find.byKey(const Key('add-to-cart-1')));
      await tester.pump();
      expect(find.text('Laptop added to cart'), findsOneWidget);
      await tester.pump(const Duration(seconds: 2));

      // Step 3: Check cart icon shows item count
      expect(find.text('1'), findsOneWidget); // Badge on cart icon

      // Step 4: Go to cart
      await tester.tap(find.byKey(const Key('cart-icon')));
      await tester.pumpAndSettle();

      // Step 5: Verify cart contents
      expect(find.text('Shopping Cart'), findsOneWidget);
      expect(find.text('Laptop'), findsOneWidget);
      expect(find.text('\$999.99'), findsOneWidget);

      // Step 6: Proceed to checkout
      await tester.tap(find.byKey(const Key('checkout-button')));
      await tester.pumpAndSettle();

      // Step 7: Fill in shipping information
      expect(find.text('Checkout'), findsOneWidget);
      await tester.enterText(find.byKey(const Key('checkout-name')), 'John Doe');
      await tester.enterText(find.byKey(const Key('checkout-address')), '123 Main St, City, State');
      await tester.enterText(find.byKey(const Key('checkout-phone')), '123-456-7890');

      // Step 8: Place order
      await tester.tap(find.byKey(const Key('place-order')));
      await tester.pump();

      // Verify loading state
      expect(find.byType(CircularProgressIndicator), findsOneWidget);

      // Wait for processing
      await tester.pump(const Duration(seconds: 3));
      await tester.pumpAndSettle();

      // Step 9: Verify success screen
      expect(find.byKey(const Key('success-icon')), findsOneWidget);
      expect(find.text('Order Placed Successfully!'), findsOneWidget);

      // Step 10: Return to shopping
      await tester.tap(find.byKey(const Key('back-to-products')));
      await tester.pumpAndSettle();

      // Should be back at product list
      expect(find.text('Products'), findsOneWidget);
    });

    testWidgets('multiple products shopping journey', (WidgetTester tester) async {
      await tester.pumpWidget(const ShoppingApp());
      await tester.pumpAndSettle();

      // Add multiple products to cart
      await tester.tap(find.byKey(const Key('add-to-cart-1'))); // Laptop
      await tester.pump(const Duration(seconds: 1));
      
      await tester.tap(find.byKey(const Key('add-to-cart-2'))); // Phone
      await tester.pump(const Duration(seconds: 1));
      
      await tester.tap(find.byKey(const Key('add-to-cart-3'))); // Headphones
      await tester.pump(const Duration(seconds: 1));

      // Verify cart count
      expect(find.text('3'), findsOneWidget);

      // Go to cart
      await tester.tap(find.byKey(const Key('cart-icon')));
      await tester.pumpAndSettle();

      // Increase quantity of laptop
      await tester.tap(find.byKey(const Key('increase-1')));
      await tester.pump();

      // Remove phone from cart
      await tester.tap(find.byKey(const Key('remove-2')));
      await tester.pump();

      // Verify total calculation
      // Should be: (999.99 * 2) + 199.99 = 2399.97
      expect(find.text('\$2399.97'), findsOneWidget);

      // Complete checkout
      await tester.tap(find.byKey(const Key('checkout-button')));
      await tester.pumpAndSettle();

      await tester.enterText(find.byKey(const Key('checkout-name')), 'Jane Smith');
      await tester.enterText(find.byKey(const Key('checkout-address')), '456 Oak St');
      await tester.enterText(find.byKey(const Key('checkout-phone')), '987-654-3210');

      await tester.tap(find.byKey(const Key('place-order')));
      await tester.pump(const Duration(seconds: 3));
      await tester.pumpAndSettle();

      expect(find.text('Order Placed Successfully!'), findsOneWidget);
    });

    testWidgets('product detail view shopping journey', (WidgetTester tester) async {
      await tester.pumpWidget(const ShoppingApp());
      await tester.pumpAndSettle();

      // Tap on product to view details
      await tester.tap(find.byKey(const Key('product-1')));
      await tester.pumpAndSettle();

      // Verify product detail screen
      expect(find.text('Laptop'), findsOneWidget);
      expect(find.text('\$999.99'), findsOneWidget);
      expect(find.text('High-performance laptop'), findsOneWidget);
      expect(find.byKey(const Key('product-image')), findsOneWidget);

      // Add to cart from detail view
      await tester.tap(find.byKey(const Key('add-to-cart-detail')));
      await tester.pump();
      expect(find.byKey(const Key('detail-added-snackbar')), findsOneWidget);

      // Go back and check cart
      await tester.pageBack();
      await tester.pumpAndSettle();

      await tester.tap(find.byKey(const Key('cart-icon')));
      await tester.pumpAndSettle();

      expect(find.text('Laptop'), findsOneWidget);
    });

    testWidgets('search and purchase journey', (WidgetTester tester) async {
      await tester.pumpWidget(const ShoppingApp());
      await tester.pumpAndSettle();

      // Open search
      await tester.tap(find.byKey(const Key('search-icon')));
      await tester.pumpAndSettle();

      // Search for mouse
      await tester.enterText(find.byKey(const Key('search-field')), 'mouse');
      await tester.pump();

      // Tap on search result
      await tester.tap(find.byKey(const Key('search-result-5')));
      await tester.pumpAndSettle();

      // Should be on mouse detail page
      expect(find.text('Mouse'), findsOneWidget);
      expect(find.text('\$29.99'), findsOneWidget);

      // Add to cart
      await tester.tap(find.byKey(const Key('add-to-cart-detail')));
      await tester.pump();

      // Navigate to cart and checkout
      await tester.pageBack();
      await tester.pumpAndSettle();
      await tester.pageBack();
      await tester.pumpAndSettle();

      await tester.tap(find.byKey(const Key('cart-icon')));
      await tester.pumpAndSettle();

      await tester.tap(find.byKey(const Key('checkout-button')));
      await tester.pumpAndSettle();

      // Complete order
      await tester.enterText(find.byKey(const Key('checkout-name')), 'Test User');
      await tester.enterText(find.byKey(const Key('checkout-address')), 'Test Address');
      await tester.enterText(find.byKey(const Key('checkout-phone')), '555-0123');

      await tester.tap(find.byKey(const Key('place-order')));
      await tester.pump(const Duration(seconds: 3));
      await tester.pumpAndSettle();

      expect(find.text('Order Placed Successfully!'), findsOneWidget);
    });

    testWidgets('cart management journey', (WidgetTester tester) async {
      await tester.pumpWidget(const ShoppingApp());
      await tester.pumpAndSettle();

      // Add multiple items
      await tester.tap(find.byKey(const Key('add-to-cart-1')));
      await tester.pump(const Duration(seconds: 1));
      await tester.tap(find.byKey(const Key('add-to-cart-2')));
      await tester.pump(const Duration(seconds: 1));

      // Go to cart
      await tester.tap(find.byKey(const Key('cart-icon')));
      await tester.pumpAndSettle();

      // Test quantity manipulation
      await tester.tap(find.byKey(const Key('increase-1'))); // Laptop: 2
      await tester.pump();
      await tester.tap(find.byKey(const Key('increase-2'))); // Phone: 2
      await tester.pump();
      await tester.tap(find.byKey(const Key('decrease-1'))); // Laptop: 1
      await tester.pump();

      // Verify total: (999.99 * 1) + (699.99 * 2) = 2399.97
      expect(find.text('\$2399.97'), findsOneWidget);

      // Remove an item completely
      await tester.tap(find.byKey(const Key('decrease-2'))); // Phone: 1
      await tester.pump();
      await tester.tap(find.byKey(const Key('decrease-2'))); // Phone: 0 (removed)
      await tester.pump();

      // Should only have laptop now
      expect(find.text('\$999.99'), findsOneWidget);
      expect(find.text('Phone'), findsNothing);
    });

    testWidgets('error handling during checkout', (WidgetTester tester) async {
      await tester.pumpWidget(const ShoppingApp());
      await tester.pumpAndSettle();

      // Add item and go to checkout
      await tester.tap(find.byKey(const Key('add-to-cart-1')));
      await tester.pump();

      await tester.tap(find.byKey(const Key('cart-icon')));
      await tester.pumpAndSettle();

      await tester.tap(find.byKey(const Key('checkout-button')));
      await tester.pumpAndSettle();

      // Try to place order without filling fields
      await tester.tap(find.byKey(const Key('place-order')));
      await tester.pump();

      // Should show error message
      expect(find.byKey(const Key('checkout-error')), findsOneWidget);
      expect(find.text('Please fill in all fields'), findsOneWidget);

      // Fill in partial information
      await tester.enterText(find.byKey(const Key('checkout-name')), 'Test Name');

      // Try again
      await tester.tap(find.byKey(const Key('place-order')));
      await tester.pump();

      // Should still show error
      expect(find.text('Please fill in all fields'), findsOneWidget);

      // Complete all fields
      await tester.enterText(find.byKey(const Key('checkout-address')), 'Complete Address');
      await tester.enterText(find.byKey(const Key('checkout-phone')), '123-456-7890');

      // Now should succeed
      await tester.tap(find.byKey(const Key('place-order')));
      await tester.pump(const Duration(seconds: 3));
      await tester.pumpAndSettle();

      expect(find.text('Order Placed Successfully!'), findsOneWidget);
    });
  });
}
```

This example demonstrates comprehensive end-to-end testing scenarios  
including complete shopping workflows, multi-product cart management,  
search functionality, product detail navigation, error handling, and  
complex user journeys that test the entire application flow.

## Testing App Performance

Performance testing to measure app startup time, scroll performance,  
memory usage, and identifying performance bottlenecks.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';
import 'dart:math' as math;

// Performance test app with heavy operations
class PerformanceTestApp extends StatelessWidget {
  const PerformanceTestApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Performance Test App',
      theme: ThemeData.dark(),
      home: const MainScreen(),
    );
  }
}

class MainScreen extends StatefulWidget {
  const MainScreen({super.key});

  @override
  State<MainScreen> createState() => _MainScreenState();
}

class _MainScreenState extends State<MainScreen> {
  int _selectedIndex = 0;

  static const List<Widget> _pages = [
    HeavyListScreen(),
    AnimatedScreen(),
    ImageGridScreen(),
    ComplexLayoutScreen(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: IndexedStack(
        index: _selectedIndex,
        children: _pages,
      ),
      bottomNavigationBar: BottomNavigationBar(
        key: const Key('bottom-nav'),
        type: BottomNavigationBarType.fixed,
        currentIndex: _selectedIndex,
        onTap: (index) => setState(() => _selectedIndex = index),
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.list), label: 'List'),
          BottomNavigationBarItem(icon: Icon(Icons.animation), label: 'Animation'),
          BottomNavigationBarItem(icon: Icon(Icons.image), label: 'Images'),
          BottomNavigationBarItem(icon: Icon(Icons.dashboard), label: 'Layout'),
        ],
      ),
    );
  }
}

// Heavy list with complex items
class HeavyListScreen extends StatefulWidget {
  const HeavyListScreen({super.key});

  @override
  State<HeavyListScreen> createState() => _HeavyListScreenState();
}

class _HeavyListScreenState extends State<HeavyListScreen> {
  List<ComplexItem> _items = [];

  @override
  void initState() {
    super.initState();
    _generateItems();
  }

  void _generateItems() {
    _items = List.generate(1000, (index) {
      return ComplexItem(
        id: index,
        title: 'Complex Item $index',
        subtitle: 'This is item $index with some complex content',
        value: math.Random().nextDouble() * 100,
        category: ['Category A', 'Category B', 'Category C'][index % 3],
        isImportant: index % 10 == 0,
      );
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Heavy List'),
        actions: [
          IconButton(
            key: const Key('refresh-list'),
            icon: const Icon(Icons.refresh),
            onPressed: () {
              setState(() {
                _generateItems();
              });
            },
          ),
        ],
      ),
      body: ListView.builder(
        key: const Key('heavy-list'),
        itemCount: _items.length,
        itemBuilder: (context, index) {
          return ComplexListItem(
            key: Key('item-$index'),
            item: _items[index],
            onTap: () {
              // Simulate expensive operation
              final result = _performHeavyCalculation(index);
              ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(content: Text('Calculated: $result')),
              );
            },
          );
        },
      ),
    );
  }

  double _performHeavyCalculation(int seed) {
    // Simulate CPU-intensive operation
    double result = 0.0;
    for (int i = 0; i < 10000; i++) {
      result += math.sin(seed + i) * math.cos(seed + i);
    }
    return result;
  }
}

class ComplexItem {
  final int id;
  final String title;
  final String subtitle;
  final double value;
  final String category;
  final bool isImportant;

  ComplexItem({
    required this.id,
    required this.title,
    required this.subtitle,
    required this.value,
    required this.category,
    required this.isImportant,
  });
}

class ComplexListItem extends StatelessWidget {
  final ComplexItem item;
  final VoidCallback? onTap;

  const ComplexListItem({super.key, required this.item, this.onTap});

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.all(4),
      child: ListTile(
        leading: Container(
          width: 50,
          height: 50,
          decoration: BoxDecoration(
            color: item.isImportant ? Colors.red : Colors.blue,
            shape: BoxShape.circle,
          ),
          child: Center(
            child: Text(
              '${item.id}',
              style: const TextStyle(color: Colors.white, fontWeight: FontWeight.bold),
            ),
          ),
        ),
        title: Text(item.title),
        subtitle: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(item.subtitle),
            const SizedBox(height: 4),
            Row(
              children: [
                Chip(
                  label: Text(item.category),
                  materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
                ),
                const SizedBox(width: 8),
                Text('Value: ${item.value.toStringAsFixed(2)}'),
              ],
            ),
          ],
        ),
        trailing: item.isImportant
            ? const Icon(Icons.star, color: Colors.amber)
            : null,
        onTap: onTap,
      ),
    );
  }
}

// Screen with multiple simultaneous animations
class AnimatedScreen extends StatefulWidget {
  const AnimatedScreen({super.key});

  @override
  State<AnimatedScreen> createState() => _AnimatedScreenState();
}

class _AnimatedScreenState extends State<AnimatedScreen>
    with TickerProviderStateMixin {
  late List<AnimationController> _controllers;
  late List<Animation<double>> _animations;
  bool _isAnimating = false;

  @override
  void initState() {
    super.initState();
    _initializeAnimations();
  }

  void _initializeAnimations() {
    _controllers = List.generate(20, (index) {
      return AnimationController(
        duration: Duration(milliseconds: 1000 + (index * 100)),
        vsync: this,
      );
    });

    _animations = _controllers.map((controller) {
      return Tween<double>(begin: 0, end: 1).animate(
        CurvedAnimation(parent: controller, curve: Curves.easeInOut),
      );
    }).toList();
  }

  @override
  void dispose() {
    for (final controller in _controllers) {
      controller.dispose();
    }
    super.dispose();
  }

  void _startAnimations() {
    setState(() => _isAnimating = true);
    for (final controller in _controllers) {
      controller.repeat(reverse: true);
    }
  }

  void _stopAnimations() {
    setState(() => _isAnimating = false);
    for (final controller in _controllers) {
      controller.stop();
      controller.reset();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Animations'),
        actions: [
          IconButton(
            key: const Key('toggle-animations'),
            icon: Icon(_isAnimating ? Icons.stop : Icons.play_arrow),
            onPressed: _isAnimating ? _stopAnimations : _startAnimations,
          ),
        ],
      ),
      body: GridView.builder(
        key: const Key('animation-grid'),
        padding: const EdgeInsets.all(16),
        gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 4,
          crossAxisSpacing: 10,
          mainAxisSpacing: 10,
        ),
        itemCount: _animations.length,
        itemBuilder: (context, index) {
          return AnimatedBuilder(
            animation: _animations[index],
            builder: (context, child) {
              return Transform.scale(
                scale: 0.5 + (_animations[index].value * 0.5),
                child: Transform.rotate(
                  angle: _animations[index].value * 2 * math.pi,
                  child: Container(
                    decoration: BoxDecoration(
                      color: Color.lerp(
                        Colors.blue,
                        Colors.red,
                        _animations[index].value,
                      ),
                      borderRadius: BorderRadius.circular(8),
                    ),
                    child: Center(
                      child: Text(
                        '$index',
                        style: const TextStyle(
                          color: Colors.white,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                  ),
                ),
              );
            },
          );
        },
      ),
    );
  }
}

// Image grid screen for memory testing
class ImageGridScreen extends StatelessWidget {
  const ImageGridScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Image Grid'),
      ),
      body: GridView.builder(
        key: const Key('image-grid'),
        padding: const EdgeInsets.all(8),
        gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 3,
          crossAxisSpacing: 4,
          mainAxisSpacing: 4,
        ),
        itemCount: 100,
        itemBuilder: (context, index) {
          return Container(
            key: Key('image-$index'),
            decoration: BoxDecoration(
              color: Color.fromRGBO(
                (index * 50) % 255,
                (index * 80) % 255,
                (index * 120) % 255,
                1.0,
              ),
              borderRadius: BorderRadius.circular(8),
            ),
            child: Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Icon(
                    Icons.image,
                    size: 40,
                    color: Colors.white.withOpacity(0.7),
                  ),
                  const SizedBox(height: 4),
                  Text(
                    'Img $index',
                    style: const TextStyle(
                      color: Colors.white,
                      fontSize: 12,
                    ),
                  ),
                ],
              ),
            ),
          );
        },
      ),
    );
  }
}

// Complex layout screen
class ComplexLayoutScreen extends StatelessWidget {
  const ComplexLayoutScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Complex Layout'),
      ),
      body: SingleChildScrollView(
        key: const Key('complex-scroll'),
        child: Column(
          children: [
            // Header section
            Container(
              height: 200,
              decoration: const BoxDecoration(
                gradient: LinearGradient(
                  colors: [Colors.purple, Colors.blue],
                  begin: Alignment.topLeft,
                  end: Alignment.bottomRight,
                ),
              ),
              child: const Center(
                child: Text(
                  'Complex Layout Header',
                  style: TextStyle(fontSize: 24, color: Colors.white),
                ),
              ),
            ),
            
            // Cards section
            ...List.generate(10, (index) {
              return Card(
                key: Key('complex-card-$index'),
                margin: const EdgeInsets.all(8),
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    children: [
                      Row(
                        children: [
                          Container(
                            width: 60,
                            height: 60,
                            decoration: BoxDecoration(
                              color: Colors.primaries[index % Colors.primaries.length],
                              shape: BoxShape.circle,
                            ),
                            child: Center(child: Text('$index')),
                          ),
                          const SizedBox(width: 16),
                          Expanded(
                            child: Column(
                              crossAxisAlignment: CrossAxisAlignment.start,
                              children: [
                                Text(
                                  'Complex Item $index',
                                  style: const TextStyle(
                                    fontSize: 18,
                                    fontWeight: FontWeight.bold,
                                  ),
                                ),
                                const SizedBox(height: 4),
                                Text('Description for item $index'),
                              ],
                            ),
                          ),
                        ],
                      ),
                      const SizedBox(height: 16),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: [
                          ElevatedButton(
                            onPressed: () {},
                            child: const Text('Action 1'),
                          ),
                          ElevatedButton(
                            onPressed: () {},
                            child: const Text('Action 2'),
                          ),
                          ElevatedButton(
                            onPressed: () {},
                            child: const Text('Action 3'),
                          ),
                        ],
                      ),
                    ],
                  ),
                ),
              );
            }),
          ],
        ),
      ),
    );
  }
}

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();
  final binding = IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  group('Performance Tests', () {
    testWidgets('app startup performance', (WidgetTester tester) async {
      final startTime = DateTime.now();
      
      await tester.pumpWidget(const PerformanceTestApp());
      await tester.pumpAndSettle();
      
      final endTime = DateTime.now();
      final startupTime = endTime.difference(startTime);
      
      // Log startup time
      print('App startup time: ${startupTime.inMilliseconds}ms');
      
      // Verify app loads within reasonable time (adjust threshold as needed)
      expect(startupTime.inMilliseconds, lessThan(5000)); // 5 seconds
      expect(find.byType(BottomNavigationBar), findsOneWidget);
    });

    testWidgets('heavy list scroll performance', (WidgetTester tester) async {
      await tester.pumpWidget(const PerformanceTestApp());
      await tester.pumpAndSettle();

      // Navigate to list screen (already default)
      expect(find.byKey(const Key('heavy-list')), findsOneWidget);

      // Measure scroll performance
      final timeline = await binding.traceAction(() async {
        // Perform multiple scroll operations
        final listFinder = find.byKey(const Key('heavy-list'));
        
        // Scroll down multiple times
        for (int i = 0; i < 10; i++) {
          await tester.fling(listFinder, const Offset(0, -300), 1000);
          await tester.pumpAndSettle(const Duration(milliseconds: 100));
        }
        
        // Scroll back up
        for (int i = 0; i < 10; i++) {
          await tester.fling(listFinder, const Offset(0, 300), 1000);
          await tester.pumpAndSettle(const Duration(milliseconds: 100));
        }
      });

      // Extract performance metrics
      final summary = TimelineSummary.summarize(timeline);
      
      // Check frame rate (should be close to 60fps)
      final averageFrameTime = summary.averageFrameBuildTimeMillis;
      print('Average frame build time: ${averageFrameTime}ms');
      expect(averageFrameTime, lessThan(16.67)); // ~60fps threshold

      // Check for missed frames
      final missedFrames = summary.countFrames();
      print('Total frames during scroll: $missedFrames');
    });

    testWidgets('animation performance test', (WidgetTester tester) async {
      await tester.pumpWidget(const PerformanceTestApp());
      await tester.pumpAndSettle();

      // Navigate to animation screen
      await tester.tap(find.text('Animation'));
      await tester.pumpAndSettle();

      expect(find.byKey(const Key('animation-grid')), findsOneWidget);

      // Start animations and measure performance
      final timeline = await binding.traceAction(() async {
        await tester.tap(find.byKey(const Key('toggle-animations')));
        await tester.pump();

        // Let animations run for a period
        await tester.pump(const Duration(seconds: 3));

        // Stop animations
        await tester.tap(find.byKey(const Key('toggle-animations')));
        await tester.pumpAndSettle();
      });

      final summary = TimelineSummary.summarize(timeline);
      print('Animation average frame time: ${summary.averageFrameBuildTimeMillis}ms');
      
      // Animation should maintain good performance
      expect(summary.averageFrameBuildTimeMillis, lessThan(20)); // Allow some overhead for animations
    });

    testWidgets('image grid memory test', (WidgetTester tester) async {
      await tester.pumpWidget(const PerformanceTestApp());
      await tester.pumpAndSettle();

      // Navigate to image screen
      await tester.tap(find.text('Images'));
      await tester.pumpAndSettle();

      // Scroll through images to test memory usage
      final imageGrid = find.byKey(const Key('image-grid'));
      expect(imageGrid, findsOneWidget);

      // Scroll through the grid
      for (int i = 0; i < 20; i++) {
        await tester.fling(imageGrid, const Offset(0, -200), 800);
        await tester.pump(const Duration(milliseconds: 50));
      }

      await tester.pumpAndSettle();
      
      // Verify images are still loaded
      expect(find.byKey(const Key('image-0')), findsNothing); // Scrolled out of view
      expect(find.textContaining('Img'), findsWidgets); // Some images should be visible
    });

    testWidgets('complex layout performance', (WidgetTester tester) async {
      await tester.pumpWidget(const PerformanceTestApp());
      await tester.pumpAndSettle();

      // Navigate to complex layout screen
      await tester.tap(find.text('Layout'));
      await tester.pumpAndSettle();

      // Measure complex layout performance
      final timeline = await binding.traceAction(() async {
        final scrollView = find.byKey(const Key('complex-scroll'));
        
        // Scroll through complex layouts
        for (int i = 0; i < 5; i++) {
          await tester.fling(scrollView, const Offset(0, -400), 1200);
          await tester.pumpAndSettle(const Duration(milliseconds: 200));
        }
        
        // Scroll back to top
        for (int i = 0; i < 5; i++) {
          await tester.fling(scrollView, const Offset(0, 400), 1200);
          await tester.pumpAndSettle(const Duration(milliseconds: 200));
        }
      });

      final summary = TimelineSummary.summarize(timeline);
      print('Complex layout frame time: ${summary.averageFrameBuildTimeMillis}ms');
      
      // Complex layouts should still perform reasonably
      expect(summary.averageFrameBuildTimeMillis, lessThan(25));
    });

    testWidgets('navigation performance test', (WidgetTester tester) async {
      await tester.pumpWidget(const PerformanceTestApp());
      await tester.pumpAndSettle();

      // Measure performance of rapid tab switching
      final timeline = await binding.traceAction(() async {
        final tabs = ['List', 'Animation', 'Images', 'Layout'];
        
        // Switch between tabs rapidly
        for (int cycle = 0; cycle < 3; cycle++) {
          for (final tab in tabs) {
            await tester.tap(find.text(tab));
            await tester.pump();
            await tester.pump(const Duration(milliseconds: 50));
          }
        }
        
        await tester.pumpAndSettle();
      });

      final summary = TimelineSummary.summarize(timeline);
      print('Tab navigation frame time: ${summary.averageFrameBuildTimeMillis}ms');
      
      // Tab switching should be fast
      expect(summary.averageFrameBuildTimeMillis, lessThan(16.67));
    });

    testWidgets('stress test - combined operations', (WidgetTester tester) async {
      await tester.pumpWidget(const PerformanceTestApp());
      await tester.pumpAndSettle();

      // Stress test with multiple operations
      final timeline = await binding.traceAction(() async {
        // Navigate to list and interact
        await tester.tap(find.text('List'));
        await tester.pump();
        
        // Scroll and interact with list
        await tester.fling(find.byKey(const Key('heavy-list')), const Offset(0, -500), 2000);
        await tester.pumpAndSettle(const Duration(milliseconds: 100));
        
        // Tap on an item to trigger heavy calculation
        await tester.tap(find.byKey(const Key('item-10')));
        await tester.pump();
        await tester.pump(const Duration(seconds: 1)); // Wait for snackbar
        
        // Switch to animations
        await tester.tap(find.text('Animation'));
        await tester.pump();
        
        // Start animations
        await tester.tap(find.byKey(const Key('toggle-animations')));
        await tester.pump();
        await tester.pump(const Duration(milliseconds: 500));
        
        // Switch to images while animations are running
        await tester.tap(find.text('Images'));
        await tester.pump();
        
        // Quick scroll through images
        await tester.fling(find.byKey(const Key('image-grid')), const Offset(0, -300), 1500);
        await tester.pump(const Duration(milliseconds: 200));
        
        // Go back to animations and stop them
        await tester.tap(find.text('Animation'));
        await tester.pump();
        await tester.tap(find.byKey(const Key('toggle-animations')));
        await tester.pumpAndSettle();
      });

      final summary = TimelineSummary.summarize(timeline);
      print('Stress test frame time: ${summary.averageFrameBuildTimeMillis}ms');
      
      // Even under stress, should maintain reasonable performance
      expect(summary.averageFrameBuildTimeMillis, lessThan(30));
    });
  });
}
```

This example demonstrates comprehensive performance testing including  
startup time measurement, scroll performance testing, animation frame  
rate monitoring, memory usage testing, and stress testing with multiple  
simultaneous operations to identify performance bottlenecks.

## Testing Helpers and Utilities

Creating reusable test helpers, utilities, and custom matchers to  
improve test maintainability and reduce code duplication.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

// Custom test utilities and helpers
class TestHelpers {
  // Widget wrapper for consistent testing environment
  static Widget createTestApp({
    required Widget child,
    ThemeData? theme,
    List<NavigatorObserver>? navigatorObservers,
    Map<String, WidgetBuilder>? routes,
  }) {
    return MaterialApp(
      theme: theme ?? ThemeData.light(),
      navigatorObservers: navigatorObservers ?? [],
      routes: routes ?? {},
      home: Scaffold(body: child),
    );
  }

  // Helper for finding widgets with timeout
  static Future<void> waitForWidget(
    WidgetTester tester,
    Finder finder, {
    Duration timeout = const Duration(seconds: 5),
    Duration interval = const Duration(milliseconds: 100),
  }) async {
    final endTime = DateTime.now().add(timeout);
    
    while (DateTime.now().isBefore(endTime)) {
      if (tester.any(finder)) {
        return;
      }
      await tester.pump(interval);
    }
    
    throw TimeoutException('Widget not found within timeout', timeout);
  }

  // Helper for entering text with delay (useful for testing input validation)
  static Future<void> enterTextSlowly(
    WidgetTester tester,
    Finder finder,
    String text, {
    Duration delay = const Duration(milliseconds: 100),
  }) async {
    for (int i = 0; i < text.length; i++) {
      await tester.enterText(finder, text.substring(0, i + 1));
      await tester.pump(delay);
    }
  }

  // Helper for scrolling to find a widget
  static Future<void> scrollToWidget(
    WidgetTester tester,
    Finder scrollable,
    Finder target, {
    double delta = 300.0,
    int maxAttempts = 10,
  }) async {
    for (int attempt = 0; attempt < maxAttempts; attempt++) {
      if (tester.any(target)) {
        return;
      }
      
      await tester.fling(scrollable, Offset(0, -delta), 1000);
      await tester.pumpAndSettle();
    }
    
    throw Exception('Could not scroll to target widget');
  }

  // Helper for taking screenshots during tests
  static Future<void> takeScreenshot(
    WidgetTester tester,
    String name, {
    String? description,
  }) async {
    // In a real implementation, you might save actual screenshots
    print('📸 Screenshot: $name${description != null ? ' - $description' : ''}');
    await tester.pump();
  }

  // Helper for simulating network delays
  static Future<void> simulateNetworkDelay({
    Duration min = const Duration(milliseconds: 500),
    Duration max = const Duration(milliseconds: 2000),
  }) async {
    final delay = Duration(
      milliseconds: min.inMilliseconds + 
          (max.inMilliseconds - min.inMilliseconds) * (0.5), // Could use Random here
    );
    await Future.delayed(delay);
  }

  // Helper for verifying widget properties
  static void verifyWidgetProperties<T extends Widget>(
    WidgetTester tester,
    Finder finder,
    Map<String, dynamic> expectedProperties,
  ) {
    final widget = tester.widget<T>(finder);
    final widgetProps = _getWidgetProperties(widget);
    
    expectedProperties.forEach((property, expectedValue) {
      final actualValue = widgetProps[property];
      expect(actualValue, equals(expectedValue), 
          reason: 'Property $property mismatch');
    });
  }

  static Map<String, dynamic> _getWidgetProperties(Widget widget) {
    // This would be implemented based on the specific widget type
    // For demonstration, returning a simple map
    if (widget is Text) {
      return {
        'data': widget.data,
        'style': widget.style,
        'textAlign': widget.textAlign,
      };
    } else if (widget is ElevatedButton) {
      return {
        'onPressed': widget.onPressed,
        'style': widget.style,
      };
    }
    return {};
  }
}

// Custom matchers for more expressive tests
class CustomMatchers {
  // Matcher for checking if a widget is visible on screen
  static Matcher isVisible() {
    return predicate<Finder>((finder) {
      return finder.evaluate().isNotEmpty;
    }, 'is visible on screen');
  }

  // Matcher for checking text content with case insensitive matching
  static Matcher containsTextIgnoreCase(String text) {
    return predicate<Widget>((widget) {
      if (widget is Text) {
        return widget.data?.toLowerCase().contains(text.toLowerCase()) ?? false;
      }
      return false;
    }, 'contains text "$text" (case insensitive)');
  }

  // Matcher for checking widget colors
  static Matcher hasColor(Color expectedColor) {
    return predicate<Widget>((widget) {
      if (widget is Container) {
        final decoration = widget.decoration;
        if (decoration is BoxDecoration) {
          return decoration.color == expectedColor;
        }
      }
      return false;
    }, 'has color $expectedColor');
  }

  // Matcher for checking list length
  static Matcher hasLength(int expectedLength) {
    return predicate<List>((list) {
      return list.length == expectedLength;
    }, 'has length $expectedLength');
  }

  // Matcher for checking if a number is within range
  static Matcher isWithinRange(num min, num max) {
    return predicate<num>((value) {
      return value >= min && value <= max;
    }, 'is between $min and $max');
  }
}

// Test data factory for creating consistent test objects
class TestDataFactory {
  static List<Map<String, dynamic>> createSampleUsers(int count) {
    return List.generate(count, (index) => {
      'id': 'user_$index',
      'name': 'Test User $index',
      'email': 'user$index@example.com',
      'age': 20 + (index % 50),
      'isActive': index % 2 == 0,
    });
  }

  static List<Map<String, dynamic>> createSampleProducts(int count) {
    final categories = ['Electronics', 'Clothing', 'Books', 'Home', 'Sports'];
    
    return List.generate(count, (index) => {
      'id': 'product_$index',
      'name': 'Product $index',
      'price': 10.0 + (index * 5.5),
      'category': categories[index % categories.length],
      'inStock': index % 3 != 0,
      'rating': 1.0 + (index % 5),
    });
  }

  static Map<String, dynamic> createSampleOrder({
    String? orderId,
    int? itemCount,
    double? total,
  }) {
    return {
      'id': orderId ?? 'order_${DateTime.now().millisecondsSinceEpoch}',
      'items': createSampleProducts(itemCount ?? 3),
      'total': total ?? 99.99,
      'status': 'pending',
      'createdAt': DateTime.now().toIso8601String(),
    };
  }
}

// Mock response generator for API testing
class MockResponseGenerator {
  static Map<String, dynamic> successResponse(dynamic data) {
    return {
      'success': true,
      'data': data,
      'timestamp': DateTime.now().toIso8601String(),
    };
  }

  static Map<String, dynamic> errorResponse(String message, {int? code}) {
    return {
      'success': false,
      'error': {
        'message': message,
        'code': code ?? 400,
      },
      'timestamp': DateTime.now().toIso8601String(),
    };
  }

  static Map<String, dynamic> paginatedResponse(
    List<dynamic> data, {
    int page = 1,
    int perPage = 10,
    int? total,
  }) {
    final totalItems = total ?? data.length;
    final totalPages = (totalItems / perPage).ceil();
    
    return successResponse({
      'items': data,
      'pagination': {
        'page': page,
        'perPage': perPage,
        'total': totalItems,
        'totalPages': totalPages,
        'hasNext': page < totalPages,
        'hasPrev': page > 1,
      },
    });
  }
}

// Test execution wrapper with common setup/teardown
class TestRunner {
  static Future<void> runTest(
    String description,
    Future<void> Function(WidgetTester tester) testFunction,
    WidgetTester tester, {
    Duration? timeout,
    bool takeScreenshots = false,
    Map<String, dynamic>? setup,
  }) async {
    print('🧪 Starting test: $description');
    final startTime = DateTime.now();
    
    try {
      // Setup phase
      if (setup != null) {
        print('⚙️ Setting up test environment...');
        // Apply any setup configuration
      }
      
      // Run the actual test
      await testFunction(tester);
      
      // Success
      final duration = DateTime.now().difference(startTime);
      print('✅ Test completed successfully in ${duration.inMilliseconds}ms');
      
      if (takeScreenshots) {
        await TestHelpers.takeScreenshot(tester, description);
      }
      
    } catch (error) {
      final duration = DateTime.now().difference(startTime);
      print('❌ Test failed after ${duration.inMilliseconds}ms: $error');
      
      if (takeScreenshots) {
        await TestHelpers.takeScreenshot(tester, '$description-FAILED');
      }
      
      rethrow;
    }
  }
}

// Example widget to demonstrate utilities
class UtilityDemoWidget extends StatefulWidget {
  const UtilityDemoWidget({super.key});

  @override
  State<UtilityDemoWidget> createState() => _UtilityDemoWidgetState();
}

class _UtilityDemoWidgetState extends State<UtilityDemoWidget> {
  final _controller = TextEditingController();
  Color _backgroundColor = Colors.blue;
  List<String> _items = [];
  bool _isLoading = false;

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  Future<void> _loadData() async {
    setState(() => _isLoading = true);
    
    await TestHelpers.simulateNetworkDelay();
    
    setState(() {
      _items = ['Item 1', 'Item 2', 'Item 3', 'Item 4', 'Item 5'];
      _isLoading = false;
    });
  }

  void _changeColor() {
    setState(() {
      _backgroundColor = _backgroundColor == Colors.blue 
          ? Colors.green 
          : Colors.blue;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      key: const Key('main-container'),
      color: _backgroundColor,
      padding: const EdgeInsets.all(16),
      child: Column(
        children: [
          TextField(
            key: const Key('text-input'),
            controller: _controller,
            decoration: const InputDecoration(
              labelText: 'Enter text',
              border: OutlineInputBorder(),
            ),
          ),
          const SizedBox(height: 16),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: [
              ElevatedButton(
                key: const Key('load-button'),
                onPressed: _isLoading ? null : _loadData,
                child: const Text('Load Data'),
              ),
              ElevatedButton(
                key: const Key('color-button'),
                onPressed: _changeColor,
                child: const Text('Change Color'),
              ),
            ],
          ),
          const SizedBox(height: 16),
          Expanded(
            child: _isLoading
                ? const Center(
                    child: CircularProgressIndicator(key: Key('loading-indicator')),
                  )
                : ListView.builder(
                    key: const Key('items-list'),
                    itemCount: _items.length,
                    itemBuilder: (context, index) {
                      return ListTile(
                        key: Key('item-$index'),
                        title: Text(_items[index]),
                        onTap: () {
                          ScaffoldMessenger.of(context).showSnackBar(
                            SnackBar(content: Text('Tapped ${_items[index]}')),
                          );
                        },
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }
}

void main() {
  group('Test Utilities and Helpers', () {
    testWidgets('TestHelpers.createTestApp creates proper environment', (WidgetTester tester) async {
      await tester.pumpWidget(
        TestHelpers.createTestApp(
          child: const Text('Test Widget'),
          theme: ThemeData.dark(),
        ),
      );

      expect(find.text('Test Widget'), findsOneWidget);
      expect(find.byType(MaterialApp), findsOneWidget);
      expect(find.byType(Scaffold), findsOneWidget);
    });

    testWidgets('TestHelpers.waitForWidget waits for widget to appear', (WidgetTester tester) async {
      bool showWidget = false;

      await tester.pumpWidget(
        TestHelpers.createTestApp(
          child: StatefulBuilder(
            builder: (context, setState) {
              return Column(
                children: [
                  ElevatedButton(
                    onPressed: () => setState(() => showWidget = true),
                    child: const Text('Show Widget'),
                  ),
                  if (showWidget) const Text('Delayed Widget', key: Key('delayed-widget')),
                ],
              );
            },
          ),
        ),
      );

      // Trigger widget appearance
      await tester.tap(find.text('Show Widget'));
      
      // Wait for widget to appear
      await TestHelpers.waitForWidget(
        tester,
        find.byKey(const Key('delayed-widget')),
        timeout: const Duration(seconds: 2),
      );

      expect(find.text('Delayed Widget'), findsOneWidget);
    });

    testWidgets('TestHelpers.enterTextSlowly simulates gradual input', (WidgetTester tester) async {
      await tester.pumpWidget(
        TestHelpers.createTestApp(
          child: const TextField(key: Key('slow-input')),
        ),
      );

      await TestHelpers.enterTextSlowly(
        tester,
        find.byKey(const Key('slow-input')),
        'Hello',
        delay: const Duration(milliseconds: 50),
      );

      expect(find.text('Hello'), findsOneWidget);
    });

    testWidgets('CustomMatchers.containsTextIgnoreCase works correctly', (WidgetTester tester) async {
      await tester.pumpWidget(
        TestHelpers.createTestApp(
          child: const Text('Hello World'),
        ),
      );

      final textWidget = tester.widget(find.text('Hello World'));
      expect(textWidget, CustomMatchers.containsTextIgnoreCase('HELLO'));
      expect(textWidget, CustomMatchers.containsTextIgnoreCase('world'));
    });

    testWidgets('CustomMatchers.hasColor validates container colors', (WidgetTester tester) async {
      await tester.pumpWidget(
        TestHelpers.createTestApp(
          child: Container(
            decoration: const BoxDecoration(color: Colors.red),
            child: const Text('Colored Container'),
          ),
        ),
      );

      final containerWidget = tester.widget(find.byType(Container).first);
      expect(containerWidget, CustomMatchers.hasColor(Colors.red));
    });

    testWidgets('TestDataFactory creates consistent test data', (WidgetTester tester) async {
      final users = TestDataFactory.createSampleUsers(3);
      final products = TestDataFactory.createSampleProducts(2);
      final order = TestDataFactory.createSampleOrder();

      expect(users, CustomMatchers.hasLength(3));
      expect(products, CustomMatchers.hasLength(2));
      expect(users.first['name'], equals('Test User 0'));
      expect(products.first['category'], equals('Electronics'));
      expect(order['status'], equals('pending'));
    });

    testWidgets('MockResponseGenerator creates proper response formats', (WidgetTester tester) async {
      final successResp = MockResponseGenerator.successResponse({'message': 'OK'});
      final errorResp = MockResponseGenerator.errorResponse('Not found', code: 404);
      final pagedResp = MockResponseGenerator.paginatedResponse([1, 2, 3], page: 1, perPage: 2);

      expect(successResp['success'], isTrue);
      expect(errorResp['success'], isFalse);
      expect(errorResp['error']['code'], equals(404));
      expect(pagedResp['data']['pagination']['totalPages'], equals(2));
    });

    testWidgets('complete utility demo test', (WidgetTester tester) async {
      await TestRunner.runTest(
        'Utility Demo Widget Test',
        (tester) async {
          await tester.pumpWidget(
            TestHelpers.createTestApp(child: const UtilityDemoWidget()),
          );

          // Test initial state
          expect(find.byKey(const Key('main-container')), findsOneWidget);
          final container = tester.widget<Container>(find.byKey(const Key('main-container')));
          expect(container.color, equals(Colors.blue));

          // Test text input
          await TestHelpers.enterTextSlowly(
            tester,
            find.byKey(const Key('text-input')),
            'Test Input',
          );

          // Test color change
          await tester.tap(find.byKey(const Key('color-button')));
          await tester.pump();

          final updatedContainer = tester.widget<Container>(find.byKey(const Key('main-container')));
          expect(updatedContainer.color, equals(Colors.green));

          // Test data loading
          await tester.tap(find.byKey(const Key('load-button')));
          await tester.pump();

          // Wait for loading to complete
          await TestHelpers.waitForWidget(
            tester,
            find.byKey(const Key('items-list')),
            timeout: const Duration(seconds: 3),
          );

          // Verify data loaded
          expect(find.text('Item 1'), findsOneWidget);
          expect(find.text('Item 5'), findsOneWidget);

          // Test item interaction
          await tester.tap(find.byKey(const Key('item-2')));
          await tester.pump();

          expect(find.text('Tapped Item 3'), findsOneWidget);

        },
        tester,
        takeScreenshots: true,
      );
    });

    testWidgets('error handling in utilities', (WidgetTester tester) async {
      await tester.pumpWidget(
        TestHelpers.createTestApp(child: const Text('Simple Widget')),
      );

      // Test timeout in waitForWidget
      expect(
        () => TestHelpers.waitForWidget(
          tester,
          find.byKey(const Key('non-existent')),
          timeout: const Duration(milliseconds: 100),
        ),
        throwsA(isA<TimeoutException>()),
      );
    });

    testWidgets('performance monitoring with utilities', (WidgetTester tester) async {
      await tester.pumpWidget(
        TestHelpers.createTestApp(child: const UtilityDemoWidget()),
      );

      final startTime = DateTime.now();

      // Perform multiple operations
      await tester.tap(find.byKey(const Key('color-button')));
      await tester.pump();

      await TestHelpers.enterTextSlowly(
        tester,
        find.byKey(const Key('text-input')),
        'Performance Test',
        delay: const Duration(milliseconds: 10),
      );

      await tester.tap(find.byKey(const Key('load-button')));
      await TestHelpers.waitForWidget(
        tester,
        find.byKey(const Key('items-list')),
      );

      final duration = DateTime.now().difference(startTime);
      print('Operations completed in ${duration.inMilliseconds}ms');
      
      // Should complete within reasonable time
      expect(duration.inSeconds, lessThan(10));
    });
  });
}
```

This example demonstrates comprehensive testing utilities including custom  
test helpers, matchers, data factories, mock generators, and test runners  
that improve test maintainability and provide reusable testing  
infrastructure.

## Advanced Test Configuration

Advanced testing setup with custom test environments, CI/CD integration,  
code coverage analysis, and testing best practices.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';

// Test configuration class
class TestConfig {
  static const bool isCI = bool.fromEnvironment('CI', defaultValue: false);
  static const String testEnvironment = String.fromEnvironment('TEST_ENV', defaultValue: 'development');
  static const bool enablePerformanceTests = bool.fromEnvironment('PERFORMANCE_TESTS', defaultValue: false);
  static const int defaultTimeout = int.fromEnvironment('TEST_TIMEOUT', defaultValue: 30000);
  static const bool verboseLogging = bool.fromEnvironment('VERBOSE_TESTS', defaultValue: false);
  
  static Duration get timeoutDuration => Duration(milliseconds: defaultTimeout);
  
  static void log(String message) {
    if (verboseLogging) {
      print('🔍 [TEST] $message');
    }
  }
}

// Test environment setup
class TestEnvironment {
  static late Map<String, dynamic> _config;
  static bool _isInitialized = false;

  static Future<void> initialize() async {
    if (_isInitialized) return;
    
    TestConfig.log('Initializing test environment: ${TestConfig.testEnvironment}');
    
    _config = await _loadConfig();
    await _setupTestData();
    await _configureServices();
    
    _isInitialized = true;
    TestConfig.log('Test environment initialized successfully');
  }

  static Future<Map<String, dynamic>> _loadConfig() async {
    // In a real app, this might load from a config file
    switch (TestConfig.testEnvironment) {
      case 'ci':
        return {
          'apiUrl': 'https://test-api.example.com',
          'enableMocking': true,
          'logLevel': 'error',
          'animationDuration': 0, // Disable animations in CI
        };
      case 'development':
        return {
          'apiUrl': 'https://dev-api.example.com',
          'enableMocking': false,
          'logLevel': 'debug',
          'animationDuration': 200,
        };
      default:
        return {
          'apiUrl': 'https://staging-api.example.com',
          'enableMocking': true,
          'logLevel': 'info',
          'animationDuration': 100,
        };
    }
  }

  static Future<void> _setupTestData() async {
    TestConfig.log('Setting up test data...');
    // Initialize test database or mock data
    await Future.delayed(const Duration(milliseconds: 100));
  }

  static Future<void> _configureServices() async {
    TestConfig.log('Configuring services...');
    // Configure HTTP client, database, etc.
    await Future.delayed(const Duration(milliseconds: 100));
  }

  static T getConfig<T>(String key, T defaultValue) {
    return _config[key] as T? ?? defaultValue;
  }

  static Future<void> cleanup() async {
    TestConfig.log('Cleaning up test environment...');
    // Cleanup test data, close connections, etc.
    _isInitialized = false;
  }
}

// Custom test wrapper with environment setup
class AdvancedTestWidget extends StatelessWidget {
  final Widget child;

  const AdvancedTestWidget({super.key, required this.child});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        // Configure theme for testing
        animations: TestEnvironment.getConfig('animationDuration', 200) == 0
            ? null // Disable animations
            : const PageTransitionsTheme(),
      ),
      home: child,
    );
  }
}

// Test suite runner with advanced configuration
class TestSuiteRunner {
  static final List<String> _executedTests = [];
  static final Map<String, TestResult> _testResults = {};

  static Future<void> runTestSuite(
    String suiteName,
    Map<String, Future<void> Function(WidgetTester)> tests,
    WidgetTester tester, {
    bool parallel = false,
    List<String>? tags,
    bool skipIfCI = false,
  }) async {
    if (skipIfCI && TestConfig.isCI) {
      TestConfig.log('Skipping test suite $suiteName (CI environment)');
      return;
    }

    TestConfig.log('Running test suite: $suiteName');
    final suiteStartTime = DateTime.now();

    await TestEnvironment.initialize();

    try {
      if (parallel && !TestConfig.isCI) {
        await _runTestsInParallel(suiteName, tests, tester);
      } else {
        await _runTestsSequentially(suiteName, tests, tester);
      }
    } finally {
      await TestEnvironment.cleanup();
    }

    final suiteDuration = DateTime.now().difference(suiteStartTime);
    _generateTestReport(suiteName, suiteDuration);
  }

  static Future<void> _runTestsSequentially(
    String suiteName,
    Map<String, Future<void> Function(WidgetTester)> tests,
    WidgetTester tester,
  ) async {
    for (final entry in tests.entries) {
      await _runSingleTest(suiteName, entry.key, entry.value, tester);
    }
  }

  static Future<void> _runTestsInParallel(
    String suiteName,
    Map<String, Future<void> Function(WidgetTester)> tests,
    WidgetTester tester,
  ) async {
    // Note: In reality, widget tests can't run in parallel with the same tester
    // This is a conceptual example of how you might structure parallel execution
    TestConfig.log('Running ${tests.length} tests in sequence (widget tests cannot be truly parallel)');
    await _runTestsSequentially(suiteName, tests, tester);
  }

  static Future<void> _runSingleTest(
    String suiteName,
    String testName,
    Future<void> Function(WidgetTester) testFunction,
    WidgetTester tester,
  ) async {
    final fullTestName = '$suiteName::$testName';
    TestConfig.log('Executing test: $fullTestName');
    
    final startTime = DateTime.now();
    try {
      await testFunction(tester).timeout(TestConfig.timeoutDuration);
      
      final duration = DateTime.now().difference(startTime);
      _testResults[fullTestName] = TestResult.success(duration);
      _executedTests.add(fullTestName);
      
      TestConfig.log('✅ Test passed: $fullTestName (${duration.inMilliseconds}ms)');
      
    } catch (error, stackTrace) {
      final duration = DateTime.now().difference(startTime);
      _testResults[fullTestName] = TestResult.failure(duration, error.toString());
      
      TestConfig.log('❌ Test failed: $fullTestName (${duration.inMilliseconds}ms)');
      TestConfig.log('Error: $error');
      if (TestConfig.verboseLogging) {
        TestConfig.log('Stack trace: $stackTrace');
      }
      
      rethrow;
    }
  }

  static void _generateTestReport(String suiteName, Duration totalDuration) {
    final suiteResults = _testResults.entries
        .where((entry) => entry.key.startsWith(suiteName))
        .toList();

    final passed = suiteResults.where((result) => result.value.passed).length;
    final failed = suiteResults.where((result) => !result.value.passed).length;
    
    print('\n📊 Test Suite Report: $suiteName');
    print('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━');
    print('Total Duration: ${totalDuration.inMilliseconds}ms');
    print('Tests Run: ${suiteResults.length}');
    print('Passed: $passed');
    print('Failed: $failed');
    print('Success Rate: ${(passed / suiteResults.length * 100).toStringAsFixed(1)}%');
    
    if (failed > 0) {
      print('\nFailed Tests:');
      for (final result in suiteResults.where((r) => !r.value.passed)) {
        print('  ❌ ${result.key}: ${result.value.error}');
      }
    }
    
    print('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n');
  }

  static Map<String, TestResult> getResults() => Map.unmodifiable(_testResults);
  static List<String> getExecutedTests() => List.unmodifiable(_executedTests);
  
  static void reset() {
    _executedTests.clear();
    _testResults.clear();
  }
}

class TestResult {
  final Duration duration;
  final bool passed;
  final String? error;

  TestResult.success(this.duration) : passed = true, error = null;
  TestResult.failure(this.duration, this.error) : passed = false;
}

// Coverage analysis helper
class CoverageAnalyzer {
  static final Set<String> _coveredComponents = {};
  static final Set<String> _allComponents = {
    'LoginScreen',
    'HomeScreen',
    'ProfileScreen',
    'SettingsScreen',
    'CustomButton',
    'FormField',
    'LoadingIndicator',
  };

  static void markCovered(String component) {
    _coveredComponents.add(component);
    TestConfig.log('Component covered: $component');
  }

  static double getCoveragePercentage() {
    return _coveredComponents.length / _allComponents.length;
  }

  static List<String> getUncoveredComponents() {
    return _allComponents.difference(_coveredComponents).toList();
  }

  static void generateCoverageReport() {
    final coverage = getCoveragePercentage();
    final uncovered = getUncoveredComponents();
    
    print('\n📈 Code Coverage Report');
    print('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━');
    print('Overall Coverage: ${(coverage * 100).toStringAsFixed(1)}%');
    print('Covered Components: ${_coveredComponents.length}/${_allComponents.length}');
    
    if (uncovered.isNotEmpty) {
      print('\nUncovered Components:');
      for (final component in uncovered) {
        print('  ⚠️  $component');
      }
    }
    
    print('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n');
  }

  static void reset() {
    _coveredComponents.clear();
  }
}

// Sample components for testing
class SampleComponent extends StatelessWidget {
  final String name;
  final Widget child;

  const SampleComponent({super.key, required this.name, required this.child});

  @override
  Widget build(BuildContext context) {
    // Mark component as covered when built
    CoverageAnalyzer.markCovered(name);
    return child;
  }
}

class TestableApp extends StatefulWidget {
  const TestableApp({super.key});

  @override
  State<TestableApp> createState() => _TestableAppState();
}

class _TestableAppState extends State<TestableApp> {
  int _currentIndex = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Advanced Test App'),
      ),
      body: IndexedStack(
        index: _currentIndex,
        children: [
          SampleComponent(
            name: 'HomeScreen',
            child: const Center(
              child: Text('Home Screen', key: Key('home-content')),
            ),
          ),
          SampleComponent(
            name: 'ProfileScreen', 
            child: const Center(
              child: Text('Profile Screen', key: Key('profile-content')),
            ),
          ),
          SampleComponent(
            name: 'SettingsScreen',
            child: const Center(
              child: Text('Settings Screen', key: Key('settings-content')),
            ),
          ),
        ],
      ),
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _currentIndex,
        onTap: (index) => setState(() => _currentIndex = index),
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
          BottomNavigationBarItem(icon: Icon(Icons.person), label: 'Profile'),
          BottomNavigationBarItem(icon: Icon(Icons.settings), label: 'Settings'),
        ],
      ),
    );
  }
}

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  // Global test setup
  setUpAll(() async {
    TestConfig.log('Setting up global test environment...');
    await TestEnvironment.initialize();
    
    // Configure test-specific settings
    if (TestConfig.isCI) {
      TestConfig.log('Running in CI environment - optimizing for speed');
    }
  });

  tearDownAll(() async {
    TestConfig.log('Tearing down global test environment...');
    await TestEnvironment.cleanup();
    
    // Generate final reports
    CoverageAnalyzer.generateCoverageReport();
    TestConfig.log('All tests completed');
  });

  group('Advanced Configuration Tests', () {
    setUp(() {
      TestSuiteRunner.reset();
      CoverageAnalyzer.reset();
    });

    testWidgets('environment configuration test', (WidgetTester tester) async {
      await TestEnvironment.initialize();
      
      expect(TestEnvironment.getConfig('apiUrl', ''), isNotEmpty);
      expect(TestEnvironment.getConfig('logLevel', 'info'), isIn(['debug', 'info', 'error']));
      
      TestConfig.log('Environment configuration validated');
    });

    testWidgets('test suite execution with coverage', (WidgetTester tester) async {
      await TestSuiteRunner.runTestSuite(
        'Sample Test Suite',
        {
          'navigation_test': (tester) async {
            await tester.pumpWidget(const AdvancedTestWidget(child: TestableApp()));
            await tester.pumpAndSettle();
            
            // Test navigation
            expect(find.byKey(const Key('home-content')), findsOneWidget);
            
            await tester.tap(find.text('Profile'));
            await tester.pumpAndSettle();
            
            expect(find.byKey(const Key('profile-content')), findsOneWidget);
            
            await tester.tap(find.text('Settings'));
            await tester.pumpAndSettle();
            
            expect(find.byKey(const Key('settings-content')), findsOneWidget);
          },
          
          'component_rendering_test': (tester) async {
            await tester.pumpWidget(
              const AdvancedTestWidget(
                child: SampleComponent(
                  name: 'CustomButton',
                  child: ElevatedButton(
                    onPressed: null,
                    child: Text('Test Button'),
                  ),
                ),
              ),
            );
            
            expect(find.text('Test Button'), findsOneWidget);
            expect(find.byType(ElevatedButton), findsOneWidget);
          },
          
          'performance_aware_test': (tester) async {
            final startTime = DateTime.now();
            
            await tester.pumpWidget(const AdvancedTestWidget(child: TestableApp()));
            await tester.pumpAndSettle();
            
            // Perform rapid navigation
            for (int i = 0; i < 10; i++) {
              await tester.tap(find.text('Profile'));
              await tester.pump();
              await tester.tap(find.text('Home'));
              await tester.pump();
            }
            
            await tester.pumpAndSettle();
            
            final duration = DateTime.now().difference(startTime);
            expect(duration.inMilliseconds, lessThan(5000)); // Should complete quickly
            
            TestConfig.log('Performance test completed in ${duration.inMilliseconds}ms');
          },
        },
        tester,
      );

      // Verify test suite ran successfully
      final results = TestSuiteRunner.getResults();
      expect(results.length, equals(3));
      expect(results.values.where((r) => r.passed).length, equals(3));
    });

    testWidgets('coverage analysis test', (WidgetTester tester) async {
      await tester.pumpWidget(const AdvancedTestWidget(child: TestableApp()));
      await tester.pumpAndSettle();
      
      // Navigate through app to increase coverage
      await tester.tap(find.text('Profile'));
      await tester.pumpAndSettle();
      
      await tester.tap(find.text('Settings'));
      await tester.pumpAndSettle();
      
      // Check coverage
      final coverage = CoverageAnalyzer.getCoveragePercentage();
      expect(coverage, greaterThan(0.3)); // Should have covered at least 30%
      
      final coveredComponents = CoverageAnalyzer.getUncoveredComponents();
      TestConfig.log('Uncovered components: $coveredComponents');
    });

    testWidgets('error handling and recovery test', (WidgetTester tester) async {
      bool errorHandled = false;
      
      try {
        await TestSuiteRunner.runTestSuite(
          'Error Test Suite',
          {
            'failing_test': (tester) async {
              throw Exception('Intentional test failure');
            },
            'recovery_test': (tester) async {
              errorHandled = true;
              // This test should still run despite previous failure
              expect(true, isTrue);
            },
          },
          tester,
        );
      } catch (e) {
        // Expected to catch the failing test
      }
      
      // Verify error was handled and recovery test ran
      expect(errorHandled, isTrue);
      
      final results = TestSuiteRunner.getResults();
      expect(results.length, equals(2));
      expect(results.values.where((r) => !r.passed).length, equals(1)); // One failure
      expect(results.values.where((r) => r.passed).length, equals(1)); // One success
    });

    testWidgets('conditional test execution', (WidgetTester tester) async {
      // Test that should skip in CI
      if (!TestConfig.isCI) {
        await tester.pumpWidget(const AdvancedTestWidget(child: TestableApp()));
        await tester.pumpAndSettle();
        
        // Perform some expensive operation that we don't want in CI
        await Future.delayed(const Duration(milliseconds: 100));
        
        expect(find.byType(TestableApp), findsOneWidget);
        TestConfig.log('Non-CI test executed');
      } else {
        TestConfig.log('Skipped expensive test in CI environment');
      }
    });

    testWidgets('test timeout configuration', (WidgetTester tester) async {
      final startTime = DateTime.now();
      
      try {
        await Future.delayed(const Duration(milliseconds: 100)).timeout(
          const Duration(milliseconds: 50),
        );
        fail('Should have timed out');
      } catch (e) {
        expect(e, isA<TimeoutException>());
      }
      
      final duration = DateTime.now().difference(startTime);
      expect(duration.inMilliseconds, lessThan(100)); // Should timeout before completing
    });

    testWidgets('comprehensive integration test', (WidgetTester tester) async {
      // This test combines multiple aspects of advanced testing
      await TestSuiteRunner.runTestSuite(
        'Comprehensive Suite',
        {
          'full_app_flow': (tester) async {
            // Initialize with coverage tracking
            await tester.pumpWidget(const AdvancedTestWidget(child: TestableApp()));
            await tester.pumpAndSettle();
            
            // Navigate through all screens
            final screens = ['Profile', 'Settings', 'Home'];
            for (final screen in screens) {
              await tester.tap(find.text(screen));
              await tester.pumpAndSettle();
              
              // Verify navigation worked
              expect(find.text('$screen Screen'), findsOneWidget);
              TestConfig.log('Navigated to $screen');
            }
            
            // Check performance
            final startTime = DateTime.now();
            for (int i = 0; i < 5; i++) {
              await tester.tap(find.text('Profile'));
              await tester.pump();
              await tester.tap(find.text('Home'));
              await tester.pump();
            }
            final duration = DateTime.now().difference(startTime);
            
            expect(duration.inMilliseconds, lessThan(1000));
            
            // Verify coverage improved
            final coverage = CoverageAnalyzer.getCoveragePercentage();
            expect(coverage, greaterThan(0.4));
            
            TestConfig.log('Comprehensive test completed successfully');
          },
        },
        tester,
      );
      
      // Final verification
      final results = TestSuiteRunner.getResults();
      final allPassed = results.values.every((result) => result.passed);
      expect(allPassed, isTrue);
      
      // Generate final reports
      CoverageAnalyzer.generateCoverageReport();
    });
  });
}
```

This final example demonstrates advanced testing configuration including  
environment setup, test suite management, coverage analysis, conditional  
test execution, performance monitoring, error handling, and comprehensive  
reporting for professional Flutter testing workflows.

---

## Summary

This comprehensive guide covers 25 essential Flutter testing examples:

**Unit Testing (8 examples):**  
- Basic unit test setup and organization  
- Testing functions, methods, and classes with dependencies  
- Mocking external services and APIs  
- Testing asynchronous operations and error scenarios  
- Using fixtures, test data, and parameterized tests  

**Widget Testing (8 examples):**  
- Basic widget testing and rendering verification  
- Testing user interactions and form inputs  
- Navigation testing between screens  
- Testing stateful widgets and custom components  
- Animation testing and performance verification  

**Integration Testing (7 examples):**  
- End-to-end user workflows and complete app testing  
- Performance testing and bottleneck identification  
- Testing with external services and platform features  
- Golden file testing and accessibility verification  

**Testing Utilities (2 examples):**  
- Reusable test helpers, matchers, and data factories  
- Advanced configuration, coverage analysis, and CI/CD integration  

Each example builds upon previous concepts while demonstrating real-world  
testing scenarios. The examples follow Flutter and Dart best practices,  
use descriptive test names, and include proper error handling and edge  
case coverage to ensure robust, maintainable test suites.
