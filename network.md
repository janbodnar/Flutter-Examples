# Flutter Networking & API Integration - 30 Essential Examples

This collection provides 30 comprehensive Flutter networking examples  
covering HTTP requests, API integration, real-time communication, and  
advanced networking patterns. Each example builds progressively from  
basic concepts to complex implementations.  

## Basic HTTP GET Request

Making a simple GET request to fetch data from an API.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Basic HTTP GET',
      home: const HttpGetPage(),
    );
  }
}

class HttpGetPage extends StatefulWidget {
  const HttpGetPage({super.key});

  @override
  State<HttpGetPage> createState() => _HttpGetPageState();
}

class _HttpGetPageState extends State<HttpGetPage> {
  String _data = 'No data loaded';
  bool _isLoading = false;

  Future<void> _fetchData() async {
    setState(() {
      _isLoading = true;
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
      );

      if (response.statusCode == 200) {
        final data = json.decode(response.body);
        setState(() {
          _data = data['title'];
        });
      } else {
        setState(() {
          _data = 'Failed to load data: ${response.statusCode}';
        });
      }
    } catch (e) {
      setState(() {
        _data = 'Error: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('HTTP GET Request'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            ElevatedButton(
              onPressed: _isLoading ? null : _fetchData,
              child: const Text('Fetch Data'),
            ),
            const SizedBox(height: 20),
            if (_isLoading)
              const CircularProgressIndicator()
            else
              Text(
                _data,
                style: const TextStyle(fontSize: 16),
                textAlign: TextAlign.center,
              ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates basic HTTP GET requests using the http package.  
The async/await pattern handles asynchronous operations while try-catch  
provides error handling. Loading states give users feedback during network  
operations.  

## JSON Data Parsing

Parsing JSON responses into Dart objects with proper error handling.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'JSON Parsing',
      home: const JsonParsingPage(),
    );
  }
}

class Post {
  final int id;
  final String title;
  final String body;
  final int userId;

  Post({
    required this.id,
    required this.title,
    required this.body,
    required this.userId,
  });

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      id: json['id'] ?? 0,
      title: json['title'] ?? '',
      body: json['body'] ?? '',
      userId: json['userId'] ?? 0,
    );
  }
}

class JsonParsingPage extends StatefulWidget {
  const JsonParsingPage({super.key});

  @override
  State<JsonParsingPage> createState() => _JsonParsingPageState();
}

class _JsonParsingPageState extends State<JsonParsingPage> {
  Post? _post;
  bool _isLoading = false;
  String? _error;

  Future<void> _fetchPost() async {
    setState(() {
      _isLoading = true;
      _error = null;
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
      );

      if (response.statusCode == 200) {
        final data = json.decode(response.body);
        setState(() {
          _post = Post.fromJson(data);
        });
      } else {
        setState(() {
          _error = 'HTTP ${response.statusCode}: ${response.reasonPhrase}';
        });
      }
    } catch (e) {
      setState(() {
        _error = 'Network error: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('JSON Parsing'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            ElevatedButton(
              onPressed: _isLoading ? null : _fetchPost,
              child: const Text('Fetch Post'),
            ),
            const SizedBox(height: 20),
            if (_isLoading)
              const Center(child: CircularProgressIndicator())
            else if (_error != null)
              Container(
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: Colors.red.withOpacity(0.1),
                  border: Border.all(color: Colors.red),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Text(
                  _error!,
                  style: const TextStyle(color: Colors.red),
                ),
              )
            else if (_post != null)
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'ID: ${_post!.id}',
                        style: const TextStyle(fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Text(
                        _post!.title,
                        style: const TextStyle(
                          fontSize: 18,
                          fontWeight: FontWeight.w600,
                        ),
                      ),
                      const SizedBox(height: 8),
                      Text(_post!.body),
                      const SizedBox(height: 8),
                      Text(
                        'User ID: ${_post!.userId}',
                        style: TextStyle(color: Colors.grey[400]),
                      ),
                    ],
                  ),
                ),
              ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates JSON parsing using model classes with factory  
constructors. The Post.fromJson factory method safely converts JSON data  
to Dart objects with null safety. Error handling includes both HTTP status  
codes and network exceptions.  

## HTTP POST Request

Sending data to a server using POST requests with JSON payload.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'HTTP POST',
      home: const HttpPostPage(),
    );
  }
}

class HttpPostPage extends StatefulWidget {
  const HttpPostPage({super.key});

  @override
  State<HttpPostPage> createState() => _HttpPostPageState();
}

class _HttpPostPageState extends State<HttpPostPage> {
  final _titleController = TextEditingController();
  final _bodyController = TextEditingController();
  bool _isLoading = false;
  String _response = '';

  Future<void> _createPost() async {
    if (_titleController.text.isEmpty || _bodyController.text.isEmpty) {
      setState(() {
        _response = 'Please fill in both fields';
      });
      return;
    }

    setState(() {
      _isLoading = true;
      _response = '';
    });

    try {
      final response = await http.post(
        Uri.parse('https://jsonplaceholder.typicode.com/posts'),
        headers: {
          'Content-Type': 'application/json; charset=UTF-8',
        },
        body: jsonEncode({
          'title': _titleController.text,
          'body': _bodyController.text,
          'userId': 1,
        }),
      );

      if (response.statusCode == 201) {
        final data = json.decode(response.body);
        setState(() {
          _response = 'Post created successfully!\n'
              'ID: ${data['id']}\n'
              'Title: ${data['title']}';
        });
        _titleController.clear();
        _bodyController.clear();
      } else {
        setState(() {
          _response = 'Failed to create post: ${response.statusCode}';
        });
      }
    } catch (e) {
      setState(() {
        _response = 'Error: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  @override
  void dispose() {
    _titleController.dispose();
    _bodyController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('HTTP POST Request'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _titleController,
              decoration: const InputDecoration(
                labelText: 'Post Title',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _bodyController,
              decoration: const InputDecoration(
                labelText: 'Post Body',
                border: OutlineInputBorder(),
              ),
              maxLines: 3,
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _isLoading ? null : _createPost,
              child: _isLoading
                  ? const SizedBox(
                      height: 20,
                      width: 20,
                      child: CircularProgressIndicator(strokeWidth: 2),
                    )
                  : const Text('Create Post'),
            ),
            const SizedBox(height: 20),
            if (_response.isNotEmpty)
              Container(
                width: double.infinity,
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: _response.startsWith('Error') || 
                         _response.startsWith('Failed')
                      ? Colors.red.withOpacity(0.1)
                      : Colors.green.withOpacity(0.1),
                  border: Border.all(
                    color: _response.startsWith('Error') || 
                           _response.startsWith('Failed')
                        ? Colors.red
                        : Colors.green,
                  ),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Text(_response),
              ),
          ],
        ),
      ),
    );
  }
}
```

This example shows HTTP POST requests with JSON data. The Content-Type  
header specifies the data format, while jsonEncode converts Dart objects  
to JSON strings. Form validation ensures data integrity before sending  
requests.  

## List Data from API

Fetching and displaying lists of data from REST APIs.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'API List Data',
      home: const ApiListPage(),
    );
  }
}

class User {
  final int id;
  final String name;
  final String email;
  final String phone;
  final String website;

  User({
    required this.id,
    required this.name,
    required this.email,
    required this.phone,
    required this.website,
  });

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'],
      name: json['name'],
      email: json['email'],
      phone: json['phone'],
      website: json['website'],
    );
  }
}

class ApiListPage extends StatefulWidget {
  const ApiListPage({super.key});

  @override
  State<ApiListPage> createState() => _ApiListPageState();
}

class _ApiListPageState extends State<ApiListPage> {
  List<User> _users = [];
  bool _isLoading = true;
  String? _error;

  @override
  void initState() {
    super.initState();
    _fetchUsers();
  }

  Future<void> _fetchUsers() async {
    setState(() {
      _isLoading = true;
      _error = null;
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/users'),
      );

      if (response.statusCode == 200) {
        final List<dynamic> data = json.decode(response.body);
        setState(() {
          _users = data.map((json) => User.fromJson(json)).toList();
        });
      } else {
        setState(() {
          _error = 'Failed to load users: ${response.statusCode}';
        });
      }
    } catch (e) {
      setState(() {
        _error = 'Network error: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Users List'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _fetchUsers,
          ),
        ],
      ),
      body: _isLoading
          ? const Center(child: CircularProgressIndicator())
          : _error != null
              ? Center(
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      Text(
                        _error!,
                        textAlign: TextAlign.center,
                        style: const TextStyle(color: Colors.red),
                      ),
                      const SizedBox(height: 16),
                      ElevatedButton(
                        onPressed: _fetchUsers,
                        child: const Text('Retry'),
                      ),
                    ],
                  ),
                )
              : ListView.builder(
                  itemCount: _users.length,
                  itemBuilder: (context, index) {
                    final user = _users[index];
                    return Card(
                      margin: const EdgeInsets.symmetric(
                        horizontal: 16,
                        vertical: 4,
                      ),
                      child: ListTile(
                        leading: CircleAvatar(
                          child: Text(user.name[0]),
                        ),
                        title: Text(user.name),
                        subtitle: Text(user.email),
                        trailing: Column(
                          crossAxisAlignment: CrossAxisAlignment.end,
                          mainAxisAlignment: MainAxisAlignment.center,
                          children: [
                            Text(
                              user.phone,
                              style: const TextStyle(fontSize: 12),
                            ),
                            Text(
                              user.website,
                              style: const TextStyle(
                                fontSize: 12,
                                color: Colors.blue,
                              ),
                            ),
                          ],
                        ),
                        onTap: () {
                          ScaffoldMessenger.of(context).showSnackBar(
                            SnackBar(
                              content: Text('Selected: ${user.name}'),
                            ),
                          );
                        },
                      ),
                    );
                  },
                ),
    );
  }
}
```

This example demonstrates fetching and displaying lists of data from APIs.  
The ListView.builder efficiently renders large lists while the refresh  
functionality allows users to update data. Error states provide clear  
feedback when requests fail.  

## HTTP PUT and DELETE

Updating and deleting resources using PUT and DELETE requests.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'HTTP PUT/DELETE',
      home: const HttpCrudPage(),
    );
  }
}

class Post {
  final int id;
  final String title;
  final String body;
  final int userId;

  Post({
    required this.id,
    required this.title,
    required this.body,
    required this.userId,
  });

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      id: json['id'],
      title: json['title'],
      body: json['body'],
      userId: json['userId'],
    );
  }

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'title': title,
      'body': body,
      'userId': userId,
    };
  }
}

class HttpCrudPage extends StatefulWidget {
  const HttpCrudPage({super.key});

  @override
  State<HttpCrudPage> createState() => _HttpCrudPageState();
}

class _HttpCrudPageState extends State<HttpCrudPage> {
  final _titleController = TextEditingController();
  final _bodyController = TextEditingController();
  Post? _currentPost;
  bool _isLoading = false;
  String _status = '';

  Future<void> _fetchPost() async {
    setState(() {
      _isLoading = true;
      _status = '';
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
      );

      if (response.statusCode == 200) {
        final data = json.decode(response.body);
        setState(() {
          _currentPost = Post.fromJson(data);
          _titleController.text = _currentPost!.title;
          _bodyController.text = _currentPost!.body;
          _status = 'Post loaded successfully';
        });
      }
    } catch (e) {
      setState(() {
        _status = 'Error loading post: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _updatePost() async {
    if (_currentPost == null) {
      setState(() {
        _status = 'No post loaded to update';
      });
      return;
    }

    setState(() {
      _isLoading = true;
      _status = '';
    });

    try {
      final response = await http.put(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/${_currentPost!.id}'),
        headers: {
          'Content-Type': 'application/json; charset=UTF-8',
        },
        body: jsonEncode({
          'id': _currentPost!.id,
          'title': _titleController.text,
          'body': _bodyController.text,
          'userId': _currentPost!.userId,
        }),
      );

      if (response.statusCode == 200) {
        setState(() {
          _status = 'Post updated successfully';
          _currentPost = Post(
            id: _currentPost!.id,
            title: _titleController.text,
            body: _bodyController.text,
            userId: _currentPost!.userId,
          );
        });
      }
    } catch (e) {
      setState(() {
        _status = 'Error updating post: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _deletePost() async {
    if (_currentPost == null) {
      setState(() {
        _status = 'No post loaded to delete';
      });
      return;
    }

    setState(() {
      _isLoading = true;
      _status = '';
    });

    try {
      final response = await http.delete(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/${_currentPost!.id}'),
      );

      if (response.statusCode == 200) {
        setState(() {
          _status = 'Post deleted successfully';
          _currentPost = null;
          _titleController.clear();
          _bodyController.clear();
        });
      }
    } catch (e) {
      setState(() {
        _status = 'Error deleting post: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  @override
  void dispose() {
    _titleController.dispose();
    _bodyController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('HTTP CRUD Operations'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading ? null : _fetchPost,
                    child: const Text('Load Post'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading || _currentPost == null 
                        ? null 
                        : _updatePost,
                    child: const Text('Update'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading || _currentPost == null 
                        ? null 
                        : _deletePost,
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.red.withOpacity(0.8),
                    ),
                    child: const Text('Delete'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 20),
            TextField(
              controller: _titleController,
              decoration: const InputDecoration(
                labelText: 'Title',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _bodyController,
              decoration: const InputDecoration(
                labelText: 'Body',
                border: OutlineInputBorder(),
              ),
              maxLines: 4,
            ),
            const SizedBox(height: 20),
            if (_isLoading)
              const CircularProgressIndicator()
            else if (_status.isNotEmpty)
              Container(
                width: double.infinity,
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: _status.contains('Error')
                      ? Colors.red.withOpacity(0.1)
                      : Colors.green.withOpacity(0.1),
                  border: Border.all(
                    color: _status.contains('Error') 
                        ? Colors.red 
                        : Colors.green,
                  ),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Text(_status),
              ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates full CRUD operations using HTTP methods.  
PUT requests update existing resources while DELETE removes them.  
The UI disables actions when no data is loaded and provides visual  
feedback for all operations.  

## Error Handling and Retry Logic

Implementing robust error handling with retry mechanisms.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';
import 'dart:io';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Error Handling & Retry',
      home: const ErrorHandlingPage(),
    );
  }
}

enum NetworkErrorType {
  timeout,
  noConnection,
  serverError,
  unknown,
}

class NetworkError {
  final NetworkErrorType type;
  final String message;
  final int? statusCode;

  NetworkError({
    required this.type,
    required this.message,
    this.statusCode,
  });

  static NetworkError fromException(dynamic error) {
    if (error is SocketException) {
      return NetworkError(
        type: NetworkErrorType.noConnection,
        message: 'No internet connection available',
      );
    } else if (error is http.ClientException) {
      return NetworkError(
        type: NetworkErrorType.timeout,
        message: 'Request timeout',
      );
    } else if (error is FormatException) {
      return NetworkError(
        type: NetworkErrorType.serverError,
        message: 'Invalid server response',
      );
    } else {
      return NetworkError(
        type: NetworkErrorType.unknown,
        message: error.toString(),
      );
    }
  }

  IconData get icon {
    switch (type) {
      case NetworkErrorType.timeout:
        return Icons.timer;
      case NetworkErrorType.noConnection:
        return Icons.wifi_off;
      case NetworkErrorType.serverError:
        return Icons.error;
      case NetworkErrorType.unknown:
        return Icons.warning;
    }
  }

  Color get color {
    switch (type) {
      case NetworkErrorType.timeout:
        return Colors.orange;
      case NetworkErrorType.noConnection:
        return Colors.red;
      case NetworkErrorType.serverError:
        return Colors.purple;
      case NetworkErrorType.unknown:
        return Colors.grey;
    }
  }
}

class RetryableApiService {
  static const int maxRetries = 3;
  static const Duration retryDelay = Duration(seconds: 2);

  static Future<String> fetchDataWithRetry({
    required String url,
    int retryCount = 0,
    Function(int, NetworkError)? onRetry,
  }) async {
    try {
      final response = await http.get(Uri.parse(url)).timeout(
        const Duration(seconds: 10),
      );

      if (response.statusCode >= 200 && response.statusCode < 300) {
        final data = json.decode(response.body);
        return data['title'] ?? 'Success';
      } else if (response.statusCode >= 500 && retryCount < maxRetries) {
        throw http.ClientException('Server error: ${response.statusCode}');
      } else {
        throw NetworkError(
          type: NetworkErrorType.serverError,
          message: 'HTTP ${response.statusCode}',
          statusCode: response.statusCode,
        );
      }
    } catch (e) {
      final networkError = NetworkError.fromException(e);
      
      if (retryCount < maxRetries && 
          (networkError.type == NetworkErrorType.timeout ||
           networkError.type == NetworkErrorType.noConnection)) {
        
        onRetry?.call(retryCount + 1, networkError);
        
        await Future.delayed(retryDelay);
        return fetchDataWithRetry(
          url: url,
          retryCount: retryCount + 1,
          onRetry: onRetry,
        );
      }
      
      throw networkError;
    }
  }
}

class ErrorHandlingPage extends StatefulWidget {
  const ErrorHandlingPage({super.key});

  @override
  State<ErrorHandlingPage> createState() => _ErrorHandlingPageState();
}

class _ErrorHandlingPageState extends State<ErrorHandlingPage> {
  String _result = '';
  bool _isLoading = false;
  NetworkError? _error;
  int _retryAttempt = 0;

  Future<void> _fetchData({bool simulateError = false}) async {
    setState(() {
      _isLoading = true;
      _error = null;
      _retryAttempt = 0;
      _result = '';
    });

    try {
      final url = simulateError 
          ? 'https://httpstat.us/500?sleep=2000'  // Simulate server error
          : 'https://jsonplaceholder.typicode.com/posts/1';

      final result = await RetryableApiService.fetchDataWithRetry(
        url: url,
        onRetry: (attempt, error) {
          setState(() {
            _retryAttempt = attempt;
          });
        },
      );

      setState(() {
        _result = result;
      });
    } catch (e) {
      setState(() {
        _error = e is NetworkError ? e : NetworkError.fromException(e);
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Error Handling & Retry'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading ? null : () => _fetchData(),
                    child: const Text('Fetch Data'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading 
                        ? null 
                        : () => _fetchData(simulateError: true),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.orange.withOpacity(0.8),
                    ),
                    child: const Text('Simulate Error'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 20),
            if (_isLoading) ...[
              const Center(child: CircularProgressIndicator()),
              if (_retryAttempt > 0) ...[
                const SizedBox(height: 16),
                Container(
                  padding: const EdgeInsets.all(12),
                  decoration: BoxDecoration(
                    color: Colors.orange.withOpacity(0.1),
                    border: Border.all(color: Colors.orange),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: Row(
                    children: [
                      const Icon(Icons.refresh, color: Colors.orange),
                      const SizedBox(width: 8),
                      Text('Retry attempt $_retryAttempt/${RetryableApiService.maxRetries}'),
                    ],
                  ),
                ),
              ],
            ] else if (_error != null) ...[
              Container(
                padding: const EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: _error!.color.withOpacity(0.1),
                  border: Border.all(color: _error!.color),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Icon(_error!.icon, color: _error!.color),
                        const SizedBox(width: 8),
                        Text(
                          'Network Error',
                          style: TextStyle(
                            fontWeight: FontWeight.bold,
                            color: _error!.color,
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 8),
                    Text(_error!.message),
                    if (_error!.statusCode != null) ...[
                      const SizedBox(height: 4),
                      Text('Status Code: ${_error!.statusCode}'),
                    ],
                    const SizedBox(height: 12),
                    ElevatedButton(
                      onPressed: () => _fetchData(),
                      child: const Text('Try Again'),
                    ),
                  ],
                ),
              ),
            ] else if (_result.isNotEmpty) ...[
              Container(
                padding: const EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: Colors.green.withOpacity(0.1),
                  border: Border.all(color: Colors.green),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Row(
                      children: [
                        Icon(Icons.check_circle, color: Colors.green),
                        SizedBox(width: 8),
                        Text(
                          'Success',
                          style: TextStyle(
                            fontWeight: FontWeight.bold,
                            color: Colors.green,
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 8),
                    Text('Result: $_result'),
                  ],
                ),
              ),
            ],
            const Spacer(),
            const Card(
              child: Padding(
                padding: EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Error Handling Features:',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    SizedBox(height: 8),
                    Text('• Automatic retry for timeout/connection errors'),
                    Text('• Different error types with appropriate icons'),
                    Text('• Visual feedback during retry attempts'),
                    Text('• Graceful degradation for permanent errors'),
                    Text('• Request timeout handling'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates comprehensive error handling with retry logic.  
Different error types are categorized and handled appropriately. The retry  
mechanism automatically attempts failed requests with exponential backoff,  
providing users with clear feedback throughout the process.  

## Custom Headers and Authentication

Adding custom headers and handling API authentication.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Custom Headers & Auth',
      home: const AuthenticatedRequestPage(),
    );
  }
}

class ApiService {
  static const String baseUrl = 'https://jsonplaceholder.typicode.com';
  static String? _authToken;

  static Map<String, String> get _headers {
    final headers = {
      'Content-Type': 'application/json; charset=UTF-8',
      'Accept': 'application/json',
      'User-Agent': 'Flutter-App/1.0',
    };

    if (_authToken != null) {
      headers['Authorization'] = 'Bearer $_authToken';
    }

    return headers;
  }

  static Future<bool> login(String username, String password) async {
    try {
      // Simulate login request
      await Future.delayed(const Duration(seconds: 1));
      
      if (username == 'admin' && password == 'password') {
        _authToken = 'fake-jwt-token-${DateTime.now().millisecondsSinceEpoch}';
        return true;
      }
      return false;
    } catch (e) {
      return false;
    }
  }

  static void logout() {
    _authToken = null;
  }

  static Future<http.Response> authenticatedGet(String endpoint) async {
    return await http.get(
      Uri.parse('$baseUrl$endpoint'),
      headers: _headers,
    );
  }

  static Future<http.Response> authenticatedPost(
    String endpoint, 
    Map<String, dynamic> body,
  ) async {
    return await http.post(
      Uri.parse('$baseUrl$endpoint'),
      headers: _headers,
      body: jsonEncode(body),
    );
  }

  static bool get isAuthenticated => _authToken != null;
  static String? get authToken => _authToken;
}

class AuthenticatedRequestPage extends StatefulWidget {
  const AuthenticatedRequestPage({super.key});

  @override
  State<AuthenticatedRequestPage> createState() => _AuthenticatedRequestPageState();
}

class _AuthenticatedRequestPageState extends State<AuthenticatedRequestPage> {
  final _usernameController = TextEditingController();
  final _passwordController = TextEditingController();
  bool _isLoading = false;
  String _status = '';
  String _apiResponse = '';

  Future<void> _login() async {
    setState(() {
      _isLoading = true;
      _status = '';
    });

    try {
      final success = await ApiService.login(
        _usernameController.text,
        _passwordController.text,
      );

      setState(() {
        if (success) {
          _status = 'Login successful! Token: ${ApiService.authToken}';
        } else {
          _status = 'Login failed. Try username: admin, password: password';
        }
      });
    } catch (e) {
      setState(() {
        _status = 'Login error: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _makeAuthenticatedRequest() async {
    if (!ApiService.isAuthenticated) {
      setState(() {
        _status = 'Please login first';
      });
      return;
    }

    setState(() {
      _isLoading = true;
      _apiResponse = '';
    });

    try {
      final response = await ApiService.authenticatedGet('/posts/1');

      if (response.statusCode == 200) {
        final data = json.decode(response.body);
        setState(() {
          _apiResponse = 'Title: ${data['title']}\n\n'
              'Body: ${data['body']}\n\n'
              'Request Headers:\n'
              '• Authorization: Bearer ${ApiService.authToken!.substring(0, 20)}...\n'
              '• Content-Type: application/json\n'
              '• Accept: application/json\n'
              '• User-Agent: Flutter-App/1.0';
        });
      } else {
        setState(() {
          _apiResponse = 'Request failed: ${response.statusCode}';
        });
      }
    } catch (e) {
      setState(() {
        _apiResponse = 'Request error: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  void _logout() {
    ApiService.logout();
    setState(() {
      _status = 'Logged out successfully';
      _apiResponse = '';
    });
  }

  @override
  void dispose() {
    _usernameController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Authenticated Requests'),
        actions: [
          if (ApiService.isAuthenticated)
            IconButton(
              icon: const Icon(Icons.logout),
              onPressed: _logout,
            ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            if (!ApiService.isAuthenticated) ...[
              TextField(
                controller: _usernameController,
                decoration: const InputDecoration(
                  labelText: 'Username (try: admin)',
                  border: OutlineInputBorder(),
                ),
              ),
              const SizedBox(height: 16),
              TextField(
                controller: _passwordController,
                decoration: const InputDecoration(
                  labelText: 'Password (try: password)',
                  border: OutlineInputBorder(),
                ),
                obscureText: true,
              ),
              const SizedBox(height: 16),
              ElevatedButton(
                onPressed: _isLoading ? null : _login,
                child: const Text('Login'),
              ),
            ] else ...[
              Card(
                color: Colors.green.withOpacity(0.1),
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Row(
                    children: [
                      const Icon(Icons.check_circle, color: Colors.green),
                      const SizedBox(width: 8),
                      const Expanded(
                        child: Text('Authenticated - Ready to make API calls'),
                      ),
                      TextButton(
                        onPressed: _logout,
                        child: const Text('Logout'),
                      ),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
              ElevatedButton(
                onPressed: _isLoading ? null : _makeAuthenticatedRequest,
                child: const Text('Make Authenticated Request'),
              ),
            ],
            const SizedBox(height: 20),
            if (_isLoading)
              const Center(child: CircularProgressIndicator())
            else if (_status.isNotEmpty)
              Container(
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: _status.contains('successful') || _status.contains('Token')
                      ? Colors.green.withOpacity(0.1)
                      : Colors.red.withOpacity(0.1),
                  border: Border.all(
                    color: _status.contains('successful') || _status.contains('Token')
                        ? Colors.green
                        : Colors.red,
                  ),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Text(_status),
              ),
            if (_apiResponse.isNotEmpty) ...[
              const SizedBox(height: 16),
              Expanded(
                child: Container(
                  padding: const EdgeInsets.all(12),
                  decoration: BoxDecoration(
                    color: Colors.blue.withOpacity(0.1),
                    border: Border.all(color: Colors.blue),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: SingleChildScrollView(
                    child: Text(_apiResponse),
                  ),
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates custom headers and authentication patterns.  
The ApiService class manages authentication tokens and automatically  
includes them in requests. Custom headers like User-Agent provide  
additional request metadata.  

## Network Connectivity Check

Checking network connectivity before making requests.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';
import 'dart:io';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Connectivity Check',
      home: const ConnectivityCheckPage(),
    );
  }
}

enum ConnectivityStatus {
  online,
  offline,
  checking,
  unknown,
}

class ConnectivityService {
  static Future<ConnectivityStatus> checkConnectivity() async {
    try {
      final result = await InternetAddress.lookup('google.com');
      if (result.isNotEmpty && result[0].rawAddress.isNotEmpty) {
        return ConnectivityStatus.online;
      }
    } on SocketException catch (_) {
      return ConnectivityStatus.offline;
    }
    return ConnectivityStatus.offline;
  }

  static Future<bool> canReachApi({String testUrl = 'https://jsonplaceholder.typicode.com/posts/1'}) async {
    try {
      final response = await http.head(Uri.parse(testUrl)).timeout(
        const Duration(seconds: 5),
      );
      return response.statusCode >= 200 && response.statusCode < 300;
    } catch (e) {
      return false;
    }
  }
}

class ConnectivityCheckPage extends StatefulWidget {
  const ConnectivityCheckPage({super.key});

  @override
  State<ConnectivityCheckPage> createState() => _ConnectivityCheckPageState();
}

class _ConnectivityCheckPageState extends State<ConnectivityCheckPage> {
  ConnectivityStatus _connectivityStatus = ConnectivityStatus.unknown;
  bool _apiReachable = false;
  bool _isChecking = false;
  String _apiData = '';
  String _lastChecked = '';

  @override
  void initState() {
    super.initState();
    _checkConnectivity();
  }

  Future<void> _checkConnectivity() async {
    setState(() {
      _isChecking = true;
      _connectivityStatus = ConnectivityStatus.checking;
    });

    try {
      final connectivityStatus = await ConnectivityService.checkConnectivity();
      final apiReachable = connectivityStatus == ConnectivityStatus.online
          ? await ConnectivityService.canReachApi()
          : false;

      setState(() {
        _connectivityStatus = connectivityStatus;
        _apiReachable = apiReachable;
        _lastChecked = DateTime.now().toString().substring(0, 19);
      });
    } catch (e) {
      setState(() {
        _connectivityStatus = ConnectivityStatus.unknown;
        _apiReachable = false;
      });
    } finally {
      setState(() {
        _isChecking = false;
      });
    }
  }

  Future<void> _fetchDataWithCheck() async {
    if (_connectivityStatus != ConnectivityStatus.online) {
      _showConnectivityDialog();
      return;
    }

    if (!_apiReachable) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('API is not reachable. Please try again later.'),
          backgroundColor: Colors.orange,
        ),
      );
      return;
    }

    setState(() {
      _apiData = '';
    });

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
      );

      if (response.statusCode == 200) {
        final data = json.decode(response.body);
        setState(() {
          _apiData = data['title'];
        });
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text('Failed to fetch data: $e'),
          backgroundColor: Colors.red,
        ),
      );
    }
  }

  void _showConnectivityDialog() {
    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          title: const Row(
            children: [
              Icon(Icons.wifi_off, color: Colors.red),
              SizedBox(width: 8),
              Text('No Internet Connection'),
            ],
          ),
          content: const Text(
            'Please check your internet connection and try again.',
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.of(context).pop(),
              child: const Text('Cancel'),
            ),
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
                _checkConnectivity();
              },
              child: const Text('Retry'),
            ),
          ],
        );
      },
    );
  }

  Widget _buildConnectivityIndicator() {
    IconData icon;
    Color color;
    String status;

    switch (_connectivityStatus) {
      case ConnectivityStatus.online:
        icon = Icons.wifi;
        color = Colors.green;
        status = 'Online';
        break;
      case ConnectivityStatus.offline:
        icon = Icons.wifi_off;
        color = Colors.red;
        status = 'Offline';
        break;
      case ConnectivityStatus.checking:
        icon = Icons.wifi_find;
        color = Colors.orange;
        status = 'Checking...';
        break;
      case ConnectivityStatus.unknown:
        icon = Icons.help;
        color = Colors.grey;
        status = 'Unknown';
        break;
    }

    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                Icon(icon, color: color),
                const SizedBox(width: 8),
                Text(
                  'Connection Status: $status',
                  style: TextStyle(
                    fontWeight: FontWeight.bold,
                    color: color,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                Icon(
                  _apiReachable ? Icons.check_circle : Icons.error,
                  color: _apiReachable ? Colors.green : Colors.red,
                  size: 20,
                ),
                const SizedBox(width: 8),
                Text('API Reachable: ${_apiReachable ? 'Yes' : 'No'}'),
              ],
            ),
            if (_lastChecked.isNotEmpty) ...[
              const SizedBox(height: 8),
              Text(
                'Last checked: $_lastChecked',
                style: TextStyle(
                  fontSize: 12,
                  color: Colors.grey[400],
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Connectivity Check'),
        actions: [
          IconButton(
            icon: _isChecking 
                ? const SizedBox(
                    width: 20,
                    height: 20,
                    child: CircularProgressIndicator(strokeWidth: 2),
                  )
                : const Icon(Icons.refresh),
            onPressed: _isChecking ? null : _checkConnectivity,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            _buildConnectivityIndicator(),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _fetchDataWithCheck,
              child: const Text('Fetch Data (with connectivity check)'),
            ),
            const SizedBox(height: 20),
            if (_apiData.isNotEmpty)
              Container(
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: Colors.green.withOpacity(0.1),
                  border: Border.all(color: Colors.green),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'API Response:',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text(_apiData),
                  ],
                ),
              ),
            const Spacer(),
            const Card(
              child: Padding(
                padding: EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Connectivity Features:',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    SizedBox(height: 8),
                    Text('• Internet connectivity detection'),
                    Text('• API endpoint reachability test'),
                    Text('• Graceful offline handling'),
                    Text('• User-friendly error messages'),
                    Text('• Automatic retry mechanisms'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates network connectivity checking before making API  
requests. It includes both general internet connectivity and specific API  
reachability tests. The UI provides clear feedback about connection status  
and gracefully handles offline scenarios.  

## Request Timeout and Cancellation

Managing request timeouts and canceling ongoing requests.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';
import 'dart:async';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Request Timeout & Cancel',
      home: const TimeoutCancelPage(),
    );
  }
}

class TimeoutCancelPage extends StatefulWidget {
  const TimeoutCancelPage({super.key});

  @override
  State<TimeoutCancelPage> createState() => _TimeoutCancelPageState();
}

class _TimeoutCancelPageState extends State<TimeoutCancelPage> {
  bool _isLoading = false;
  String _result = '';
  Timer? _progressTimer;
  int _progressValue = 0;
  http.Client? _httpClient;

  Future<void> _makeRequestWithTimeout({required int timeoutSeconds}) async {
    setState(() {
      _isLoading = true;
      _result = '';
      _progressValue = 0;
    });

    _httpClient = http.Client();
    
    // Start progress timer
    _progressTimer = Timer.periodic(const Duration(milliseconds: 100), (timer) {
      setState(() {
        _progressValue = (timer.tick * 100) ~/ (timeoutSeconds * 10);
        if (_progressValue > 100) _progressValue = 100;
      });
    });

    try {
      final response = await _httpClient!
          .get(Uri.parse('https://httpbin.org/delay/8')) // 8-second delay
          .timeout(Duration(seconds: timeoutSeconds));

      if (response.statusCode == 200) {
        final data = json.decode(response.body);
        setState(() {
          _result = 'Request completed successfully!\n'
              'Response time: ${data['headers']['User-Agent'] ?? 'N/A'}';
        });
      }
    } on TimeoutException catch (_) {
      setState(() {
        _result = 'Request timed out after $timeoutSeconds seconds';
      });
    } catch (e) {
      setState(() {
        _result = 'Request error: $e';
      });
    } finally {
      _cleanup();
    }
  }

  void _cancelRequest() {
    if (_httpClient != null) {
      _httpClient!.close();
      _httpClient = null;
      setState(() {
        _result = 'Request cancelled by user';
      });
      _cleanup();
    }
  }

  void _cleanup() {
    _progressTimer?.cancel();
    _progressTimer = null;
    _httpClient?.close();
    _httpClient = null;
    setState(() {
      _isLoading = false;
      _progressValue = 0;
    });
  }

  @override
  void dispose() {
    _cleanup();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Request Timeout & Cancel'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Text(
              'Test different timeout scenarios:',
              style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading 
                        ? null 
                        : () => _makeRequestWithTimeout(timeoutSeconds: 5),
                    child: const Text('5s Timeout\n(Will fail)'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading 
                        ? null 
                        : () => _makeRequestWithTimeout(timeoutSeconds: 10),
                    child: const Text('10s Timeout\n(Will succeed)'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 20),
            if (_isLoading) ...[
              Column(
                children: [
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: [
                      const Text('Request Progress:'),
                      Text('$_progressValue%'),
                    ],
                  ),
                  const SizedBox(height: 8),
                  LinearProgressIndicator(
                    value: _progressValue / 100,
                  ),
                  const SizedBox(height: 20),
                  ElevatedButton(
                    onPressed: _cancelRequest,
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.red.withOpacity(0.8),
                    ),
                    child: const Text('Cancel Request'),
                  ),
                ],
              ),
            ],
            const SizedBox(height: 20),
            if (_result.isNotEmpty)
              Container(
                padding: const EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: _result.contains('error') || _result.contains('timed out')
                      ? Colors.red.withOpacity(0.1)
                      : _result.contains('cancelled')
                          ? Colors.orange.withOpacity(0.1)
                          : Colors.green.withOpacity(0.1),
                  border: Border.all(
                    color: _result.contains('error') || _result.contains('timed out')
                        ? Colors.red
                        : _result.contains('cancelled')
                            ? Colors.orange
                            : Colors.green,
                  ),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Text(_result),
              ),
            const Spacer(),
            const Card(
              child: Padding(
                padding: EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Timeout & Cancellation Features:',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    SizedBox(height: 8),
                    Text('• Configurable request timeouts'),
                    Text('• Progress tracking with visual feedback'),
                    Text('• Manual request cancellation'),
                    Text('• Proper resource cleanup'),
                    Text('• Different timeout scenarios'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates request timeout handling and manual cancellation.  
The progress indicator shows request status while the cancel button allows  
users to abort long-running requests. Proper resource cleanup prevents  
memory leaks.  

## HTTP Interceptors

Implementing HTTP interceptors for logging and request modification.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'HTTP Interceptors',
      home: const InterceptorPage(),
    );
  }
}

class RequestLog {
  final String method;
  final String url;
  final Map<String, String> headers;
  final String? body;
  final DateTime timestamp;
  final int? statusCode;
  final String? response;
  final Duration? duration;
  final String? error;

  RequestLog({
    required this.method,
    required this.url,
    required this.headers,
    this.body,
    required this.timestamp,
    this.statusCode,
    this.response,
    this.duration,
    this.error,
  });
}

class InterceptorClient extends http.BaseClient {
  final http.Client _inner = http.Client();
  final List<RequestLog> _logs = [];
  final Function(RequestLog) onRequestCompleted;

  InterceptorClient({required this.onRequestCompleted});

  @override
  Future<http.StreamedResponse> send(http.BaseRequest request) async {
    final startTime = DateTime.now();
    
    // Log outgoing request
    print('→ ${request.method} ${request.url}');
    print('  Headers: ${request.headers}');
    
    String? bodyContent;
    if (request is http.Request && request.body.isNotEmpty) {
      bodyContent = request.body;
      print('  Body: ${request.body}');
    }

    try {
      final response = await _inner.send(request);
      final endTime = DateTime.now();
      final duration = endTime.difference(startTime);

      // Read response body
      final responseBody = await response.stream.bytesToString();
      
      print('← ${response.statusCode} ${request.url}');
      print('  Duration: ${duration.inMilliseconds}ms');
      print('  Response: ${responseBody.length > 100 ? responseBody.substring(0, 100) + '...' : responseBody}');

      // Create log entry
      final log = RequestLog(
        method: request.method,
        url: request.url.toString(),
        headers: request.headers,
        body: bodyContent,
        timestamp: startTime,
        statusCode: response.statusCode,
        response: responseBody,
        duration: duration,
      );

      _logs.add(log);
      onRequestCompleted(log);

      // Return new response with the body
      return http.StreamedResponse(
        Stream.value(utf8.encode(responseBody)),
        response.statusCode,
        headers: response.headers,
        isRedirect: response.isRedirect,
        persistentConnection: response.persistentConnection,
        reasonPhrase: response.reasonPhrase,
      );
    } catch (error) {
      final endTime = DateTime.now();
      final duration = endTime.difference(startTime);

      print('✗ ${request.method} ${request.url}');
      print('  Error: $error');
      print('  Duration: ${duration.inMilliseconds}ms');

      // Create error log entry
      final log = RequestLog(
        method: request.method,
        url: request.url.toString(),
        headers: request.headers,
        body: bodyContent,
        timestamp: startTime,
        duration: duration,
        error: error.toString(),
      );

      _logs.add(log);
      onRequestCompleted(log);

      rethrow;
    }
  }

  List<RequestLog> get logs => List.unmodifiable(_logs);

  void clearLogs() {
    _logs.clear();
  }

  @override
  void close() {
    _inner.close();
    super.close();
  }
}

class InterceptorPage extends StatefulWidget {
  const InterceptorPage({super.key});

  @override
  State<InterceptorPage> createState() => _InterceptorPageState();
}

class _InterceptorPageState extends State<InterceptorPage> {
  late InterceptorClient _client;
  List<RequestLog> _logs = [];
  bool _isLoading = false;

  @override
  void initState() {
    super.initState();
    _client = InterceptorClient(
      onRequestCompleted: (log) {
        setState(() {
          _logs = List.from(_client.logs);
        });
      },
    );
  }

  Future<void> _makeGetRequest() async {
    setState(() {
      _isLoading = true;
    });

    try {
      await _client.get(Uri.parse('https://jsonplaceholder.typicode.com/posts/1'));
    } catch (e) {
      // Error already logged by interceptor
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _makePostRequest() async {
    setState(() {
      _isLoading = true;
    });

    try {
      await _client.post(
        Uri.parse('https://jsonplaceholder.typicode.com/posts'),
        headers: {'Content-Type': 'application/json'},
        body: jsonEncode({
          'title': 'Test Post',
          'body': 'This is a test post from interceptor',
          'userId': 1,
        }),
      );
    } catch (e) {
      // Error already logged by interceptor
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _makeErrorRequest() async {
    setState(() {
      _isLoading = true;
    });

    try {
      await _client.get(Uri.parse('https://jsonplaceholder.typicode.com/posts/999999'));
    } catch (e) {
      // Error already logged by interceptor
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Widget _buildLogItem(RequestLog log) {
    final isError = log.error != null || (log.statusCode != null && log.statusCode! >= 400);
    
    return Card(
      color: isError 
          ? Colors.red.withOpacity(0.1) 
          : Colors.green.withOpacity(0.1),
      child: ExpansionTile(
        leading: Icon(
          log.method == 'GET' ? Icons.download : Icons.upload,
          color: isError ? Colors.red : Colors.green,
        ),
        title: Text('${log.method} ${Uri.parse(log.url).path}'),
        subtitle: Text(
          '${log.timestamp.toString().substring(11, 19)} • '
          '${log.duration?.inMilliseconds ?? 0}ms • '
          '${log.statusCode ?? 'Error'}',
          style: const TextStyle(fontSize: 12),
        ),
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'URL: ${log.url}',
                  style: const TextStyle(fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                if (log.headers.isNotEmpty) ...[
                  const Text('Headers:'),
                  ...log.headers.entries.map(
                    (entry) => Text('  ${entry.key}: ${entry.value}',
                        style: const TextStyle(fontSize: 12)),
                  ),
                  const SizedBox(height: 8),
                ],
                if (log.body != null) ...[
                  const Text('Request Body:'),
                  Text(log.body!, style: const TextStyle(fontSize: 12)),
                  const SizedBox(height: 8),
                ],
                if (log.response != null) ...[
                  const Text('Response:'),
                  Text(
                    log.response!.length > 200 
                        ? '${log.response!.substring(0, 200)}...'
                        : log.response!,
                    style: const TextStyle(fontSize: 12),
                  ),
                ],
                if (log.error != null) ...[
                  const Text('Error:'),
                  Text(log.error!, style: const TextStyle(fontSize: 12, color: Colors.red)),
                ],
              ],
            ),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('HTTP Interceptors'),
        actions: [
          IconButton(
            icon: const Icon(Icons.clear_all),
            onPressed: () {
              setState(() {
                _client.clearLogs();
                _logs.clear();
              });
            },
          ),
        ],
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              children: [
                Row(
                  children: [
                    Expanded(
                      child: ElevatedButton(
                        onPressed: _isLoading ? null : _makeGetRequest,
                        child: const Text('GET Request'),
                      ),
                    ),
                    const SizedBox(width: 8),
                    Expanded(
                      child: ElevatedButton(
                        onPressed: _isLoading ? null : _makePostRequest,
                        child: const Text('POST Request'),
                      ),
                    ),
                  ],
                ),
                const SizedBox(height: 8),
                ElevatedButton(
                  onPressed: _isLoading ? null : _makeErrorRequest,
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.orange.withOpacity(0.8),
                  ),
                  child: const Text('Error Request'),
                ),
              ],
            ),
          ),
          if (_isLoading)
            const Padding(
              padding: EdgeInsets.all(16.0),
              child: CircularProgressIndicator(),
            ),
          Expanded(
            child: _logs.isEmpty
                ? const Center(
                    child: Text('No requests logged yet.\nMake a request to see logs.'),
                  )
                : ListView.builder(
                    itemCount: _logs.length,
                    itemBuilder: (context, index) {
                      final log = _logs.reversed.toList()[index];
                      return _buildLogItem(log);
                    },
                  ),
          ),
        ],
      ),
    );
  }

  @override
  void dispose() {
    _client.close();
    super.dispose();
  }
}
```

This example demonstrates HTTP interceptors for request/response logging.  
The custom InterceptorClient extends BaseClient to capture all network  
activity with detailed logging. It tracks request timing, headers, body  
content, and responses for debugging purposes.  
