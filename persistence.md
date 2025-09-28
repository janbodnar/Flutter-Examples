# Flutter Persistence & Local Storage - 25 Essential Examples

This collection provides 25 comprehensive examples covering persistence and  
local storage in Flutter applications. Each example demonstrates different  
storage mechanisms and patterns, progressing from simple to complex scenarios.  

## Basic SharedPreferences

Storing simple key-value pairs locally with SharedPreferences.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

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
      title: 'Basic SharedPreferences',
      home: const PreferencesPage(),
    );
  }
}

class PreferencesPage extends StatefulWidget {
  const PreferencesPage({super.key});

  @override
  State<PreferencesPage> createState() => _PreferencesPageState();
}

class _PreferencesPageState extends State<PreferencesPage> {
  final _textController = TextEditingController();
  String _savedText = '';
  int _counter = 0;
  bool _isEnabled = false;

  @override
  void initState() {
    super.initState();
    _loadPreferences();
  }

  Future<void> _loadPreferences() async {
    final prefs = await SharedPreferences.getInstance();
    setState(() {
      _savedText = prefs.getString('saved_text') ?? '';
      _counter = prefs.getInt('counter') ?? 0;
      _isEnabled = prefs.getBool('is_enabled') ?? false;
      _textController.text = _savedText;
    });
  }

  Future<void> _savePreferences() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setString('saved_text', _textController.text);
    await prefs.setInt('counter', _counter);
    await prefs.setBool('is_enabled', _isEnabled);
    
    setState(() {
      _savedText = _textController.text;
    });

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Preferences saved!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic SharedPreferences'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _textController,
              decoration: const InputDecoration(
                labelText: 'Enter text',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Text('Counter: $_counter'),
                const Spacer(),
                IconButton(
                  onPressed: () {
                    setState(() {
                      _counter++;
                    });
                  },
                  icon: const Icon(Icons.add),
                ),
              ],
            ),
            SwitchListTile(
              title: const Text('Enable feature'),
              value: _isEnabled,
              onChanged: (value) {
                setState(() {
                  _isEnabled = value;
                });
              },
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _savePreferences,
              child: const Text('Save All Preferences'),
            ),
          ],
        ),
      ),
    );
  }

  @override
  void dispose() {
    _textController.dispose();
    super.dispose();
  }
}
```

SharedPreferences provides simple key-value storage for basic data types  
including strings, integers, booleans, and lists. Data persists between app  
launches and is perfect for user settings and simple application state.  

## User Settings Manager

Managing complex user settings with structured preferences.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class UserSettings {
  final String username;
  final String email;
  final bool notifications;
  final bool darkMode;
  final double fontSize;
  final String language;

  UserSettings({
    required this.username,
    required this.email,
    required this.notifications,
    required this.darkMode,
    required this.fontSize,
    required this.language,
  });

  Map<String, dynamic> toMap() {
    return {
      'username': username,
      'email': email,
      'notifications': notifications,
      'darkMode': darkMode,
      'fontSize': fontSize,
      'language': language,
    };
  }

  factory UserSettings.fromMap(Map<String, dynamic> map) {
    return UserSettings(
      username: map['username'] ?? '',
      email: map['email'] ?? '',
      notifications: map['notifications'] ?? true,
      darkMode: map['darkMode'] ?? false,
      fontSize: map['fontSize']?.toDouble() ?? 14.0,
      language: map['language'] ?? 'English',
    );
  }

  String toJson() => json.encode(toMap());

  factory UserSettings.fromJson(String source) => 
      UserSettings.fromMap(json.decode(source));

  static UserSettings get defaultSettings => UserSettings(
    username: '',
    email: '',
    notifications: true,
    darkMode: false,
    fontSize: 14.0,
    language: 'English',
  );

  UserSettings copyWith({
    String? username,
    String? email,
    bool? notifications,
    bool? darkMode,
    double? fontSize,
    String? language,
  }) {
    return UserSettings(
      username: username ?? this.username,
      email: email ?? this.email,
      notifications: notifications ?? this.notifications,
      darkMode: darkMode ?? this.darkMode,
      fontSize: fontSize ?? this.fontSize,
      language: language ?? this.language,
    );
  }
}

class SettingsManager {
  static const String _settingsKey = 'user_settings';

  static Future<UserSettings> loadSettings() async {
    final prefs = await SharedPreferences.getInstance();
    final settingsJson = prefs.getString(_settingsKey);
    
    if (settingsJson != null) {
      try {
        return UserSettings.fromJson(settingsJson);
      } catch (e) {
        return UserSettings.defaultSettings;
      }
    }
    
    return UserSettings.defaultSettings;
  }

  static Future<void> saveSettings(UserSettings settings) async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setString(_settingsKey, settings.toJson());
  }

  static Future<void> clearSettings() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.remove(_settingsKey);
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'User Settings Manager',
      home: const SettingsPage(),
    );
  }
}

class SettingsPage extends StatefulWidget {
  const SettingsPage({super.key});

  @override
  State<SettingsPage> createState() => _SettingsPageState();
}

class _SettingsPageState extends State<SettingsPage> {
  late UserSettings _settings;
  bool _isLoading = true;
  final _usernameController = TextEditingController();
  final _emailController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _loadSettings();
  }

  Future<void> _loadSettings() async {
    _settings = await SettingsManager.loadSettings();
    _usernameController.text = _settings.username;
    _emailController.text = _settings.email;
    setState(() {
      _isLoading = false;
    });
  }

  Future<void> _saveSettings() async {
    final updatedSettings = _settings.copyWith(
      username: _usernameController.text,
      email: _emailController.text,
    );
    
    await SettingsManager.saveSettings(updatedSettings);
    setState(() {
      _settings = updatedSettings;
    });

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Settings saved successfully!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    if (_isLoading) {
      return const Scaffold(
        body: Center(child: CircularProgressIndicator()),
      );
    }

    return Scaffold(
      appBar: AppBar(
        title: const Text('User Settings'),
        actions: [
          IconButton(
            icon: const Icon(Icons.save),
            onPressed: _saveSettings,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _usernameController,
              decoration: const InputDecoration(
                labelText: 'Username',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _emailController,
              decoration: const InputDecoration(
                labelText: 'Email',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            SwitchListTile(
              title: const Text('Enable notifications'),
              value: _settings.notifications,
              onChanged: (value) {
                setState(() {
                  _settings = _settings.copyWith(notifications: value);
                });
              },
            ),
            SwitchListTile(
              title: const Text('Dark mode'),
              value: _settings.darkMode,
              onChanged: (value) {
                setState(() {
                  _settings = _settings.copyWith(darkMode: value);
                });
              },
            ),
            ListTile(
              title: const Text('Font size'),
              subtitle: Slider(
                value: _settings.fontSize,
                min: 10.0,
                max: 24.0,
                divisions: 14,
                label: _settings.fontSize.toStringAsFixed(0),
                onChanged: (value) {
                  setState(() {
                    _settings = _settings.copyWith(fontSize: value);
                  });
                },
              ),
            ),
          ],
        ),
      ),
    );
  }

  @override
  void dispose() {
    _usernameController.dispose();
    _emailController.dispose();
    super.dispose();
  }
}
```

User settings management demonstrates storing complex structured data using  
JSON serialization. The SettingsManager provides a clean API for saving and  
loading preferences, while the UserSettings class encapsulates all user data  
with type safety and default values.  

## File System Operations

Reading and writing files to the device's file system.  

```dart
import 'package:flutter/material.dart';
import 'dart:io';
import 'package:path_provider/path_provider.dart';

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
      title: 'File System Operations',
      home: const FileOperationsPage(),
    );
  }
}

class FileOperationsPage extends StatefulWidget {
  const FileOperationsPage({super.key});

  @override
  State<FileOperationsPage> createState() => _FileOperationsPageState();
}

class _FileOperationsPageState extends State<FileOperationsPage> {
  final _textController = TextEditingController();
  String _fileContent = '';
  String _filePath = '';
  bool _fileExists = false;

  @override
  void initState() {
    super.initState();
    _initializeFile();
  }

  Future<void> _initializeFile() async {
    final directory = await getApplicationDocumentsDirectory();
    _filePath = '${directory.path}/sample_file.txt';
    await _checkFileExists();
    if (_fileExists) {
      await _readFile();
    }
  }

  Future<void> _checkFileExists() async {
    final file = File(_filePath);
    setState(() {
      _fileExists = file.existsSync();
    });
  }

  Future<void> _writeFile() async {
    try {
      final file = File(_filePath);
      await file.writeAsString(_textController.text);
      
      setState(() {
        _fileContent = _textController.text;
        _fileExists = true;
      });

      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('File written successfully!')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error writing file: $e')),
        );
      }
    }
  }

  Future<void> _readFile() async {
    try {
      final file = File(_filePath);
      final content = await file.readAsString();
      
      setState(() {
        _fileContent = content;
        _textController.text = content;
      });

      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('File read successfully!')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error reading file: $e')),
        );
      }
    }
  }

  Future<void> _appendToFile() async {
    try {
      final file = File(_filePath);
      final timestamp = DateTime.now().toIso8601String();
      final newLine = '\n[$timestamp] ${_textController.text}';
      
      await file.writeAsString(newLine, mode: FileMode.append);
      await _readFile();

      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Content appended to file!')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error appending to file: $e')),
        );
      }
    }
  }

  Future<void> _deleteFile() async {
    try {
      final file = File(_filePath);
      if (await file.exists()) {
        await file.delete();
        setState(() {
          _fileExists = false;
          _fileContent = '';
          _textController.clear();
        });

        if (mounted) {
          ScaffoldMessenger.of(context).showSnackBar(
            const SnackBar(content: Text('File deleted successfully!')),
          );
        }
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error deleting file: $e')),
        );
      }
    }
  }

  Future<void> _getFileInfo() async {
    try {
      final file = File(_filePath);
      if (await file.exists()) {
        final stat = await file.stat();
        final size = stat.size;
        final modified = stat.modified;

        if (mounted) {
          showDialog(
            context: context,
            builder: (context) => AlertDialog(
              title: const Text('File Information'),
              content: Column(
                mainAxisSize: MainAxisSize.min,
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text('Path: $_filePath'),
                  Text('Size: $size bytes'),
                  Text('Modified: $modified'),
                ],
              ),
              actions: [
                TextButton(
                  onPressed: () => Navigator.pop(context),
                  child: const Text('OK'),
                ),
              ],
            ),
          );
        }
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error getting file info: $e')),
        );
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('File System Operations'),
        actions: [
          IconButton(
            icon: const Icon(Icons.info),
            onPressed: _getFileInfo,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _textController,
              maxLines: 3,
              decoration: const InputDecoration(
                labelText: 'Enter text to save',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton.icon(
                    onPressed: _writeFile,
                    icon: const Icon(Icons.save),
                    label: const Text('Write File'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton.icon(
                    onPressed: _fileExists ? _readFile : null,
                    icon: const Icon(Icons.folder_open),
                    label: const Text('Read File'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton.icon(
                    onPressed: _fileExists ? _appendToFile : null,
                    icon: const Icon(Icons.add),
                    label: const Text('Append'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton.icon(
                    onPressed: _fileExists ? _deleteFile : null,
                    icon: const Icon(Icons.delete),
                    label: const Text('Delete'),
                    style: ElevatedButton.styleFrom(
                      foregroundColor: Colors.red,
                    ),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 16),
            Text(
              'File exists: ${_fileExists ? 'Yes' : 'No'}',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 8),
            Expanded(
              child: Container(
                width: double.infinity,
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  border: Border.all(color: Colors.grey),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: SingleChildScrollView(
                  child: Text(
                    _fileContent.isEmpty ? 'No content' : _fileContent,
                    style: const TextStyle(fontFamily: 'monospace'),
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
    _textController.dispose();
    super.dispose();
  }
}
```

File system operations provide direct access to device storage for reading  
and writing files. The path_provider package helps locate appropriate  
directories, while File operations handle reading, writing, appending, and  
deleting content with proper error handling.  

## SQLite Database Basics

Creating and managing a SQLite database with basic CRUD operations.  

```dart
import 'package:flutter/material.dart';
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';

void main() {
  runApp(const MyApp());
}

class Task {
  final int? id;
  final String title;
  final String description;
  final bool isCompleted;
  final DateTime createdAt;

  Task({
    this.id,
    required this.title,
    required this.description,
    required this.isCompleted,
    required this.createdAt,
  });

  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'title': title,
      'description': description,
      'isCompleted': isCompleted ? 1 : 0,
      'createdAt': createdAt.millisecondsSinceEpoch,
    };
  }

  factory Task.fromMap(Map<String, dynamic> map) {
    return Task(
      id: map['id'],
      title: map['title'],
      description: map['description'],
      isCompleted: map['isCompleted'] == 1,
      createdAt: DateTime.fromMillisecondsSinceEpoch(map['createdAt']),
    );
  }
}

class DatabaseHelper {
  static final DatabaseHelper _instance = DatabaseHelper._internal();
  factory DatabaseHelper() => _instance;
  DatabaseHelper._internal();

  static Database? _database;

  Future<Database> get database async {
    _database ??= await _initDatabase();
    return _database!;
  }

  Future<Database> _initDatabase() async {
    String path = join(await getDatabasesPath(), 'tasks.db');
    
    return await openDatabase(
      path,
      version: 1,
      onCreate: _onCreate,
    );
  }

  Future<void> _onCreate(Database db, int version) async {
    await db.execute('''
      CREATE TABLE tasks (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        title TEXT NOT NULL,
        description TEXT NOT NULL,
        isCompleted INTEGER NOT NULL DEFAULT 0,
        createdAt INTEGER NOT NULL
      )
    ''');
  }

  Future<int> insertTask(Task task) async {
    final db = await database;
    return await db.insert('tasks', task.toMap());
  }

  Future<List<Task>> getAllTasks() async {
    final db = await database;
    final List<Map<String, dynamic>> maps = await db.query('tasks',
        orderBy: 'createdAt DESC');

    return List.generate(maps.length, (i) => Task.fromMap(maps[i]));
  }

  Future<Task?> getTask(int id) async {
    final db = await database;
    final List<Map<String, dynamic>> maps = await db.query(
      'tasks',
      where: 'id = ?',
      whereArgs: [id],
    );

    if (maps.isNotEmpty) {
      return Task.fromMap(maps.first);
    }
    return null;
  }

  Future<int> updateTask(Task task) async {
    final db = await database;
    return await db.update(
      'tasks',
      task.toMap(),
      where: 'id = ?',
      whereArgs: [task.id],
    );
  }

  Future<int> deleteTask(int id) async {
    final db = await database;
    return await db.delete(
      'tasks',
      where: 'id = ?',
      whereArgs: [id],
    );
  }

  Future<List<Task>> getCompletedTasks() async {
    final db = await database;
    final List<Map<String, dynamic>> maps = await db.query(
      'tasks',
      where: 'isCompleted = ?',
      whereArgs: [1],
      orderBy: 'createdAt DESC',
    );

    return List.generate(maps.length, (i) => Task.fromMap(maps[i]));
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'SQLite Database Basics',
      home: const TaskListPage(),
    );
  }
}

class TaskListPage extends StatefulWidget {
  const TaskListPage({super.key});

  @override
  State<TaskListPage> createState() => _TaskListPageState();
}

class _TaskListPageState extends State<TaskListPage> {
  final DatabaseHelper _databaseHelper = DatabaseHelper();
  List<Task> _tasks = [];
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _loadTasks();
  }

  Future<void> _loadTasks() async {
    setState(() {
      _isLoading = true;
    });

    final tasks = await _databaseHelper.getAllTasks();
    setState(() {
      _tasks = tasks;
      _isLoading = false;
    });
  }

  Future<void> _addTask() async {
    final result = await Navigator.push<bool>(
      context,
      MaterialPageRoute(
        builder: (context) => const AddTaskPage(),
      ),
    );

    if (result == true) {
      await _loadTasks();
    }
  }

  Future<void> _toggleTaskCompletion(Task task) async {
    final updatedTask = Task(
      id: task.id,
      title: task.title,
      description: task.description,
      isCompleted: !task.isCompleted,
      createdAt: task.createdAt,
    );

    await _databaseHelper.updateTask(updatedTask);
    await _loadTasks();
  }

  Future<void> _deleteTask(int id) async {
    await _databaseHelper.deleteTask(id);
    await _loadTasks();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('SQLite Task Manager'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _loadTasks,
          ),
        ],
      ),
      body: _isLoading
          ? const Center(child: CircularProgressIndicator())
          : _tasks.isEmpty
              ? const Center(
                  child: Text(
                    'No tasks yet.\nTap + to add a task',
                    textAlign: TextAlign.center,
                  ),
                )
              : ListView.builder(
                  itemCount: _tasks.length,
                  itemBuilder: (context, index) {
                    final task = _tasks[index];
                    return Card(
                      margin: const EdgeInsets.symmetric(
                        horizontal: 16,
                        vertical: 4,
                      ),
                      child: ListTile(
                        leading: Checkbox(
                          value: task.isCompleted,
                          onChanged: (_) => _toggleTaskCompletion(task),
                        ),
                        title: Text(
                          task.title,
                          style: TextStyle(
                            decoration: task.isCompleted
                                ? TextDecoration.lineThrough
                                : null,
                          ),
                        ),
                        subtitle: Text(task.description),
                        trailing: IconButton(
                          icon: const Icon(Icons.delete, color: Colors.red),
                          onPressed: () => _deleteTask(task.id!),
                        ),
                      ),
                    );
                  },
                ),
      floatingActionButton: FloatingActionButton(
        onPressed: _addTask,
        child: const Icon(Icons.add),
      ),
    );
  }
}

class AddTaskPage extends StatefulWidget {
  const AddTaskPage({super.key});

  @override
  State<AddTaskPage> createState() => _AddTaskPageState();
}

class _AddTaskPageState extends State<AddTaskPage> {
  final _titleController = TextEditingController();
  final _descriptionController = TextEditingController();
  final DatabaseHelper _databaseHelper = DatabaseHelper();

  Future<void> _saveTask() async {
    if (_titleController.text.trim().isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please enter a title')),
      );
      return;
    }

    final task = Task(
      title: _titleController.text.trim(),
      description: _descriptionController.text.trim(),
      isCompleted: false,
      createdAt: DateTime.now(),
    );

    await _databaseHelper.insertTask(task);
    
    if (mounted) {
      Navigator.pop(context, true);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Add Task'),
        actions: [
          IconButton(
            icon: const Icon(Icons.save),
            onPressed: _saveTask,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _titleController,
              decoration: const InputDecoration(
                labelText: 'Task Title',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _descriptionController,
              maxLines: 3,
              decoration: const InputDecoration(
                labelText: 'Description',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 20),
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                onPressed: _saveTask,
                child: const Text('Save Task'),
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
    _descriptionController.dispose();
    super.dispose();
  }
}
```

SQLite database operations provide structured data storage with CRUD  
functionality. The sqflite package offers SQL database capabilities with  
table creation, indexing, and complex queries. This example demonstrates  
a complete task management system with proper database initialization,  
model classes, and data access patterns.  

## JSON Data Persistence

Storing and retrieving complex data structures using JSON serialization.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class User {
  final String id;
  final String name;
  final String email;
  final int age;
  final List<String> hobbies;
  final Map<String, dynamic> preferences;

  User({
    required this.id,
    required this.name,
    required this.email,
    required this.age,
    required this.hobbies,
    required this.preferences,
  });

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'email': email,
      'age': age,
      'hobbies': hobbies,
      'preferences': preferences,
    };
  }

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'],
      name: json['name'],
      email: json['email'],
      age: json['age'],
      hobbies: List<String>.from(json['hobbies']),
      preferences: Map<String, dynamic>.from(json['preferences']),
    );
  }
}

class UserProfile {
  final User user;
  final DateTime lastLogin;
  final List<String> recentActivities;
  final bool isActive;

  UserProfile({
    required this.user,
    required this.lastLogin,
    required this.recentActivities,
    required this.isActive,
  });

  Map<String, dynamic> toJson() {
    return {
      'user': user.toJson(),
      'lastLogin': lastLogin.toIso8601String(),
      'recentActivities': recentActivities,
      'isActive': isActive,
    };
  }

  factory UserProfile.fromJson(Map<String, dynamic> json) {
    return UserProfile(
      user: User.fromJson(json['user']),
      lastLogin: DateTime.parse(json['lastLogin']),
      recentActivities: List<String>.from(json['recentActivities']),
      isActive: json['isActive'],
    );
  }
}

class JsonDataManager {
  static const String _userProfileKey = 'user_profile';
  static const String _userListKey = 'user_list';

  static Future<void> saveUserProfile(UserProfile profile) async {
    final prefs = await SharedPreferences.getInstance();
    final jsonString = json.encode(profile.toJson());
    await prefs.setString(_userProfileKey, jsonString);
  }

  static Future<UserProfile?> loadUserProfile() async {
    final prefs = await SharedPreferences.getInstance();
    final jsonString = prefs.getString(_userProfileKey);
    
    if (jsonString != null) {
      try {
        final jsonMap = json.decode(jsonString);
        return UserProfile.fromJson(jsonMap);
      } catch (e) {
        return null;
      }
    }
    return null;
  }

  static Future<void> saveUserList(List<User> users) async {
    final prefs = await SharedPreferences.getInstance();
    final jsonList = users.map((user) => user.toJson()).toList();
    final jsonString = json.encode(jsonList);
    await prefs.setString(_userListKey, jsonString);
  }

  static Future<List<User>> loadUserList() async {
    final prefs = await SharedPreferences.getInstance();
    final jsonString = prefs.getString(_userListKey);
    
    if (jsonString != null) {
      try {
        final jsonList = json.decode(jsonString) as List;
        return jsonList.map((json) => User.fromJson(json)).toList();
      } catch (e) {
        return [];
      }
    }
    return [];
  }

  static Future<void> clearAllData() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.remove(_userProfileKey);
    await prefs.remove(_userListKey);
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'JSON Data Persistence',
      home: const JsonDataPage(),
    );
  }
}

class JsonDataPage extends StatefulWidget {
  const JsonDataPage({super.key});

  @override
  State<JsonDataPage> createState() => _JsonDataPageState();
}

class _JsonDataPageState extends State<JsonDataPage> {
  UserProfile? _userProfile;
  List<User> _users = [];
  bool _isLoading = false;
  
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _ageController = TextEditingController();
  final _hobbiesController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _loadData();
  }

  Future<void> _loadData() async {
    setState(() {
      _isLoading = true;
    });

    _userProfile = await JsonDataManager.loadUserProfile();
    _users = await JsonDataManager.loadUserList();

    setState(() {
      _isLoading = false;
    });
  }

  Future<void> _createSampleProfile() async {
    final user = User(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      name: _nameController.text.isEmpty ? 'John Doe' : _nameController.text,
      email: _emailController.text.isEmpty ? 'john@example.com' : _emailController.text,
      age: int.tryParse(_ageController.text) ?? 25,
      hobbies: _hobbiesController.text.split(',').map((e) => e.trim()).toList(),
      preferences: {
        'theme': 'dark',
        'notifications': true,
        'language': 'en',
        'fontSize': 16,
      },
    );

    final profile = UserProfile(
      user: user,
      lastLogin: DateTime.now(),
      recentActivities: [
        'Logged in',
        'Updated profile',
        'Changed settings',
      ],
      isActive: true,
    );

    await JsonDataManager.saveUserProfile(profile);
    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Profile saved successfully!')),
      );
    }
  }

  Future<void> _addUserToList() async {
    if (_nameController.text.isEmpty || _emailController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please fill in name and email')),
      );
      return;
    }

    final user = User(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      name: _nameController.text,
      email: _emailController.text,
      age: int.tryParse(_ageController.text) ?? 18,
      hobbies: _hobbiesController.text.split(',').map((e) => e.trim()).toList(),
      preferences: {
        'theme': 'light',
        'notifications': false,
        'language': 'en',
      },
    );

    _users.add(user);
    await JsonDataManager.saveUserList(_users);
    
    _nameController.clear();
    _emailController.clear();
    _ageController.clear();
    _hobbiesController.clear();

    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('User added to list!')),
      );
    }
  }

  Future<void> _clearAllData() async {
    await JsonDataManager.clearAllData();
    await _loadData();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('All data cleared!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('JSON Data Persistence'),
        actions: [
          IconButton(
            icon: const Icon(Icons.delete_sweep),
            onPressed: _clearAllData,
          ),
        ],
      ),
      body: _isLoading
          ? const Center(child: CircularProgressIndicator())
          : SingleChildScrollView(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Create Profile',
                    style: Theme.of(context).textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 16),
                  TextField(
                    controller: _nameController,
                    decoration: const InputDecoration(
                      labelText: 'Name',
                      border: OutlineInputBorder(),
                    ),
                  ),
                  const SizedBox(height: 8),
                  TextField(
                    controller: _emailController,
                    decoration: const InputDecoration(
                      labelText: 'Email',
                      border: OutlineInputBorder(),
                    ),
                  ),
                  const SizedBox(height: 8),
                  TextField(
                    controller: _ageController,
                    keyboardType: TextInputType.number,
                    decoration: const InputDecoration(
                      labelText: 'Age',
                      border: OutlineInputBorder(),
                    ),
                  ),
                  const SizedBox(height: 8),
                  TextField(
                    controller: _hobbiesController,
                    decoration: const InputDecoration(
                      labelText: 'Hobbies (comma separated)',
                      border: OutlineInputBorder(),
                    ),
                  ),
                  const SizedBox(height: 16),
                  Row(
                    children: [
                      Expanded(
                        child: ElevatedButton(
                          onPressed: _createSampleProfile,
                          child: const Text('Save as Profile'),
                        ),
                      ),
                      const SizedBox(width: 8),
                      Expanded(
                        child: ElevatedButton(
                          onPressed: _addUserToList,
                          child: const Text('Add to List'),
                        ),
                      ),
                    ],
                  ),
                  const SizedBox(height: 24),
                  if (_userProfile != null) ...[
                    Text(
                      'Current Profile',
                      style: Theme.of(context).textTheme.headlineSmall,
                    ),
                    Card(
                      child: Padding(
                        padding: const EdgeInsets.all(16.0),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text('Name: ${_userProfile!.user.name}'),
                            Text('Email: ${_userProfile!.user.email}'),
                            Text('Age: ${_userProfile!.user.age}'),
                            Text('Hobbies: ${_userProfile!.user.hobbies.join(', ')}'),
                            Text('Last Login: ${_userProfile!.lastLogin}'),
                            Text('Active: ${_userProfile!.isActive}'),
                          ],
                        ),
                      ),
                    ),
                    const SizedBox(height: 16),
                  ],
                  Text(
                    'User List (${_users.length})',
                    style: Theme.of(context).textTheme.headlineSmall,
                  ),
                  ..._users.map(
                    (user) => Card(
                      child: ListTile(
                        title: Text(user.name),
                        subtitle: Text(user.email),
                        trailing: Text('${user.age} years'),
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
    _ageController.dispose();
    _hobbiesController.dispose();
    super.dispose();
  }
}
```

JSON data persistence enables storing complex nested data structures with  
proper serialization. This approach handles objects, lists, and maps while  
maintaining type safety through model classes. The pattern supports both  
single objects and collections with proper error handling for malformed data.  

## Secure Storage Implementation

Storing sensitive data securely using encrypted storage.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_secure_storage/flutter_secure_storage.dart';

void main() {
  runApp(const MyApp());
}

class SecureCredentials {
  final String username;
  final String password;
  final String apiKey;
  final String authToken;

  SecureCredentials({
    required this.username,
    required this.password,
    required this.apiKey,
    required this.authToken,
  });

  Map<String, String> toMap() {
    return {
      'username': username,
      'password': password,
      'apiKey': apiKey,
      'authToken': authToken,
    };
  }
}

class SecureStorageManager {
  static const FlutterSecureStorage _storage = FlutterSecureStorage(
    aOptions: AndroidOptions(
      encryptedSharedPreferences: true,
    ),
    iOptions: IOSOptions(
      accessibility: IOSAccessibility.first_unlock_this_device,
    ),
  );

  // Keys for secure storage
  static const String _usernameKey = 'secure_username';
  static const String _passwordKey = 'secure_password';
  static const String _apiKeyKey = 'secure_api_key';
  static const String _authTokenKey = 'secure_auth_token';
  static const String _biometricKey = 'biometric_enabled';

  static Future<void> storeCredentials(SecureCredentials credentials) async {
    await Future.wait([
      _storage.write(key: _usernameKey, value: credentials.username),
      _storage.write(key: _passwordKey, value: credentials.password),
      _storage.write(key: _apiKeyKey, value: credentials.apiKey),
      _storage.write(key: _authTokenKey, value: credentials.authToken),
    ]);
  }

  static Future<SecureCredentials?> getCredentials() async {
    try {
      final values = await Future.wait([
        _storage.read(key: _usernameKey),
        _storage.read(key: _passwordKey),
        _storage.read(key: _apiKeyKey),
        _storage.read(key: _authTokenKey),
      ]);

      final username = values[0];
      final password = values[1];
      final apiKey = values[2];
      final authToken = values[3];

      if (username != null && password != null && apiKey != null && authToken != null) {
        return SecureCredentials(
          username: username,
          password: password,
          apiKey: apiKey,
          authToken: authToken,
        );
      }
    } catch (e) {
      return null;
    }
    return null;
  }

  static Future<void> deleteCredentials() async {
    await Future.wait([
      _storage.delete(key: _usernameKey),
      _storage.delete(key: _passwordKey),
      _storage.delete(key: _apiKeyKey),
      _storage.delete(key: _authTokenKey),
    ]);
  }

  static Future<void> setBiometricEnabled(bool enabled) async {
    await _storage.write(key: _biometricKey, value: enabled.toString());
  }

  static Future<bool> getBiometricEnabled() async {
    final value = await _storage.read(key: _biometricKey);
    return value == 'true';
  }

  static Future<Map<String, String>> getAllStoredData() async {
    return await _storage.readAll();
  }

  static Future<void> clearAll() async {
    await _storage.deleteAll();
  }

  static Future<bool> containsKey(String key) async {
    return await _storage.containsKey(key: key);
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Secure Storage',
      home: const SecureStoragePage(),
    );
  }
}

class SecureStoragePage extends StatefulWidget {
  const SecureStoragePage({super.key});

  @override
  State<SecureStoragePage> createState() => _SecureStoragePageState();
}

class _SecureStoragePageState extends State<SecureStoragePage> {
  final _usernameController = TextEditingController();
  final _passwordController = TextEditingController();
  final _apiKeyController = TextEditingController();
  final _authTokenController = TextEditingController();
  
  SecureCredentials? _storedCredentials;
  bool _biometricEnabled = false;
  bool _isLoading = false;
  bool _showPasswords = false;

  @override
  void initState() {
    super.initState();
    _loadStoredData();
  }

  Future<void> _loadStoredData() async {
    setState(() {
      _isLoading = true;
    });

    try {
      final credentials = await SecureStorageManager.getCredentials();
      final biometric = await SecureStorageManager.getBiometricEnabled();
      
      setState(() {
        _storedCredentials = credentials;
        _biometricEnabled = biometric;
        
        if (credentials != null) {
          _usernameController.text = credentials.username;
          _passwordController.text = credentials.password;
          _apiKeyController.text = credentials.apiKey;
          _authTokenController.text = credentials.authToken;
        }
      });
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error loading data: $e')),
        );
      }
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _saveCredentials() async {
    if (_usernameController.text.isEmpty || _passwordController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please fill in username and password')),
      );
      return;
    }

    setState(() {
      _isLoading = true;
    });

    try {
      final credentials = SecureCredentials(
        username: _usernameController.text,
        password: _passwordController.text,
        apiKey: _apiKeyController.text,
        authToken: _authTokenController.text,
      );

      await SecureStorageManager.storeCredentials(credentials);
      await SecureStorageManager.setBiometricEnabled(_biometricEnabled);
      
      setState(() {
        _storedCredentials = credentials;
      });

      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Credentials saved securely!')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error saving credentials: $e')),
        );
      }
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _deleteCredentials() async {
    setState(() {
      _isLoading = true;
    });

    try {
      await SecureStorageManager.deleteCredentials();
      
      setState(() {
        _storedCredentials = null;
      });

      _usernameController.clear();
      _passwordController.clear();
      _apiKeyController.clear();
      _authTokenController.clear();

      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Credentials deleted!')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error deleting credentials: $e')),
        );
      }
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _showAllStoredData() async {
    try {
      final allData = await SecureStorageManager.getAllStoredData();
      
      if (mounted) {
        showDialog(
          context: context,
          builder: (context) => AlertDialog(
            title: const Text('All Stored Data'),
            content: SingleChildScrollView(
              child: Column(
                mainAxisSize: MainAxisSize.min,
                crossAxisAlignment: CrossAxisAlignment.start,
                children: allData.entries
                    .map((entry) => Padding(
                          padding: const EdgeInsets.symmetric(vertical: 4),
                          child: Text('${entry.key}: ${entry.value}'),
                        ))
                    .toList(),
              ),
            ),
            actions: [
              TextButton(
                onPressed: () => Navigator.pop(context),
                child: const Text('Close'),
              ),
            ],
          ),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error reading data: $e')),
        );
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Secure Storage'),
        actions: [
          IconButton(
            icon: Icon(_showPasswords ? Icons.visibility_off : Icons.visibility),
            onPressed: () {
              setState(() {
                _showPasswords = !_showPasswords;
              });
            },
          ),
          IconButton(
            icon: const Icon(Icons.info),
            onPressed: _showAllStoredData,
          ),
        ],
      ),
      body: _isLoading
          ? const Center(child: CircularProgressIndicator())
          : Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                children: [
                  TextField(
                    controller: _usernameController,
                    decoration: const InputDecoration(
                      labelText: 'Username',
                      border: OutlineInputBorder(),
                      prefixIcon: Icon(Icons.person),
                    ),
                  ),
                  const SizedBox(height: 16),
                  TextField(
                    controller: _passwordController,
                    obscureText: !_showPasswords,
                    decoration: const InputDecoration(
                      labelText: 'Password',
                      border: OutlineInputBorder(),
                      prefixIcon: Icon(Icons.lock),
                    ),
                  ),
                  const SizedBox(height: 16),
                  TextField(
                    controller: _apiKeyController,
                    obscureText: !_showPasswords,
                    decoration: const InputDecoration(
                      labelText: 'API Key',
                      border: OutlineInputBorder(),
                      prefixIcon: Icon(Icons.key),
                    ),
                  ),
                  const SizedBox(height: 16),
                  TextField(
                    controller: _authTokenController,
                    obscureText: !_showPasswords,
                    decoration: const InputDecoration(
                      labelText: 'Auth Token',
                      border: OutlineInputBorder(),
                      prefixIcon: Icon(Icons.token),
                    ),
                  ),
                  const SizedBox(height: 16),
                  SwitchListTile(
                    title: const Text('Enable Biometric Authentication'),
                    value: _biometricEnabled,
                    onChanged: (value) {
                      setState(() {
                        _biometricEnabled = value;
                      });
                    },
                  ),
                  const SizedBox(height: 20),
                  Row(
                    children: [
                      Expanded(
                        child: ElevatedButton.icon(
                          onPressed: _saveCredentials,
                          icon: const Icon(Icons.save),
                          label: const Text('Save Securely'),
                        ),
                      ),
                      const SizedBox(width: 8),
                      Expanded(
                        child: ElevatedButton.icon(
                          onPressed: _storedCredentials != null 
                              ? _deleteCredentials 
                              : null,
                          icon: const Icon(Icons.delete),
                          label: const Text('Delete'),
                          style: ElevatedButton.styleFrom(
                            foregroundColor: Colors.red,
                          ),
                        ),
                      ),
                    ],
                  ),
                  const SizedBox(height: 20),
                  if (_storedCredentials != null) ...[
                    Card(
                      child: Padding(
                        padding: const EdgeInsets.all(16.0),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              'Stored Credentials',
                              style: Theme.of(context).textTheme.titleLarge,
                            ),
                            const SizedBox(height: 8),
                            Text('✓ Username: ${_storedCredentials!.username}'),
                            Text('✓ Password: [ENCRYPTED]'),
                            Text('✓ API Key: [ENCRYPTED]'),
                            Text('✓ Auth Token: [ENCRYPTED]'),
                            Text('✓ Biometric: ${_biometricEnabled ? 'Enabled' : 'Disabled'}'),
                          ],
                        ),
                      ),
                    ),
                  ],
                ],
              ),
            ),
    );
  }

  @override
  void dispose() {
    _usernameController.dispose();
    _passwordController.dispose();
    _apiKeyController.dispose();
    _authTokenController.dispose();
    super.dispose();
  }
}
```

Secure storage provides encrypted data persistence for sensitive information  
like passwords, tokens, and API keys. The flutter_secure_storage package uses  
platform-specific secure storage mechanisms including Keychain on iOS and  
EncryptedSharedPreferences on Android with additional security options.  

## Cache Management System

Implementing a caching system for efficient data storage and retrieval.  

```dart
import 'package:flutter/material.dart';
import 'dart:convert';
import 'dart:io';
import 'package:path_provider/path_provider.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() {
  runApp(const MyApp());
}

class CacheItem<T> {
  final String key;
  final T data;
  final DateTime createdAt;
  final DateTime expiresAt;

  CacheItem({
    required this.key,
    required this.data,
    required this.createdAt,
    required this.expiresAt,
  });

  bool get isExpired => DateTime.now().isAfter(expiresAt);

  Map<String, dynamic> toJson() {
    return {
      'key': key,
      'data': data,
      'createdAt': createdAt.toIso8601String(),
      'expiresAt': expiresAt.toIso8601String(),
    };
  }

  factory CacheItem.fromJson(Map<String, dynamic> json) {
    return CacheItem<T>(
      key: json['key'],
      data: json['data'],
      createdAt: DateTime.parse(json['createdAt']),
      expiresAt: DateTime.parse(json['expiresAt']),
    );
  }
}

enum CacheStrategy {
  memory,
  disk,
  hybrid,
}

class CacheManager {
  static final CacheManager _instance = CacheManager._internal();
  factory CacheManager() => _instance;
  CacheManager._internal();

  final Map<String, CacheItem> _memoryCache = {};
  late SharedPreferences _prefs;
  late String _diskCachePath;
  bool _initialized = false;

  Future<void> initialize() async {
    if (_initialized) return;
    
    _prefs = await SharedPreferences.getInstance();
    final directory = await getApplicationDocumentsDirectory();
    _diskCachePath = '${directory.path}/cache';
    
    // Create cache directory
    await Directory(_diskCachePath).create(recursive: true);
    
    _initialized = true;
  }

  Future<void> put<T>(
    String key, 
    T data, {
    Duration expiry = const Duration(hours: 1),
    CacheStrategy strategy = CacheStrategy.hybrid,
  }) async {
    await initialize();
    
    final cacheItem = CacheItem<T>(
      key: key,
      data: data,
      createdAt: DateTime.now(),
      expiresAt: DateTime.now().add(expiry),
    );

    switch (strategy) {
      case CacheStrategy.memory:
        _memoryCache[key] = cacheItem;
        break;
      case CacheStrategy.disk:
        await _saveToDisk(key, cacheItem);
        break;
      case CacheStrategy.hybrid:
        _memoryCache[key] = cacheItem;
        await _saveToDisk(key, cacheItem);
        break;
    }
  }

  Future<T?> get<T>(String key) async {
    await initialize();
    
    // Check memory cache first
    if (_memoryCache.containsKey(key)) {
      final item = _memoryCache[key]!;
      if (!item.isExpired) {
        return item.data as T?;
      } else {
        _memoryCache.remove(key);
      }
    }

    // Check disk cache
    final diskItem = await _loadFromDisk<T>(key);
    if (diskItem != null && !diskItem.isExpired) {
      // Add back to memory cache for faster access
      _memoryCache[key] = diskItem;
      return diskItem.data as T?;
    }

    return null;
  }

  Future<bool> contains(String key) async {
    await initialize();
    
    // Check memory first
    if (_memoryCache.containsKey(key) && !_memoryCache[key]!.isExpired) {
      return true;
    }

    // Check disk
    final diskItem = await _loadFromDisk(key);
    return diskItem != null && !diskItem.isExpired;
  }

  Future<void> remove(String key) async {
    await initialize();
    
    _memoryCache.remove(key);
    await _removeFromDisk(key);
  }

  Future<void> clear() async {
    await initialize();
    
    _memoryCache.clear();
    await _clearDiskCache();
  }

  Future<int> size() async {
    await initialize();
    
    final diskFiles = await Directory(_diskCachePath).list().toList();
    return _memoryCache.length + diskFiles.length;
  }

  Future<List<String>> getAllKeys() async {
    await initialize();
    
    final keys = <String>[];
    keys.addAll(_memoryCache.keys);
    
    final diskFiles = await Directory(_diskCachePath).list().toList();
    for (final file in diskFiles) {
      if (file is File) {
        final filename = file.path.split('/').last;
        if (filename.endsWith('.cache')) {
          keys.add(filename.replaceAll('.cache', ''));
        }
      }
    }
    
    return keys.toSet().toList();
  }

  Future<void> cleanExpired() async {
    await initialize();
    
    // Clean memory cache
    _memoryCache.removeWhere((key, item) => item.isExpired);
    
    // Clean disk cache
    final diskFiles = await Directory(_diskCachePath).list().toList();
    for (final file in diskFiles) {
      if (file is File) {
        try {
          final content = await file.readAsString();
          final json = jsonDecode(content);
          final item = CacheItem.fromJson(json);
          if (item.isExpired) {
            await file.delete();
          }
        } catch (e) {
          // Invalid cache file, delete it
          await file.delete();
        }
      }
    }
  }

  Future<void> _saveToDisk(String key, CacheItem item) async {
    final file = File('$_diskCachePath/$key.cache');
    await file.writeAsString(jsonEncode(item.toJson()));
  }

  Future<CacheItem?> _loadFromDisk<T>(String key) async {
    try {
      final file = File('$_diskCachePath/$key.cache');
      if (await file.exists()) {
        final content = await file.readAsString();
        final json = jsonDecode(content);
        return CacheItem<T>.fromJson(json);
      }
    } catch (e) {
      // Error reading file
    }
    return null;
  }

  Future<void> _removeFromDisk(String key) async {
    final file = File('$_diskCachePath/$key.cache');
    if (await file.exists()) {
      await file.delete();
    }
  }

  Future<void> _clearDiskCache() async {
    final directory = Directory(_diskCachePath);
    if (await directory.exists()) {
      await directory.delete(recursive: true);
      await directory.create();
    }
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Cache Management System',
      home: const CacheManagementPage(),
    );
  }
}

class CacheManagementPage extends StatefulWidget {
  const CacheManagementPage({super.key});

  @override
  State<CacheManagementPage> createState() => _CacheManagementPageState();
}

class _CacheManagementPageState extends State<CacheManagementPage> {
  final CacheManager _cacheManager = CacheManager();
  final _keyController = TextEditingController();
  final _valueController = TextEditingController();
  
  List<String> _cacheKeys = [];
  int _cacheSize = 0;
  CacheStrategy _selectedStrategy = CacheStrategy.hybrid;
  Duration _selectedExpiry = const Duration(minutes: 5);
  String _retrievedValue = '';

  @override
  void initState() {
    super.initState();
    _refreshCacheInfo();
  }

  Future<void> _refreshCacheInfo() async {
    final keys = await _cacheManager.getAllKeys();
    final size = await _cacheManager.size();
    
    setState(() {
      _cacheKeys = keys;
      _cacheSize = size;
    });
  }

  Future<void> _putCache() async {
    if (_keyController.text.isEmpty || _valueController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please enter key and value')),
      );
      return;
    }

    await _cacheManager.put(
      _keyController.text,
      _valueController.text,
      expiry: _selectedExpiry,
      strategy: _selectedStrategy,
    );

    _keyController.clear();
    _valueController.clear();
    await _refreshCacheInfo();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Data cached successfully!')),
      );
    }
  }

  Future<void> _getCache() async {
    if (_keyController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please enter key to retrieve')),
      );
      return;
    }

    final value = await _cacheManager.get<String>(_keyController.text);
    
    setState(() {
      _retrievedValue = value ?? 'Not found or expired';
    });
  }

  Future<void> _removeCache() async {
    if (_keyController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please enter key to remove')),
      );
      return;
    }

    await _cacheManager.remove(_keyController.text);
    await _refreshCacheInfo();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Cache item removed!')),
      );
    }
  }

  Future<void> _clearAllCache() async {
    await _cacheManager.clear();
    await _refreshCacheInfo();
    
    setState(() {
      _retrievedValue = '';
    });

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('All cache cleared!')),
      );
    }
  }

  Future<void> _cleanExpiredCache() async {
    await _cacheManager.cleanExpired();
    await _refreshCacheInfo();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Expired cache items cleaned!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Cache Management'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _refreshCacheInfo,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _keyController,
              decoration: const InputDecoration(
                labelText: 'Cache Key',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 8),
            TextField(
              controller: _valueController,
              decoration: const InputDecoration(
                labelText: 'Cache Value',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Text('Strategy: ', style: Theme.of(context).textTheme.titleSmall),
                DropdownButton<CacheStrategy>(
                  value: _selectedStrategy,
                  items: CacheStrategy.values.map((strategy) {
                    return DropdownMenuItem(
                      value: strategy,
                      child: Text(strategy.name),
                    );
                  }).toList(),
                  onChanged: (value) {
                    if (value != null) {
                      setState(() {
                        _selectedStrategy = value;
                      });
                    }
                  },
                ),
              ],
            ),
            Row(
              children: [
                Text('Expiry: ', style: Theme.of(context).textTheme.titleSmall),
                DropdownButton<Duration>(
                  value: _selectedExpiry,
                  items: const [
                    DropdownMenuItem(
                      value: Duration(minutes: 1),
                      child: Text('1 minute'),
                    ),
                    DropdownMenuItem(
                      value: Duration(minutes: 5),
                      child: Text('5 minutes'),
                    ),
                    DropdownMenuItem(
                      value: Duration(hours: 1),
                      child: Text('1 hour'),
                    ),
                    DropdownMenuItem(
                      value: Duration(days: 1),
                      child: Text('1 day'),
                    ),
                  ],
                  onChanged: (value) {
                    if (value != null) {
                      setState(() {
                        _selectedExpiry = value;
                      });
                    }
                  },
                ),
              ],
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _putCache,
                    child: const Text('Put'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _getCache,
                    child: const Text('Get'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _removeCache,
                    child: const Text('Remove'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _clearAllCache,
                    child: const Text('Clear All'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _cleanExpiredCache,
                    child: const Text('Clean Expired'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 20),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Cache Information',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 8),
                    Text('Total items: $_cacheSize'),
                    Text('Keys: ${_cacheKeys.join(', ')}'),
                    if (_retrievedValue.isNotEmpty) ...[
                      const SizedBox(height: 8),
                      Text('Retrieved value: $_retrievedValue'),
                    ],
                  ],
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
    _keyController.dispose();
    _valueController.dispose();
    super.dispose();
  }
}
```

Cache management provides efficient data storage and retrieval with multiple  
strategies including memory-only, disk-only, and hybrid approaches. The system  
handles expiration, cleanup, and different storage mechanisms to optimize  
performance while managing memory usage effectively.  

## Key-Value List Storage

Storing and managing lists of data with SharedPreferences.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() {
  runApp(const MyApp());
}

class ListStorageManager {
  static const String _todoListKey = 'todo_list';
  static const String _favoritesKey = 'favorites_list';
  static const String _historyKey = 'search_history';
  static const String _tagsKey = 'tags_list';

  static Future<void> addToStringList(String key, String value) async {
    final prefs = await SharedPreferences.getInstance();
    List<String> currentList = prefs.getStringList(key) ?? [];
    
    if (!currentList.contains(value)) {
      currentList.add(value);
      await prefs.setStringList(key, currentList);
    }
  }

  static Future<void> removeFromStringList(String key, String value) async {
    final prefs = await SharedPreferences.getInstance();
    List<String> currentList = prefs.getStringList(key) ?? [];
    
    currentList.remove(value);
    await prefs.setStringList(key, currentList);
  }

  static Future<List<String>> getStringList(String key) async {
    final prefs = await SharedPreferences.getInstance();
    return prefs.getStringList(key) ?? [];
  }

  static Future<void> updateStringList(String key, List<String> newList) async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setStringList(key, newList);
  }

  static Future<void> clearStringList(String key) async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.remove(key);
  }

  static Future<bool> containsInList(String key, String value) async {
    final list = await getStringList(key);
    return list.contains(value);
  }

  static Future<int> getListLength(String key) async {
    final list = await getStringList(key);
    return list.length;
  }

  // Specialized methods for different list types
  static Future<void> addTodoItem(String item) async {
    await addToStringList(_todoListKey, item);
  }

  static Future<void> removeTodoItem(String item) async {
    await removeFromStringList(_todoListKey, item);
  }

  static Future<List<String>> getTodoItems() async {
    return await getStringList(_todoListKey);
  }

  static Future<void> addToFavorites(String item) async {
    await addToStringList(_favoritesKey, item);
  }

  static Future<void> removeFromFavorites(String item) async {
    await removeFromStringList(_favoritesKey, item);
  }

  static Future<List<String>> getFavorites() async {
    return await getStringList(_favoritesKey);
  }

  static Future<void> addToSearchHistory(String query) async {
    final prefs = await SharedPreferences.getInstance();
    List<String> history = prefs.getStringList(_historyKey) ?? [];
    
    // Remove if already exists to avoid duplicates
    history.remove(query);
    
    // Add to beginning of list (most recent first)
    history.insert(0, query);
    
    // Keep only last 10 items
    if (history.length > 10) {
      history = history.take(10).toList();
    }
    
    await prefs.setStringList(_historyKey, history);
  }

  static Future<List<String>> getSearchHistory() async {
    return await getStringList(_historyKey);
  }

  static Future<void> addTag(String tag) async {
    await addToStringList(_tagsKey, tag);
  }

  static Future<List<String>> getTags() async {
    return await getStringList(_tagsKey);
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Key-Value List Storage',
      home: const ListStoragePage(),
    );
  }
}

class ListStoragePage extends StatefulWidget {
  const ListStoragePage({super.key});

  @override
  State<ListStoragePage> createState() => _ListStoragePageState();
}

class _ListStoragePageState extends State<ListStoragePage> 
    with TickerProviderStateMixin {
  late TabController _tabController;
  
  final _todoController = TextEditingController();
  final _favoriteController = TextEditingController();
  final _searchController = TextEditingController();
  final _tagController = TextEditingController();
  
  List<String> _todoItems = [];
  List<String> _favorites = [];
  List<String> _searchHistory = [];
  List<String> _tags = [];

  @override
  void initState() {
    super.initState();
    _tabController = TabController(length: 4, vsync: this);
    _loadAllLists();
  }

  Future<void> _loadAllLists() async {
    final todos = await ListStorageManager.getTodoItems();
    final favorites = await ListStorageManager.getFavorites();
    final history = await ListStorageManager.getSearchHistory();
    final tags = await ListStorageManager.getTags();
    
    setState(() {
      _todoItems = todos;
      _favorites = favorites;
      _searchHistory = history;
      _tags = tags;
    });
  }

  Future<void> _addTodoItem() async {
    if (_todoController.text.trim().isEmpty) return;
    
    await ListStorageManager.addTodoItem(_todoController.text.trim());
    _todoController.clear();
    await _loadAllLists();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Todo item added!')),
      );
    }
  }

  Future<void> _removeTodoItem(String item) async {
    await ListStorageManager.removeTodoItem(item);
    await _loadAllLists();
  }

  Future<void> _addToFavorites() async {
    if (_favoriteController.text.trim().isEmpty) return;
    
    await ListStorageManager.addToFavorites(_favoriteController.text.trim());
    _favoriteController.clear();
    await _loadAllLists();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Added to favorites!')),
      );
    }
  }

  Future<void> _removeFromFavorites(String item) async {
    await ListStorageManager.removeFromFavorites(item);
    await _loadAllLists();
  }

  Future<void> _performSearch() async {
    if (_searchController.text.trim().isEmpty) return;
    
    await ListStorageManager.addToSearchHistory(_searchController.text.trim());
    _searchController.clear();
    await _loadAllLists();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Search added to history!')),
      );
    }
  }

  Future<void> _addTag() async {
    if (_tagController.text.trim().isEmpty) return;
    
    await ListStorageManager.addTag(_tagController.text.trim());
    _tagController.clear();
    await _loadAllLists();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Tag added!')),
      );
    }
  }

  Future<void> _clearList(String listType) async {
    switch (listType) {
      case 'todos':
        await ListStorageManager.clearStringList('todo_list');
        break;
      case 'favorites':
        await ListStorageManager.clearStringList('favorites_list');
        break;
      case 'history':
        await ListStorageManager.clearStringList('search_history');
        break;
      case 'tags':
        await ListStorageManager.clearStringList('tags_list');
        break;
    }
    
    await _loadAllLists();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('$listType cleared!')),
      );
    }
  }

  Widget _buildTodoTab() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        children: [
          Row(
            children: [
              Expanded(
                child: TextField(
                  controller: _todoController,
                  decoration: const InputDecoration(
                    labelText: 'Todo item',
                    border: OutlineInputBorder(),
                  ),
                ),
              ),
              const SizedBox(width: 8),
              IconButton(
                icon: const Icon(Icons.add),
                onPressed: _addTodoItem,
              ),
            ],
          ),
          const SizedBox(height: 16),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text(
                'Todo Items (${_todoItems.length})',
                style: Theme.of(context).textTheme.titleMedium,
              ),
              TextButton(
                onPressed: () => _clearList('todos'),
                child: const Text('Clear All'),
              ),
            ],
          ),
          Expanded(
            child: _todoItems.isEmpty
                ? const Center(child: Text('No todo items'))
                : ListView.builder(
                    itemCount: _todoItems.length,
                    itemBuilder: (context, index) {
                      final item = _todoItems[index];
                      return Card(
                        child: ListTile(
                          leading: const Icon(Icons.check_box_outline_blank),
                          title: Text(item),
                          trailing: IconButton(
                            icon: const Icon(Icons.delete),
                            onPressed: () => _removeTodoItem(item),
                          ),
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }

  Widget _buildFavoritesTab() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        children: [
          Row(
            children: [
              Expanded(
                child: TextField(
                  controller: _favoriteController,
                  decoration: const InputDecoration(
                    labelText: 'Favorite item',
                    border: OutlineInputBorder(),
                  ),
                ),
              ),
              const SizedBox(width: 8),
              IconButton(
                icon: const Icon(Icons.favorite),
                onPressed: _addToFavorites,
              ),
            ],
          ),
          const SizedBox(height: 16),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text(
                'Favorites (${_favorites.length})',
                style: Theme.of(context).textTheme.titleMedium,
              ),
              TextButton(
                onPressed: () => _clearList('favorites'),
                child: const Text('Clear All'),
              ),
            ],
          ),
          Expanded(
            child: _favorites.isEmpty
                ? const Center(child: Text('No favorites'))
                : ListView.builder(
                    itemCount: _favorites.length,
                    itemBuilder: (context, index) {
                      final item = _favorites[index];
                      return Card(
                        child: ListTile(
                          leading: const Icon(Icons.favorite, color: Colors.red),
                          title: Text(item),
                          trailing: IconButton(
                            icon: const Icon(Icons.remove),
                            onPressed: () => _removeFromFavorites(item),
                          ),
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }

  Widget _buildSearchHistoryTab() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        children: [
          Row(
            children: [
              Expanded(
                child: TextField(
                  controller: _searchController,
                  decoration: const InputDecoration(
                    labelText: 'Search query',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.search),
                  ),
                ),
              ),
              const SizedBox(width: 8),
              IconButton(
                icon: const Icon(Icons.add),
                onPressed: _performSearch,
              ),
            ],
          ),
          const SizedBox(height: 16),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text(
                'Search History (${_searchHistory.length})',
                style: Theme.of(context).textTheme.titleMedium,
              ),
              TextButton(
                onPressed: () => _clearList('history'),
                child: const Text('Clear All'),
              ),
            ],
          ),
          Expanded(
            child: _searchHistory.isEmpty
                ? const Center(child: Text('No search history'))
                : ListView.builder(
                    itemCount: _searchHistory.length,
                    itemBuilder: (context, index) {
                      final item = _searchHistory[index];
                      return Card(
                        child: ListTile(
                          leading: const Icon(Icons.history),
                          title: Text(item),
                          subtitle: Text('Query #${index + 1}'),
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }

  Widget _buildTagsTab() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        children: [
          Row(
            children: [
              Expanded(
                child: TextField(
                  controller: _tagController,
                  decoration: const InputDecoration(
                    labelText: 'Tag name',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.tag),
                  ),
                ),
              ),
              const SizedBox(width: 8),
              IconButton(
                icon: const Icon(Icons.add),
                onPressed: _addTag,
              ),
            ],
          ),
          const SizedBox(height: 16),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text(
                'Tags (${_tags.length})',
                style: Theme.of(context).textTheme.titleMedium,
              ),
              TextButton(
                onPressed: () => _clearList('tags'),
                child: const Text('Clear All'),
              ),
            ],
          ),
          Expanded(
            child: _tags.isEmpty
                ? const Center(child: Text('No tags'))
                : Wrap(
                    spacing: 8,
                    runSpacing: 8,
                    children: _tags.map((tag) {
                      return Chip(
                        label: Text(tag),
                        deleteIcon: const Icon(Icons.close),
                        onDeleted: () async {
                          await ListStorageManager.removeFromStringList('tags_list', tag);
                          await _loadAllLists();
                        },
                      );
                    }).toList(),
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
        title: const Text('List Storage Management'),
        bottom: TabBar(
          controller: _tabController,
          tabs: const [
            Tab(icon: Icon(Icons.check_box), text: 'Todos'),
            Tab(icon: Icon(Icons.favorite), text: 'Favorites'),
            Tab(icon: Icon(Icons.history), text: 'History'),
            Tab(icon: Icon(Icons.tag), text: 'Tags'),
          ],
        ),
      ),
      body: TabBarView(
        controller: _tabController,
        children: [
          _buildTodoTab(),
          _buildFavoritesTab(),
          _buildSearchHistoryTab(),
          _buildTagsTab(),
        ],
      ),
    );
  }

  @override
  void dispose() {
    _tabController.dispose();
    _todoController.dispose();
    _favoriteController.dispose();
    _searchController.dispose();
    _tagController.dispose();
    super.dispose();
  }
}
```

Key-value list storage demonstrates managing multiple string lists using  
SharedPreferences. The system provides specialized methods for different  
list types including todos, favorites, search history, and tags with  
operations like adding, removing, and clearing entire lists.  

## Database Migration System

Implementing database versioning and migration strategies.  

```dart
import 'package:flutter/material.dart';
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';

void main() {
  runApp(const MyApp());
}

class DatabaseMigration {
  final int version;
  final String description;
  final Future<void> Function(Database db) upgrade;
  final Future<void> Function(Database db)? downgrade;

  DatabaseMigration({
    required this.version,
    required this.description,
    required this.upgrade,
    this.downgrade,
  });
}

class MigrationManager {
  static final List<DatabaseMigration> _migrations = [
    // Version 1: Initial schema
    DatabaseMigration(
      version: 1,
      description: 'Initial database creation',
      upgrade: (db) async {
        await db.execute('''
          CREATE TABLE users (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            email TEXT UNIQUE NOT NULL,
            created_at INTEGER NOT NULL
          )
        ''');
      },
    ),
    
    // Version 2: Add phone number
    DatabaseMigration(
      version: 2,
      description: 'Add phone number to users',
      upgrade: (db) async {
        await db.execute('ALTER TABLE users ADD COLUMN phone TEXT');
      },
      downgrade: (db) async {
        // SQLite doesn't support DROP COLUMN, so we recreate the table
        await db.execute('ALTER TABLE users RENAME TO users_backup');
        await db.execute('''
          CREATE TABLE users (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            email TEXT UNIQUE NOT NULL,
            created_at INTEGER NOT NULL
          )
        ''');
        await db.execute('''
          INSERT INTO users (id, name, email, created_at)
          SELECT id, name, email, created_at FROM users_backup
        ''');
        await db.execute('DROP TABLE users_backup');
      },
    ),
    
    // Version 3: Create posts table
    DatabaseMigration(
      version: 3,
      description: 'Create posts table',
      upgrade: (db) async {
        await db.execute('''
          CREATE TABLE posts (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            user_id INTEGER NOT NULL,
            title TEXT NOT NULL,
            content TEXT NOT NULL,
            created_at INTEGER NOT NULL,
            FOREIGN KEY (user_id) REFERENCES users (id)
          )
        ''');
        await db.execute('CREATE INDEX idx_posts_user_id ON posts (user_id)');
      },
      downgrade: (db) async {
        await db.execute('DROP INDEX IF EXISTS idx_posts_user_id');
        await db.execute('DROP TABLE IF EXISTS posts');
      },
    ),
    
    // Version 4: Add is_published to posts
    DatabaseMigration(
      version: 4,
      description: 'Add published status to posts',
      upgrade: (db) async {
        await db.execute('ALTER TABLE posts ADD COLUMN is_published INTEGER DEFAULT 0');
      },
    ),
    
    // Version 5: Create categories table and add category_id to posts
    DatabaseMigration(
      version: 5,
      description: 'Add categories support',
      upgrade: (db) async {
        await db.execute('''
          CREATE TABLE categories (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT UNIQUE NOT NULL,
            description TEXT,
            created_at INTEGER NOT NULL
          )
        ''');
        
        await db.execute('ALTER TABLE posts ADD COLUMN category_id INTEGER');
        await db.execute('CREATE INDEX idx_posts_category_id ON posts (category_id)');
        
        // Insert default category
        await db.execute('''
          INSERT INTO categories (name, description, created_at)
          VALUES ('General', 'Default category', ?)
        ''', [DateTime.now().millisecondsSinceEpoch]);
      },
    ),
  ];

  static DatabaseMigration? getMigration(int version) {
    try {
      return _migrations.firstWhere((migration) => migration.version == version);
    } catch (e) {
      return null;
    }
  }

  static List<DatabaseMigration> getMigrationsFromVersion(int fromVersion) {
    return _migrations.where((migration) => migration.version > fromVersion).toList();
  }

  static int get latestVersion => _migrations.isNotEmpty ? 
      _migrations.map((m) => m.version).reduce((a, b) => a > b ? a : b) : 1;
}

class MigrationDatabase {
  static final MigrationDatabase _instance = MigrationDatabase._internal();
  factory MigrationDatabase() => _instance;
  MigrationDatabase._internal();

  static Database? _database;

  Future<Database> get database async {
    _database ??= await _initDatabase();
    return _database!;
  }

  Future<Database> _initDatabase() async {
    String path = join(await getDatabasesPath(), 'migration_example.db');
    
    return await openDatabase(
      path,
      version: MigrationManager.latestVersion,
      onCreate: _onCreate,
      onUpgrade: _onUpgrade,
      onDowngrade: _onDowngrade,
    );
  }

  Future<void> _onCreate(Database db, int version) async {
    // Run all migrations up to the current version
    for (int i = 1; i <= version; i++) {
      final migration = MigrationManager.getMigration(i);
      if (migration != null) {
        await migration.upgrade(db);
        await _logMigration(db, migration, 'created');
      }
    }
  }

  Future<void> _onUpgrade(Database db, int oldVersion, int newVersion) async {
    final migrations = MigrationManager.getMigrationsFromVersion(oldVersion);
    
    for (final migration in migrations) {
      if (migration.version <= newVersion) {
        await migration.upgrade(db);
        await _logMigration(db, migration, 'upgraded');
      }
    }
  }

  Future<void> _onDowngrade(Database db, int oldVersion, int newVersion) async {
    // Run downgrades in reverse order
    for (int i = oldVersion; i > newVersion; i--) {
      final migration = MigrationManager.getMigration(i);
      if (migration?.downgrade != null) {
        await migration!.downgrade!(db);
        await _logMigration(db, migration, 'downgraded');
      }
    }
  }

  Future<void> _logMigration(Database db, DatabaseMigration migration, String action) async {
    try {
      await db.execute('''
        CREATE TABLE IF NOT EXISTS migration_logs (
          id INTEGER PRIMARY KEY AUTOINCREMENT,
          version INTEGER NOT NULL,
          description TEXT NOT NULL,
          action TEXT NOT NULL,
          executed_at INTEGER NOT NULL
        )
      ''');
      
      await db.insert('migration_logs', {
        'version': migration.version,
        'description': migration.description,
        'action': action,
        'executed_at': DateTime.now().millisecondsSinceEpoch,
      });
    } catch (e) {
      // If logging fails, don't fail the migration
      print('Failed to log migration: $e');
    }
  }

  Future<List<Map<String, dynamic>>> getMigrationLogs() async {
    final db = await database;
    return await db.query('migration_logs', orderBy: 'executed_at DESC');
  }

  Future<int> getCurrentVersion() async {
    final db = await database;
    final result = await db.rawQuery('PRAGMA user_version');
    return result.first['user_version'] as int;
  }

  Future<List<String>> getTableNames() async {
    final db = await database;
    final result = await db.rawQuery(
      "SELECT name FROM sqlite_master WHERE type='table' AND name NOT LIKE 'sqlite_%'"
    );
    return result.map((row) => row['name'] as String).toList();
  }

  Future<List<Map<String, dynamic>>> getTableInfo(String tableName) async {
    final db = await database;
    return await db.rawQuery('PRAGMA table_info($tableName)');
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Database Migration System',
      home: const MigrationPage(),
    );
  }
}

class MigrationPage extends StatefulWidget {
  const MigrationPage({super.key});

  @override
  State<MigrationPage> createState() => _MigrationPageState();
}

class _MigrationPageState extends State<MigrationPage> {
  final MigrationDatabase _database = MigrationDatabase();
  
  int _currentVersion = 0;
  int _latestVersion = 0;
  List<String> _tableNames = [];
  List<Map<String, dynamic>> _migrationLogs = [];
  Map<String, List<Map<String, dynamic>>> _tableInfos = {};

  @override
  void initState() {
    super.initState();
    _loadDatabaseInfo();
  }

  Future<void> _loadDatabaseInfo() async {
    try {
      final currentVersion = await _database.getCurrentVersion();
      final latestVersion = MigrationManager.latestVersion;
      final tableNames = await _database.getTableNames();
      final migrationLogs = await _database.getMigrationLogs();
      
      final Map<String, List<Map<String, dynamic>>> tableInfos = {};
      for (final tableName in tableNames) {
        tableInfos[tableName] = await _database.getTableInfo(tableName);
      }
      
      setState(() {
        _currentVersion = currentVersion;
        _latestVersion = latestVersion;
        _tableNames = tableNames;
        _migrationLogs = migrationLogs;
        _tableInfos = tableInfos;
      });
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error loading database info: $e')),
        );
      }
    }
  }

  Future<void> _refreshDatabase() async {
    await _loadDatabaseInfo();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Database info refreshed!')),
      );
    }
  }

  Widget _buildVersionInfo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Database Version Information',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                const Icon(Icons.info, size: 16),
                const SizedBox(width: 8),
                Text('Current Version: $_currentVersion'),
              ],
            ),
            Row(
              children: [
                const Icon(Icons.update, size: 16),
                const SizedBox(width: 8),
                Text('Latest Version: $_latestVersion'),
              ],
            ),
            Row(
              children: [
                const Icon(Icons.table_chart, size: 16),
                const SizedBox(width: 8),
                Text('Tables: ${_tableNames.length}'),
              ],
            ),
            if (_currentVersion < _latestVersion) ...[
              const SizedBox(height: 8),
              Container(
                padding: const EdgeInsets.all(8),
                decoration: BoxDecoration(
                  color: Colors.orange.withOpacity(0.2),
                  borderRadius: BorderRadius.circular(4),
                ),
                child: const Row(
                  children: [
                    Icon(Icons.warning, color: Colors.orange, size: 16),
                    SizedBox(width: 8),
                    Text('Database update available'),
                  ],
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }

  Widget _buildTablesInfo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Database Schema',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            ..._tableNames.map((tableName) {
              final tableInfo = _tableInfos[tableName] ?? [];
              return ExpansionTile(
                leading: const Icon(Icons.table_chart),
                title: Text(tableName),
                subtitle: Text('${tableInfo.length} columns'),
                children: tableInfo.map((column) {
                  return ListTile(
                    dense: true,
                    leading: const Icon(Icons.view_column, size: 16),
                    title: Text(column['name']),
                    subtitle: Text('${column['type']} ${column['pk'] == 1 ? '(PK)' : ''}'),
                    trailing: column['notnull'] == 1 
                        ? const Icon(Icons.star, size: 16, color: Colors.orange)
                        : null,
                  );
                }).toList(),
              );
            }),
          ],
        ),
      ),
    );
  }

  Widget _buildMigrationLogs() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Migration History',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            if (_migrationLogs.isEmpty)
              const Text('No migration logs available')
            else
              Column(
                children: _migrationLogs.take(10).map((log) {
                  final executedAt = DateTime.fromMillisecondsSinceEpoch(log['executed_at']);
                  return ListTile(
                    dense: true,
                    leading: Icon(
                      log['action'] == 'upgraded' ? Icons.upgrade :
                      log['action'] == 'downgraded' ? Icons.downgrade :
                      Icons.create,
                      size: 16,
                    ),
                    title: Text('Version ${log['version']} - ${log['action']}'),
                    subtitle: Text(log['description']),
                    trailing: Text(
                      '${executedAt.day}/${executedAt.month} ${executedAt.hour}:${executedAt.minute}',
                      style: const TextStyle(fontSize: 12),
                    ),
                  );
                }).toList(),
              ),
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Database Migration System'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _refreshDatabase,
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            _buildVersionInfo(),
            const SizedBox(height: 16),
            _buildTablesInfo(),
            const SizedBox(height: 16),
            _buildMigrationLogs(),
          ],
        ),
      ),
    );
  }
}
```

Database migration system provides structured schema evolution with version  
control and rollback capabilities. The system tracks migration history,  
supports both upgrade and downgrade operations, and provides detailed schema  
inspection tools for database management and debugging.  

## Settings Persistence Manager

Comprehensive settings management with categories and validation.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

enum SettingType {
  string,
  integer,
  double,
  boolean,
  stringList,
}

class SettingDefinition {
  final String key;
  final String displayName;
  final String description;
  final SettingType type;
  final dynamic defaultValue;
  final dynamic? minValue;
  final dynamic? maxValue;
  final List<dynamic>? allowedValues;
  final bool isRequired;
  final String category;

  SettingDefinition({
    required this.key,
    required this.displayName,
    required this.description,
    required this.type,
    required this.defaultValue,
    this.minValue,
    this.maxValue,
    this.allowedValues,
    this.isRequired = false,
    required this.category,
  });
}

class SettingsManager {
  static final SettingsManager _instance = SettingsManager._internal();
  factory SettingsManager() => _instance;
  SettingsManager._internal();

  static const String _settingsPrefix = 'setting_';
  
  static final List<SettingDefinition> _settingDefinitions = [
    // User Interface Settings
    SettingDefinition(
      key: 'theme_mode',
      displayName: 'Theme Mode',
      description: 'Choose between light, dark, or system theme',
      type: SettingType.string,
      defaultValue: 'system',
      allowedValues: ['light', 'dark', 'system'],
      category: 'User Interface',
    ),
    SettingDefinition(
      key: 'font_size',
      displayName: 'Font Size',
      description: 'Adjust text size throughout the app',
      type: SettingType.double,
      defaultValue: 16.0,
      minValue: 12.0,
      maxValue: 24.0,
      category: 'User Interface',
    ),
    SettingDefinition(
      key: 'language',
      displayName: 'Language',
      description: 'Select your preferred language',
      type: SettingType.string,
      defaultValue: 'English',
      allowedValues: ['English', 'Spanish', 'French', 'German', 'Chinese'],
      category: 'User Interface',
    ),
    SettingDefinition(
      key: 'animations_enabled',
      displayName: 'Enable Animations',
      description: 'Turn animations on or off',
      type: SettingType.boolean,
      defaultValue: true,
      category: 'User Interface',
    ),
    
    // Notification Settings
    SettingDefinition(
      key: 'push_notifications',
      displayName: 'Push Notifications',
      description: 'Receive push notifications',
      type: SettingType.boolean,
      defaultValue: true,
      category: 'Notifications',
    ),
    SettingDefinition(
      key: 'notification_sound',
      displayName: 'Notification Sound',
      description: 'Play sound for notifications',
      type: SettingType.boolean,
      defaultValue: true,
      category: 'Notifications',
    ),
    SettingDefinition(
      key: 'notification_types',
      displayName: 'Notification Types',
      description: 'Types of notifications to receive',
      type: SettingType.stringList,
      defaultValue: ['messages', 'updates'],
      allowedValues: ['messages', 'updates', 'promotions', 'security'],
      category: 'Notifications',
    ),
    
    // Privacy Settings
    SettingDefinition(
      key: 'data_collection',
      displayName: 'Allow Data Collection',
      description: 'Allow anonymous usage data collection',
      type: SettingType.boolean,
      defaultValue: false,
      category: 'Privacy',
    ),
    SettingDefinition(
      key: 'location_sharing',
      displayName: 'Location Sharing',
      description: 'Share location for relevant features',
      type: SettingType.boolean,
      defaultValue: false,
      category: 'Privacy',
    ),
    
    // Performance Settings
    SettingDefinition(
      key: 'cache_size',
      displayName: 'Cache Size (MB)',
      description: 'Maximum cache size in megabytes',
      type: SettingType.integer,
      defaultValue: 100,
      minValue: 50,
      maxValue: 500,
      category: 'Performance',
    ),
    SettingDefinition(
      key: 'auto_sync',
      displayName: 'Auto Sync',
      description: 'Automatically sync data in background',
      type: SettingType.boolean,
      defaultValue: true,
      category: 'Performance',
    ),
  ];

  Future<T> getSetting<T>(String key) async {
    final definition = _getSettingDefinition(key);
    if (definition == null) {
      throw ArgumentError('Setting definition not found for key: $key');
    }

    final prefs = await SharedPreferences.getInstance();
    final prefKey = '$_settingsPrefix$key';

    switch (definition.type) {
      case SettingType.string:
        return prefs.getString(prefKey) as T? ?? definition.defaultValue;
      case SettingType.integer:
        return prefs.getInt(prefKey) as T? ?? definition.defaultValue;
      case SettingType.double:
        return prefs.getDouble(prefKey) as T? ?? definition.defaultValue;
      case SettingType.boolean:
        return prefs.getBool(prefKey) as T? ?? definition.defaultValue;
      case SettingType.stringList:
        return prefs.getStringList(prefKey) as T? ?? definition.defaultValue;
    }
  }

  Future<bool> setSetting<T>(String key, T value) async {
    final definition = _getSettingDefinition(key);
    if (definition == null) {
      throw ArgumentError('Setting definition not found for key: $key');
    }

    if (!_validateValue(definition, value)) {
      return false;
    }

    final prefs = await SharedPreferences.getInstance();
    final prefKey = '$_settingsPrefix$key';

    switch (definition.type) {
      case SettingType.string:
        return await prefs.setString(prefKey, value as String);
      case SettingType.integer:
        return await prefs.setInt(prefKey, value as int);
      case SettingType.double:
        return await prefs.setDouble(prefKey, value as double);
      case SettingType.boolean:
        return await prefs.setBool(prefKey, value as bool);
      case SettingType.stringList:
        return await prefs.setStringList(prefKey, value as List<String>);
    }
  }

  Future<void> resetSetting(String key) async {
    final definition = _getSettingDefinition(key);
    if (definition != null) {
      await setSetting(key, definition.defaultValue);
    }
  }

  Future<void> resetAllSettings() async {
    for (final definition in _settingDefinitions) {
      await resetSetting(definition.key);
    }
  }

  Future<Map<String, dynamic>> getAllSettings() async {
    final Map<String, dynamic> settings = {};
    
    for (final definition in _settingDefinitions) {
      settings[definition.key] = await getSetting(definition.key);
    }
    
    return settings;
  }

  Future<void> importSettings(Map<String, dynamic> settings) async {
    for (final entry in settings.entries) {
      final definition = _getSettingDefinition(entry.key);
      if (definition != null && _validateValue(definition, entry.value)) {
        await setSetting(entry.key, entry.value);
      }
    }
  }

  Future<String> exportSettings() async {
    final settings = await getAllSettings();
    return json.encode(settings);
  }

  List<SettingDefinition> getSettingsByCategory(String category) {
    return _settingDefinitions.where((def) => def.category == category).toList();
  }

  List<String> getCategories() {
    return _settingDefinitions.map((def) => def.category).toSet().toList();
  }

  SettingDefinition? _getSettingDefinition(String key) {
    try {
      return _settingDefinitions.firstWhere((def) => def.key == key);
    } catch (e) {
      return null;
    }
  }

  bool _validateValue(SettingDefinition definition, dynamic value) {
    // Type validation
    switch (definition.type) {
      case SettingType.string:
        if (value is! String) return false;
        break;
      case SettingType.integer:
        if (value is! int) return false;
        break;
      case SettingType.double:
        if (value is! double) return false;
        break;
      case SettingType.boolean:
        if (value is! bool) return false;
        break;
      case SettingType.stringList:
        if (value is! List<String>) return false;
        break;
    }

    // Range validation
    if (definition.minValue != null && value < definition.minValue) {
      return false;
    }
    if (definition.maxValue != null && value > definition.maxValue) {
      return false;
    }

    // Allowed values validation
    if (definition.allowedValues != null) {
      if (definition.type == SettingType.stringList) {
        final list = value as List<String>;
        for (final item in list) {
          if (!definition.allowedValues!.contains(item)) {
            return false;
          }
        }
      } else {
        if (!definition.allowedValues!.contains(value)) {
          return false;
        }
      }
    }

    return true;
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Settings Persistence Manager',
      home: const SettingsPage(),
    );
  }
}

class SettingsPage extends StatefulWidget {
  const SettingsPage({super.key});

  @override
  State<SettingsPage> createState() => _SettingsPageState();
}

class _SettingsPageState extends State<SettingsPage> {
  final SettingsManager _settingsManager = SettingsManager();
  Map<String, dynamic> _currentSettings = {};
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _loadSettings();
  }

  Future<void> _loadSettings() async {
    setState(() {
      _isLoading = true;
    });

    final settings = await _settingsManager.getAllSettings();
    
    setState(() {
      _currentSettings = settings;
      _isLoading = false;
    });
  }

  Future<void> _updateSetting(String key, dynamic value) async {
    final success = await _settingsManager.setSetting(key, value);
    
    if (success) {
      setState(() {
        _currentSettings[key] = value;
      });
      
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Setting updated!')),
        );
      }
    } else {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Invalid setting value!')),
        );
      }
    }
  }

  Future<void> _resetAllSettings() async {
    await _settingsManager.resetAllSettings();
    await _loadSettings();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('All settings reset to defaults!')),
      );
    }
  }

  Future<void> _exportSettings() async {
    final exportData = await _settingsManager.exportSettings();
    
    if (mounted) {
      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: const Text('Export Settings'),
          content: SingleChildScrollView(
            child: Text(exportData),
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.pop(context),
              child: const Text('Close'),
            ),
          ],
        ),
      );
    }
  }

  Widget _buildSettingWidget(SettingDefinition definition) {
    final currentValue = _currentSettings[definition.key];

    switch (definition.type) {
      case SettingType.boolean:
        return SwitchListTile(
          title: Text(definition.displayName),
          subtitle: Text(definition.description),
          value: currentValue ?? definition.defaultValue,
          onChanged: (value) => _updateSetting(definition.key, value),
        );

      case SettingType.string:
        if (definition.allowedValues != null) {
          return ListTile(
            title: Text(definition.displayName),
            subtitle: Text(definition.description),
            trailing: DropdownButton<String>(
              value: currentValue ?? definition.defaultValue,
              items: definition.allowedValues!.map((value) {
                return DropdownMenuItem<String>(
                  value: value,
                  child: Text(value),
                );
              }).toList(),
              onChanged: (value) {
                if (value != null) {
                  _updateSetting(definition.key, value);
                }
              },
            ),
          );
        }
        break;

      case SettingType.integer:
        return ListTile(
          title: Text(definition.displayName),
          subtitle: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(definition.description),
              Slider(
                value: (currentValue ?? definition.defaultValue).toDouble(),
                min: (definition.minValue ?? 0).toDouble(),
                max: (definition.maxValue ?? 100).toDouble(),
                divisions: (definition.maxValue! - definition.minValue!).toInt(),
                label: currentValue?.toString() ?? definition.defaultValue.toString(),
                onChanged: (value) => _updateSetting(definition.key, value.toInt()),
              ),
            ],
          ),
        );

      case SettingType.double:
        return ListTile(
          title: Text(definition.displayName),
          subtitle: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(definition.description),
              Slider(
                value: currentValue ?? definition.defaultValue,
                min: definition.minValue ?? 0.0,
                max: definition.maxValue ?? 100.0,
                divisions: ((definition.maxValue! - definition.minValue!) * 10).toInt(),
                label: (currentValue ?? definition.defaultValue).toStringAsFixed(1),
                onChanged: (value) => _updateSetting(definition.key, value),
              ),
            ],
          ),
        );

      case SettingType.stringList:
        return ExpansionTile(
          title: Text(definition.displayName),
          subtitle: Text(definition.description),
          children: definition.allowedValues!.map((option) {
            final currentList = List<String>.from(currentValue ?? definition.defaultValue);
            final isSelected = currentList.contains(option);
            
            return CheckboxListTile(
              title: Text(option),
              value: isSelected,
              onChanged: (selected) {
                if (selected == true) {
                  currentList.add(option);
                } else {
                  currentList.remove(option);
                }
                _updateSetting(definition.key, currentList);
              },
            );
          }).toList(),
        );
    }

    return ListTile(
      title: Text(definition.displayName),
      subtitle: Text(definition.description),
    );
  }

  @override
  Widget build(BuildContext context) {
    if (_isLoading) {
      return const Scaffold(
        body: Center(child: CircularProgressIndicator()),
      );
    }

    final categories = _settingsManager.getCategories();

    return Scaffold(
      appBar: AppBar(
        title: const Text('Settings Manager'),
        actions: [
          IconButton(
            icon: const Icon(Icons.download),
            onPressed: _exportSettings,
          ),
          IconButton(
            icon: const Icon(Icons.restore),
            onPressed: _resetAllSettings,
          ),
        ],
      ),
      body: ListView(
        children: categories.map((category) {
          final categorySettings = _settingsManager.getSettingsByCategory(category);
          
          return ExpansionTile(
            title: Text(category),
            initiallyExpanded: true,
            children: categorySettings.map(_buildSettingWidget).toList(),
          );
        }).toList(),
      ),
    );
  }
}
```

Settings persistence manager provides comprehensive configuration management  
with type validation, categories, and import/export functionality. The system  
supports various data types, range validation, and allowed values with a  
clean separation between definition and storage layers.  

## Local Image Storage

Storing and managing images in local file system.  

```dart
import 'package:flutter/material.dart';
import 'dart:io';
import 'package:path_provider/path_provider.dart';
import 'package:image_picker/image_picker.dart';
import 'dart:typed_data';
import 'package:crypto/crypto.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class ImageMetadata {
  final String id;
  final String originalName;
  final String storedPath;
  final DateTime createdAt;
  final int sizeBytes;
  final String mimeType;
  final Map<String, String> tags;

  ImageMetadata({
    required this.id,
    required this.originalName,
    required this.storedPath,
    required this.createdAt,
    required this.sizeBytes,
    required this.mimeType,
    required this.tags,
  });

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'originalName': originalName,
      'storedPath': storedPath,
      'createdAt': createdAt.toIso8601String(),
      'sizeBytes': sizeBytes,
      'mimeType': mimeType,
      'tags': tags,
    };
  }

  factory ImageMetadata.fromJson(Map<String, dynamic> json) {
    return ImageMetadata(
      id: json['id'],
      originalName: json['originalName'],
      storedPath: json['storedPath'],
      createdAt: DateTime.parse(json['createdAt']),
      sizeBytes: json['sizeBytes'],
      mimeType: json['mimeType'],
      tags: Map<String, String>.from(json['tags']),
    );
  }
}

class LocalImageStorage {
  static final LocalImageStorage _instance = LocalImageStorage._internal();
  factory LocalImageStorage() => _instance;
  LocalImageStorage._internal();

  late String _imagesDirectory;
  late String _metadataFile;
  final Map<String, ImageMetadata> _imageCache = {};
  bool _initialized = false;

  Future<void> initialize() async {
    if (_initialized) return;

    final appDir = await getApplicationDocumentsDirectory();
    _imagesDirectory = '${appDir.path}/stored_images';
    _metadataFile = '${appDir.path}/image_metadata.json';

    // Create directories if they don't exist
    await Directory(_imagesDirectory).create(recursive: true);

    // Load existing metadata
    await _loadMetadata();

    _initialized = true;
  }

  Future<void> _loadMetadata() async {
    try {
      final file = File(_metadataFile);
      if (await file.exists()) {
        final jsonString = await file.readAsString();
        final Map<String, dynamic> data = json.decode(jsonString);
        
        _imageCache.clear();
        data.forEach((key, value) {
          _imageCache[key] = ImageMetadata.fromJson(value);
        });
      }
    } catch (e) {
      // If metadata is corrupted, start fresh
      _imageCache.clear();
    }
  }

  Future<void> _saveMetadata() async {
    try {
      final file = File(_metadataFile);
      final Map<String, dynamic> data = {};
      
      _imageCache.forEach((key, value) {
        data[key] = value.toJson();
      });
      
      await file.writeAsString(json.encode(data));
    } catch (e) {
      throw Exception('Failed to save metadata: $e');
    }
  }

  String _generateImageId(Uint8List imageBytes) {
    final digest = sha256.convert(imageBytes);
    return digest.toString().substring(0, 16);
  }

  Future<String> storeImage(
    File imageFile, {
    String? originalName,
    Map<String, String>? tags,
  }) async {
    await initialize();

    final imageBytes = await imageFile.readAsBytes();
    final imageId = _generateImageId(imageBytes);
    
    // Check if image already exists
    if (_imageCache.containsKey(imageId)) {
      return imageId;
    }

    final extension = imageFile.path.split('.').last.toLowerCase();
    final storedPath = '$_imagesDirectory/$imageId.$extension';
    
    // Copy image to storage directory
    await File(imageFile.path).copy(storedPath);

    final metadata = ImageMetadata(
      id: imageId,
      originalName: originalName ?? imageFile.path.split('/').last,
      storedPath: storedPath,
      createdAt: DateTime.now(),
      sizeBytes: imageBytes.length,
      mimeType: _getMimeType(extension),
      tags: tags ?? {},
    );

    _imageCache[imageId] = metadata;
    await _saveMetadata();

    return imageId;
  }

  Future<File?> getImage(String imageId) async {
    await initialize();

    final metadata = _imageCache[imageId];
    if (metadata == null) return null;

    final file = File(metadata.storedPath);
    if (await file.exists()) {
      return file;
    } else {
      // File was deleted externally, remove from cache
      _imageCache.remove(imageId);
      await _saveMetadata();
      return null;
    }
  }

  Future<ImageMetadata?> getImageMetadata(String imageId) async {
    await initialize();
    return _imageCache[imageId];
  }

  Future<List<ImageMetadata>> getAllImages({
    String? tag,
    DateTime? fromDate,
    DateTime? toDate,
  }) async {
    await initialize();

    var images = _imageCache.values.toList();

    if (tag != null) {
      images = images.where((img) => img.tags.containsValue(tag)).toList();
    }

    if (fromDate != null) {
      images = images.where((img) => img.createdAt.isAfter(fromDate)).toList();
    }

    if (toDate != null) {
      images = images.where((img) => img.createdAt.isBefore(toDate)).toList();
    }

    images.sort((a, b) => b.createdAt.compareTo(a.createdAt));
    return images;
  }

  Future<bool> deleteImage(String imageId) async {
    await initialize();

    final metadata = _imageCache[imageId];
    if (metadata == null) return false;

    try {
      final file = File(metadata.storedPath);
      if (await file.exists()) {
        await file.delete();
      }
      
      _imageCache.remove(imageId);
      await _saveMetadata();
      return true;
    } catch (e) {
      return false;
    }
  }

  Future<void> updateImageTags(String imageId, Map<String, String> tags) async {
    await initialize();

    final metadata = _imageCache[imageId];
    if (metadata == null) return;

    final updatedMetadata = ImageMetadata(
      id: metadata.id,
      originalName: metadata.originalName,
      storedPath: metadata.storedPath,
      createdAt: metadata.createdAt,
      sizeBytes: metadata.sizeBytes,
      mimeType: metadata.mimeType,
      tags: tags,
    );

    _imageCache[imageId] = updatedMetadata;
    await _saveMetadata();
  }

  Future<int> getTotalStorageSize() async {
    await initialize();
    
    return _imageCache.values
        .map((metadata) => metadata.sizeBytes)
        .fold(0, (total, size) => total + size);
  }

  Future<void> clearAll() async {
    await initialize();

    try {
      final directory = Directory(_imagesDirectory);
      if (await directory.exists()) {
        await directory.delete(recursive: true);
        await directory.create();
      }

      _imageCache.clear();
      await _saveMetadata();
    } catch (e) {
      throw Exception('Failed to clear storage: $e');
    }
  }

  String _getMimeType(String extension) {
    switch (extension.toLowerCase()) {
      case 'jpg':
      case 'jpeg':
        return 'image/jpeg';
      case 'png':
        return 'image/png';
      case 'gif':
        return 'image/gif';
      case 'webp':
        return 'image/webp';
      default:
        return 'application/octet-stream';
    }
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Local Image Storage',
      home: const ImageStoragePage(),
    );
  }
}

class ImageStoragePage extends StatefulWidget {
  const ImageStoragePage({super.key});

  @override
  State<ImageStoragePage> createState() => _ImageStoragePageState();
}

class _ImageStoragePageState extends State<ImageStoragePage> {
  final LocalImageStorage _storage = LocalImageStorage();
  final ImagePicker _picker = ImagePicker();
  
  List<ImageMetadata> _images = [];
  int _totalStorage = 0;
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _loadImages();
  }

  Future<void> _loadImages() async {
    setState(() {
      _isLoading = true;
    });

    final images = await _storage.getAllImages();
    final totalSize = await _storage.getTotalStorageSize();

    setState(() {
      _images = images;
      _totalStorage = totalSize;
      _isLoading = false;
    });
  }

  Future<void> _pickAndStoreImage() async {
    try {
      final XFile? image = await _picker.pickImage(source: ImageSource.gallery);
      if (image == null) return;

      setState(() {
        _isLoading = true;
      });

      final file = File(image.path);
      await _storage.storeImage(
        file,
        originalName: image.name,
        tags: {'source': 'gallery', 'imported': DateTime.now().toIso8601String()},
      );

      await _loadImages();

      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Image stored successfully!')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error storing image: $e')),
        );
      }
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _deleteImage(String imageId) async {
    final success = await _storage.deleteImage(imageId);
    
    if (success) {
      await _loadImages();
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Image deleted!')),
        );
      }
    }
  }

  Future<void> _showImageDetails(ImageMetadata metadata) async {
    final file = await _storage.getImage(metadata.id);
    
    if (mounted && file != null) {
      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: Text(metadata.originalName),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Image.file(file, height: 200, fit: BoxFit.cover),
              const SizedBox(height: 16),
              Text('Size: ${(_formatBytes(metadata.sizeBytes))}'),
              Text('Type: ${metadata.mimeType}'),
              Text('Created: ${metadata.createdAt.toString().split('.')[0]}'),
              if (metadata.tags.isNotEmpty) ...[
                const SizedBox(height: 8),
                const Text('Tags:'),
                ...metadata.tags.entries.map(
                  (entry) => Text('  ${entry.key}: ${entry.value}'),
                ),
              ],
            ],
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.pop(context),
              child: const Text('Close'),
            ),
            TextButton(
              onPressed: () {
                Navigator.pop(context);
                _deleteImage(metadata.id);
              },
              child: const Text('Delete'),
            ),
          ],
        ),
      );
    }
  }

  String _formatBytes(int bytes) {
    if (bytes < 1024) return '$bytes B';
    if (bytes < 1024 * 1024) return '${(bytes / 1024).toStringAsFixed(1)} KB';
    return '${(bytes / (1024 * 1024)).toStringAsFixed(1)} MB';
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Local Image Storage'),
        actions: [
          IconButton(
            icon: const Icon(Icons.info),
            onPressed: () {
              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  title: const Text('Storage Info'),
                  content: Column(
                    mainAxisSize: MainAxisSize.min,
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text('Total Images: ${_images.length}'),
                      Text('Total Size: ${_formatBytes(_totalStorage)}'),
                    ],
                  ),
                  actions: [
                    TextButton(
                      onPressed: () => Navigator.pop(context),
                      child: const Text('Close'),
                    ),
                  ],
                ),
              );
            },
          ),
        ],
      ),
      body: _isLoading
          ? const Center(child: CircularProgressIndicator())
          : _images.isEmpty
              ? const Center(
                  child: Text('No images stored\nTap + to add images'),
                )
              : GridView.builder(
                  padding: const EdgeInsets.all(8),
                  gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 2,
                    crossAxisSpacing: 8,
                    mainAxisSpacing: 8,
                  ),
                  itemCount: _images.length,
                  itemBuilder: (context, index) {
                    final metadata = _images[index];
                    return FutureBuilder<File?>(
                      future: _storage.getImage(metadata.id),
                      builder: (context, snapshot) {
                        if (snapshot.hasData && snapshot.data != null) {
                          return GestureDetector(
                            onTap: () => _showImageDetails(metadata),
                            child: Card(
                              clipBehavior: Clip.antiAlias,
                              child: Column(
                                children: [
                                  Expanded(
                                    child: Image.file(
                                      snapshot.data!,
                                      fit: BoxFit.cover,
                                      width: double.infinity,
                                    ),
                                  ),
                                  Padding(
                                    padding: const EdgeInsets.all(8.0),
                                    child: Text(
                                      metadata.originalName,
                                      style: const TextStyle(fontSize: 12),
                                      maxLines: 1,
                                      overflow: TextOverflow.ellipsis,
                                    ),
                                  ),
                                ],
                              ),
                            ),
                          );
                        }
                        return const Card(
                          child: Center(child: CircularProgressIndicator()),
                        );
                      },
                    );
                  },
                ),
      floatingActionButton: FloatingActionButton(
        onPressed: _pickAndStoreImage,
        child: const Icon(Icons.add_a_photo),
      ),
    );
  }
}
```

Local image storage provides comprehensive image management with metadata  
tracking, deduplication, and efficient retrieval. The system handles file  
operations, generates unique identifiers, and maintains searchable metadata  
with tags and timestamps for organized image storage.  

## Data Export and Import

Implementing data export and import functionality for backup and migration.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';
import 'dart:io';
import 'package:path_provider/path_provider.dart';
import 'package:file_picker/file_picker.dart';

void main() {
  runApp(const MyApp());
}

enum ExportFormat {
  json,
  csv,
  xml,
}

class DataExportImport {
  static final DataExportImport _instance = DataExportImport._internal();
  factory DataExportImport() => _instance;
  DataExportImport._internal();

  static const String _dataPrefix = 'export_data_';
  static const String _metadataKey = 'export_metadata';

  Future<String> exportAllData({ExportFormat format = ExportFormat.json}) async {
    final prefs = await SharedPreferences.getInstance();
    final allKeys = prefs.getKeys();
    
    final Map<String, dynamic> exportData = {
      'metadata': {
        'exportedAt': DateTime.now().toIso8601String(),
        'version': '1.0',
        'format': format.name,
        'totalKeys': allKeys.length,
      },
      'data': {},
    };

    // Export all preferences
    for (final key in allKeys) {
      final value = prefs.get(key);
      if (value != null) {
        exportData['data'][key] = {
          'value': value,
          'type': value.runtimeType.toString(),
        };
      }
    }

    // Add application-specific data
    final appDir = await getApplicationDocumentsDirectory();
    final dataFiles = await _getDataFiles(appDir);
    
    exportData['files'] = dataFiles;

    switch (format) {
      case ExportFormat.json:
        return _exportAsJson(exportData);
      case ExportFormat.csv:
        return _exportAsCsv(exportData);
      case ExportFormat.xml:
        return _exportAsXml(exportData);
    }
  }

  Future<Map<String, List<String>>> _getDataFiles(Directory appDir) async {
    final Map<String, List<String>> files = {};
    
    try {
      final entities = await appDir.list(recursive: true).toList();
      
      for (final entity in entities) {
        if (entity is File && !entity.path.contains('.DS_Store')) {
          final relativePath = entity.path.replaceAll(appDir.path, '');
          final content = await entity.readAsString();
          
          files[relativePath] = [content];
        }
      }
    } catch (e) {
      // Handle permission or access errors
    }
    
    return files;
  }

  String _exportAsJson(Map<String, dynamic> data) {
    return const JsonEncoder.withIndent('  ').convert(data);
  }

  String _exportAsCsv(Map<String, dynamic> data) {
    final buffer = StringBuffer();
    
    // CSV Header
    buffer.writeln('key,value,type,category');
    
    // Metadata
    buffer.writeln('"metadata.exportedAt","${data['metadata']['exportedAt']}","DateTime","metadata"');
    buffer.writeln('"metadata.version","${data['metadata']['version']}","String","metadata"');
    
    // Data entries
    final dataMap = data['data'] as Map<String, dynamic>;
    dataMap.forEach((key, valueData) {
      final value = valueData['value'].toString().replaceAll('"', '""');
      final type = valueData['type'];
      buffer.writeln('"$key","$value","$type","data"');
    });
    
    return buffer.toString();
  }

  String _exportAsXml(Map<String, dynamic> data) {
    final buffer = StringBuffer();
    
    buffer.writeln('<?xml version="1.0" encoding="UTF-8"?>');
    buffer.writeln('<export>');
    
    // Metadata
    buffer.writeln('  <metadata>');
    final metadata = data['metadata'] as Map<String, dynamic>;
    metadata.forEach((key, value) {
      buffer.writeln('    <$key>$value</$key>');
    });
    buffer.writeln('  </metadata>');
    
    // Data
    buffer.writeln('  <data>');
    final dataMap = data['data'] as Map<String, dynamic>;
    dataMap.forEach((key, valueData) {
      final value = valueData['value'].toString()
          .replaceAll('&', '&amp;')
          .replaceAll('<', '&lt;')
          .replaceAll('>', '&gt;');
      final type = valueData['type'];
      buffer.writeln('    <entry key="$key" type="$type">$value</entry>');
    });
    buffer.writeln('  </data>');
    
    buffer.writeln('</export>');
    return buffer.toString();
  }

  Future<bool> importData(String importData, {ExportFormat? format}) async {
    try {
      // Auto-detect format if not specified
      format ??= _detectFormat(importData);
      
      late Map<String, dynamic> parsedData;
      
      switch (format) {
        case ExportFormat.json:
          parsedData = json.decode(importData);
          break;
        case ExportFormat.csv:
          parsedData = _parseCsv(importData);
          break;
        case ExportFormat.xml:
          parsedData = _parseXml(importData);
          break;
      }

      await _importParsedData(parsedData);
      return true;
    } catch (e) {
      return false;
    }
  }

  ExportFormat _detectFormat(String data) {
    final trimmed = data.trim();
    
    if (trimmed.startsWith('{') && trimmed.endsWith('}')) {
      return ExportFormat.json;
    } else if (trimmed.startsWith('<?xml')) {
      return ExportFormat.xml;
    } else {
      return ExportFormat.csv;
    }
  }

  Map<String, dynamic> _parseCsv(String csvData) {
    final lines = csvData.split('\n');
    final Map<String, dynamic> result = {
      'metadata': {},
      'data': {},
    };
    
    // Skip header
    for (int i = 1; i < lines.length; i++) {
      final line = lines[i].trim();
      if (line.isEmpty) continue;
      
      final parts = _parseCsvLine(line);
      if (parts.length >= 4) {
        final key = parts[0];
        final value = parts[1];
        final type = parts[2];
        final category = parts[3];
        
        if (category == 'metadata') {
          result['metadata'][key.replaceAll('metadata.', '')] = value;
        } else {
          result['data'][key] = {
            'value': _convertValue(value, type),
            'type': type,
          };
        }
      }
    }
    
    return result;
  }

  List<String> _parseCsvLine(String line) {
    final List<String> result = [];
    bool inQuotes = false;
    String current = '';
    
    for (int i = 0; i < line.length; i++) {
      final char = line[i];
      
      if (char == '"' && !inQuotes) {
        inQuotes = true;
      } else if (char == '"' && inQuotes) {
        if (i + 1 < line.length && line[i + 1] == '"') {
          current += '"';
          i++; // Skip next quote
        } else {
          inQuotes = false;
        }
      } else if (char == ',' && !inQuotes) {
        result.add(current);
        current = '';
      } else {
        current += char;
      }
    }
    
    result.add(current);
    return result;
  }

  Map<String, dynamic> _parseXml(String xmlData) {
    // Simple XML parser for this example
    // In production, use a proper XML parser
    final Map<String, dynamic> result = {
      'metadata': {},
      'data': {},
    };
    
    // Extract metadata
    final metadataRegex = RegExp(r'<(\w+)>([^<]+)</\1>');
    final metadataMatches = metadataRegex.allMatches(xmlData);
    
    for (final match in metadataMatches) {
      if (xmlData.substring(0, match.start).contains('<metadata>')) {
        result['metadata'][match.group(1)!] = match.group(2)!;
      }
    }
    
    // Extract data entries
    final entryRegex = RegExp(r'<entry key="([^"]+)" type="([^"]+)">([^<]*)</entry>');
    final entryMatches = entryRegex.allMatches(xmlData);
    
    for (final match in entryMatches) {
      final key = match.group(1)!;
      final type = match.group(2)!;
      final value = match.group(3)!
          .replaceAll('&lt;', '<')
          .replaceAll('&gt;', '>')
          .replaceAll('&amp;', '&');
      
      result['data'][key] = {
        'value': _convertValue(value, type),
        'type': type,
      };
    }
    
    return result;
  }

  dynamic _convertValue(String value, String type) {
    switch (type) {
      case 'int':
        return int.tryParse(value) ?? 0;
      case 'double':
        return double.tryParse(value) ?? 0.0;
      case 'bool':
        return value.toLowerCase() == 'true';
      case 'List<String>':
        try {
          return List<String>.from(json.decode(value));
        } catch (e) {
          return <String>[];
        }
      default:
        return value;
    }
  }

  Future<void> _importParsedData(Map<String, dynamic> parsedData) async {
    final prefs = await SharedPreferences.getInstance();
    final dataMap = parsedData['data'] as Map<String, dynamic>;
    
    for (final entry in dataMap.entries) {
      final key = entry.key;
      final valueData = entry.value;
      final value = valueData['value'];
      final type = valueData['type'];
      
      // Store based on type
      switch (type) {
        case 'String':
          await prefs.setString(key, value as String);
          break;
        case 'int':
          await prefs.setInt(key, value as int);
          break;
        case 'double':
          await prefs.setDouble(key, value as double);
          break;
        case 'bool':
          await prefs.setBool(key, value as bool);
          break;
        case 'List<String>':
          await prefs.setStringList(key, value as List<String>);
          break;
      }
    }
  }

  Future<String> saveExportToFile(String data, String filename) async {
    final directory = await getApplicationDocumentsDirectory();
    final file = File('${directory.path}/$filename');
    await file.writeAsString(data);
    return file.path;
  }

  Future<String?> loadImportFromFile() async {
    try {
      final result = await FilePicker.platform.pickFiles(
        type: FileType.custom,
        allowedExtensions: ['json', 'csv', 'xml', 'txt'],
      );
      
      if (result != null && result.files.isNotEmpty) {
        final file = File(result.files.first.path!);
        return await file.readAsString();
      }
    } catch (e) {
      // Handle file picker errors
    }
    
    return null;
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Data Export and Import',
      home: const ExportImportPage(),
    );
  }
}

class ExportImportPage extends StatefulWidget {
  const ExportImportPage({super.key});

  @override
  State<ExportImportPage> createState() => _ExportImportPageState();
}

class _ExportImportPageState extends State<ExportImportPage> {
  final DataExportImport _exportImport = DataExportImport();
  
  ExportFormat _selectedFormat = ExportFormat.json;
  String _exportData = '';
  bool _isProcessing = false;
  
  // Sample data for demonstration
  final Map<String, dynamic> _sampleData = {
    'user_name': 'John Doe',
    'user_email': 'john@example.com',
    'theme_preference': 'dark',
    'notifications_enabled': true,
    'font_size': 16.0,
    'favorite_categories': ['technology', 'science', 'art'],
  };

  @override
  void initState() {
    super.initState();
    _populateSampleData();
  }

  Future<void> _populateSampleData() async {
    final prefs = await SharedPreferences.getInstance();
    
    for (final entry in _sampleData.entries) {
      final key = 'sample_${entry.key}';
      final value = entry.value;
      
      if (value is String) {
        await prefs.setString(key, value);
      } else if (value is int) {
        await prefs.setInt(key, value);
      } else if (value is double) {
        await prefs.setDouble(key, value);
      } else if (value is bool) {
        await prefs.setBool(key, value);
      } else if (value is List<String>) {
        await prefs.setStringList(key, value);
      }
    }
  }

  Future<void> _exportData() async {
    setState(() {
      _isProcessing = true;
    });

    try {
      final exportedData = await _exportImport.exportAllData(format: _selectedFormat);
      
      setState(() {
        _exportData = exportedData;
      });

      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Data exported successfully!')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Export failed: $e')),
        );
      }
    } finally {
      setState(() {
        _isProcessing = false;
      });
    }
  }

  Future<void> _saveExportToFile() async {
    if (_exportData.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('No data to save. Export first!')),
      );
      return;
    }

    try {
      final timestamp = DateTime.now().millisecondsSinceEpoch;
      final extension = _selectedFormat.name;
      final filename = 'app_export_$timestamp.$extension';
      
      final filePath = await _exportImport.saveExportToFile(_exportData, filename);
      
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Saved to: $filePath')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Save failed: $e')),
        );
      }
    }
  }

  Future<void> _importFromFile() async {
    setState(() {
      _isProcessing = true;
    });

    try {
      final importData = await _exportImport.loadImportFromFile();
      
      if (importData != null) {
        final success = await _exportImport.importData(importData);
        
        if (mounted) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              content: Text(success 
                  ? 'Data imported successfully!' 
                  : 'Import failed - invalid format or data'),
            ),
          );
        }
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Import failed: $e')),
        );
      }
    } finally {
      setState(() {
        _isProcessing = false;
      });
    }
  }

  Future<void> _clearAllData() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.clear();
    
    setState(() {
      _exportData = '';
    });

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('All data cleared!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Data Export & Import'),
      ),
      body: _isProcessing
          ? const Center(child: CircularProgressIndicator())
          : Padding(
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
                          Text(
                            'Export Settings',
                            style: Theme.of(context).textTheme.titleLarge,
                          ),
                          const SizedBox(height: 16),
                          Row(
                            children: [
                              const Text('Format: '),
                              DropdownButton<ExportFormat>(
                                value: _selectedFormat,
                                items: ExportFormat.values.map((format) {
                                  return DropdownMenuItem(
                                    value: format,
                                    child: Text(format.name.toUpperCase()),
                                  );
                                }).toList(),
                                onChanged: (format) {
                                  if (format != null) {
                                    setState(() {
                                      _selectedFormat = format;
                                    });
                                  }
                                },
                              ),
                            ],
                          ),
                          const SizedBox(height: 16),
                          Row(
                            children: [
                              Expanded(
                                child: ElevatedButton.icon(
                                  onPressed: _exportData,
                                  icon: const Icon(Icons.download),
                                  label: const Text('Export Data'),
                                ),
                              ),
                              const SizedBox(width: 8),
                              Expanded(
                                child: ElevatedButton.icon(
                                  onPressed: _exportData.isNotEmpty ? _saveExportToFile : null,
                                  icon: const Icon(Icons.save),
                                  label: const Text('Save to File'),
                                ),
                              ),
                            ],
                          ),
                        ],
                      ),
                    ),
                  ),
                  const SizedBox(height: 16),
                  Card(
                    child: Padding(
                      padding: const EdgeInsets.all(16.0),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'Import Settings',
                            style: Theme.of(context).textTheme.titleLarge,
                          ),
                          const SizedBox(height: 16),
                          Row(
                            children: [
                              Expanded(
                                child: ElevatedButton.icon(
                                  onPressed: _importFromFile,
                                  icon: const Icon(Icons.upload),
                                  label: const Text('Import from File'),
                                ),
                              ),
                              const SizedBox(width: 8),
                              Expanded(
                                child: ElevatedButton.icon(
                                  onPressed: _clearAllData,
                                  icon: const Icon(Icons.clear),
                                  label: const Text('Clear Data'),
                                  style: ElevatedButton.styleFrom(
                                    foregroundColor: Colors.red,
                                  ),
                                ),
                              ),
                            ],
                          ),
                        ],
                      ),
                    ),
                  ),
                  const SizedBox(height: 16),
                  if (_exportData.isNotEmpty) ...[
                    Text(
                      'Exported Data Preview',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 8),
                    Expanded(
                      child: Container(
                        padding: const EdgeInsets.all(12),
                        decoration: BoxDecoration(
                          border: Border.all(color: Colors.grey),
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: SingleChildScrollView(
                          child: Text(
                            _exportData,
                            style: const TextStyle(
                              fontFamily: 'monospace',
                              fontSize: 12,
                            ),
                          ),
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

Data export and import functionality provides comprehensive backup and  
migration capabilities supporting multiple formats including JSON, CSV, and  
XML. The system handles data conversion, format detection, and provides  
both in-memory operations and file-based import/export with proper error  
handling and validation.  

## State Persistence Across App Restarts

Maintaining application state and user session across app lifecycles.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class AppState {
  final String currentScreen;
  final Map<String, dynamic> screenStates;
  final DateTime lastActiveTime;
  final int sessionId;
  final List<String> navigationHistory;

  AppState({
    required this.currentScreen,
    required this.screenStates,
    required this.lastActiveTime,
    required this.sessionId,
    required this.navigationHistory,
  });

  Map<String, dynamic> toJson() {
    return {
      'currentScreen': currentScreen,
      'screenStates': screenStates,
      'lastActiveTime': lastActiveTime.toIso8601String(),
      'sessionId': sessionId,
      'navigationHistory': navigationHistory,
    };
  }

  factory AppState.fromJson(Map<String, dynamic> json) {
    return AppState(
      currentScreen: json['currentScreen'],
      screenStates: Map<String, dynamic>.from(json['screenStates']),
      lastActiveTime: DateTime.parse(json['lastActiveTime']),
      sessionId: json['sessionId'],
      navigationHistory: List<String>.from(json['navigationHistory']),
    );
  }

  AppState copyWith({
    String? currentScreen,
    Map<String, dynamic>? screenStates,
    DateTime? lastActiveTime,
    int? sessionId,
    List<String>? navigationHistory,
  }) {
    return AppState(
      currentScreen: currentScreen ?? this.currentScreen,
      screenStates: screenStates ?? Map<String, dynamic>.from(this.screenStates),
      lastActiveTime: lastActiveTime ?? this.lastActiveTime,
      sessionId: sessionId ?? this.sessionId,
      navigationHistory: navigationHistory ?? List<String>.from(this.navigationHistory),
    );
  }
}

class StateManager {
  static final StateManager _instance = StateManager._internal();
  factory StateManager() => _instance;
  StateManager._internal();

  static const String _stateKey = 'app_state';
  static const String _sessionKey = 'session_data';
  
  AppState? _currentState;
  final Map<String, dynamic> _tempData = {};

  Future<AppState> initializeState() async {
    final prefs = await SharedPreferences.getInstance();
    final stateJson = prefs.getString(_stateKey);
    
    if (stateJson != null) {
      try {
        final stateData = json.decode(stateJson);
        _currentState = AppState.fromJson(stateData);
        
        // Check if the session is still valid (not older than 24 hours)
        final timeDiff = DateTime.now().difference(_currentState!.lastActiveTime);
        if (timeDiff.inHours > 24) {
          _currentState = _createDefaultState();
        }
      } catch (e) {
        _currentState = _createDefaultState();
      }
    } else {
      _currentState = _createDefaultState();
    }

    return _currentState!;
  }

  AppState _createDefaultState() {
    return AppState(
      currentScreen: 'home',
      screenStates: {},
      lastActiveTime: DateTime.now(),
      sessionId: DateTime.now().millisecondsSinceEpoch,
      navigationHistory: ['home'],
    );
  }

  Future<void> saveState() async {
    if (_currentState == null) return;

    final prefs = await SharedPreferences.getInstance();
    final stateJson = json.encode(_currentState!.toJson());
    await prefs.setString(_stateKey, stateJson);
  }

  Future<void> updateCurrentScreen(String screenName) async {
    if (_currentState == null) return;

    final history = List<String>.from(_currentState!.navigationHistory);
    if (history.isEmpty || history.last != screenName) {
      history.add(screenName);
      // Keep only last 10 screens
      if (history.length > 10) {
        history.removeAt(0);
      }
    }

    _currentState = _currentState!.copyWith(
      currentScreen: screenName,
      navigationHistory: history,
      lastActiveTime: DateTime.now(),
    );

    await saveState();
  }

  Future<void> updateScreenState(String screenName, Map<String, dynamic> state) async {
    if (_currentState == null) return;

    final screenStates = Map<String, dynamic>.from(_currentState!.screenStates);
    screenStates[screenName] = state;

    _currentState = _currentState!.copyWith(
      screenStates: screenStates,
      lastActiveTime: DateTime.now(),
    );

    await saveState();
  }

  Map<String, dynamic>? getScreenState(String screenName) {
    return _currentState?.screenStates[screenName];
  }

  String get currentScreen => _currentState?.currentScreen ?? 'home';
  
  List<String> get navigationHistory => _currentState?.navigationHistory ?? [];

  Future<void> clearState() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.remove(_stateKey);
    _currentState = _createDefaultState();
  }

  // Temporary data for current session only
  void setTempData(String key, dynamic value) {
    _tempData[key] = value;
  }

  T? getTempData<T>(String key) {
    return _tempData[key] as T?;
  }

  void clearTempData() {
    _tempData.clear();
  }

  // Session management
  Future<void> saveSessionData(Map<String, dynamic> sessionData) async {
    final prefs = await SharedPreferences.getInstance();
    final sessionJson = json.encode(sessionData);
    await prefs.setString(_sessionKey, sessionJson);
  }

  Future<Map<String, dynamic>?> getSessionData() async {
    final prefs = await SharedPreferences.getInstance();
    final sessionJson = prefs.getString(_sessionKey);
    
    if (sessionJson != null) {
      try {
        return Map<String, dynamic>.from(json.decode(sessionJson));
      } catch (e) {
        return null;
      }
    }
    return null;
  }

  Future<void> clearSession() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.remove(_sessionKey);
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'State Persistence',
      home: const StateManagerDemo(),
    );
  }
}

class StateManagerDemo extends StatefulWidget {
  const StateManagerDemo({super.key});

  @override
  State<StateManagerDemo> createState() => _StateManagerDemoState();
}

class _StateManagerDemoState extends State<StateManagerDemo> with WidgetsBindingObserver {
  final StateManager _stateManager = StateManager();
  AppState? _appState;
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    WidgetsBinding.instance.addObserver(this);
    _initializeState();
  }

  @override
  void dispose() {
    WidgetsBinding.instance.removeObserver(this);
    super.dispose();
  }

  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    switch (state) {
      case AppLifecycleState.paused:
      case AppLifecycleState.detached:
        _stateManager.saveState();
        break;
      case AppLifecycleState.resumed:
        _stateManager.updateScreenState('demo', {
          'lastResumed': DateTime.now().toIso8601String(),
        });
        break;
      default:
        break;
    }
  }

  Future<void> _initializeState() async {
    final appState = await _stateManager.initializeState();
    
    setState(() {
      _appState = appState;
      _isLoading = false;
    });

    // Restore any screen-specific state
    final screenState = _stateManager.getScreenState('demo');
    if (screenState != null && mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text('Restored state: ${screenState.toString()}'),
          duration: const Duration(seconds: 2),
        ),
      );
    }
  }

  Future<void> _navigateToScreen(String screenName) async {
    await _stateManager.updateCurrentScreen(screenName);
    setState(() {
      _appState = _stateManager._currentState;
    });

    if (mounted) {
      Navigator.push(
        context,
        MaterialPageRoute(
          builder: (context) => DemoScreen(screenName: screenName),
        ),
      );
    }
  }

  Future<void> _updateScreenState() async {
    final currentState = {
      'counter': DateTime.now().millisecondsSinceEpoch % 100,
      'lastUpdate': DateTime.now().toIso8601String(),
      'randomValue': (DateTime.now().millisecondsSinceEpoch % 1000).toString(),
    };

    await _stateManager.updateScreenState('demo', currentState);
    
    setState(() {
      _appState = _stateManager._currentState;
    });

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Screen state updated!')),
      );
    }
  }

  Future<void> _clearAllState() async {
    await _stateManager.clearState();
    await _initializeState();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('All state cleared!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    if (_isLoading) {
      return const Scaffold(
        body: Center(child: CircularProgressIndicator()),
      );
    }

    return Scaffold(
      appBar: AppBar(
        title: const Text('State Persistence Demo'),
        actions: [
          IconButton(
            icon: const Icon(Icons.delete_sweep),
            onPressed: _clearAllState,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Current App State',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 16),
                    Text('Current Screen: ${_appState?.currentScreen}'),
                    Text('Session ID: ${_appState?.sessionId}'),
                    Text('Last Active: ${_appState?.lastActiveTime.toString().split('.')[0]}'),
                    const SizedBox(height: 8),
                    Text(
                      'Navigation History:',
                      style: Theme.of(context).textTheme.titleSmall,
                    ),
                    Text('${_appState?.navigationHistory.join(' → ')}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Screen States',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 16),
                    if (_appState?.screenStates.isEmpty ?? true)
                      const Text('No screen states saved')
                    else
                      ..._appState!.screenStates.entries.map((entry) {
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 4),
                          child: Text(
                            '${entry.key}: ${entry.value.toString()}',
                            style: const TextStyle(fontFamily: 'monospace', fontSize: 12),
                          ),
                        );
                      }),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 20),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _updateScreenState,
                    child: const Text('Update Screen State'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: () => _navigateToScreen('details'),
                    child: const Text('Navigate to Details'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: () => _navigateToScreen('settings'),
                    child: const Text('Navigate to Settings'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: () => _navigateToScreen('profile'),
                    child: const Text('Navigate to Profile'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 20),
            Container(
              width: double.infinity,
              padding: const EdgeInsets.all(16),
              decoration: BoxDecoration(
                color: Colors.blue.withOpacity(0.1),
                borderRadius: BorderRadius.circular(8),
              ),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  const Text(
                    'State Persistence Features:',
                    style: TextStyle(fontWeight: FontWeight.bold),
                  ),
                  const SizedBox(height: 8),
                  const Text('• Automatically saves state on app pause/close'),
                  const Text('• Restores state on app restart'),
                  const Text('• Tracks navigation history'),
                  const Text('• Manages screen-specific state'),
                  const Text('• Session data management'),
                  const Text('• Temporary data for current session'),
                  const Text('• State expiration (24 hours)'),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class DemoScreen extends StatefulWidget {
  final String screenName;

  const DemoScreen({super.key, required this.screenName});

  @override
  State<DemoScreen> createState() => _DemoScreenState();
}

class _DemoScreenState extends State<DemoScreen> {
  final StateManager _stateManager = StateManager();
  final TextEditingController _controller = TextEditingController();
  Map<String, dynamic>? _screenState;

  @override
  void initState() {
    super.initState();
    _loadScreenState();
  }

  void _loadScreenState() {
    _screenState = _stateManager.getScreenState(widget.screenName);
    if (_screenState != null) {
      _controller.text = _screenState!['text_input'] ?? '';
    }
  }

  Future<void> _saveScreenState() async {
    final state = {
      'text_input': _controller.text,
      'saved_at': DateTime.now().toIso8601String(),
      'visit_count': (_screenState?['visit_count'] ?? 0) + 1,
    };

    await _stateManager.updateScreenState(widget.screenName, state);
    
    setState(() {
      _screenState = state;
    });

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Screen state saved!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('${widget.screenName.toUpperCase()} Screen'),
        actions: [
          IconButton(
            icon: const Icon(Icons.save),
            onPressed: _saveScreenState,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _controller,
              decoration: const InputDecoration(
                labelText: 'Enter some text (will be saved)',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 20),
            if (_screenState != null) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Screen State',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                      const SizedBox(height: 8),
                      Text('Visit Count: ${_screenState!['visit_count'] ?? 0}'),
                      if (_screenState!['saved_at'] != null)
                        Text('Last Saved: ${_screenState!['saved_at']}'),
                    ],
                  ),
                ),
              ),
            ],
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _saveScreenState,
              child: const Text('Save Screen State'),
            ),
          ],
        ),
      ),
    );
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
}
```

State persistence across app restarts maintains application and user session  
state through app lifecycle events. The system tracks navigation history,  
screen-specific states, and session data with automatic saving during app  
pause/resume cycles and intelligent state restoration with expiration handling.  

## Offline Data Synchronization

Managing data synchronization between local storage and remote servers.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';
import 'dart:convert';
import 'dart:async';

void main() {
  runApp(const MyApp());
}

enum SyncStatus {
  pending,
  syncing,
  synced,
  conflict,
  failed,
}

enum SyncOperation {
  create,
  update,
  delete,
}

class SyncableItem {
  final String id;
  final String type;
  final Map<String, dynamic> data;
  final DateTime localModified;
  final DateTime? serverModified;
  final SyncStatus syncStatus;
  final SyncOperation? pendingOperation;
  final String? conflictData;

  SyncableItem({
    required this.id,
    required this.type,
    required this.data,
    required this.localModified,
    this.serverModified,
    required this.syncStatus,
    this.pendingOperation,
    this.conflictData,
  });

  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'type': type,
      'data': json.encode(data),
      'local_modified': localModified.millisecondsSinceEpoch,
      'server_modified': serverModified?.millisecondsSinceEpoch,
      'sync_status': syncStatus.index,
      'pending_operation': pendingOperation?.index,
      'conflict_data': conflictData,
    };
  }

  factory SyncableItem.fromMap(Map<String, dynamic> map) {
    return SyncableItem(
      id: map['id'],
      type: map['type'],
      data: json.decode(map['data']),
      localModified: DateTime.fromMillisecondsSinceEpoch(map['local_modified']),
      serverModified: map['server_modified'] != null 
          ? DateTime.fromMillisecondsSinceEpoch(map['server_modified'])
          : null,
      syncStatus: SyncStatus.values[map['sync_status']],
      pendingOperation: map['pending_operation'] != null 
          ? SyncOperation.values[map['pending_operation']]
          : null,
      conflictData: map['conflict_data'],
    );
  }

  SyncableItem copyWith({
    String? id,
    String? type,
    Map<String, dynamic>? data,
    DateTime? localModified,
    DateTime? serverModified,
    SyncStatus? syncStatus,
    SyncOperation? pendingOperation,
    String? conflictData,
  }) {
    return SyncableItem(
      id: id ?? this.id,
      type: type ?? this.type,
      data: data ?? Map<String, dynamic>.from(this.data),
      localModified: localModified ?? this.localModified,
      serverModified: serverModified ?? this.serverModified,
      syncStatus: syncStatus ?? this.syncStatus,
      pendingOperation: pendingOperation ?? this.pendingOperation,
      conflictData: conflictData ?? this.conflictData,
    );
  }
}

class OfflineSyncManager {
  static final OfflineSyncManager _instance = OfflineSyncManager._internal();
  factory OfflineSyncManager() => _instance;
  OfflineSyncManager._internal();

  static Database? _database;
  Timer? _syncTimer;
  bool _isSyncing = false;
  final StreamController<List<SyncableItem>> _itemsController = StreamController.broadcast();
  final StreamController<String> _statusController = StreamController.broadcast();

  Future<Database> get database async {
    _database ??= await _initDatabase();
    return _database!;
  }

  Stream<List<SyncableItem>> get itemsStream => _itemsController.stream;
  Stream<String> get statusStream => _statusController.stream;

  Future<Database> _initDatabase() async {
    String path = join(await getDatabasesPath(), 'offline_sync.db');
    
    return await openDatabase(
      path,
      version: 1,
      onCreate: (db, version) async {
        await db.execute('''
          CREATE TABLE sync_items (
            id TEXT PRIMARY KEY,
            type TEXT NOT NULL,
            data TEXT NOT NULL,
            local_modified INTEGER NOT NULL,
            server_modified INTEGER,
            sync_status INTEGER NOT NULL,
            pending_operation INTEGER,
            conflict_data TEXT
          )
        ''');

        await db.execute('''
          CREATE TABLE sync_metadata (
            key TEXT PRIMARY KEY,
            value TEXT NOT NULL
          )
        ''');
      },
    );
  }

  Future<void> startPeriodicSync({Duration interval = const Duration(minutes: 5)}) async {
    _syncTimer?.cancel();
    _syncTimer = Timer.periodic(interval, (_) => syncAll());
  }

  Future<void> stopPeriodicSync() async {
    _syncTimer?.cancel();
    _syncTimer = null;
  }

  Future<String> createItem(String type, Map<String, dynamic> data) async {
    final id = DateTime.now().millisecondsSinceEpoch.toString();
    
    final item = SyncableItem(
      id: id,
      type: type,
      data: data,
      localModified: DateTime.now(),
      syncStatus: SyncStatus.pending,
      pendingOperation: SyncOperation.create,
    );

    await _saveItem(item);
    _notifyItemsChanged();
    
    return id;
  }

  Future<void> updateItem(String id, Map<String, dynamic> data) async {
    final existingItem = await getItem(id);
    if (existingItem == null) return;

    final updatedItem = existingItem.copyWith(
      data: data,
      localModified: DateTime.now(),
      syncStatus: SyncStatus.pending,
      pendingOperation: SyncOperation.update,
    );

    await _saveItem(updatedItem);
    _notifyItemsChanged();
  }

  Future<void> deleteItem(String id) async {
    final existingItem = await getItem(id);
    if (existingItem == null) return;

    final deletedItem = existingItem.copyWith(
      localModified: DateTime.now(),
      syncStatus: SyncStatus.pending,
      pendingOperation: SyncOperation.delete,
    );

    await _saveItem(deletedItem);
    _notifyItemsChanged();
  }

  Future<SyncableItem?> getItem(String id) async {
    final db = await database;
    final maps = await db.query(
      'sync_items',
      where: 'id = ?',
      whereArgs: [id],
    );

    if (maps.isNotEmpty) {
      return SyncableItem.fromMap(maps.first);
    }
    return null;
  }

  Future<List<SyncableItem>> getAllItems({String? type}) async {
    final db = await database;
    
    String where = '';
    List<dynamic> whereArgs = [];
    
    if (type != null) {
      where = 'type = ?';
      whereArgs.add(type);
    }

    final maps = await db.query(
      'sync_items',
      where: where.isNotEmpty ? where : null,
      whereArgs: whereArgs.isNotEmpty ? whereArgs : null,
      orderBy: 'local_modified DESC',
    );

    return maps.map((map) => SyncableItem.fromMap(map)).toList();
  }

  Future<List<SyncableItem>> getPendingItems() async {
    final db = await database;
    final maps = await db.query(
      'sync_items',
      where: 'sync_status = ?',
      whereArgs: [SyncStatus.pending.index],
      orderBy: 'local_modified ASC',
    );

    return maps.map((map) => SyncableItem.fromMap(map)).toList();
  }

  Future<void> _saveItem(SyncableItem item) async {
    final db = await database;
    await db.insert(
      'sync_items',
      item.toMap(),
      conflictAlgorithm: ConflictAlgorithm.replace,
    );
  }

  Future<void> syncAll() async {
    if (_isSyncing) return;
    
    _isSyncing = true;
    _statusController.add('Starting synchronization...');

    try {
      final pendingItems = await getPendingItems();
      
      for (final item in pendingItems) {
        await _syncItem(item);
      }

      _statusController.add('Synchronization completed');
    } catch (e) {
      _statusController.add('Synchronization failed: $e');
    } finally {
      _isSyncing = false;
      _notifyItemsChanged();
    }
  }

  Future<void> _syncItem(SyncableItem item) async {
    _statusController.add('Syncing ${item.type} ${item.id}...');

    try {
      // Simulate server communication
      await _simulateServerSync(item);
      
      // Update item as synced
      final syncedItem = item.copyWith(
        syncStatus: SyncStatus.synced,
        serverModified: DateTime.now(),
        pendingOperation: null,
      );

      await _saveItem(syncedItem);
      
    } catch (e) {
      // Mark as failed
      final failedItem = item.copyWith(
        syncStatus: SyncStatus.failed,
      );
      
      await _saveItem(failedItem);
    }
  }

  Future<void> _simulateServerSync(SyncableItem item) async {
    // Simulate network delay
    await Future.delayed(const Duration(milliseconds: 500 + (DateTime.now().millisecondsSinceEpoch % 1000)));

    // Simulate occasional conflicts or failures
    final random = DateTime.now().millisecondsSinceEpoch % 10;
    
    if (random == 0) {
      // Simulate conflict
      throw ConflictException('Server version is newer');
    } else if (random == 1) {
      // Simulate network error
      throw NetworkException('Network timeout');
    }
    
    // Success case - item synced
  }

  Future<void> resolveConflict(String itemId, bool useLocal) async {
    final item = await getItem(itemId);
    if (item == null) return;

    if (useLocal) {
      // Keep local version, mark for sync
      final resolvedItem = item.copyWith(
        syncStatus: SyncStatus.pending,
        conflictData: null,
      );
      await _saveItem(resolvedItem);
    } else {
      // Use server version (simulate fetching from server)
      final serverData = json.decode(item.conflictData ?? '{}');
      final resolvedItem = item.copyWith(
        data: serverData,
        syncStatus: SyncStatus.synced,
        conflictData: null,
      );
      await _saveItem(resolvedItem);
    }
    
    _notifyItemsChanged();
  }

  Future<void> _notifyItemsChanged() async {
    final items = await getAllItems();
    _itemsController.add(items);
  }

  Future<void> clearAllData() async {
    final db = await database;
    await db.delete('sync_items');
    await db.delete('sync_metadata');
    _notifyItemsChanged();
  }

  void dispose() {
    _syncTimer?.cancel();
    _itemsController.close();
    _statusController.close();
  }
}

class ConflictException implements Exception {
  final String message;
  ConflictException(this.message);
}

class NetworkException implements Exception {
  final String message;
  NetworkException(this.message);
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Offline Data Sync',
      home: const OfflineSyncPage(),
    );
  }
}

class OfflineSyncPage extends StatefulWidget {
  const OfflineSyncPage({super.key});

  @override
  State<OfflineSyncPage> createState() => _OfflineSyncPageState();
}

class _OfflineSyncPageState extends State<OfflineSyncPage> {
  final OfflineSyncManager _syncManager = OfflineSyncManager();
  final TextEditingController _titleController = TextEditingController();
  final TextEditingController _contentController = TextEditingController();
  
  List<SyncableItem> _items = [];
  String _syncStatus = 'Ready';

  @override
  void initState() {
    super.initState();
    _initializeSync();
  }

  @override
  void dispose() {
    _syncManager.dispose();
    _titleController.dispose();
    _contentController.dispose();
    super.dispose();
  }

  Future<void> _initializeSync() async {
    // Load existing items
    final items = await _syncManager.getAllItems();
    setState(() {
      _items = items;
    });

    // Listen to changes
    _syncManager.itemsStream.listen((items) {
      if (mounted) {
        setState(() {
          _items = items;
        });
      }
    });

    _syncManager.statusStream.listen((status) {
      if (mounted) {
        setState(() {
          _syncStatus = status;
        });
      }
    });

    // Start periodic sync
    await _syncManager.startPeriodicSync(interval: const Duration(seconds: 30));
  }

  Future<void> _createItem() async {
    if (_titleController.text.isEmpty) return;

    final data = {
      'title': _titleController.text,
      'content': _contentController.text,
      'created_at': DateTime.now().toIso8601String(),
    };

    await _syncManager.createItem('note', data);
    
    _titleController.clear();
    _contentController.clear();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Note created (pending sync)')),
      );
    }
  }

  Future<void> _updateItem(SyncableItem item) async {
    final updatedData = Map<String, dynamic>.from(item.data);
    updatedData['title'] = '${updatedData['title']} (Updated)';
    updatedData['updated_at'] = DateTime.now().toIso8601String();

    await _syncManager.updateItem(item.id, updatedData);

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Note updated (pending sync)')),
      );
    }
  }

  Future<void> _deleteItem(SyncableItem item) async {
    await _syncManager.deleteItem(item.id);

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Note deleted (pending sync)')),
      );
    }
  }

  String _getSyncStatusText(SyncableItem item) {
    switch (item.syncStatus) {
      case SyncStatus.pending:
        return 'Pending ${item.pendingOperation?.name ?? ''}';
      case SyncStatus.syncing:
        return 'Syncing...';
      case SyncStatus.synced:
        return 'Synced';
      case SyncStatus.conflict:
        return 'Conflict';
      case SyncStatus.failed:
        return 'Failed';
    }
  }

  Color _getSyncStatusColor(SyncableItem item) {
    switch (item.syncStatus) {
      case SyncStatus.pending:
        return Colors.orange;
      case SyncStatus.syncing:
        return Colors.blue;
      case SyncStatus.synced:
        return Colors.green;
      case SyncStatus.conflict:
        return Colors.red;
      case SyncStatus.failed:
        return Colors.red;
    }
  }

  @override
  Widget build(BuildContext context) {
    final pendingCount = _items.where((item) => item.syncStatus == SyncStatus.pending).length;
    final conflictCount = _items.where((item) => item.syncStatus == SyncStatus.conflict).length;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Offline Data Sync'),
        actions: [
          IconButton(
            icon: const Icon(Icons.sync),
            onPressed: _syncManager.syncAll,
          ),
        ],
      ),
      body: Column(
        children: [
          // Status bar
          Container(
            width: double.infinity,
            padding: const EdgeInsets.all(12),
            color: Colors.blue.withOpacity(0.1),
            child: Column(
              children: [
                Text(_syncStatus),
                if (pendingCount > 0 || conflictCount > 0) ...[
                  const SizedBox(height: 4),
                  Text(
                    'Pending: $pendingCount, Conflicts: $conflictCount',
                    style: TextStyle(
                      color: conflictCount > 0 ? Colors.red : Colors.orange,
                      fontSize: 12,
                    ),
                  ),
                ],
              ],
            ),
          ),
          
          // Input form
          Padding(
            padding: const EdgeInsets.all(16),
            child: Column(
              children: [
                TextField(
                  controller: _titleController,
                  decoration: const InputDecoration(
                    labelText: 'Note Title',
                    border: OutlineInputBorder(),
                  ),
                ),
                const SizedBox(height: 8),
                TextField(
                  controller: _contentController,
                  maxLines: 3,
                  decoration: const InputDecoration(
                    labelText: 'Note Content',
                    border: OutlineInputBorder(),
                  ),
                ),
                const SizedBox(height: 16),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: _createItem,
                    child: const Text('Create Note'),
                  ),
                ),
              ],
            ),
          ),
          
          const Divider(height: 1),
          
          // Items list
          Expanded(
            child: _items.isEmpty
                ? const Center(child: Text('No notes yet'))
                : ListView.builder(
                    itemCount: _items.length,
                    itemBuilder: (context, index) {
                      final item = _items[index];
                      
                      return Card(
                        margin: const EdgeInsets.symmetric(
                          horizontal: 16,
                          vertical: 4,
                        ),
                        child: ListTile(
                          title: Text(item.data['title'] ?? 'No title'),
                          subtitle: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              if (item.data['content']?.isNotEmpty == true)
                                Text(
                                  item.data['content'],
                                  maxLines: 2,
                                  overflow: TextOverflow.ellipsis,
                                ),
                              const SizedBox(height: 4),
                              Row(
                                children: [
                                  Container(
                                    padding: const EdgeInsets.symmetric(
                                      horizontal: 8,
                                      vertical: 2,
                                    ),
                                    decoration: BoxDecoration(
                                      color: _getSyncStatusColor(item).withOpacity(0.2),
                                      borderRadius: BorderRadius.circular(12),
                                    ),
                                    child: Text(
                                      _getSyncStatusText(item),
                                      style: TextStyle(
                                        fontSize: 10,
                                        color: _getSyncStatusColor(item),
                                      ),
                                    ),
                                  ),
                                ],
                              ),
                            ],
                          ),
                          trailing: item.syncStatus != SyncStatus.syncing
                              ? PopupMenuButton<String>(
                                  onSelected: (value) {
                                    switch (value) {
                                      case 'update':
                                        _updateItem(item);
                                        break;
                                      case 'delete':
                                        _deleteItem(item);
                                        break;
                                    }
                                  },
                                  itemBuilder: (context) => [
                                    const PopupMenuItem(
                                      value: 'update',
                                      child: Text('Update'),
                                    ),
                                    const PopupMenuItem(
                                      value: 'delete',
                                      child: Text('Delete'),
                                    ),
                                  ],
                                )
                              : const SizedBox(
                                  width: 20,
                                  height: 20,
                                  child: CircularProgressIndicator(strokeWidth: 2),
                                ),
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }
}
```

Offline data synchronization manages bidirectional sync between local storage  
and remote servers with conflict resolution, retry mechanisms, and status  
tracking. The system handles CRUD operations offline, queues changes for  
sync, and provides comprehensive state management for networked applications.  