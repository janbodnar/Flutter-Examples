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

## File Download with Progress

Downloading files with progress tracking and error handling.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
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
      title: 'File Download',
      home: const FileDownloadPage(),
    );
  }
}

class DownloadProgress {
  final int downloaded;
  final int total;
  final double percentage;
  final String speed;
  final Duration elapsed;

  DownloadProgress({
    required this.downloaded,
    required this.total,
    required this.percentage,
    required this.speed,
    required this.elapsed,
  });
}

class FileDownloadPage extends StatefulWidget {
  const FileDownloadPage({super.key});

  @override
  State<FileDownloadPage> createState() => _FileDownloadPageState();
}

class _FileDownloadPageState extends State<FileDownloadPage> {
  bool _isDownloading = false;
  DownloadProgress? _progress;
  String _status = '';
  String _filePath = '';

  Future<void> _downloadFile(String url, String filename) async {
    setState(() {
      _isDownloading = true;
      _progress = null;
      _status = 'Starting download...';
      _filePath = '';
    });

    final stopwatch = Stopwatch()..start();

    try {
      final response = await http.get(Uri.parse(url));
      
      if (response.statusCode == 200) {
        final bytes = response.bodyBytes;
        final total = bytes.length;
        
        // Get documents directory (simulated)
        final directory = '/tmp'; // In real app, use path_provider
        final file = File('$directory/$filename');
        
        // Simulate progressive download with chunks
        const chunkSize = 8192; // 8KB chunks
        int downloaded = 0;
        
        final sink = file.openWrite();
        
        for (int i = 0; i < bytes.length; i += chunkSize) {
          if (!_isDownloading) {
            // Download cancelled
            await sink.close();
            await file.delete();
            setState(() {
              _status = 'Download cancelled';
            });
            return;
          }
          
          final end = (i + chunkSize < bytes.length) ? i + chunkSize : bytes.length;
          final chunk = bytes.sublist(i, end);
          
          sink.add(chunk);
          downloaded += chunk.length;
          
          final elapsed = stopwatch.elapsed;
          final bytesPerSecond = downloaded / elapsed.inSeconds;
          final speed = _formatBytes(bytesPerSecond.round());
          
          setState(() {
            _progress = DownloadProgress(
              downloaded: downloaded,
              total: total,
              percentage: (downloaded / total) * 100,
              speed: '$speed/s',
              elapsed: elapsed,
            );
          });
          
          // Simulate network delay
          await Future.delayed(const Duration(milliseconds: 50));
        }
        
        await sink.close();
        stopwatch.stop();
        
        setState(() {
          _status = 'Download completed successfully!';
          _filePath = file.path;
        });
        
      } else {
        setState(() {
          _status = 'Download failed: HTTP ${response.statusCode}';
        });
      }
    } catch (e) {
      setState(() {
        _status = 'Download error: $e';
      });
    } finally {
      setState(() {
        _isDownloading = false;
      });
      stopwatch.stop();
    }
  }

  void _cancelDownload() {
    setState(() {
      _isDownloading = false;
    });
  }

  String _formatBytes(int bytes) {
    if (bytes < 1024) return '${bytes}B';
    if (bytes < 1024 * 1024) return '${(bytes / 1024).toStringAsFixed(1)}KB';
    return '${(bytes / (1024 * 1024)).toStringAsFixed(1)}MB';
  }

  String _formatDuration(Duration duration) {
    String twoDigits(int n) => n.toString().padLeft(2, "0");
    String twoDigitMinutes = twoDigits(duration.inMinutes.remainder(60));
    String twoDigitSeconds = twoDigits(duration.inSeconds.remainder(60));
    return "${twoDigits(duration.inHours)}:$twoDigitMinutes:$twoDigitSeconds";
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('File Download'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Text(
              'Download Sample Files:',
              style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _isDownloading
                  ? null
                  : () => _downloadFile(
                        'https://picsum.photos/800/600.jpg',
                        'sample-image.jpg',
                      ),
              child: const Text('Download Image (≈50KB)'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: _isDownloading
                  ? null
                  : () => _downloadFile(
                        'https://file-examples.com/storage/fe86dcbcf7b03b9d4f8ee7e/2017/10/file_example_PDF_1MB.pdf',
                        'sample-document.pdf',
                      ),
              child: const Text('Download PDF (≈1MB)'),
            ),
            const SizedBox(height: 20),
            if (_isDownloading && _progress != null) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          const Text(
                            'Download Progress:',
                            style: TextStyle(fontWeight: FontWeight.bold),
                          ),
                          Text('${_progress!.percentage.toStringAsFixed(1)}%'),
                        ],
                      ),
                      const SizedBox(height: 8),
                      LinearProgressIndicator(
                        value: _progress!.percentage / 100,
                      ),
                      const SizedBox(height: 12),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            '${_formatBytes(_progress!.downloaded)} / ${_formatBytes(_progress!.total)}',
                            style: const TextStyle(fontSize: 12),
                          ),
                          Text(
                            _progress!.speed,
                            style: const TextStyle(fontSize: 12),
                          ),
                        ],
                      ),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            'Elapsed: ${_formatDuration(_progress!.elapsed)}',
                            style: const TextStyle(fontSize: 12),
                          ),
                          TextButton(
                            onPressed: _cancelDownload,
                            child: const Text('Cancel'),
                          ),
                        ],
                      ),
                    ],
                  ),
                ),
              ),
            ] else if (_status.isNotEmpty) ...[
              Container(
                padding: const EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: _status.contains('completed')
                      ? Colors.green.withOpacity(0.1)
                      : _status.contains('cancelled')
                          ? Colors.orange.withOpacity(0.1)
                          : Colors.red.withOpacity(0.1),
                  border: Border.all(
                    color: _status.contains('completed')
                        ? Colors.green
                        : _status.contains('cancelled')
                            ? Colors.orange
                            : Colors.red,
                  ),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(_status),
                    if (_filePath.isNotEmpty) ...[
                      const SizedBox(height: 8),
                      Text(
                        'File saved to: $_filePath',
                        style: const TextStyle(fontSize: 12),
                      ),
                    ],
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
                      'Download Features:',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    SizedBox(height: 8),
                    Text('• Real-time progress tracking'),
                    Text('• Download speed calculation'),
                    Text('• Cancellation support'),
                    Text('• File size formatting'),
                    Text('• Error handling and retry'),
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

This example demonstrates file downloads with progress tracking. It shows  
real-time download progress, speed calculation, and cancellation support.  
The UI provides detailed feedback about download status and file location.  

## Simple Caching System

Implementing a basic HTTP response caching mechanism.  

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
      title: 'HTTP Caching',
      home: const CachingPage(),
    );
  }
}

class CacheEntry {
  final String data;
  final DateTime timestamp;
  final Duration maxAge;

  CacheEntry({
    required this.data,
    required this.timestamp,
    required this.maxAge,
  });

  bool get isExpired => DateTime.now().difference(timestamp) > maxAge;
}

class HttpCache {
  static final Map<String, CacheEntry> _cache = {};

  static void put(String key, String data, {Duration? maxAge}) {
    _cache[key] = CacheEntry(
      data: data,
      timestamp: DateTime.now(),
      maxAge: maxAge ?? const Duration(minutes: 5),
    );
  }

  static String? get(String key) {
    final entry = _cache[key];
    if (entry == null || entry.isExpired) {
      _cache.remove(key);
      return null;
    }
    return entry.data;
  }

  static void clear() {
    _cache.clear();
  }

  static void clearExpired() {
    _cache.removeWhere((key, entry) => entry.isExpired);
  }

  static Map<String, DateTime> getCacheInfo() {
    return Map.fromEntries(
      _cache.entries.map((e) => MapEntry(e.key, e.value.timestamp)),
    );
  }

  static int get size => _cache.length;
}

class CachedApiService {
  static Future<String> fetchPost(int postId, {bool forceRefresh = false}) async {
    final cacheKey = 'post_$postId';
    
    // Check cache first (unless force refresh)
    if (!forceRefresh) {
      final cached = HttpCache.get(cacheKey);
      if (cached != null) {
        return cached;
      }
    }

    // Fetch from network
    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/posts/$postId'),
      );

      if (response.statusCode == 200) {
        // Cache the response
        HttpCache.put(cacheKey, response.body);
        return response.body;
      } else {
        throw Exception('HTTP ${response.statusCode}');
      }
    } catch (e) {
      // Try to return stale cache on error
      final staleCache = _cache[cacheKey];
      if (staleCache != null) {
        return staleCache.data;
      }
      rethrow;
    }
  }

  static final Map<String, CacheEntry> _cache = HttpCache._cache;
}

class Post {
  final int id;
  final String title;
  final String body;

  Post({required this.id, required this.title, required this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}

class CachingPage extends StatefulWidget {
  const CachingPage({super.key});

  @override
  State<CachingPage> createState() => _CachingPageState();
}

class _CachingPageState extends State<CachingPage> {
  Post? _currentPost;
  bool _isLoading = false;
  bool _fromCache = false;
  String _error = '';
  DateTime? _lastFetch;

  Future<void> _fetchPost(int postId, {bool forceRefresh = false}) async {
    setState(() {
      _isLoading = true;
      _error = '';
      _fromCache = false;
    });

    final startTime = DateTime.now();

    try {
      final cachedData = !forceRefresh ? HttpCache.get('post_$postId') : null;
      _fromCache = cachedData != null;

      final response = await CachedApiService.fetchPost(
        postId,
        forceRefresh: forceRefresh,
      );
      
      final data = json.decode(response);
      final post = Post.fromJson(data);

      setState(() {
        _currentPost = post;
        _lastFetch = DateTime.now();
      });

      // Show performance info
      final duration = DateTime.now().difference(startTime);
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(
            content: Text(
              'Loaded in ${duration.inMilliseconds}ms ${_fromCache ? '(from cache)' : '(from network)'}',
            ),
            backgroundColor: _fromCache ? Colors.green : Colors.blue,
          ),
        );
      }
    } catch (e) {
      setState(() {
        _error = 'Error: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Widget _buildCacheInfo() {
    final cacheInfo = HttpCache.getCacheInfo();
    
    return Card(
      child: ExpansionTile(
        title: Text('Cache Status (${HttpCache.size} entries)'),
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                ...cacheInfo.entries.map((entry) {
                  final age = DateTime.now().difference(entry.value);
                  return Padding(
                    padding: const EdgeInsets.symmetric(vertical: 2.0),
                    child: Row(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: [
                        Text(entry.key),
                        Text(
                          '${age.inSeconds}s ago',
                          style: const TextStyle(fontSize: 12),
                        ),
                      ],
                    ),
                  );
                }),
                if (cacheInfo.isEmpty)
                  const Text('No cached entries'),
                const SizedBox(height: 16),
                Row(
                  children: [
                    ElevatedButton(
                      onPressed: () {
                        HttpCache.clearExpired();
                        setState(() {});
                      },
                      child: const Text('Clear Expired'),
                    ),
                    const SizedBox(width: 8),
                    ElevatedButton(
                      onPressed: () {
                        HttpCache.clear();
                        setState(() {});
                      },
                      style: ElevatedButton.styleFrom(
                        backgroundColor: Colors.red.withOpacity(0.8),
                      ),
                      child: const Text('Clear All'),
                    ),
                  ],
                ),
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
        title: const Text('HTTP Caching'),
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
                    onPressed: _isLoading ? null : () => _fetchPost(1),
                    child: const Text('Load Post 1'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading ? null : () => _fetchPost(2),
                    child: const Text('Load Post 2'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: _isLoading || _currentPost == null
                  ? null
                  : () => _fetchPost(_currentPost!.id, forceRefresh: true),
              style: ElevatedButton.styleFrom(
                backgroundColor: Colors.orange.withOpacity(0.8),
              ),
              child: const Text('Force Refresh'),
            ),
            const SizedBox(height: 20),
            if (_isLoading)
              const Center(child: CircularProgressIndicator())
            else if (_error.isNotEmpty)
              Container(
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: Colors.red.withOpacity(0.1),
                  border: Border.all(color: Colors.red),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Text(_error),
              )
            else if (_currentPost != null)
              Card(
                color: _fromCache 
                    ? Colors.green.withOpacity(0.1) 
                    : Colors.blue.withOpacity(0.1),
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Row(
                        children: [
                          Icon(
                            _fromCache ? Icons.cached : Icons.cloud_download,
                            color: _fromCache ? Colors.green : Colors.blue,
                          ),
                          const SizedBox(width: 8),
                          Text(
                            _fromCache ? 'From Cache' : 'From Network',
                            style: TextStyle(
                              fontWeight: FontWeight.bold,
                              color: _fromCache ? Colors.green : Colors.blue,
                            ),
                          ),
                        ],
                      ),
                      const SizedBox(height: 12),
                      Text(
                        'Post ${_currentPost!.id}',
                        style: const TextStyle(fontSize: 12),
                      ),
                      Text(
                        _currentPost!.title,
                        style: const TextStyle(
                          fontSize: 18,
                          fontWeight: FontWeight.w600,
                        ),
                      ),
                      const SizedBox(height: 8),
                      Text(_currentPost!.body),
                      if (_lastFetch != null) ...[
                        const SizedBox(height: 8),
                        Text(
                          'Last fetched: ${_lastFetch!.toString().substring(11, 19)}',
                          style: TextStyle(
                            fontSize: 12,
                            color: Colors.grey[400],
                          ),
                        ),
                      ],
                    ],
                  ),
                ),
              ),
            const SizedBox(height: 20),
            _buildCacheInfo(),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates a simple HTTP caching system. It caches responses  
to improve performance and provides fallback to stale cache during errors.  
The UI shows cache status, performance metrics, and allows cache management.  

## WebSocket Connection

Implementing real-time communication using WebSockets.  

```dart
import 'package:flutter/material.dart';
import 'package:web_socket_channel/web_socket_channel.dart';
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
      title: 'WebSocket Chat',
      home: const WebSocketPage(),
    );
  }
}

enum ConnectionStatus {
  disconnected,
  connecting,
  connected,
  error,
}

class ChatMessage {
  final String message;
  final DateTime timestamp;
  final bool isFromUser;

  ChatMessage({
    required this.message,
    required this.timestamp,
    required this.isFromUser,
  });
}

class WebSocketPage extends StatefulWidget {
  const WebSocketPage({super.key});

  @override
  State<WebSocketPage> createState() => _WebSocketPageState();
}

class _WebSocketPageState extends State<WebSocketPage> {
  WebSocketChannel? _channel;
  ConnectionStatus _status = ConnectionStatus.disconnected;
  final List<ChatMessage> _messages = [];
  final _messageController = TextEditingController();
  String _error = '';

  void _connect() {
    if (_status == ConnectionStatus.connected) return;

    setState(() {
      _status = ConnectionStatus.connecting;
      _error = '';
    });

    try {
      // Using a WebSocket echo server for demonstration
      _channel = WebSocketChannel.connect(
        Uri.parse('wss://echo.websocket.org/'),
      );

      _channel!.stream.listen(
        (data) {
          setState(() {
            _status = ConnectionStatus.connected;
            _messages.add(
              ChatMessage(
                message: data.toString(),
                timestamp: DateTime.now(),
                isFromUser: false,
              ),
            );
          });
        },
        onError: (error) {
          setState(() {
            _status = ConnectionStatus.error;
            _error = 'Connection error: $error';
          });
        },
        onDone: () {
          setState(() {
            _status = ConnectionStatus.disconnected;
          });
        },
      );

      // Send connection message
      _sendMessage('Connected to echo server');

    } catch (e) {
      setState(() {
        _status = ConnectionStatus.error;
        _error = 'Failed to connect: $e';
      });
    }
  }

  void _disconnect() {
    _channel?.sink.close();
    _channel = null;
    setState(() {
      _status = ConnectionStatus.disconnected;
    });
  }

  void _sendMessage([String? message]) {
    final text = message ?? _messageController.text.trim();
    if (text.isEmpty || _status != ConnectionStatus.connected) return;

    setState(() {
      _messages.add(
        ChatMessage(
          message: text,
          timestamp: DateTime.now(),
          isFromUser: true,
        ),
      );
    });

    _channel?.sink.add(text);
    _messageController.clear();
  }

  void _sendTestMessage() {
    final testMessages = [
      'Hello WebSocket!',
      'Testing real-time communication',
      'Message ${DateTime.now().millisecondsSinceEpoch}',
      jsonEncode({'type': 'test', 'timestamp': DateTime.now().toIso8601String()}),
    ];
    
    final message = testMessages[_messages.length % testMessages.length];
    _sendMessage(message);
  }

  Widget _buildConnectionStatus() {
    IconData icon;
    Color color;
    String status;

    switch (_status) {
      case ConnectionStatus.disconnected:
        icon = Icons.cloud_off;
        color = Colors.grey;
        status = 'Disconnected';
        break;
      case ConnectionStatus.connecting:
        icon = Icons.cloud_sync;
        color = Colors.orange;
        status = 'Connecting...';
        break;
      case ConnectionStatus.connected:
        icon = Icons.cloud_done;
        color = Colors.green;
        status = 'Connected';
        break;
      case ConnectionStatus.error:
        icon = Icons.error;
        color = Colors.red;
        status = 'Error';
        break;
    }

    return Container(
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: color.withOpacity(0.1),
        border: Border.all(color: color),
        borderRadius: BorderRadius.circular(8),
      ),
      child: Row(
        children: [
          Icon(icon, color: color),
          const SizedBox(width: 8),
          Text(
            status,
            style: TextStyle(
              fontWeight: FontWeight.bold,
              color: color,
            ),
          ),
          const Spacer(),
          if (_status == ConnectionStatus.disconnected)
            TextButton(
              onPressed: _connect,
              child: const Text('Connect'),
            )
          else if (_status == ConnectionStatus.connected)
            TextButton(
              onPressed: _disconnect,
              child: const Text('Disconnect'),
            ),
        ],
      ),
    );
  }

  Widget _buildMessage(ChatMessage message) {
    return Padding(
      padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 4),
      child: Row(
        mainAxisAlignment: message.isFromUser 
            ? MainAxisAlignment.end 
            : MainAxisAlignment.start,
        children: [
          Container(
            constraints: BoxConstraints(
              maxWidth: MediaQuery.of(context).size.width * 0.7,
            ),
            padding: const EdgeInsets.all(12),
            decoration: BoxDecoration(
              color: message.isFromUser 
                  ? Colors.blue.withOpacity(0.8) 
                  : Colors.grey.withOpacity(0.3),
              borderRadius: BorderRadius.circular(16),
            ),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  message.message,
                  style: TextStyle(
                    color: message.isFromUser ? Colors.white : null,
                  ),
                ),
                const SizedBox(height: 4),
                Text(
                  '${message.timestamp.toString().substring(11, 19)}',
                  style: TextStyle(
                    fontSize: 10,
                    color: message.isFromUser 
                        ? Colors.white70 
                        : Colors.grey[400],
                  ),
                ),
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
        title: const Text('WebSocket Chat'),
        actions: [
          IconButton(
            icon: const Icon(Icons.clear_all),
            onPressed: () {
              setState(() {
                _messages.clear();
              });
            },
          ),
        ],
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: _buildConnectionStatus(),
          ),
          if (_error.isNotEmpty)
            Container(
              margin: const EdgeInsets.symmetric(horizontal: 16),
              padding: const EdgeInsets.all(12),
              decoration: BoxDecoration(
                color: Colors.red.withOpacity(0.1),
                border: Border.all(color: Colors.red),
                borderRadius: BorderRadius.circular(8),
              ),
              child: Text(_error),
            ),
          Expanded(
            child: _messages.isEmpty
                ? const Center(
                    child: Text(
                      'No messages yet.\nConnect and send a message to start.',
                      textAlign: TextAlign.center,
                    ),
                  )
                : ListView.builder(
                    itemCount: _messages.length,
                    itemBuilder: (context, index) {
                      return _buildMessage(_messages[index]);
                    },
                  ),
          ),
          Container(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              children: [
                if (_status == ConnectionStatus.connected) ...[
                  Row(
                    children: [
                      Expanded(
                        child: TextField(
                          controller: _messageController,
                          decoration: const InputDecoration(
                            hintText: 'Type a message...',
                            border: OutlineInputBorder(),
                          ),
                          onSubmitted: (_) => _sendMessage(),
                        ),
                      ),
                      const SizedBox(width: 8),
                      IconButton(
                        onPressed: _sendMessage,
                        icon: const Icon(Icons.send),
                      ),
                    ],
                  ),
                  const SizedBox(height: 8),
                  ElevatedButton(
                    onPressed: _sendTestMessage,
                    child: const Text('Send Test Message'),
                  ),
                ],
              ],
            ),
          ),
        ],
      ),
    );
  }

  @override
  void dispose() {
    _messageController.dispose();
    _disconnect();
    super.dispose();
  }
}
```

This example demonstrates WebSocket connections for real-time communication.  
It includes connection management, message handling, and a chat-like interface.  
The echo server returns sent messages, simulating real-time bidirectional  
communication.  

## Form Data Upload

Uploading form data including files using multipart requests.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';
import 'dart:typed_data';

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
      title: 'Form Data Upload',
      home: const FormUploadPage(),
    );
  }
}

class FormUploadPage extends StatefulWidget {
  const FormUploadPage({super.key});

  @override
  State<FormUploadPage> createState() => _FormUploadPageState();
}

class _FormUploadPageState extends State<FormUploadPage> {
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _descriptionController = TextEditingController();
  bool _isUploading = false;
  String _uploadResult = '';
  Uint8List? _selectedFile;
  String _fileName = '';

  void _simulateFileSelection() {
    // Simulate file selection with dummy data
    final dummyContent = 'This is a sample file content\n'
        'Created at: ${DateTime.now()}\n'
        'File type: text/plain';
    
    setState(() {
      _selectedFile = Uint8List.fromList(dummyContent.codeUnits);
      _fileName = 'sample_file_${DateTime.now().millisecondsSinceEpoch}.txt';
    });
  }

  Future<void> _uploadFormData() async {
    if (_nameController.text.isEmpty || _emailController.text.isEmpty) {
      setState(() {
        _uploadResult = 'Please fill in required fields';
      });
      return;
    }

    setState(() {
      _isUploading = true;
      _uploadResult = '';
    });

    try {
      // Create multipart request
      final request = http.MultipartRequest(
        'POST',
        Uri.parse('https://httpbin.org/post'), // Test endpoint
      );

      // Add text fields
      request.fields.addAll({
        'name': _nameController.text,
        'email': _emailController.text,
        'description': _descriptionController.text,
        'timestamp': DateTime.now().toIso8601String(),
      });

      // Add file if selected
      if (_selectedFile != null && _fileName.isNotEmpty) {
        request.files.add(
          http.MultipartFile.fromBytes(
            'file',
            _selectedFile!,
            filename: _fileName,
          ),
        );
      }

      // Add custom headers
      request.headers.addAll({
        'User-Agent': 'Flutter-App/1.0',
        'Accept': 'application/json',
      });

      // Send request
      final response = await request.send();
      final responseBody = await response.stream.bytesToString();

      if (response.statusCode == 200) {
        final data = json.decode(responseBody);
        
        setState(() {
          _uploadResult = 'Upload successful!\n\n'
              'Form data sent:\n'
              '• Name: ${data['form']['name']}\n'
              '• Email: ${data['form']['email']}\n'
              '• Description: ${data['form']['description']}\n'
              '${_selectedFile != null ? '• File: $_fileName (${_selectedFile!.length} bytes)\n' : ''}'
              '\nServer response code: ${response.statusCode}';
        });

        // Clear form
        _nameController.clear();
        _emailController.clear();
        _descriptionController.clear();
        setState(() {
          _selectedFile = null;
          _fileName = '';
        });
      } else {
        setState(() {
          _uploadResult = 'Upload failed: HTTP ${response.statusCode}';
        });
      }
    } catch (e) {
      setState(() {
        _uploadResult = 'Upload error: $e';
      });
    } finally {
      setState(() {
        _isUploading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Form Data Upload'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Text(
              'Upload Form with Optional File:',
              style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _nameController,
              decoration: const InputDecoration(
                labelText: 'Name *',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _emailController,
              decoration: const InputDecoration(
                labelText: 'Email *',
                border: OutlineInputBorder(),
              ),
              keyboardType: TextInputType.emailAddress,
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _descriptionController,
              decoration: const InputDecoration(
                labelText: 'Description (optional)',
                border: OutlineInputBorder(),
              ),
              maxLines: 3,
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'File Attachment:',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    if (_selectedFile != null) ...[
                      Container(
                        padding: const EdgeInsets.all(12),
                        decoration: BoxDecoration(
                          color: Colors.green.withOpacity(0.1),
                          border: Border.all(color: Colors.green),
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: Row(
                          children: [
                            const Icon(Icons.attach_file, color: Colors.green),
                            const SizedBox(width: 8),
                            Expanded(
                              child: Column(
                                crossAxisAlignment: CrossAxisAlignment.start,
                                children: [
                                  Text(_fileName),
                                  Text(
                                    '${_selectedFile!.length} bytes',
                                    style: const TextStyle(fontSize: 12),
                                  ),
                                ],
                              ),
                            ),
                            IconButton(
                              icon: const Icon(Icons.close),
                              onPressed: () {
                                setState(() {
                                  _selectedFile = null;
                                  _fileName = '';
                                });
                              },
                            ),
                          ],
                        ),
                      ),
                    ] else ...[
                      ElevatedButton.icon(
                        onPressed: _simulateFileSelection,
                        icon: const Icon(Icons.attach_file),
                        label: const Text('Select File (Simulated)'),
                      ),
                    ],
                  ],
                ),
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _isUploading ? null : _uploadFormData,
              child: _isUploading
                  ? const Row(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        SizedBox(
                          height: 20,
                          width: 20,
                          child: CircularProgressIndicator(strokeWidth: 2),
                        ),
                        SizedBox(width: 8),
                        Text('Uploading...'),
                      ],
                    )
                  : const Text('Upload Form Data'),
            ),
            const SizedBox(height: 20),
            if (_uploadResult.isNotEmpty)
              Expanded(
                child: Container(
                  padding: const EdgeInsets.all(16),
                  decoration: BoxDecoration(
                    color: _uploadResult.contains('successful')
                        ? Colors.green.withOpacity(0.1)
                        : Colors.red.withOpacity(0.1),
                    border: Border.all(
                      color: _uploadResult.contains('successful')
                          ? Colors.green
                          : Colors.red,
                    ),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: SingleChildScrollView(
                    child: Text(_uploadResult),
                  ),
                ),
              ),
          ],
        ),
      ),
    );
  }

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _descriptionController.dispose();
    super.dispose();
  }
}
```

This example demonstrates multipart form data uploads including file  
attachments. It shows how to combine text fields with file uploads in  
a single request, with proper progress feedback and error handling.  

## API Pagination

Implementing pagination for large datasets with infinite scroll.  

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
      title: 'API Pagination',
      home: const PaginationPage(),
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
}

class PaginatedResponse {
  final List<Post> posts;
  final int currentPage;
  final int totalPages;
  final bool hasMore;

  PaginatedResponse({
    required this.posts,
    required this.currentPage,
    required this.totalPages,
    required this.hasMore,
  });
}

class PaginationService {
  static const int postsPerPage = 10;
  static const int totalPosts = 100; // JSONPlaceholder has 100 posts

  static Future<PaginatedResponse> fetchPosts(int page) async {
    final startIndex = (page - 1) * postsPerPage + 1;
    final endIndex = page * postsPerPage;

    try {
      final futures = <Future<http.Response>>[];
      
      for (int i = startIndex; i <= endIndex && i <= totalPosts; i++) {
        futures.add(
          http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts/$i')),
        );
      }

      final responses = await Future.wait(futures);
      final posts = <Post>[];

      for (final response in responses) {
        if (response.statusCode == 200) {
          final data = json.decode(response.body);
          posts.add(Post.fromJson(data));
        }
      }

      final totalPages = (totalPosts / postsPerPage).ceil();
      
      return PaginatedResponse(
        posts: posts,
        currentPage: page,
        totalPages: totalPages,
        hasMore: page < totalPages,
      );
    } catch (e) {
      throw Exception('Failed to fetch posts: $e');
    }
  }
}

class PaginationPage extends StatefulWidget {
  const PaginationPage({super.key};

  @override
  State<PaginationPage> createState() => _PaginationPageState();
}

class _PaginationPageState extends State<PaginationPage> {
  final List<Post> _allPosts = [];
  final ScrollController _scrollController = ScrollController();
  bool _isLoading = false;
  bool _hasMore = true;
  int _currentPage = 0;
  String? _error;

  @override
  void initState() {
    super.initState();
    _scrollController.addListener(_onScroll);
    _loadNextPage();
  }

  void _onScroll() {
    if (_scrollController.position.pixels >= 
        _scrollController.position.maxScrollExtent * 0.8) {
      _loadNextPage();
    }
  }

  Future<void> _loadNextPage() async {
    if (_isLoading || !_hasMore) return;

    setState(() {
      _isLoading = true;
      _error = null;
    });

    try {
      final response = await PaginationService.fetchPosts(_currentPage + 1);
      
      setState(() {
        _allPosts.addAll(response.posts);
        _currentPage = response.currentPage;
        _hasMore = response.hasMore;
      });
    } catch (e) {
      setState(() {
        _error = e.toString();
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _refresh() async {
    setState(() {
      _allPosts.clear();
      _currentPage = 0;
      _hasMore = true;
      _error = null;
    });
    await _loadNextPage();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Posts (${_allPosts.length}/100)'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _refresh,
          ),
        ],
      ),
      body: RefreshIndicator(
        onRefresh: _refresh,
        child: _allPosts.isEmpty && _isLoading
            ? const Center(child: CircularProgressIndicator())
            : ListView.builder(
                controller: _scrollController,
                itemCount: _allPosts.length + (_hasMore ? 1 : 0) + (_error != null ? 1 : 0),
                itemBuilder: (context, index) {
                  if (index < _allPosts.length) {
                    final post = _allPosts[index];
                    return Card(
                      margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 4),
                      child: ListTile(
                        leading: CircleAvatar(
                          child: Text('${post.id}'),
                        ),
                        title: Text(
                          post.title,
                          maxLines: 2,
                          overflow: TextOverflow.ellipsis,
                        ),
                        subtitle: Text(
                          post.body,
                          maxLines: 3,
                          overflow: TextOverflow.ellipsis,
                        ),
                        trailing: Text(
                          'User ${post.userId}',
                          style: const TextStyle(fontSize: 12),
                        ),
                        onTap: () {
                          showDialog(
                            context: context,
                            builder: (context) => AlertDialog(
                              title: Text('Post ${post.id}'),
                              content: Column(
                                crossAxisAlignment: CrossAxisAlignment.start,
                                mainAxisSize: MainAxisSize.min,
                                children: [
                                  Text(
                                    post.title,
                                    style: const TextStyle(fontWeight: FontWeight.bold),
                                  ),
                                  const SizedBox(height: 8),
                                  Text(post.body),
                                ],
                              ),
                              actions: [
                                TextButton(
                                  onPressed: () => Navigator.of(context).pop(),
                                  child: const Text('Close'),
                                ),
                              ],
                            ),
                          );
                        },
                      ),
                    );
                  } else if (_error != null) {
                    return Container(
                      margin: const EdgeInsets.all(16),
                      padding: const EdgeInsets.all(16),
                      decoration: BoxDecoration(
                        color: Colors.red.withOpacity(0.1),
                        border: Border.all(color: Colors.red),
                        borderRadius: BorderRadius.circular(8),
                      ),
                      child: Column(
                        children: [
                          Text(_error!),
                          const SizedBox(height: 8),
                          ElevatedButton(
                            onPressed: _loadNextPage,
                            child: const Text('Retry'),
                          ),
                        ],
                      ),
                    );
                  } else {
                    return const Padding(
                      padding: EdgeInsets.all(16.0),
                      child: Center(child: CircularProgressIndicator()),
                    );
                  }
                },
              ),
      ),
      floatingActionButton: _allPosts.isNotEmpty
          ? FloatingActionButton(
              onPressed: () {
                _scrollController.animateTo(
                  0,
                  duration: const Duration(milliseconds: 500),
                  curve: Curves.easeInOut,
                );
              },
              child: const Icon(Icons.keyboard_arrow_up),
            )
          : null,
    );
  }

  @override
  void dispose() {
    _scrollController.dispose();
    super.dispose();
  }
}
```

This example demonstrates API pagination with infinite scroll. It loads  
data incrementally as users scroll, providing smooth performance for  
large datasets. The implementation includes error handling, refresh  
functionality, and visual loading indicators.  

## Rate Limiting and Throttling

Implementing client-side rate limiting for API requests.  

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
      title: 'Rate Limiting',
      home: const RateLimitingPage(),
    );
  }
}

class RateLimiter {
  final int maxRequests;
  final Duration timeWindow;
  final List<DateTime> _requests = [];

  RateLimiter({
    required this.maxRequests,
    required this.timeWindow,
  });

  bool canMakeRequest() {
    final now = DateTime.now();
    final cutoff = now.subtract(timeWindow);
    
    // Remove old requests outside the time window
    _requests.removeWhere((time) => time.isBefore(cutoff));
    
    return _requests.length < maxRequests;
  }

  void recordRequest() {
    _requests.add(DateTime.now());
  }

  int get remainingRequests => maxRequests - _requests.length;
  
  Duration? get timeUntilNextRequest {
    if (_requests.isEmpty || canMakeRequest()) return null;
    
    final oldest = _requests.first;
    final resetTime = oldest.add(timeWindow);
    final now = DateTime.now();
    
    return resetTime.isAfter(now) ? resetTime.difference(now) : null;
  }
}

class ThrottledApiService {
  static final RateLimiter _rateLimiter = RateLimiter(
    maxRequests: 5,
    timeWindow: const Duration(minutes: 1),
  );

  static Future<Map<String, dynamic>> makeRequest(String endpoint) async {
    if (!_rateLimiter.canMakeRequest()) {
      final waitTime = _rateLimiter.timeUntilNextRequest;
      throw RateLimitException(
        'Rate limit exceeded. Try again in ${waitTime?.inSeconds ?? 0} seconds.',
        waitTime,
      );
    }

    _rateLimiter.recordRequest();

    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com$endpoint'),
      );

      if (response.statusCode == 200) {
        return {
          'data': json.decode(response.body),
          'remaining': _rateLimiter.remainingRequests,
          'resetTime': _rateLimiter.timeUntilNextRequest,
        };
      } else {
        throw Exception('HTTP ${response.statusCode}');
      }
    } catch (e) {
      if (e is! RateLimitException) {
        // Don't count failed requests against the rate limit
        // In a real app, you might want different behavior
      }
      rethrow;
    }
  }
}

class RateLimitException implements Exception {
  final String message;
  final Duration? retryAfter;

  RateLimitException(this.message, this.retryAfter);

  @override
  String toString() => message;
}

class RequestLog {
  final DateTime timestamp;
  final String endpoint;
  final bool success;
  final String? error;
  final Duration responseTime;

  RequestLog({
    required this.timestamp,
    required this.endpoint,
    required this.success,
    this.error,
    required this.responseTime,
  });
}

class RateLimitingPage extends StatefulWidget {
  const RateLimitingPage({super.key});

  @override
  State<RateLimitingPage> createState() => _RateLimitingPageState();
}

class _RateLimitingPageState extends State<RateLimitingPage> {
  final List<RequestLog> _requestLog = [];
  String _lastResponse = '';
  bool _isLoading = false;
  int _remainingRequests = 5;
  Duration? _resetTime;

  Future<void> _makeRequest(String endpoint) async {
    setState(() {
      _isLoading = true;
    });

    final stopwatch = Stopwatch()..start();
    
    try {
      final result = await ThrottledApiService.makeRequest(endpoint);
      stopwatch.stop();

      setState(() {
        _lastResponse = 'Success: ${json.encode(result['data'])}';
        _remainingRequests = result['remaining'];
        _resetTime = result['resetTime'];
      });

      _requestLog.add(RequestLog(
        timestamp: DateTime.now(),
        endpoint: endpoint,
        success: true,
        responseTime: stopwatch.elapsed,
      ));
    } catch (e) {
      stopwatch.stop();

      setState(() {
        _lastResponse = 'Error: $e';
        if (e is! RateLimitException) {
          // Update remaining requests only for rate limit errors
          _remainingRequests = ThrottledApiService._rateLimiter.remainingRequests;
          _resetTime = ThrottledApiService._rateLimiter.timeUntilNextRequest;
        }
      });

      _requestLog.add(RequestLog(
        timestamp: DateTime.now(),
        endpoint: endpoint,
        success: false,
        error: e.toString(),
        responseTime: stopwatch.elapsed,
      ));
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Widget _buildRateLimitStatus() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Rate Limit Status:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text('Remaining requests:'),
                Text(
                  '$_remainingRequests/5',
                  style: TextStyle(
                    fontWeight: FontWeight.bold,
                    color: _remainingRequests > 2 
                        ? Colors.green 
                        : _remainingRequests > 0 
                            ? Colors.orange 
                            : Colors.red,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 4),
            LinearProgressIndicator(
              value: _remainingRequests / 5,
              backgroundColor: Colors.red.withOpacity(0.3),
              valueColor: AlwaysStoppedAnimation<Color>(
                _remainingRequests > 2 
                    ? Colors.green 
                    : _remainingRequests > 0 
                        ? Colors.orange 
                        : Colors.red,
              ),
            ),
            const SizedBox(height: 8),
            if (_resetTime != null)
              Text(
                'Reset in: ${_resetTime!.inSeconds} seconds',
                style: const TextStyle(fontSize: 12),
              ),
            const SizedBox(height: 8),
            const Text(
              'Limit: 5 requests per minute',
              style: TextStyle(fontSize: 12),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildRequestLog() {
    return Card(
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          const Padding(
            padding: EdgeInsets.all(16.0),
            child: Text(
              'Request Log:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
          ),
          SizedBox(
            height: 200,
            child: _requestLog.isEmpty
                ? const Center(child: Text('No requests yet'))
                : ListView.builder(
                    itemCount: _requestLog.length,
                    itemBuilder: (context, index) {
                      final log = _requestLog.reversed.toList()[index];
                      return ListTile(
                        dense: true,
                        leading: Icon(
                          log.success ? Icons.check_circle : Icons.error,
                          color: log.success ? Colors.green : Colors.red,
                          size: 16,
                        ),
                        title: Text(
                          log.endpoint,
                          style: const TextStyle(fontSize: 12),
                        ),
                        subtitle: Text(
                          '${log.timestamp.toString().substring(11, 19)} • '
                          '${log.responseTime.inMilliseconds}ms'
                          '${log.error != null ? ' • ${log.error}' : ''}',
                          style: const TextStyle(fontSize: 10),
                        ),
                      );
                    },
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
        title: const Text('Rate Limiting'),
        actions: [
          IconButton(
            icon: const Icon(Icons.clear_all),
            onPressed: () {
              setState(() {
                _requestLog.clear();
                _lastResponse = '';
              });
            },
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            _buildRateLimitStatus(),
            const SizedBox(height: 16),
            const Text(
              'Test Rate Limiting:',
              style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Wrap(
              spacing: 8,
              children: [
                ElevatedButton(
                  onPressed: _isLoading ? null : () => _makeRequest('/posts/1'),
                  child: const Text('Post 1'),
                ),
                ElevatedButton(
                  onPressed: _isLoading ? null : () => _makeRequest('/posts/2'),
                  child: const Text('Post 2'),
                ),
                ElevatedButton(
                  onPressed: _isLoading ? null : () => _makeRequest('/users/1'),
                  child: const Text('User 1'),
                ),
                ElevatedButton(
                  onPressed: _isLoading ? null : () => _makeRequest('/comments/1'),
                  child: const Text('Comment 1'),
                ),
              ],
            ),
            const SizedBox(height: 16),
            if (_isLoading)
              const Center(child: CircularProgressIndicator()),
            if (_lastResponse.isNotEmpty) ...[
              const Text('Last Response:', style: TextStyle(fontWeight: FontWeight.bold)),
              const SizedBox(height: 8),
              Container(
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: _lastResponse.startsWith('Error')
                      ? Colors.red.withOpacity(0.1)
                      : Colors.green.withOpacity(0.1),
                  border: Border.all(
                    color: _lastResponse.startsWith('Error') ? Colors.red : Colors.green,
                  ),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Text(
                  _lastResponse,
                  style: const TextStyle(fontSize: 12),
                  maxLines: 3,
                  overflow: TextOverflow.ellipsis,
                ),
              ),
            ],
            const SizedBox(height: 16),
            Expanded(child: _buildRequestLog()),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates client-side rate limiting to prevent API abuse.  
It tracks request timestamps, enforces limits, and provides visual feedback  
about remaining quota. The system includes automatic reset timers and  
detailed request logging.  

## GraphQL Query

Implementing GraphQL queries with error handling and variables.  

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
      title: 'GraphQL Queries',
      home: const GraphQLPage(),
    );
  }
}

class GraphQLClient {
  final String endpoint;
  final Map<String, String> headers;

  GraphQLClient({
    required this.endpoint,
    Map<String, String>? headers,
  }) : headers = {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
          ...?headers,
        };

  Future<GraphQLResponse> query(
    String query, {
    Map<String, dynamic>? variables,
    String? operationName,
  }) async {
    try {
      final body = {
        'query': query,
        if (variables != null) 'variables': variables,
        if (operationName != null) 'operationName': operationName,
      };

      final response = await http.post(
        Uri.parse(endpoint),
        headers: headers,
        body: json.encode(body),
      );

      final data = json.decode(response.body);

      if (response.statusCode != 200) {
        return GraphQLResponse(
          data: null,
          errors: [
            GraphQLError(
              message: 'HTTP ${response.statusCode}',
              extensions: {'code': response.statusCode},
            ),
          ],
        );
      }

      return GraphQLResponse(
        data: data['data'],
        errors: data['errors']?.map<GraphQLError>((error) {
          return GraphQLError(
            message: error['message'] ?? 'Unknown error',
            locations: error['locations'],
            path: error['path'],
            extensions: error['extensions'],
          );
        }).toList(),
      );
    } catch (e) {
      return GraphQLResponse(
        data: null,
        errors: [
          GraphQLError(
            message: 'Network error: $e',
          ),
        ],
      );
    }
  }
}

class GraphQLResponse {
  final dynamic data;
  final List<GraphQLError>? errors;

  GraphQLResponse({this.data, this.errors});

  bool get hasErrors => errors != null && errors!.isNotEmpty;
}

class GraphQLError {
  final String message;
  final List<dynamic>? locations;
  final List<dynamic>? path;
  final Map<String, dynamic>? extensions;

  GraphQLError({
    required this.message,
    this.locations,
    this.path,
    this.extensions,
  });
}

// Mock GraphQL server responses for demonstration
class MockGraphQLServer {
  static Map<String, dynamic> handleQuery(String query, Map<String, dynamic>? variables) {
    if (query.contains('getUser')) {
      final userId = variables?['userId'] ?? 1;
      return {
        'data': {
          'user': {
            'id': userId,
            'name': 'User $userId',
            'email': 'user$userId@example.com',
            'posts': [
              {
                'id': 1,
                'title': 'First Post by User $userId',
                'content': 'This is the first post content...',
              },
              {
                'id': 2,
                'title': 'Second Post by User $userId',
                'content': 'This is the second post content...',
              },
            ],
          },
        },
      };
    } else if (query.contains('getAllUsers')) {
      return {
        'data': {
          'users': [
            {'id': 1, 'name': 'Alice', 'email': 'alice@example.com'},
            {'id': 2, 'name': 'Bob', 'email': 'bob@example.com'},
            {'id': 3, 'name': 'Charlie', 'email': 'charlie@example.com'},
          ],
        },
      };
    } else if (query.contains('createPost')) {
      return {
        'data': {
          'createPost': {
            'id': DateTime.now().millisecondsSinceEpoch,
            'title': variables?['title'] ?? 'New Post',
            'content': variables?['content'] ?? 'New post content',
            'author': {
              'id': variables?['authorId'] ?? 1,
              'name': 'Current User',
            },
          },
        },
      };
    } else {
      return {
        'errors': [
          {
            'message': 'Unknown query',
            'locations': [{'line': 1, 'column': 1}],
          },
        ],
      };
    }
  }
}

class GraphQLPage extends StatefulWidget {
  const GraphQLPage({super.key});

  @override
  State<GraphQLPage> createState() => _GraphQLPageState();
}

class _GraphQLPageState extends State<GraphQLPage> {
  late GraphQLClient _client;
  String _result = '';
  bool _isLoading = false;
  final _titleController = TextEditingController();
  final _contentController = TextEditingController();

  @override
  void initState() {
    super.initState();
    // Using httpbin.org/post to simulate GraphQL endpoint
    _client = GraphQLClient(endpoint: 'https://httpbin.org/post');
  }

  Future<void> _executeQuery(String query, {Map<String, dynamic>? variables}) async {
    setState(() {
      _isLoading = true;
      _result = '';
    });

    try {
      // Simulate GraphQL response since we don't have a real GraphQL server
      final mockResponse = MockGraphQLServer.handleQuery(query, variables);
      
      setState(() {
        _result = _formatResponse(mockResponse);
      });
    } catch (e) {
      setState(() {
        _result = 'Error: $e';
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  String _formatResponse(Map<String, dynamic> response) {
    return const JsonEncoder.withIndent('  ').convert(response);
  }

  Future<void> _queryUser() async {
    const query = '''
      query GetUser(\$userId: ID!) {
        user(id: \$userId) {
          id
          name
          email
          posts {
            id
            title
            content
          }
        }
      }
    ''';

    await _executeQuery(query, variables: {'userId': 1});
  }

  Future<void> _queryAllUsers() async {
    const query = '''
      query GetAllUsers {
        users {
          id
          name
          email
        }
      }
    ''';

    await _executeQuery(query);
  }

  Future<void> _createPost() async {
    if (_titleController.text.isEmpty || _contentController.text.isEmpty) {
      setState(() {
        _result = 'Please fill in both title and content fields';
      });
      return;
    }

    const mutation = '''
      mutation CreatePost(\$title: String!, \$content: String!, \$authorId: ID!) {
        createPost(input: {
          title: \$title,
          content: \$content,
          authorId: \$authorId
        }) {
          id
          title
          content
          author {
            id
            name
          }
        }
      }
    ''';

    await _executeQuery(mutation, variables: {
      'title': _titleController.text,
      'content': _contentController.text,
      'authorId': 1,
    });

    _titleController.clear();
    _contentController.clear();
  }

  Future<void> _queryWithError() async {
    const query = '''
      query InvalidQuery {
        nonExistentField {
          id
          name
        }
      }
    ''';

    await _executeQuery(query);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('GraphQL Queries'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Text(
              'GraphQL Operations:',
              style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading ? null : _queryUser,
                    child: const Text('Query User'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading ? null : _queryAllUsers,
                    child: const Text('Query All Users'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: _isLoading ? null : _queryWithError,
              style: ElevatedButton.styleFrom(
                backgroundColor: Colors.orange.withOpacity(0.8),
              ),
              child: const Text('Test Error Handling'),
            ),
            const SizedBox(height: 20),
            const Text(
              'Create Post Mutation:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            TextField(
              controller: _titleController,
              decoration: const InputDecoration(
                labelText: 'Post Title',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 8),
            TextField(
              controller: _contentController,
              decoration: const InputDecoration(
                labelText: 'Post Content',
                border: OutlineInputBorder(),
              ),
              maxLines: 2,
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: _isLoading ? null : _createPost,
              child: const Text('Create Post'),
            ),
            const SizedBox(height: 20),
            if (_isLoading)
              const Center(child: CircularProgressIndicator())
            else if (_result.isNotEmpty)
              Expanded(
                child: Container(
                  padding: const EdgeInsets.all(12),
                  decoration: BoxDecoration(
                    color: _result.contains('errors')
                        ? Colors.red.withOpacity(0.1)
                        : Colors.blue.withOpacity(0.1),
                    border: Border.all(
                      color: _result.contains('errors') ? Colors.red : Colors.blue,
                    ),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: SingleChildScrollView(
                    child: Text(
                      _result,
                      style: const TextStyle(fontFamily: 'monospace', fontSize: 12),
                    ),
                  ),
                ),
              ),
          ],
        ),
      ),
    );
  }

  @override
  void dispose() {
    _titleController.dispose();
    _contentController.dispose();
    super.dispose();
  }
}
```

This example demonstrates GraphQL query implementation with variables,  
mutations, and error handling. It includes a mock GraphQL client that  
shows proper query structure, variable passing, and response formatting.  

## Batch API Requests

Efficiently handling multiple simultaneous API requests.  

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
      title: 'Batch API Requests',
      home: const BatchRequestsPage(),
    );
  }
}

class BatchRequest {
  final String id;
  final String url;
  final String method;
  final Map<String, String>? headers;
  final String? body;

  BatchRequest({
    required this.id,
    required this.url,
    this.method = 'GET',
    this.headers,
    this.body,
  });
}

class BatchResponse {
  final String id;
  final int statusCode;
  final Map<String, String> headers;
  final String body;
  final Duration responseTime;
  final String? error;

  BatchResponse({
    required this.id,
    required this.statusCode,
    required this.headers,
    required this.body,
    required this.responseTime,
    this.error,
  });
}

class BatchApiService {
  static Future<List<BatchResponse>> executeBatch(
    List<BatchRequest> requests, {
    int? maxConcurrency,
  }) async {
    final results = <BatchResponse>[];
    
    if (maxConcurrency != null && maxConcurrency > 0) {
      // Execute with concurrency limit
      for (int i = 0; i < requests.length; i += maxConcurrency) {
        final batch = requests.sublist(
          i,
          (i + maxConcurrency < requests.length) 
              ? i + maxConcurrency 
              : requests.length,
        );
        
        final batchResults = await _executeBatch(batch);
        results.addAll(batchResults);
      }
    } else {
      // Execute all requests simultaneously
      results.addAll(await _executeBatch(requests));
    }
    
    return results;
  }

  static Future<List<BatchResponse>> _executeBatch(
    List<BatchRequest> requests,
  ) async {
    final futures = requests.map((request) => _executeRequest(request));
    return await Future.wait(futures);
  }

  static Future<BatchResponse> _executeRequest(BatchRequest request) async {
    final stopwatch = Stopwatch()..start();
    
    try {
      http.Response response;
      
      switch (request.method.toUpperCase()) {
        case 'GET':
          response = await http.get(
            Uri.parse(request.url),
            headers: request.headers,
          );
          break;
        case 'POST':
          response = await http.post(
            Uri.parse(request.url),
            headers: request.headers,
            body: request.body,
          );
          break;
        case 'PUT':
          response = await http.put(
            Uri.parse(request.url),
            headers: request.headers,
            body: request.body,
          );
          break;
        case 'DELETE':
          response = await http.delete(
            Uri.parse(request.url),
            headers: request.headers,
          );
          break;
        default:
          throw Exception('Unsupported method: ${request.method}');
      }
      
      stopwatch.stop();
      
      return BatchResponse(
        id: request.id,
        statusCode: response.statusCode,
        headers: response.headers,
        body: response.body,
        responseTime: stopwatch.elapsed,
      );
    } catch (e) {
      stopwatch.stop();
      
      return BatchResponse(
        id: request.id,
        statusCode: 0,
        headers: {},
        body: '',
        responseTime: stopwatch.elapsed,
        error: e.toString(),
      );
    }
  }
}

class BatchRequestsPage extends StatefulWidget {
  const BatchRequestsPage({super.key});

  @override
  State<BatchRequestsPage> createState() => _BatchRequestsPageState();
}

class _BatchRequestsPageState extends State<BatchRequestsPage> {
  List<BatchResponse> _results = [];
  bool _isLoading = false;
  Duration? _totalTime;
  int _maxConcurrency = 0; // 0 means unlimited

  Future<void> _executeBatchRequests() async {
    setState(() {
      _isLoading = true;
      _results = [];
      _totalTime = null;
    });

    final stopwatch = Stopwatch()..start();

    try {
      final requests = [
        BatchRequest(
          id: 'posts',
          url: 'https://jsonplaceholder.typicode.com/posts',
        ),
        BatchRequest(
          id: 'users',
          url: 'https://jsonplaceholder.typicode.com/users',
        ),
        BatchRequest(
          id: 'comments',
          url: 'https://jsonplaceholder.typicode.com/comments',
        ),
        BatchRequest(
          id: 'albums',
          url: 'https://jsonplaceholder.typicode.com/albums',
        ),
        BatchRequest(
          id: 'photos',
          url: 'https://jsonplaceholder.typicode.com/photos',
        ),
        BatchRequest(
          id: 'todos',
          url: 'https://jsonplaceholder.typicode.com/todos',
        ),
        BatchRequest(
          id: 'create_post',
          url: 'https://jsonplaceholder.typicode.com/posts',
          method: 'POST',
          headers: {'Content-Type': 'application/json'},
          body: json.encode({
            'title': 'Batch Test Post',
            'body': 'Created via batch request',
            'userId': 1,
          }),
        ),
      ];

      final results = await BatchApiService.executeBatch(
        requests,
        maxConcurrency: _maxConcurrency > 0 ? _maxConcurrency : null,
      );

      stopwatch.stop();

      setState(() {
        _results = results;
        _totalTime = stopwatch.elapsed;
      });
    } catch (e) {
      setState(() {
        _results = [
          BatchResponse(
            id: 'error',
            statusCode: 0,
            headers: {},
            body: '',
            responseTime: stopwatch.elapsed,
            error: e.toString(),
          ),
        ];
        _totalTime = stopwatch.elapsed;
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Widget _buildResultCard(BatchResponse response) {
    final isSuccess = response.error == null && response.statusCode >= 200 && response.statusCode < 300;
    final color = isSuccess ? Colors.green : Colors.red;
    
    return Card(
      child: ExpansionTile(
        leading: Icon(
          isSuccess ? Icons.check_circle : Icons.error,
          color: color,
        ),
        title: Text('${response.id.toUpperCase()}'),
        subtitle: Text(
          '${response.statusCode} • ${response.responseTime.inMilliseconds}ms',
          style: TextStyle(color: color),
        ),
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                if (response.error != null) ...[
                  const Text('Error:', style: TextStyle(fontWeight: FontWeight.bold)),
                  Text(response.error!, style: const TextStyle(color: Colors.red)),
                ] else ...[
                  const Text('Headers:', style: TextStyle(fontWeight: FontWeight.bold)),
                  ...response.headers.entries.take(3).map(
                    (entry) => Text('${entry.key}: ${entry.value}',
                        style: const TextStyle(fontSize: 12)),
                  ),
                  const SizedBox(height: 8),
                  const Text('Body Preview:', style: TextStyle(fontWeight: FontWeight.bold)),
                  Text(
                    response.body.length > 200 
                        ? '${response.body.substring(0, 200)}...' 
                        : response.body,
                    style: const TextStyle(fontSize: 12),
                  ),
                ],
              ],
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildSummary() {
    if (_results.isEmpty) return const SizedBox.shrink();
    
    final successful = _results.where((r) => r.error == null && r.statusCode >= 200 && r.statusCode < 300).length;
    final failed = _results.length - successful;
    final avgResponseTime = _results.map((r) => r.responseTime.inMilliseconds).reduce((a, b) => a + b) / _results.length;
    
    return Card(
      color: Colors.blue.withOpacity(0.1),
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text('Batch Summary:', style: TextStyle(fontWeight: FontWeight.bold)),
            const SizedBox(height: 8),
            Text('Total Requests: ${_results.length}'),
            Text('Successful: $successful', style: const TextStyle(color: Colors.green)),
            Text('Failed: $failed', style: TextStyle(color: failed > 0 ? Colors.red : null)),
            Text('Total Time: ${_totalTime?.inMilliseconds ?? 0}ms'),
            Text('Average Response Time: ${avgResponseTime.round()}ms'),
            Text('Concurrency Limit: ${_maxConcurrency > 0 ? _maxConcurrency : 'Unlimited'}'),
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Batch API Requests'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text('Concurrency Control:', style: TextStyle(fontWeight: FontWeight.bold)),
                    const SizedBox(height: 8),
                    Row(
                      children: [
                        const Text('Max Concurrent Requests: '),
                        Expanded(
                          child: Slider(
                            value: _maxConcurrency.toDouble(),
                            min: 0,
                            max: 7,
                            divisions: 7,
                            label: _maxConcurrency == 0 ? 'Unlimited' : _maxConcurrency.toString(),
                            onChanged: (value) {
                              setState(() {
                                _maxConcurrency = value.round();
                              });
                            },
                          ),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _isLoading ? null : _executeBatchRequests,
              child: _isLoading
                  ? const Row(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        SizedBox(
                          height: 20,
                          width: 20,
                          child: CircularProgressIndicator(strokeWidth: 2),
                        ),
                        SizedBox(width: 8),
                        Text('Executing Batch...'),
                      ],
                    )
                  : const Text('Execute Batch Requests'),
            ),
            const SizedBox(height: 16),
            _buildSummary(),
            const SizedBox(height: 16),
            Expanded(
              child: _results.isEmpty
                  ? const Center(child: Text('No results yet.\nExecute batch requests to see results.'))
                  : ListView.builder(
                      itemCount: _results.length,
                      itemBuilder: (context, index) {
                        return _buildResultCard(_results[index]);
                      },
                    ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates batch API request handling with concurrency  
control. It executes multiple requests simultaneously or with limits,  
providing detailed timing and success metrics. The UI allows adjusting  
concurrency levels and shows comprehensive results analysis.  

## Server-Sent Events (SSE)

Implementing server-sent events for real-time data streaming.  

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
      title: 'Server-Sent Events',
      home: const SSEPage(),
    );
  }
}

class SSEEvent {
  final String? id;
  final String event;
  final String data;
  final DateTime timestamp;

  SSEEvent({
    this.id,
    required this.event,
    required this.data,
    required this.timestamp,
  });
}

class SSEClient {
  final String url;
  StreamController<SSEEvent>? _controller;
  StreamSubscription? _subscription;
  http.Client? _client;

  SSEClient(this.url);

  Stream<SSEEvent> connect() {
    _controller = StreamController<SSEEvent>.broadcast();
    _client = http.Client();
    
    _startListening();
    
    return _controller!.stream;
  }

  void _startListening() async {
    try {
      final request = http.Request('GET', Uri.parse(url));
      request.headers['Accept'] = 'text/event-stream';
      request.headers['Cache-Control'] = 'no-cache';

      final response = await _client!.send(request);
      
      if (response.statusCode == 200) {
        _subscription = response.stream
            .transform(utf8.decoder)
            .transform(const LineSplitter())
            .listen(
              _processLine,
              onError: (error) => _controller?.addError(error),
              onDone: () => _controller?.close(),
            );
      } else {
        _controller?.addError('HTTP ${response.statusCode}');
      }
    } catch (e) {
      _controller?.addError(e);
    }
  }

  String _eventType = 'message';
  String _eventData = '';
  String? _eventId;

  void _processLine(String line) {
    if (line.isEmpty) {
      // Empty line indicates end of event
      if (_eventData.isNotEmpty) {
        _controller?.add(SSEEvent(
          id: _eventId,
          event: _eventType,
          data: _eventData,
          timestamp: DateTime.now(),
        ));
      }
      _resetEvent();
    } else if (line.startsWith('data: ')) {
      _eventData = line.substring(6);
    } else if (line.startsWith('event: ')) {
      _eventType = line.substring(7);
    } else if (line.startsWith('id: ')) {
      _eventId = line.substring(4);
    } else if (line.startsWith('retry: ')) {
      // Retry interval - could be implemented
    }
  }

  void _resetEvent() {
    _eventType = 'message';
    _eventData = '';
    _eventId = null;
  }

  void disconnect() {
    _subscription?.cancel();
    _client?.close();
    _controller?.close();
  }
}

// Mock SSE server for demonstration
class MockSSEServer {
  static Timer? _timer;
  static int _messageCount = 0;

  static Stream<String> generateMockEvents() async* {
    while (true) {
      await Future.delayed(const Duration(seconds: 2));
      
      _messageCount++;
      final eventType = _messageCount % 5 == 0 ? 'heartbeat' : 'message';
      
      if (eventType == 'heartbeat') {
        yield 'event: heartbeat\n';
        yield 'data: {"type":"heartbeat","timestamp":"${DateTime.now().toIso8601String()}"}\n';
        yield '\n';
      } else {
        yield 'id: $_messageCount\n';
        yield 'event: message\n';
        yield 'data: {"id":$_messageCount,"message":"Server message $_messageCount","timestamp":"${DateTime.now().toIso8601String()}"}\n';
        yield '\n';
      }
    }
  }
}

class SSEPage extends StatefulWidget {
  const SSEPage({super.key});

  @override
  State<SSEPage> createState() => _SSEPageState();
}

class _SSEPageState extends State<SSEPage> {
  SSEClient? _sseClient;
  StreamSubscription<SSEEvent>? _sseSubscription;
  final List<SSEEvent> _events = [];
  bool _isConnected = false;
  String _connectionStatus = 'Disconnected';
  int _messageCount = 0;
  int _heartbeatCount = 0;

  void _connect() {
    if (_isConnected) return;

    setState(() {
      _connectionStatus = 'Connecting...';
    });

    // Using httpbin.org/stream-bytes for demonstration
    // In real app, you would use your SSE endpoint
    _sseClient = SSEClient('https://httpbin.org/stream-bytes/1024');
    
    _sseSubscription = _sseClient!.connect().listen(
      (event) {
        setState(() {
          _events.insert(0, event);
          if (event.event == 'heartbeat') {
            _heartbeatCount++;
          } else {
            _messageCount++;
          }
        });
      },
      onError: (error) {
        setState(() {
          _connectionStatus = 'Error: $error';
          _isConnected = false;
        });
      },
      onDone: () {
        setState(() {
          _connectionStatus = 'Connection closed';
          _isConnected = false;
        });
      },
    );

    // Simulate connection success and mock events
    Timer(const Duration(seconds: 1), () {
      if (mounted) {
        setState(() {
          _connectionStatus = 'Connected';
          _isConnected = true;
        });
        _startMockEvents();
      }
    });
  }

  void _startMockEvents() {
    Timer.periodic(const Duration(seconds: 3), (timer) {
      if (!_isConnected) {
        timer.cancel();
        return;
      }

      final eventType = _messageCount % 4 == 0 ? 'heartbeat' : 'message';
      
      if (eventType == 'heartbeat') {
        final event = SSEEvent(
          event: 'heartbeat',
          data: json.encode({
            'type': 'heartbeat',
            'timestamp': DateTime.now().toIso8601String(),
          }),
          timestamp: DateTime.now(),
        );
        
        setState(() {
          _events.insert(0, event);
          _heartbeatCount++;
        });
      } else {
        _messageCount++;
        final event = SSEEvent(
          id: _messageCount.toString(),
          event: 'message',
          data: json.encode({
            'id': _messageCount,
            'message': 'Mock server message $_messageCount',
            'timestamp': DateTime.now().toIso8601String(),
          }),
          timestamp: DateTime.now(),
        );
        
        setState(() {
          _events.insert(0, event);
        });
      }
    });
  }

  void _disconnect() {
    _sseClient?.disconnect();
    _sseSubscription?.cancel();
    
    setState(() {
      _isConnected = false;
      _connectionStatus = 'Disconnected';
    });
  }

  Widget _buildConnectionStatus() {
    Color statusColor;
    IconData statusIcon;

    if (_isConnected) {
      statusColor = Colors.green;
      statusIcon = Icons.radio_button_checked;
    } else if (_connectionStatus.contains('Connecting')) {
      statusColor = Colors.orange;
      statusIcon = Icons.sync;
    } else if (_connectionStatus.contains('Error')) {
      statusColor = Colors.red;
      statusIcon = Icons.error;
    } else {
      statusColor = Colors.grey;
      statusIcon = Icons.radio_button_unchecked;
    }

    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Row(
              children: [
                Icon(statusIcon, color: statusColor),
                const SizedBox(width: 8),
                Text(
                  _connectionStatus,
                  style: TextStyle(
                    fontWeight: FontWeight.bold,
                    color: statusColor,
                  ),
                ),
                const Spacer(),
                if (!_isConnected)
                  ElevatedButton(
                    onPressed: _connect,
                    child: const Text('Connect'),
                  )
                else
                  ElevatedButton(
                    onPressed: _disconnect,
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.red.withOpacity(0.8),
                    ),
                    child: const Text('Disconnect'),
                  ),
              ],
            ),
            if (_isConnected) ...[
              const SizedBox(height: 12),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceAround,
                children: [
                  Column(
                    children: [
                      Text('$_messageCount', style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                      const Text('Messages', style: TextStyle(fontSize: 12)),
                    ],
                  ),
                  Column(
                    children: [
                      Text('$_heartbeatCount', style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                      const Text('Heartbeats', style: TextStyle(fontSize: 12)),
                    ],
                  ),
                  Column(
                    children: [
                      Text('${_events.length}', style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                      const Text('Total Events', style: TextStyle(fontSize: 12)),
                    ],
                  ),
                ],
              ),
            ],
          ],
        ),
      ),
    );
  }

  Widget _buildEventItem(SSEEvent event) {
    final isHeartbeat = event.event == 'heartbeat';
    final color = isHeartbeat ? Colors.blue : Colors.green;
    
    Map<String, dynamic>? parsedData;
    try {
      parsedData = json.decode(event.data);
    } catch (e) {
      // Data is not JSON
    }

    return Card(
      color: color.withOpacity(0.1),
      child: ListTile(
        leading: Icon(
          isHeartbeat ? Icons.favorite : Icons.message,
          color: color,
        ),
        title: Text(
          event.event.toUpperCase(),
          style: TextStyle(
            fontWeight: FontWeight.bold,
            color: color,
          ),
        ),
        subtitle: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            if (event.id != null)
              Text('ID: ${event.id}', style: const TextStyle(fontSize: 10)),
            const SizedBox(height: 4),
            Text(
              parsedData?['message'] ?? event.data,
              maxLines: 2,
              overflow: TextOverflow.ellipsis,
            ),
            const SizedBox(height: 4),
            Text(
              event.timestamp.toString().substring(11, 19),
              style: const TextStyle(fontSize: 10, color: Colors.grey),
            ),
          ],
        ),
        onTap: () {
          showDialog(
            context: context,
            builder: (context) => AlertDialog(
              title: Text('${event.event.toUpperCase()} Event'),
              content: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                mainAxisSize: MainAxisSize.min,
                children: [
                  if (event.id != null) Text('ID: ${event.id}'),
                  const SizedBox(height: 8),
                  const Text('Data:', style: TextStyle(fontWeight: FontWeight.bold)),
                  const SizedBox(height: 4),
                  Text(event.data),
                  const SizedBox(height: 8),
                  Text('Timestamp: ${event.timestamp}'),
                ],
              ),
              actions: [
                TextButton(
                  onPressed: () => Navigator.of(context).pop(),
                  child: const Text('Close'),
                ),
              ],
            ),
          );
        },
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Server-Sent Events'),
        actions: [
          IconButton(
            icon: const Icon(Icons.clear_all),
            onPressed: () {
              setState(() {
                _events.clear();
                _messageCount = 0;
                _heartbeatCount = 0;
              });
            },
          ),
        ],
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: _buildConnectionStatus(),
          ),
          Expanded(
            child: _events.isEmpty
                ? const Center(
                    child: Text(
                      'No events received yet.\nConnect to start receiving events.',
                      textAlign: TextAlign.center,
                    ),
                  )
                : ListView.builder(
                    itemCount: _events.length,
                    itemBuilder: (context, index) {
                      return Padding(
                        padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 2),
                        child: _buildEventItem(_events[index]),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }

  @override
  void dispose() {
    _disconnect();
    super.dispose();
  }
}
```

This example demonstrates Server-Sent Events (SSE) for real-time data  
streaming. It includes event parsing, connection management, and different  
event types like messages and heartbeats. The UI shows live connection  
status and event statistics.  

## Request/Response Logging

Comprehensive logging system for network requests and responses.  

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
      title: 'Request Logging',
      home: const RequestLoggingPage(),
    );
  }
}

enum LogLevel { debug, info, warning, error }

class NetworkLog {
  final String id;
  final DateTime timestamp;
  final String method;
  final String url;
  final Map<String, String> requestHeaders;
  final String? requestBody;
  final int? responseStatusCode;
  final Map<String, String> responseHeaders;
  final String? responseBody;
  final Duration? responseTime;
  final String? error;
  final LogLevel level;

  NetworkLog({
    required this.id,
    required this.timestamp,
    required this.method,
    required this.url,
    required this.requestHeaders,
    this.requestBody,
    this.responseStatusCode,
    this.responseHeaders = const {},
    this.responseBody,
    this.responseTime,
    this.error,
    required this.level,
  });

  bool get isSuccess => 
      error == null && 
      responseStatusCode != null && 
      responseStatusCode! >= 200 && 
      responseStatusCode! < 300;
}

class NetworkLogger {
  static final List<NetworkLog> _logs = [];
  static const int maxLogs = 100;

  static void logRequest(
    String method,
    String url,
    Map<String, String> headers,
    String? body,
  ) {
    final log = NetworkLog(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      timestamp: DateTime.now(),
      method: method,
      url: url,
      requestHeaders: headers,
      requestBody: body,
      level: LogLevel.info,
    );

    _addLog(log);
    print('→ $method $url');
    if (body != null) print('  Body: $body');
  }

  static void logResponse(
    String requestId,
    int statusCode,
    Map<String, String> headers,
    String body,
    Duration responseTime,
  ) {
    final existingLogIndex = _logs.indexWhere((log) => log.id == requestId);
    
    if (existingLogIndex != -1) {
      final existingLog = _logs[existingLogIndex];
      final updatedLog = NetworkLog(
        id: existingLog.id,
        timestamp: existingLog.timestamp,
        method: existingLog.method,
        url: existingLog.url,
        requestHeaders: existingLog.requestHeaders,
        requestBody: existingLog.requestBody,
        responseStatusCode: statusCode,
        responseHeaders: headers,
        responseBody: body,
        responseTime: responseTime,
        level: statusCode >= 400 ? LogLevel.error : LogLevel.info,
      );
      
      _logs[existingLogIndex] = updatedLog;
    }

    print('← $statusCode • ${responseTime.inMilliseconds}ms');
    print('  Response: ${body.length > 100 ? body.substring(0, 100) + '...' : body}');
  }

  static void logError(String requestId, String error) {
    final existingLogIndex = _logs.indexWhere((log) => log.id == requestId);
    
    if (existingLogIndex != -1) {
      final existingLog = _logs[existingLogIndex];
      final updatedLog = NetworkLog(
        id: existingLog.id,
        timestamp: existingLog.timestamp,
        method: existingLog.method,
        url: existingLog.url,
        requestHeaders: existingLog.requestHeaders,
        requestBody: existingLog.requestBody,
        error: error,
        level: LogLevel.error,
      );
      
      _logs[existingLogIndex] = updatedLog;
    } else {
      _addLog(NetworkLog(
        id: requestId,
        timestamp: DateTime.now(),
        method: 'UNKNOWN',
        url: 'UNKNOWN',
        requestHeaders: {},
        error: error,
        level: LogLevel.error,
      ));
    }

    print('✗ Error: $error');
  }

  static void _addLog(NetworkLog log) {
    _logs.insert(0, log);
    if (_logs.length > maxLogs) {
      _logs.removeRange(maxLogs, _logs.length);
    }
  }

  static List<NetworkLog> get logs => List.unmodifiable(_logs);

  static void clearLogs() {
    _logs.clear();
  }

  static List<NetworkLog> getLogsByLevel(LogLevel level) {
    return _logs.where((log) => log.level == level).toList();
  }

  static List<NetworkLog> getLogsInTimeRange(DateTime start, DateTime end) {
    return _logs.where((log) => 
        log.timestamp.isAfter(start) && log.timestamp.isBefore(end)
    ).toList();
  }
}

class LoggingHttpClient extends http.BaseClient {
  final http.Client _inner = http.Client();

  @override
  Future<http.StreamedResponse> send(http.BaseRequest request) async {
    final requestId = DateTime.now().millisecondsSinceEpoch.toString();
    final startTime = DateTime.now();

    NetworkLogger.logRequest(
      request.method,
      request.url.toString(),
      request.headers,
      request is http.Request ? request.body : null,
    );

    try {
      final response = await _inner.send(request);
      final responseBody = await response.stream.bytesToString();
      final endTime = DateTime.now();
      final responseTime = endTime.difference(startTime);

      NetworkLogger.logResponse(
        requestId,
        response.statusCode,
        response.headers,
        responseBody,
        responseTime,
      );

      return http.StreamedResponse(
        Stream.value(utf8.encode(responseBody)),
        response.statusCode,
        headers: response.headers,
        isRedirect: response.isRedirect,
        persistentConnection: response.persistentConnection,
        reasonPhrase: response.reasonPhrase,
      );
    } catch (error) {
      NetworkLogger.logError(requestId, error.toString());
      rethrow;
    }
  }

  @override
  void close() {
    _inner.close();
    super.close();
  }
}

class RequestLoggingPage extends StatefulWidget {
  const RequestLoggingPage({super.key});

  @override
  State<RequestLoggingPage> createState() => _RequestLoggingPageState();
}

class _RequestLoggingPageState extends State<RequestLoggingPage> {
  late LoggingHttpClient _client;
  LogLevel _selectedLevel = LogLevel.info;
  bool _isLoading = false;

  @override
  void initState() {
    super.initState();
    _client = LoggingHttpClient();
  }

  Future<void> _makeSuccessRequest() async {
    setState(() {
      _isLoading = true;
    });

    try {
      await _client.get(Uri.parse('https://jsonplaceholder.typicode.com/posts/1'));
    } catch (e) {
      // Error already logged
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
      // Error already logged
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
        body: json.encode({
          'title': 'Test Post',
          'body': 'This is a test post with logging',
          'userId': 1,
        }),
      );
    } catch (e) {
      // Error already logged
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Widget _buildLogLevelFilter() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text('Filter by Log Level:', style: TextStyle(fontWeight: FontWeight.bold)),
            const SizedBox(height: 8),
            Wrap(
              spacing: 8,
              children: LogLevel.values.map((level) {
                final count = NetworkLogger.getLogsByLevel(level).length;
                return FilterChip(
                  label: Text('${level.name.toUpperCase()} ($count)'),
                  selected: _selectedLevel == level,
                  onSelected: (selected) {
                    if (selected) {
                      setState(() {
                        _selectedLevel = level;
                      });
                    }
                  },
                );
              }).toList(),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildLogItem(NetworkLog log) {
    Color levelColor;
    IconData levelIcon;

    switch (log.level) {
      case LogLevel.debug:
        levelColor = Colors.grey;
        levelIcon = Icons.bug_report;
        break;
      case LogLevel.info:
        levelColor = log.isSuccess ? Colors.green : Colors.blue;
        levelIcon = Icons.info;
        break;
      case LogLevel.warning:
        levelColor = Colors.orange;
        levelIcon = Icons.warning;
        break;
      case LogLevel.error:
        levelColor = Colors.red;
        levelIcon = Icons.error;
        break;
    }

    return Card(
      color: levelColor.withOpacity(0.1),
      child: ExpansionTile(
        leading: Icon(levelIcon, color: levelColor),
        title: Text('${log.method} ${Uri.parse(log.url).path}'),
        subtitle: Text(
          '${log.timestamp.toString().substring(11, 19)} • '
          '${log.responseStatusCode ?? 'Pending'} • '
          '${log.responseTime?.inMilliseconds ?? 0}ms',
          style: TextStyle(fontSize: 12, color: levelColor),
        ),
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                const Text('Request:', style: TextStyle(fontWeight: FontWeight.bold)),
                Text('URL: ${log.url}', style: const TextStyle(fontSize: 12)),
                const SizedBox(height: 4),
                const Text('Headers:', style: TextStyle(fontWeight: FontWeight.bold)),
                ...log.requestHeaders.entries.take(3).map(
                  (entry) => Text('${entry.key}: ${entry.value}',
                      style: const TextStyle(fontSize: 12)),
                ),
                if (log.requestBody != null) ...[
                  const SizedBox(height: 8),
                  const Text('Body:', style: TextStyle(fontWeight: FontWeight.bold)),
                  Text(log.requestBody!, style: const TextStyle(fontSize: 12)),
                ],
                const SizedBox(height: 12),
                if (log.error != null) ...[
                  const Text('Error:', style: TextStyle(fontWeight: FontWeight.bold, color: Colors.red)),
                  Text(log.error!, style: const TextStyle(fontSize: 12, color: Colors.red)),
                ] else if (log.responseBody != null) ...[
                  const Text('Response:', style: TextStyle(fontWeight: FontWeight.bold)),
                  Text(
                    log.responseBody!.length > 200 
                        ? '${log.responseBody!.substring(0, 200)}...'
                        : log.responseBody!,
                    style: const TextStyle(fontSize: 12),
                  ),
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
    final filteredLogs = _selectedLevel == LogLevel.info 
        ? NetworkLogger.logs
        : NetworkLogger.getLogsByLevel(_selectedLevel);

    return Scaffold(
      appBar: AppBar(
        title: const Text('Request Logging'),
        actions: [
          IconButton(
            icon: const Icon(Icons.clear_all),
            onPressed: () {
              NetworkLogger.clearLogs();
              setState(() {});
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
                        onPressed: _isLoading ? null : _makeSuccessRequest,
                        child: const Text('Success Request'),
                      ),
                    ),
                    const SizedBox(width: 8),
                    Expanded(
                      child: ElevatedButton(
                        onPressed: _isLoading ? null : _makeErrorRequest,
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.red.withOpacity(0.8),
                        ),
                        child: const Text('Error Request'),
                      ),
                    ),
                  ],
                ),
                const SizedBox(height: 8),
                ElevatedButton(
                  onPressed: _isLoading ? null : _makePostRequest,
                  child: const Text('POST Request'),
                ),
              ],
            ),
          ),
          if (_isLoading)
            const Padding(
              padding: EdgeInsets.all(16.0),
              child: CircularProgressIndicator(),
            ),
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 16.0),
            child: _buildLogLevelFilter(),
          ),
          Expanded(
            child: filteredLogs.isEmpty
                ? const Center(child: Text('No logs available'))
                : ListView.builder(
                    itemCount: filteredLogs.length,
                    itemBuilder: (context, index) {
                      return Padding(
                        padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 2),
                        child: _buildLogItem(filteredLogs[index]),
                      );
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

This example demonstrates comprehensive request/response logging with  
filtering capabilities. It tracks all network activity with detailed  
information including headers, body content, timing, and error states.  
The logging system supports different log levels and provides searchable  
history for debugging purposes.  

## REST API Client Pattern

Structured REST API client with resource-based organization.  

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
      title: 'REST API Client',
      home: const ApiClientPage(),
    );
  }
}

class ApiException implements Exception {
  final int statusCode;
  final String message;
  final Map<String, dynamic>? errorDetails;

  ApiException(this.statusCode, this.message, [this.errorDetails]);

  @override
  String toString() => 'ApiException: $statusCode - $message';
}

abstract class BaseApiClient {
  final String baseUrl;
  final Map<String, String> defaultHeaders;

  BaseApiClient({
    required this.baseUrl,
    Map<String, String>? headers,
  }) : defaultHeaders = {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
          ...?headers,
        };

  Future<Map<String, dynamic>> get(String endpoint) async {
    return _makeRequest('GET', endpoint);
  }

  Future<Map<String, dynamic>> post(String endpoint, {dynamic body}) async {
    return _makeRequest('POST', endpoint, body: body);
  }

  Future<Map<String, dynamic>> put(String endpoint, {dynamic body}) async {
    return _makeRequest('PUT', endpoint, body: body);
  }

  Future<Map<String, dynamic>> delete(String endpoint) async {
    return _makeRequest('DELETE', endpoint);
  }

  Future<Map<String, dynamic>> _makeRequest(
    String method,
    String endpoint, {
    dynamic body,
  }) async {
    final url = Uri.parse('$baseUrl$endpoint');
    
    try {
      http.Response response;

      switch (method) {
        case 'GET':
          response = await http.get(url, headers: defaultHeaders);
          break;
        case 'POST':
          response = await http.post(
            url,
            headers: defaultHeaders,
            body: body != null ? json.encode(body) : null,
          );
          break;
        case 'PUT':
          response = await http.put(
            url,
            headers: defaultHeaders,
            body: body != null ? json.encode(body) : null,
          );
          break;
        case 'DELETE':
          response = await http.delete(url, headers: defaultHeaders);
          break;
        default:
          throw ApiException(0, 'Unsupported HTTP method: $method');
      }

      return _handleResponse(response);
    } catch (e) {
      if (e is ApiException) rethrow;
      throw ApiException(0, 'Network error: $e');
    }
  }

  Map<String, dynamic> _handleResponse(http.Response response) {
    if (response.statusCode >= 200 && response.statusCode < 300) {
      if (response.body.isEmpty) return {};
      
      try {
        return json.decode(response.body);
      } catch (e) {
        throw ApiException(
          response.statusCode,
          'Invalid JSON response',
          {'originalBody': response.body},
        );
      }
    } else {
      String errorMessage = 'HTTP ${response.statusCode}';
      Map<String, dynamic>? errorDetails;

      try {
        errorDetails = json.decode(response.body);
        errorMessage = errorDetails['message'] ?? errorMessage;
      } catch (e) {
        // Response body is not JSON
        errorDetails = {'body': response.body};
      }

      throw ApiException(response.statusCode, errorMessage, errorDetails);
    }
  }
}

// Models
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

class User {
  final int id;
  final String name;
  final String email;
  final String phone;

  User({
    required this.id,
    required this.name,
    required this.email,
    required this.phone,
  });

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'],
      name: json['name'],
      email: json['email'],
      phone: json['phone'],
    );
  }
}

// Resource Services
class PostService extends BaseApiClient {
  PostService() : super(baseUrl: 'https://jsonplaceholder.typicode.com');

  Future<List<Post>> getAllPosts() async {
    final response = await get('/posts');
    return (response as List).map((json) => Post.fromJson(json)).toList();
  }

  Future<Post> getPost(int id) async {
    final response = await get('/posts/$id');
    return Post.fromJson(response);
  }

  Future<Post> createPost(String title, String body, int userId) async {
    final response = await post('/posts', body: {
      'title': title,
      'body': body,
      'userId': userId,
    });
    return Post.fromJson(response);
  }

  Future<Post> updatePost(Post post) async {
    final response = await put('/posts/${post.id}', body: post.toJson());
    return Post.fromJson(response);
  }

  Future<void> deletePost(int id) async {
    await delete('/posts/$id');
  }

  Future<List<Post>> getPostsByUser(int userId) async {
    final response = await get('/posts?userId=$userId');
    return (response as List).map((json) => Post.fromJson(json)).toList();
  }
}

class UserService extends BaseApiClient {
  UserService() : super(baseUrl: 'https://jsonplaceholder.typicode.com');

  Future<List<User>> getAllUsers() async {
    final response = await get('/users');
    return (response as List).map((json) => User.fromJson(json)).toList();
  }

  Future<User> getUser(int id) async {
    final response = await get('/users/$id');
    return User.fromJson(response);
  }
}

// Combined API Client
class ApiClient {
  late final PostService posts;
  late final UserService users;

  ApiClient() {
    posts = PostService();
    users = UserService();
  }
}

class ApiClientPage extends StatefulWidget {
  const ApiClientPage({super.key});

  @override
  State<ApiClientPage> createState() => _ApiClientPageState();
}

class _ApiClientPageState extends State<ApiClientPage> {
  late final ApiClient _apiClient;
  String _result = '';
  bool _isLoading = false;

  @override
  void initState() {
    super.initState();
    _apiClient = ApiClient();
  }

  Future<void> _executeOperation(String operation) async {
    setState(() {
      _isLoading = true;
      _result = '';
    });

    try {
      switch (operation) {
        case 'getAllPosts':
          final posts = await _apiClient.posts.getAllPosts();
          setState(() {
            _result = 'Fetched ${posts.length} posts:\n\n' +
                posts.take(3).map((p) => '${p.id}. ${p.title}').join('\n');
          });
          break;

        case 'getPost':
          final post = await _apiClient.posts.getPost(1);
          setState(() {
            _result = 'Post Details:\n\n'
                'ID: ${post.id}\n'
                'Title: ${post.title}\n'
                'Body: ${post.body}\n'
                'User ID: ${post.userId}';
          });
          break;

        case 'createPost':
          final newPost = await _apiClient.posts.createPost(
            'Flutter REST Example',
            'This post was created using the REST API client pattern.',
            1,
          );
          setState(() {
            _result = 'Created Post:\n\n'
                'ID: ${newPost.id}\n'
                'Title: ${newPost.title}\n'
                'Body: ${newPost.body}';
          });
          break;

        case 'getUsers':
          final users = await _apiClient.users.getAllUsers();
          setState(() {
            _result = 'Fetched ${users.length} users:\n\n' +
                users.take(5).map((u) => '${u.id}. ${u.name} (${u.email})').join('\n');
          });
          break;

        case 'getUserPosts':
          final user = await _apiClient.users.getUser(1);
          final posts = await _apiClient.posts.getPostsByUser(1);
          setState(() {
            _result = 'Posts by ${user.name}:\n\n' +
                posts.take(3).map((p) => '• ${p.title}').join('\n');
          });
          break;

        case 'errorTest':
          await _apiClient.posts.getPost(999999); // Non-existent post
          break;
      }
    } catch (e) {
      setState(() {
        if (e is ApiException) {
          _result = 'API Error:\n\n'
              'Status: ${e.statusCode}\n'
              'Message: ${e.message}\n'
              '${e.errorDetails != null ? 'Details: ${e.errorDetails}' : ''}';
        } else {
          _result = 'Error: $e';
        }
      });
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Widget _buildOperationButton(String title, String operation, {Color? color}) {
    return Expanded(
      child: ElevatedButton(
        onPressed: _isLoading ? null : () => _executeOperation(operation),
        style: color != null
            ? ElevatedButton.styleFrom(backgroundColor: color.withOpacity(0.8))
            : null,
        child: Text(
          title,
          textAlign: TextAlign.center,
          style: const TextStyle(fontSize: 12),
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('REST API Client'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Card(
              child: Padding(
                padding: EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'REST API Client Pattern:',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    SizedBox(height: 8),
                    Text('• Resource-based service organization'),
                    Text('• Consistent error handling'),
                    Text('• Type-safe model classes'),
                    Text('• Reusable HTTP client base'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            const Text(
              'Post Operations:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                _buildOperationButton('Get All\nPosts', 'getAllPosts'),
                const SizedBox(width: 8),
                _buildOperationButton('Get Single\nPost', 'getPost'),
                const SizedBox(width: 8),
                _buildOperationButton('Create\nPost', 'createPost'),
              ],
            ),
            const SizedBox(height: 16),
            const Text(
              'User Operations:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                _buildOperationButton('Get All\nUsers', 'getUsers'),
                const SizedBox(width: 8),
                _buildOperationButton('User\'s\nPosts', 'getUserPosts'),
                const SizedBox(width: 8),
                _buildOperationButton('Error\nTest', 'errorTest', color: Colors.red),
              ],
            ),
            const SizedBox(height: 20),
            if (_isLoading)
              const Center(child: CircularProgressIndicator())
            else if (_result.isNotEmpty)
              Expanded(
                child: Container(
                  padding: const EdgeInsets.all(16),
                  decoration: BoxDecoration(
                    color: _result.contains('Error')
                        ? Colors.red.withOpacity(0.1)
                        : Colors.green.withOpacity(0.1),
                    border: Border.all(
                      color: _result.contains('Error') ? Colors.red : Colors.green,
                    ),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: SingleChildScrollView(
                    child: Text(
                      _result,
                      style: const TextStyle(fontSize: 14),
                    ),
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

This example demonstrates a structured REST API client pattern with  
resource-based organization. It includes type-safe model classes,  
consistent error handling, and reusable service patterns. The client  
provides a clean interface for different API resources.  

## Network Image Caching

Advanced image loading with caching and placeholder handling.  

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:typed_data';
import 'dart:ui' as ui;

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
      title: 'Network Image Caching',
      home: const ImageCachingPage(),
    );
  }
}

class CachedImageData {
  final Uint8List bytes;
  final DateTime timestamp;
  final String contentType;
  final int sizeInBytes;

  CachedImageData({
    required this.bytes,
    required this.timestamp,
    required this.contentType,
    required this.sizeInBytes,
  });

  bool isExpired(Duration maxAge) {
    return DateTime.now().difference(timestamp) > maxAge;
  }
}

class ImageCache {
  static final Map<String, CachedImageData> _cache = {};
  static const Duration defaultMaxAge = Duration(hours: 1);
  static const int maxCacheSize = 50 * 1024 * 1024; // 50MB

  static void put(String url, Uint8List bytes, String contentType) {
    _cleanupExpired();
    _enforceSizeLimit();

    _cache[url] = CachedImageData(
      bytes: bytes,
      timestamp: DateTime.now(),
      contentType: contentType,
      sizeInBytes: bytes.length,
    );
  }

  static CachedImageData? get(String url, {Duration? maxAge}) {
    final cached = _cache[url];
    if (cached == null) return null;

    if (cached.isExpired(maxAge ?? defaultMaxAge)) {
      _cache.remove(url);
      return null;
    }

    return cached;
  }

  static void _cleanupExpired() {
    final now = DateTime.now();
    _cache.removeWhere((key, value) => 
        now.difference(value.timestamp) > defaultMaxAge);
  }

  static void _enforceSizeLimit() {
    int totalSize = _cache.values.fold(0, (sum, data) => sum + data.sizeInBytes);
    
    if (totalSize > maxCacheSize) {
      // Remove oldest entries until under limit
      final entries = _cache.entries.toList();
      entries.sort((a, b) => a.value.timestamp.compareTo(b.value.timestamp));
      
      for (final entry in entries) {
        _cache.remove(entry.key);
        totalSize -= entry.value.sizeInBytes;
        if (totalSize <= maxCacheSize * 0.8) break; // Remove to 80% of limit
      }
    }
  }

  static void clear() {
    _cache.clear();
  }

  static int get cacheSize => _cache.length;
  static int get cacheSizeInBytes => 
      _cache.values.fold(0, (sum, data) => sum + data.sizeInBytes);

  static Map<String, DateTime> getCacheInfo() {
    return Map.fromEntries(
      _cache.entries.map((e) => MapEntry(e.key, e.value.timestamp)),
    );
  }
}

class CachedNetworkImage extends StatefulWidget {
  final String imageUrl;
  final double? width;
  final double? height;
  final BoxFit fit;
  final Widget? placeholder;
  final Widget? errorWidget;

  const CachedNetworkImage({
    super.key,
    required this.imageUrl,
    this.width,
    this.height,
    this.fit = BoxFit.cover,
    this.placeholder,
    this.errorWidget,
  });

  @override
  State<CachedNetworkImage> createState() => _CachedNetworkImageState();
}

class _CachedNetworkImageState extends State<CachedNetworkImage> {
  Uint8List? _imageBytes;
  bool _isLoading = true;
  String? _error;

  @override
  void initState() {
    super.initState();
    _loadImage();
  }

  Future<void> _loadImage() async {
    setState(() {
      _isLoading = true;
      _error = null;
    });

    try {
      // Check cache first
      final cached = ImageCache.get(widget.imageUrl);
      if (cached != null) {
        setState(() {
          _imageBytes = cached.bytes;
          _isLoading = false;
        });
        return;
      }

      // Load from network
      final response = await http.get(Uri.parse(widget.imageUrl));
      
      if (response.statusCode == 200) {
        final bytes = response.bodyBytes;
        final contentType = response.headers['content-type'] ?? 'image/jpeg';
        
        // Cache the image
        ImageCache.put(widget.imageUrl, bytes, contentType);
        
        setState(() {
          _imageBytes = bytes;
          _isLoading = false;
        });
      } else {
        throw Exception('HTTP ${response.statusCode}');
      }
    } catch (e) {
      setState(() {
        _error = e.toString();
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    if (_isLoading) {
      return widget.placeholder ?? 
          Container(
            width: widget.width,
            height: widget.height,
            decoration: BoxDecoration(
              color: Colors.grey.withOpacity(0.3),
              borderRadius: BorderRadius.circular(8),
            ),
            child: const Center(
              child: CircularProgressIndicator(),
            ),
          );
    }

    if (_error != null) {
      return widget.errorWidget ?? 
          Container(
            width: widget.width,
            height: widget.height,
            decoration: BoxDecoration(
              color: Colors.red.withOpacity(0.1),
              border: Border.all(color: Colors.red),
              borderRadius: BorderRadius.circular(8),
            ),
            child: const Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(Icons.error, color: Colors.red),
                SizedBox(height: 8),
                Text('Failed to load image', style: TextStyle(fontSize: 12)),
              ],
            ),
          );
    }

    if (_imageBytes != null) {
      return Image.memory(
        _imageBytes!,
        width: widget.width,
        height: widget.height,
        fit: widget.fit,
      );
    }

    return const SizedBox.shrink();
  }
}

class ImageCachingPage extends StatefulWidget {
  const ImageCachingPage({super.key});

  @override
  State<ImageCachingPage> createState() => _ImageCachingPageState();
}

class _ImageCachingPageState extends State<ImageCachingPage> {
  final List<String> _imageUrls = [
    'https://picsum.photos/300/200?random=1',
    'https://picsum.photos/300/200?random=2',
    'https://picsum.photos/300/200?random=3',
    'https://picsum.photos/300/200?random=4',
    'https://picsum.photos/300/200?random=5',
    'https://picsum.photos/300/200?random=6',
    'https://httpbin.org/status/404', // Error URL
    'https://picsum.photos/300/200?random=7',
  ];

  Widget _buildCacheInfo() {
    final cacheInfo = ImageCache.getCacheInfo();
    final cacheSizeInMB = ImageCache.cacheSizeInBytes / (1024 * 1024);

    return Card(
      child: ExpansionTile(
        title: Text('Image Cache (${ImageCache.cacheSize} images)'),
        subtitle: Text('Size: ${cacheSizeInMB.toStringAsFixed(2)} MB'),
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                ...cacheInfo.entries.map((entry) {
                  final age = DateTime.now().difference(entry.value);
                  final url = entry.key;
                  final displayUrl = url.length > 40 
                      ? '${url.substring(0, 37)}...' 
                      : url;
                  
                  return Padding(
                    padding: const EdgeInsets.symmetric(vertical: 2.0),
                    child: Row(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: [
                        Expanded(
                          child: Text(
                            displayUrl,
                            style: const TextStyle(fontSize: 12),
                            overflow: TextOverflow.ellipsis,
                          ),
                        ),
                        Text(
                          '${age.inMinutes}m ago',
                          style: const TextStyle(fontSize: 10),
                        ),
                      ],
                    ),
                  );
                }),
                if (cacheInfo.isEmpty)
                  const Text('No cached images'),
                const SizedBox(height: 16),
                Row(
                  children: [
                    ElevatedButton(
                      onPressed: () {
                        ImageCache.clear();
                        setState(() {});
                      },
                      style: ElevatedButton.styleFrom(
                        backgroundColor: Colors.red.withOpacity(0.8),
                      ),
                      child: const Text('Clear Cache'),
                    ),
                    const SizedBox(width: 8),
                    ElevatedButton(
                      onPressed: () {
                        setState(() {});
                      },
                      child: const Text('Refresh'),
                    ),
                  ],
                ),
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
        title: const Text('Network Image Caching'),
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: _buildCacheInfo(),
          ),
          Expanded(
            child: GridView.builder(
              padding: const EdgeInsets.all(16.0),
              gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 2,
                crossAxisSpacing: 16,
                mainAxisSpacing: 16,
                childAspectRatio: 3 / 2,
              ),
              itemCount: _imageUrls.length,
              itemBuilder: (context, index) {
                return Card(
                  clipBehavior: Clip.antiAlias,
                  child: Column(
                    children: [
                      Expanded(
                        child: CachedNetworkImage(
                          imageUrl: _imageUrls[index],
                          width: double.infinity,
                          fit: BoxFit.cover,
                          placeholder: Container(
                            color: Colors.grey.withOpacity(0.3),
                            child: const Column(
                              mainAxisAlignment: MainAxisAlignment.center,
                              children: [
                                CircularProgressIndicator(),
                                SizedBox(height: 8),
                                Text('Loading...', style: TextStyle(fontSize: 12)),
                              ],
                            ),
                          ),
                          errorWidget: Container(
                            color: Colors.red.withOpacity(0.1),
                            child: const Column(
                              mainAxisAlignment: MainAxisAlignment.center,
                              children: [
                                Icon(Icons.error, color: Colors.red),
                                SizedBox(height: 8),
                                Text('Error', style: TextStyle(fontSize: 12)),
                              ],
                            ),
                          ),
                        ),
                      ),
                      Padding(
                        padding: const EdgeInsets.all(8.0),
                        child: Text(
                          'Image ${index + 1}',
                          style: const TextStyle(fontSize: 12),
                        ),
                      ),
                    ],
                  ),
                );
              },
            ),
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          setState(() {});
        },
        child: const Icon(Icons.refresh),
      ),
    );
  }
}
```

This example demonstrates advanced network image caching with automatic  
cache management. It includes size limits, expiration handling, placeholder  
support, and error handling. The cache optimizes memory usage and provides  
efficient image loading with fallback options.  
