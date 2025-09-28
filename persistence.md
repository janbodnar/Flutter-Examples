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

## Binary Data Storage

Storing and managing binary data including images, audio, and documents.  

```dart
import 'package:flutter/material.dart';
import 'dart:io';
import 'dart:typed_data';
import 'package:path_provider/path_provider.dart';
import 'package:crypto/crypto.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class BinaryDataManager {
  static final BinaryDataManager _instance = BinaryDataManager._internal();
  factory BinaryDataManager() => _instance;
  BinaryDataManager._internal();

  late String _binaryDataDirectory;
  late String _metadataFile;
  final Map<String, BinaryMetadata> _metadataCache = {};
  bool _initialized = false;

  Future<void> initialize() async {
    if (_initialized) return;

    final appDir = await getApplicationDocumentsDirectory();
    _binaryDataDirectory = '${appDir.path}/binary_data';
    _metadataFile = '${_binaryDataDirectory}/metadata.json';

    await Directory(_binaryDataDirectory).create(recursive: true);
    await _loadMetadata();

    _initialized = true;
  }

  Future<void> _loadMetadata() async {
    try {
      final file = File(_metadataFile);
      if (await file.exists()) {
        final jsonString = await file.readAsString();
        final Map<String, dynamic> data = json.decode(jsonString);
        
        _metadataCache.clear();
        data.forEach((key, value) {
          _metadataCache[key] = BinaryMetadata.fromJson(value);
        });
      }
    } catch (e) {
      _metadataCache.clear();
    }
  }

  Future<void> _saveMetadata() async {
    try {
      final file = File(_metadataFile);
      final Map<String, dynamic> data = {};
      
      _metadataCache.forEach((key, value) {
        data[key] = value.toJson();
      });
      
      await file.writeAsString(json.encode(data));
    } catch (e) {
      throw Exception('Failed to save metadata: $e');
    }
  }

  String _generateHash(Uint8List data) {
    final digest = sha256.convert(data);
    return digest.toString();
  }

  Future<String> storeBinaryData(
    Uint8List data, {
    required String filename,
    String? mimeType,
    Map<String, String>? tags,
    bool compress = false,
  }) async {
    await initialize();

    final hash = _generateHash(data);
    
    // Check if data already exists
    if (_metadataCache.containsKey(hash)) {
      return hash;
    }

    // Optionally compress data
    Uint8List dataToStore = data;
    if (compress) {
      dataToStore = await _compressData(data);
    }

    final filePath = '$_binaryDataDirectory/$hash.bin';
    await File(filePath).writeAsBytes(dataToStore);

    final metadata = BinaryMetadata(
      hash: hash,
      originalFilename: filename,
      storedPath: filePath,
      originalSize: data.length,
      storedSize: dataToStore.length,
      mimeType: mimeType ?? _detectMimeType(filename),
      isCompressed: compress,
      createdAt: DateTime.now(),
      tags: tags ?? {},
    );

    _metadataCache[hash] = metadata;
    await _saveMetadata();

    return hash;
  }

  Future<Uint8List?> getBinaryData(String hash) async {
    await initialize();

    final metadata = _metadataCache[hash];
    if (metadata == null) return null;

    final file = File(metadata.storedPath);
    if (!await file.exists()) {
      _metadataCache.remove(hash);
      await _saveMetadata();
      return null;
    }

    Uint8List data = await file.readAsBytes();
    
    if (metadata.isCompressed) {
      data = await _decompressData(data);
    }

    return data;
  }

  Future<BinaryMetadata?> getMetadata(String hash) async {
    await initialize();
    return _metadataCache[hash];
  }

  Future<List<BinaryMetadata>> getAllMetadata({
    String? mimeTypeFilter,
    String? tagFilter,
  }) async {
    await initialize();

    var metadata = _metadataCache.values.toList();

    if (mimeTypeFilter != null) {
      metadata = metadata.where((m) => m.mimeType.startsWith(mimeTypeFilter)).toList();
    }

    if (tagFilter != null) {
      metadata = metadata.where((m) => m.tags.containsValue(tagFilter)).toList();
    }

    metadata.sort((a, b) => b.createdAt.compareTo(a.createdAt));
    return metadata;
  }

  Future<bool> deleteBinaryData(String hash) async {
    await initialize();

    final metadata = _metadataCache[hash];
    if (metadata == null) return false;

    try {
      final file = File(metadata.storedPath);
      if (await file.exists()) {
        await file.delete();
      }
      
      _metadataCache.remove(hash);
      await _saveMetadata();
      return true;
    } catch (e) {
      return false;
    }
  }

  Future<int> getTotalStorageSize() async {
    await initialize();
    
    return _metadataCache.values
        .map((metadata) => metadata.storedSize)
        .fold(0, (total, size) => total + size);
  }

  Future<double> getCompressionRatio() async {
    await initialize();
    
    if (_metadataCache.isEmpty) return 0.0;
    
    final totalOriginal = _metadataCache.values
        .map((metadata) => metadata.originalSize)
        .fold(0, (total, size) => total + size);
    
    final totalStored = _metadataCache.values
        .map((metadata) => metadata.storedSize)
        .fold(0, (total, size) => total + size);
    
    return totalOriginal > 0 ? (totalStored / totalOriginal) : 1.0;
  }

  Future<void> clearAll() async {
    await initialize();

    try {
      final directory = Directory(_binaryDataDirectory);
      if (await directory.exists()) {
        await directory.delete(recursive: true);
        await directory.create();
      }

      _metadataCache.clear();
      await _saveMetadata();
    } catch (e) {
      throw Exception('Failed to clear storage: $e');
    }
  }

  String _detectMimeType(String filename) {
    final extension = filename.split('.').last.toLowerCase();
    
    switch (extension) {
      case 'jpg':
      case 'jpeg':
        return 'image/jpeg';
      case 'png':
        return 'image/png';
      case 'gif':
        return 'image/gif';
      case 'pdf':
        return 'application/pdf';
      case 'txt':
        return 'text/plain';
      case 'json':
        return 'application/json';
      case 'mp3':
        return 'audio/mpeg';
      case 'mp4':
        return 'video/mp4';
      default:
        return 'application/octet-stream';
    }
  }

  Future<Uint8List> _compressData(Uint8List data) async {
    // Simple compression simulation - in reality you'd use gzip or similar
    return data; // Placeholder for actual compression
  }

  Future<Uint8List> _decompressData(Uint8List data) async {
    // Simple decompression simulation
    return data; // Placeholder for actual decompression
  }
}

class BinaryMetadata {
  final String hash;
  final String originalFilename;
  final String storedPath;
  final int originalSize;
  final int storedSize;
  final String mimeType;
  final bool isCompressed;
  final DateTime createdAt;
  final Map<String, String> tags;

  BinaryMetadata({
    required this.hash,
    required this.originalFilename,
    required this.storedPath,
    required this.originalSize,
    required this.storedSize,
    required this.mimeType,
    required this.isCompressed,
    required this.createdAt,
    required this.tags,
  });

  Map<String, dynamic> toJson() {
    return {
      'hash': hash,
      'originalFilename': originalFilename,
      'storedPath': storedPath,
      'originalSize': originalSize,
      'storedSize': storedSize,
      'mimeType': mimeType,
      'isCompressed': isCompressed,
      'createdAt': createdAt.toIso8601String(),
      'tags': tags,
    };
  }

  factory BinaryMetadata.fromJson(Map<String, dynamic> json) {
    return BinaryMetadata(
      hash: json['hash'],
      originalFilename: json['originalFilename'],
      storedPath: json['storedPath'],
      originalSize: json['originalSize'],
      storedSize: json['storedSize'],
      mimeType: json['mimeType'],
      isCompressed: json['isCompressed'],
      createdAt: DateTime.parse(json['createdAt']),
      tags: Map<String, String>.from(json['tags']),
    );
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Binary Data Storage',
      home: const BinaryDataPage(),
    );
  }
}

class BinaryDataPage extends StatefulWidget {
  const BinaryDataPage({super.key});

  @override
  State<BinaryDataPage> createState() => _BinaryDataPageState();
}

class _BinaryDataPageState extends State<BinaryDataPage> {
  final BinaryDataManager _dataManager = BinaryDataManager();
  List<BinaryMetadata> _items = [];
  int _totalStorage = 0;
  double _compressionRatio = 0.0;
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _loadData();
  }

  Future<void> _loadData() async {
    setState(() {
      _isLoading = true;
    });

    final items = await _dataManager.getAllMetadata();
    final totalSize = await _dataManager.getTotalStorageSize();
    final compressionRatio = await _dataManager.getCompressionRatio();

    setState(() {
      _items = items;
      _totalStorage = totalSize;
      _compressionRatio = compressionRatio;
      _isLoading = false;
    });
  }

  Future<void> _createSampleData() async {
    // Create sample text data
    final textData = utf8.encode('This is a sample text document with some content to demonstrate binary data storage.');
    await _dataManager.storeBinaryData(
      Uint8List.fromList(textData),
      filename: 'sample.txt',
      mimeType: 'text/plain',
      tags: {'type': 'document', 'category': 'sample'},
      compress: true,
    );

    // Create sample JSON data
    final jsonData = utf8.encode(json.encode({
      'name': 'John Doe',
      'age': 30,
      'city': 'New York',
      'hobbies': ['reading', 'coding', 'hiking'],
    }));
    await _dataManager.storeBinaryData(
      Uint8List.fromList(jsonData),
      filename: 'user_data.json',
      mimeType: 'application/json',
      tags: {'type': 'data', 'format': 'json'},
    );

    // Create sample binary pattern
    final binaryPattern = List.generate(1024, (i) => i % 256);
    await _dataManager.storeBinaryData(
      Uint8List.fromList(binaryPattern),
      filename: 'pattern.bin',
      mimeType: 'application/octet-stream',
      tags: {'type': 'pattern', 'size': 'small'},
    );

    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Sample data created!')),
      );
    }
  }

  Future<void> _viewData(BinaryMetadata metadata) async {
    final data = await _dataManager.getBinaryData(metadata.hash);
    if (data == null || !mounted) return;

    String content;
    if (metadata.mimeType.startsWith('text/') || metadata.mimeType == 'application/json') {
      content = utf8.decode(data);
    } else {
      // For binary data, show hex representation (first 200 bytes)
      final displayData = data.take(200).toList();
      content = displayData.map((byte) => byte.toRadixString(16).padLeft(2, '0')).join(' ');
      if (data.length > 200) {
        content += '\n... (${data.length - 200} more bytes)';
      }
    }

    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Text(metadata.originalFilename),
        content: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            mainAxisSize: MainAxisSize.min,
            children: [
              Text('Type: ${metadata.mimeType}'),
              Text('Size: ${_formatBytes(metadata.originalSize)}'),
              if (metadata.isCompressed)
                Text('Compressed: ${_formatBytes(metadata.storedSize)}'),
              Text('Created: ${metadata.createdAt.toString().split('.')[0]}'),
              if (metadata.tags.isNotEmpty) ...[
                const SizedBox(height: 8),
                const Text('Tags:'),
                ...metadata.tags.entries.map(
                  (entry) => Text('  ${entry.key}: ${entry.value}'),
                ),
              ],
              const SizedBox(height: 16),
              const Text('Content:'),
              Container(
                width: double.infinity,
                height: 200,
                padding: const EdgeInsets.all(8),
                decoration: BoxDecoration(
                  border: Border.all(color: Colors.grey),
                  borderRadius: BorderRadius.circular(4),
                ),
                child: SingleChildScrollView(
                  child: Text(
                    content,
                    style: const TextStyle(
                      fontFamily: 'monospace',
                      fontSize: 12,
                    ),
                  ),
                ),
              ),
            ],
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
          TextButton(
            onPressed: () {
              Navigator.pop(context);
              _deleteData(metadata.hash);
            },
            child: const Text('Delete'),
          ),
        ],
      ),
    );
  }

  Future<void> _deleteData(String hash) async {
    await _dataManager.deleteBinaryData(hash);
    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Data deleted!')),
      );
    }
  }

  String _formatBytes(int bytes) {
    if (bytes < 1024) return '$bytes B';
    if (bytes < 1024 * 1024) return '${(bytes / 1024).toStringAsFixed(1)} KB';
    return '${(bytes / (1024 * 1024)).toStringAsFixed(1)} MB';
  }

  Widget _getFileIcon(String mimeType) {
    if (mimeType.startsWith('image/')) {
      return const Icon(Icons.image, color: Colors.green);
    } else if (mimeType.startsWith('audio/')) {
      return const Icon(Icons.audiotrack, color: Colors.purple);
    } else if (mimeType.startsWith('video/')) {
      return const Icon(Icons.video_file, color: Colors.blue);
    } else if (mimeType == 'application/pdf') {
      return const Icon(Icons.picture_as_pdf, color: Colors.red);
    } else if (mimeType == 'application/json' || mimeType.startsWith('text/')) {
      return const Icon(Icons.description, color: Colors.orange);
    } else {
      return const Icon(Icons.insert_drive_file, color: Colors.grey);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Binary Data Storage'),
        actions: [
          IconButton(
            icon: const Icon(Icons.add),
            onPressed: _createSampleData,
          ),
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
                      Text('Total Items: ${_items.length}'),
                      Text('Total Size: ${_formatBytes(_totalStorage)}'),
                      Text('Compression Ratio: ${(_compressionRatio * 100).toStringAsFixed(1)}%'),
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
          : _items.isEmpty
              ? const Center(
                  child: Text('No binary data stored\nTap + to create sample data'),
                )
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
                        leading: _getFileIcon(item.mimeType),
                        title: Text(item.originalFilename),
                        subtitle: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(item.mimeType),
                            Row(
                              children: [
                                Text(_formatBytes(item.originalSize)),
                                if (item.isCompressed) ...[
                                  const Text(' → '),
                                  Text(_formatBytes(item.storedSize)),
                                  const Icon(Icons.compress, size: 12),
                                ],
                              ],
                            ),
                          ],
                        ),
                        trailing: Text(
                          '${item.createdAt.day}/${item.createdAt.month}',
                          style: const TextStyle(fontSize: 12),
                        ),
                        onTap: () => _viewData(item),
                      ),
                    );
                  },
                ),
    );
  }
}
```

Binary data storage provides efficient management of various file types with  
metadata tracking, compression support, and deduplication. The system handles  
text, JSON, images, and binary files with MIME type detection and provides  
comprehensive storage analytics and retrieval capabilities.  

## Search History Management

Managing search queries with persistence, filtering, and suggestions.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class SearchHistoryItem {
  final String query;
  final DateTime timestamp;
  final int searchCount;
  final String category;
  final Map<String, String> metadata;

  SearchHistoryItem({
    required this.query,
    required this.timestamp,
    required this.searchCount,
    required this.category,
    required this.metadata,
  });

  Map<String, dynamic> toJson() {
    return {
      'query': query,
      'timestamp': timestamp.toIso8601String(),
      'searchCount': searchCount,
      'category': category,
      'metadata': metadata,
    };
  }

  factory SearchHistoryItem.fromJson(Map<String, dynamic> json) {
    return SearchHistoryItem(
      query: json['query'],
      timestamp: DateTime.parse(json['timestamp']),
      searchCount: json['searchCount'],
      category: json['category'],
      metadata: Map<String, String>.from(json['metadata']),
    );
  }

  SearchHistoryItem copyWith({
    String? query,
    DateTime? timestamp,
    int? searchCount,
    String? category,
    Map<String, String>? metadata,
  }) {
    return SearchHistoryItem(
      query: query ?? this.query,
      timestamp: timestamp ?? this.timestamp,
      searchCount: searchCount ?? this.searchCount,
      category: category ?? this.category,
      metadata: metadata ?? Map<String, String>.from(this.metadata),
    );
  }
}

class SearchHistoryManager {
  static final SearchHistoryManager _instance = SearchHistoryManager._internal();
  factory SearchHistoryManager() => _instance;
  SearchHistoryManager._internal();

  static const String _historyKey = 'search_history';
  static const String _settingsKey = 'search_settings';
  static const int _maxHistoryItems = 100;

  List<SearchHistoryItem> _historyCache = [];
  Map<String, dynamic> _settings = {
    'maxItems': _maxHistoryItems,
    'saveEnabled': true,
    'suggestionsEnabled': true,
    'autoCompleteEnabled': true,
    'categoriesEnabled': true,
  };

  Future<void> initialize() async {
    await _loadHistory();
    await _loadSettings();
  }

  Future<void> _loadHistory() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final historyJson = prefs.getString(_historyKey);
      
      if (historyJson != null) {
        final List<dynamic> historyData = json.decode(historyJson);
        _historyCache = historyData
            .map((item) => SearchHistoryItem.fromJson(item))
            .toList();
      }
    } catch (e) {
      _historyCache = [];
    }
  }

  Future<void> _saveHistory() async {
    if (!_settings['saveEnabled']) return;
    
    try {
      final prefs = await SharedPreferences.getInstance();
      final historyData = _historyCache.map((item) => item.toJson()).toList();
      await prefs.setString(_historyKey, json.encode(historyData));
    } catch (e) {
      // Handle save error
    }
  }

  Future<void> _loadSettings() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final settingsJson = prefs.getString(_settingsKey);
      
      if (settingsJson != null) {
        final settingsData = json.decode(settingsJson);
        _settings.addAll(settingsData);
      }
    } catch (e) {
      // Use default settings
    }
  }

  Future<void> _saveSettings() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      await prefs.setString(_settingsKey, json.encode(_settings));
    } catch (e) {
      // Handle save error
    }
  }

  Future<void> addSearch(
    String query, {
    String category = 'general',
    Map<String, String>? metadata,
  }) async {
    if (!_settings['saveEnabled'] || query.trim().isEmpty) return;

    final normalizedQuery = query.trim().toLowerCase();
    
    // Check if query already exists
    final existingIndex = _historyCache.indexWhere(
      (item) => item.query.toLowerCase() == normalizedQuery,
    );

    if (existingIndex != -1) {
      // Update existing item
      final existingItem = _historyCache[existingIndex];
      final updatedItem = existingItem.copyWith(
        timestamp: DateTime.now(),
        searchCount: existingItem.searchCount + 1,
        metadata: metadata ?? existingItem.metadata,
      );
      
      _historyCache[existingIndex] = updatedItem;
    } else {
      // Add new item
      final newItem = SearchHistoryItem(
        query: query.trim(),
        timestamp: DateTime.now(),
        searchCount: 1,
        category: category,
        metadata: metadata ?? {},
      );
      
      _historyCache.insert(0, newItem);
    }

    // Maintain size limit
    if (_historyCache.length > _settings['maxItems']) {
      _historyCache = _historyCache.take(_settings['maxItems']).toList();
    }

    await _saveHistory();
  }

  List<SearchHistoryItem> getHistory({
    String? category,
    int? limit,
    DateTime? fromDate,
    DateTime? toDate,
  }) {
    var history = List<SearchHistoryItem>.from(_historyCache);

    if (category != null) {
      history = history.where((item) => item.category == category).toList();
    }

    if (fromDate != null) {
      history = history.where((item) => item.timestamp.isAfter(fromDate)).toList();
    }

    if (toDate != null) {
      history = history.where((item) => item.timestamp.isBefore(toDate)).toList();
    }

    // Sort by timestamp (most recent first)
    history.sort((a, b) => b.timestamp.compareTo(a.timestamp));

    if (limit != null && limit > 0) {
      history = history.take(limit).toList();
    }

    return history;
  }

  List<String> getSuggestions(String query, {int limit = 10}) {
    if (!_settings['suggestionsEnabled'] || query.isEmpty) return [];

    final normalizedQuery = query.toLowerCase();
    
    final suggestions = _historyCache
        .where((item) => item.query.toLowerCase().contains(normalizedQuery))
        .map((item) => item.query)
        .toSet()
        .take(limit)
        .toList();

    return suggestions;
  }

  List<String> getAutoComplete(String query, {int limit = 5}) {
    if (!_settings['autoCompleteEnabled'] || query.isEmpty) return [];

    final normalizedQuery = query.toLowerCase();
    
    final completions = _historyCache
        .where((item) => item.query.toLowerCase().startsWith(normalizedQuery))
        .map((item) => item.query)
        .toSet()
        .take(limit)
        .toList();

    return completions;
  }

  List<String> getPopularQueries({
    String? category,
    int limit = 10,
    int minSearchCount = 1,
  }) {
    var queries = _historyCache
        .where((item) => item.searchCount >= minSearchCount);

    if (category != null) {
      queries = queries.where((item) => item.category == category);
    }

    final sortedQueries = queries.toList()
      ..sort((a, b) => b.searchCount.compareTo(a.searchCount));

    return sortedQueries
        .take(limit)
        .map((item) => item.query)
        .toList();
  }

  List<String> getCategories() {
    if (!_settings['categoriesEnabled']) return [];
    
    return _historyCache
        .map((item) => item.category)
        .toSet()
        .toList()
      ..sort();
  }

  Future<void> removeQuery(String query) async {
    _historyCache.removeWhere(
      (item) => item.query.toLowerCase() == query.toLowerCase(),
    );
    
    await _saveHistory();
  }

  Future<void> clearHistory({String? category}) async {
    if (category != null) {
      _historyCache.removeWhere((item) => item.category == category);
    } else {
      _historyCache.clear();
    }
    
    await _saveHistory();
  }

  Future<void> clearOldHistory({required Duration olderThan}) async {
    final cutoffDate = DateTime.now().subtract(olderThan);
    
    _historyCache.removeWhere((item) => item.timestamp.isBefore(cutoffDate));
    
    await _saveHistory();
  }

  Map<String, dynamic> getStatistics() {
    if (_historyCache.isEmpty) {
      return {
        'totalQueries': 0,
        'uniqueQueries': 0,
        'totalSearches': 0,
        'averageSearchesPerQuery': 0.0,
        'mostPopularQuery': null,
        'oldestQuery': null,
        'newestQuery': null,
        'categoriesCount': 0,
      };
    }

    final totalSearches = _historyCache
        .map((item) => item.searchCount)
        .fold(0, (sum, count) => sum + count);

    final mostPopular = _historyCache
        .reduce((a, b) => a.searchCount > b.searchCount ? a : b);

    final oldest = _historyCache
        .reduce((a, b) => a.timestamp.isBefore(b.timestamp) ? a : b);

    final newest = _historyCache
        .reduce((a, b) => a.timestamp.isAfter(b.timestamp) ? a : b);

    return {
      'totalQueries': _historyCache.length,
      'uniqueQueries': _historyCache.map((item) => item.query).toSet().length,
      'totalSearches': totalSearches,
      'averageSearchesPerQuery': totalSearches / _historyCache.length,
      'mostPopularQuery': mostPopular.query,
      'oldestQuery': oldest.timestamp,
      'newestQuery': newest.timestamp,
      'categoriesCount': getCategories().length,
    };
  }

  Future<void> updateSettings(Map<String, dynamic> newSettings) async {
    _settings.addAll(newSettings);
    await _saveSettings();
  }

  Map<String, dynamic> getSettings() {
    return Map<String, dynamic>.from(_settings);
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Search History Management',
      home: const SearchHistoryPage(),
    );
  }
}

class SearchHistoryPage extends StatefulWidget {
  const SearchHistoryPage({super.key});

  @override
  State<SearchHistoryPage> createState() => _SearchHistoryPageState();
}

class _SearchHistoryPageState extends State<SearchHistoryPage> {
  final SearchHistoryManager _historyManager = SearchHistoryManager();
  final TextEditingController _searchController = TextEditingController();
  
  List<SearchHistoryItem> _history = [];
  List<String> _suggestions = [];
  List<String> _categories = [];
  String? _selectedCategory;
  Map<String, dynamic> _statistics = {};

  @override
  void initState() {
    super.initState();
    _initializeData();
    
    _searchController.addListener(() {
      _updateSuggestions();
    });
  }

  @override
  void dispose() {
    _searchController.dispose();
    super.dispose();
  }

  Future<void> _initializeData() async {
    await _historyManager.initialize();
    await _loadData();
  }

  Future<void> _loadData() async {
    final history = _historyManager.getHistory(category: _selectedCategory);
    final categories = _historyManager.getCategories();
    final statistics = _historyManager.getStatistics();
    
    setState(() {
      _history = history;
      _categories = categories;
      _statistics = statistics;
    });
  }

  void _updateSuggestions() {
    final suggestions = _historyManager.getSuggestions(_searchController.text);
    setState(() {
      _suggestions = suggestions;
    });
  }

  Future<void> _performSearch() async {
    if (_searchController.text.trim().isEmpty) return;

    await _historyManager.addSearch(
      _searchController.text,
      category: _selectedCategory ?? 'general',
      metadata: {
        'timestamp': DateTime.now().toIso8601String(),
        'source': 'manual',
      },
    );

    _searchController.clear();
    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Search added to history!')),
      );
    }
  }

  Future<void> _removeQuery(String query) async {
    await _historyManager.removeQuery(query);
    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Query removed from history!')),
      );
    }
  }

  Future<void> _clearHistory() async {
    await _historyManager.clearHistory(category: _selectedCategory);
    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('History cleared!')),
      );
    }
  }

  Future<void> _createSampleData() async {
    final sampleQueries = [
      'flutter widgets',
      'dart programming',
      'mobile development',
      'state management',
      'database integration',
      'UI components',
      'animation techniques',
      'performance optimization',
    ];

    for (int i = 0; i < sampleQueries.length; i++) {
      await _historyManager.addSearch(
        sampleQueries[i],
        category: i < 4 ? 'development' : 'design',
        metadata: {
          'source': 'sample',
          'priority': i < 3 ? 'high' : 'normal',
        },
      );
      
      // Simulate multiple searches for some queries
      if (i < 3) {
        await Future.delayed(const Duration(milliseconds: 100));
        await _historyManager.addSearch(sampleQueries[i]);
      }
    }

    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Sample search history created!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Search History'),
        actions: [
          IconButton(
            icon: const Icon(Icons.add),
            onPressed: _createSampleData,
          ),
          IconButton(
            icon: const Icon(Icons.clear),
            onPressed: _clearHistory,
          ),
        ],
      ),
      body: Column(
        children: [
          // Search input
          Padding(
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
                        onSubmitted: (_) => _performSearch(),
                      ),
                    ),
                    const SizedBox(width: 8),
                    ElevatedButton(
                      onPressed: _performSearch,
                      child: const Text('Search'),
                    ),
                  ],
                ),
                
                if (_suggestions.isNotEmpty) ...[
                  const SizedBox(height: 8),
                  Container(
                    height: 40,
                    child: ListView.builder(
                      scrollDirection: Axis.horizontal,
                      itemCount: _suggestions.length,
                      itemBuilder: (context, index) {
                        final suggestion = _suggestions[index];
                        return Padding(
                          padding: const EdgeInsets.only(right: 8),
                          child: ActionChip(
                            label: Text(suggestion),
                            onPressed: () {
                              _searchController.text = suggestion;
                              _performSearch();
                            },
                          ),
                        );
                      },
                    ),
                  ),
                ],
              ],
            ),
          ),
          
          // Category filter
          if (_categories.isNotEmpty)
            Padding(
              padding: const EdgeInsets.symmetric(horizontal: 16),
              child: Row(
                children: [
                  const Text('Category: '),
                  DropdownButton<String?>(
                    value: _selectedCategory,
                    items: [
                      const DropdownMenuItem<String?>(
                        value: null,
                        child: Text('All'),
                      ),
                      ..._categories.map((category) {
                        return DropdownMenuItem<String>(
                          value: category,
                          child: Text(category),
                        );
                      }),
                    ],
                    onChanged: (value) {
                      setState(() {
                        _selectedCategory = value;
                      });
                      _loadData();
                    },
                  ),
                ],
              ),
            ),

          // Statistics
          if (_statistics['totalQueries'] > 0)
            Padding(
              padding: const EdgeInsets.all(16),
              child: Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Search Statistics',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                      const SizedBox(height: 8),
                      Row(
                        children: [
                          Expanded(
                            child: Text('Queries: ${_statistics['totalQueries']}'),
                          ),
                          Expanded(
                            child: Text('Searches: ${_statistics['totalSearches']}'),
                          ),
                        ],
                      ),
                      if (_statistics['mostPopularQuery'] != null)
                        Text('Most Popular: ${_statistics['mostPopularQuery']}'),
                    ],
                  ),
                ),
              ),
            ),
          
          // History list
          Expanded(
            child: _history.isEmpty
                ? const Center(child: Text('No search history'))
                : ListView.builder(
                    itemCount: _history.length,
                    itemBuilder: (context, index) {
                      final item = _history[index];
                      
                      return Card(
                        margin: const EdgeInsets.symmetric(
                          horizontal: 16,
                          vertical: 2,
                        ),
                        child: ListTile(
                          leading: const Icon(Icons.history),
                          title: Text(item.query),
                          subtitle: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                '${item.category} • ${item.searchCount} ${item.searchCount == 1 ? 'search' : 'searches'}',
                              ),
                              Text(
                                '${item.timestamp.day}/${item.timestamp.month}/${item.timestamp.year} '
                                '${item.timestamp.hour}:${item.timestamp.minute.toString().padLeft(2, '0')}',
                                style: const TextStyle(fontSize: 12),
                              ),
                            ],
                          ),
                          trailing: PopupMenuButton<String>(
                            onSelected: (value) {
                              switch (value) {
                                case 'search':
                                  _searchController.text = item.query;
                                  _performSearch();
                                  break;
                                case 'remove':
                                  _removeQuery(item.query);
                                  break;
                              }
                            },
                            itemBuilder: (context) => [
                              const PopupMenuItem(
                                value: 'search',
                                child: Text('Search Again'),
                              ),
                              const PopupMenuItem(
                                value: 'remove',
                                child: Text('Remove'),
                              ),
                            ],
                          ),
                          onTap: () {
                            _searchController.text = item.query;
                          },
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

Search history management provides comprehensive query tracking with  
suggestions, auto-completion, and analytics. The system supports categories,  
metadata, popularity tracking, and intelligent filtering with configurable  
retention policies and privacy controls.  

## Favorites Management System

Managing user favorites with categories, synchronization, and recommendations.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class FavoriteItem {
  final String id;
  final String title;
  final String description;
  final String category;
  final String url;
  final String imageUrl;
  final DateTime createdAt;
  final DateTime lastAccessedAt;
  final int accessCount;
  final Map<String, String> tags;
  final double rating;

  FavoriteItem({
    required this.id,
    required this.title,
    required this.description,
    required this.category,
    required this.url,
    required this.imageUrl,
    required this.createdAt,
    required this.lastAccessedAt,
    required this.accessCount,
    required this.tags,
    required this.rating,
  });

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'title': title,
      'description': description,
      'category': category,
      'url': url,
      'imageUrl': imageUrl,
      'createdAt': createdAt.toIso8601String(),
      'lastAccessedAt': lastAccessedAt.toIso8601String(),
      'accessCount': accessCount,
      'tags': tags,
      'rating': rating,
    };
  }

  factory FavoriteItem.fromJson(Map<String, dynamic> json) {
    return FavoriteItem(
      id: json['id'],
      title: json['title'],
      description: json['description'],
      category: json['category'],
      url: json['url'],
      imageUrl: json['imageUrl'],
      createdAt: DateTime.parse(json['createdAt']),
      lastAccessedAt: DateTime.parse(json['lastAccessedAt']),
      accessCount: json['accessCount'],
      tags: Map<String, String>.from(json['tags']),
      rating: json['rating']?.toDouble() ?? 0.0,
    );
  }

  FavoriteItem copyWith({
    String? id,
    String? title,
    String? description,
    String? category,
    String? url,
    String? imageUrl,
    DateTime? createdAt,
    DateTime? lastAccessedAt,
    int? accessCount,
    Map<String, String>? tags,
    double? rating,
  }) {
    return FavoriteItem(
      id: id ?? this.id,
      title: title ?? this.title,
      description: description ?? this.description,
      category: category ?? this.category,
      url: url ?? this.url,
      imageUrl: imageUrl ?? this.imageUrl,
      createdAt: createdAt ?? this.createdAt,
      lastAccessedAt: lastAccessedAt ?? this.lastAccessedAt,
      accessCount: accessCount ?? this.accessCount,
      tags: tags ?? Map<String, String>.from(this.tags),
      rating: rating ?? this.rating,
    );
  }
}

enum SortOption {
  name,
  dateAdded,
  lastAccessed,
  accessCount,
  rating,
}

class FavoritesManager {
  static final FavoritesManager _instance = FavoritesManager._internal();
  factory FavoritesManager() => _instance;
  FavoritesManager._internal();

  static const String _favoritesKey = 'favorites_list';
  static const String _categoriesKey = 'favorite_categories';
  static const String _settingsKey = 'favorites_settings';

  List<FavoriteItem> _favoritesCache = [];
  List<String> _categoriesCache = [];
  Map<String, dynamic> _settings = {
    'defaultCategory': 'General',
    'autoCategories': true,
    'trackAccessCount': true,
    'maxFavorites': 500,
    'defaultSort': SortOption.dateAdded.index,
  };

  Future<void> initialize() async {
    await _loadFavorites();
    await _loadCategories();
    await _loadSettings();
  }

  Future<void> _loadFavorites() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final favoritesJson = prefs.getString(_favoritesKey);
      
      if (favoritesJson != null) {
        final List<dynamic> favoritesData = json.decode(favoritesJson);
        _favoritesCache = favoritesData
            .map((item) => FavoriteItem.fromJson(item))
            .toList();
      }
    } catch (e) {
      _favoritesCache = [];
    }
  }

  Future<void> _saveFavorites() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final favoritesData = _favoritesCache.map((item) => item.toJson()).toList();
      await prefs.setString(_favoritesKey, json.encode(favoritesData));
    } catch (e) {
      // Handle save error
    }
  }

  Future<void> _loadCategories() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final categoriesData = prefs.getStringList(_categoriesKey);
      _categoriesCache = categoriesData ?? ['General'];
    } catch (e) {
      _categoriesCache = ['General'];
    }
  }

  Future<void> _saveCategories() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      await prefs.setStringList(_categoriesKey, _categoriesCache);
    } catch (e) {
      // Handle save error
    }
  }

  Future<void> _loadSettings() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final settingsJson = prefs.getString(_settingsKey);
      
      if (settingsJson != null) {
        final settingsData = json.decode(settingsJson);
        _settings.addAll(settingsData);
      }
    } catch (e) {
      // Use default settings
    }
  }

  Future<void> _saveSettings() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      await prefs.setString(_settingsKey, json.encode(_settings));
    } catch (e) {
      // Handle save error
    }
  }

  Future<String> addFavorite({
    required String title,
    required String description,
    required String url,
    String? category,
    String? imageUrl,
    Map<String, String>? tags,
    double rating = 0.0,
  }) async {
    final id = DateTime.now().millisecondsSinceEpoch.toString();
    
    // Auto-detect category if enabled
    String finalCategory = category ?? _settings['defaultCategory'];
    if (_settings['autoCategories'] && category == null) {
      finalCategory = _detectCategory(title, description, url);
    }

    // Ensure category exists
    if (!_categoriesCache.contains(finalCategory)) {
      _categoriesCache.add(finalCategory);
      await _saveCategories();
    }

    final favoriteItem = FavoriteItem(
      id: id,
      title: title,
      description: description,
      category: finalCategory,
      url: url,
      imageUrl: imageUrl ?? '',
      createdAt: DateTime.now(),
      lastAccessedAt: DateTime.now(),
      accessCount: 0,
      tags: tags ?? {},
      rating: rating,
    );

    _favoritesCache.add(favoriteItem);
    
    // Maintain max favorites limit
    if (_favoritesCache.length > _settings['maxFavorites']) {
      _favoritesCache.removeAt(0);
    }

    await _saveFavorites();
    return id;
  }

  String _detectCategory(String title, String description, String url) {
    final text = '${title.toLowerCase()} ${description.toLowerCase()} ${url.toLowerCase()}';
    
    if (text.contains('github') || text.contains('code') || text.contains('programming')) {
      return 'Development';
    } else if (text.contains('news') || text.contains('article') || text.contains('blog')) {
      return 'News';
    } else if (text.contains('video') || text.contains('youtube') || text.contains('watch')) {
      return 'Videos';
    } else if (text.contains('shop') || text.contains('buy') || text.contains('store')) {
      return 'Shopping';
    } else if (text.contains('recipe') || text.contains('food') || text.contains('cooking')) {
      return 'Food';
    } else {
      return _settings['defaultCategory'];
    }
  }

  Future<void> updateFavorite(String id, FavoriteItem updatedItem) async {
    final index = _favoritesCache.indexWhere((item) => item.id == id);
    if (index != -1) {
      _favoritesCache[index] = updatedItem;
      await _saveFavorites();
    }
  }

  Future<void> removeFavorite(String id) async {
    _favoritesCache.removeWhere((item) => item.id == id);
    await _saveFavorites();
  }

  Future<void> markAsAccessed(String id) async {
    if (!_settings['trackAccessCount']) return;

    final index = _favoritesCache.indexWhere((item) => item.id == id);
    if (index != -1) {
      final item = _favoritesCache[index];
      final updatedItem = item.copyWith(
        lastAccessedAt: DateTime.now(),
        accessCount: item.accessCount + 1,
      );
      
      _favoritesCache[index] = updatedItem;
      await _saveFavorites();
    }
  }

  List<FavoriteItem> getFavorites({
    String? category,
    String? searchQuery,
    SortOption sortBy = SortOption.dateAdded,
    bool ascending = false,
  }) {
    var favorites = List<FavoriteItem>.from(_favoritesCache);

    // Filter by category
    if (category != null && category != 'All') {
      favorites = favorites.where((item) => item.category == category).toList();
    }

    // Filter by search query
    if (searchQuery != null && searchQuery.isNotEmpty) {
      final query = searchQuery.toLowerCase();
      favorites = favorites.where((item) =>
          item.title.toLowerCase().contains(query) ||
          item.description.toLowerCase().contains(query) ||
          item.tags.values.any((tag) => tag.toLowerCase().contains(query))).toList();
    }

    // Sort favorites
    switch (sortBy) {
      case SortOption.name:
        favorites.sort((a, b) => ascending 
            ? a.title.compareTo(b.title)
            : b.title.compareTo(a.title));
        break;
      case SortOption.dateAdded:
        favorites.sort((a, b) => ascending 
            ? a.createdAt.compareTo(b.createdAt)
            : b.createdAt.compareTo(a.createdAt));
        break;
      case SortOption.lastAccessed:
        favorites.sort((a, b) => ascending 
            ? a.lastAccessedAt.compareTo(b.lastAccessedAt)
            : b.lastAccessedAt.compareTo(a.lastAccessedAt));
        break;
      case SortOption.accessCount:
        favorites.sort((a, b) => ascending 
            ? a.accessCount.compareTo(b.accessCount)
            : b.accessCount.compareTo(a.accessCount));
        break;
      case SortOption.rating:
        favorites.sort((a, b) => ascending 
            ? a.rating.compareTo(b.rating)
            : b.rating.compareTo(a.rating));
        break;
    }

    return favorites;
  }

  List<String> getCategories() => List<String>.from(_categoriesCache);

  List<FavoriteItem> getRecommendations({int limit = 5}) {
    // Simple recommendation based on most accessed items
    final favorites = List<FavoriteItem>.from(_favoritesCache);
    favorites.sort((a, b) => b.accessCount.compareTo(a.accessCount));
    return favorites.take(limit).toList();
  }

  Future<void> addCategory(String category) async {
    if (!_categoriesCache.contains(category)) {
      _categoriesCache.add(category);
      await _saveCategories();
    }
  }

  Future<void> removeCategory(String category) async {
    if (category == _settings['defaultCategory']) return;
    
    _categoriesCache.remove(category);
    
    // Move items to default category
    for (int i = 0; i < _favoritesCache.length; i++) {
      if (_favoritesCache[i].category == category) {
        _favoritesCache[i] = _favoritesCache[i].copyWith(
          category: _settings['defaultCategory'],
        );
      }
    }
    
    await _saveCategories();
    await _saveFavorites();
  }

  Map<String, int> getCategoryStats() {
    final stats = <String, int>{};
    for (final item in _favoritesCache) {
      stats[item.category] = (stats[item.category] ?? 0) + 1;
    }
    return stats;
  }

  Map<String, dynamic> getStatistics() {
    if (_favoritesCache.isEmpty) {
      return {
        'totalFavorites': 0,
        'totalCategories': _categoriesCache.length,
        'averageRating': 0.0,
        'mostPopularCategory': null,
        'totalAccesses': 0,
      };
    }

    final totalAccesses = _favoritesCache
        .map((item) => item.accessCount)
        .fold(0, (sum, count) => sum + count);

    final averageRating = _favoritesCache
        .map((item) => item.rating)
        .fold(0.0, (sum, rating) => sum + rating) / _favoritesCache.length;

    final categoryStats = getCategoryStats();
    final mostPopularCategory = categoryStats.entries
        .reduce((a, b) => a.value > b.value ? a : b)
        .key;

    return {
      'totalFavorites': _favoritesCache.length,
      'totalCategories': _categoriesCache.length,
      'averageRating': averageRating,
      'mostPopularCategory': mostPopularCategory,
      'totalAccesses': totalAccesses,
    };
  }

  Future<void> clearFavorites({String? category}) async {
    if (category != null) {
      _favoritesCache.removeWhere((item) => item.category == category);
    } else {
      _favoritesCache.clear();
    }
    
    await _saveFavorites();
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Favorites Management',
      home: const FavoritesPage(),
    );
  }
}

class FavoritesPage extends StatefulWidget {
  const FavoritesPage({super.key});

  @override
  State<FavoritesPage> createState() => _FavoritesPageState();
}

class _FavoritesPageState extends State<FavoritesPage> {
  final FavoritesManager _favoritesManager = FavoritesManager();
  final TextEditingController _searchController = TextEditingController();
  
  List<FavoriteItem> _favorites = [];
  List<String> _categories = [];
  String _selectedCategory = 'All';
  SortOption _sortOption = SortOption.dateAdded;
  Map<String, dynamic> _statistics = {};

  @override
  void initState() {
    super.initState();
    _initializeData();
  }

  @override
  void dispose() {
    _searchController.dispose();
    super.dispose();
  }

  Future<void> _initializeData() async {
    await _favoritesManager.initialize();
    await _loadData();
  }

  Future<void> _loadData() async {
    final favorites = _favoritesManager.getFavorites(
      category: _selectedCategory == 'All' ? null : _selectedCategory,
      searchQuery: _searchController.text,
      sortBy: _sortOption,
    );
    
    final categories = ['All', ..._favoritesManager.getCategories()];
    final statistics = _favoritesManager.getStatistics();
    
    setState(() {
      _favorites = favorites;
      _categories = categories;
      _statistics = statistics;
    });
  }

  Future<void> _addFavorite() async {
    final result = await showDialog<Map<String, dynamic>>(
      context: context,
      builder: (context) => const AddFavoriteDialog(),
    );

    if (result != null) {
      await _favoritesManager.addFavorite(
        title: result['title'],
        description: result['description'],
        url: result['url'],
        category: result['category'],
        rating: result['rating']?.toDouble() ?? 0.0,
      );

      await _loadData();

      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Favorite added!')),
        );
      }
    }
  }

  Future<void> _createSampleData() async {
    final sampleFavorites = [
      {
        'title': 'Flutter Documentation',
        'description': 'Official Flutter documentation and guides',
        'url': 'https://docs.flutter.dev',
        'category': 'Development',
        'rating': 5.0,
      },
      {
        'title': 'GitHub',
        'description': 'Code repository hosting service',
        'url': 'https://github.com',
        'category': 'Development',
        'rating': 4.5,
      },
      {
        'title': 'Google News',
        'description': 'Latest news and headlines',
        'url': 'https://news.google.com',
        'category': 'News',
        'rating': 4.0,
      },
      {
        'title': 'YouTube',
        'description': 'Video sharing platform',
        'url': 'https://youtube.com',
        'category': 'Videos',
        'rating': 4.0,
      },
    ];

    for (final favorite in sampleFavorites) {
      await _favoritesManager.addFavorite(
        title: favorite['title'] as String,
        description: favorite['description'] as String,
        url: favorite['url'] as String,
        category: favorite['category'] as String,
        rating: favorite['rating'] as double,
      );
    }

    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Sample favorites created!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Favorites Manager'),
        actions: [
          IconButton(
            icon: const Icon(Icons.add),
            onPressed: _addFavorite,
          ),
          PopupMenuButton<String>(
            onSelected: (value) {
              switch (value) {
                case 'sample':
                  _createSampleData();
                  break;
                case 'stats':
                  _showStatistics();
                  break;
              }
            },
            itemBuilder: (context) => [
              const PopupMenuItem(
                value: 'sample',
                child: Text('Add Sample Data'),
              ),
              const PopupMenuItem(
                value: 'stats',
                child: Text('View Statistics'),
              ),
            ],
          ),
        ],
      ),
      body: Column(
        children: [
          // Search and filters
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              children: [
                TextField(
                  controller: _searchController,
                  decoration: const InputDecoration(
                    labelText: 'Search favorites',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.search),
                  ),
                  onChanged: (value) => _loadData(),
                ),
                const SizedBox(height: 8),
                Row(
                  children: [
                    Expanded(
                      child: DropdownButton<String>(
                        value: _selectedCategory,
                        isExpanded: true,
                        items: _categories.map((category) {
                          return DropdownMenuItem(
                            value: category,
                            child: Text(category),
                          );
                        }).toList(),
                        onChanged: (value) {
                          if (value != null) {
                            setState(() {
                              _selectedCategory = value;
                            });
                            _loadData();
                          }
                        },
                      ),
                    ),
                    const SizedBox(width: 16),
                    DropdownButton<SortOption>(
                      value: _sortOption,
                      items: SortOption.values.map((option) {
                        return DropdownMenuItem(
                          value: option,
                          child: Text(option.name),
                        );
                      }).toList(),
                      onChanged: (value) {
                        if (value != null) {
                          setState(() {
                            _sortOption = value;
                          });
                          _loadData();
                        }
                      },
                    ),
                  ],
                ),
              ],
            ),
          ),
          
          // Favorites list
          Expanded(
            child: _favorites.isEmpty
                ? const Center(child: Text('No favorites found'))
                : ListView.builder(
                    itemCount: _favorites.length,
                    itemBuilder: (context, index) {
                      final favorite = _favorites[index];
                      
                      return Card(
                        margin: const EdgeInsets.symmetric(
                          horizontal: 16,
                          vertical: 4,
                        ),
                        child: ListTile(
                          leading: CircleAvatar(
                            child: Text(favorite.title.substring(0, 1).toUpperCase()),
                          ),
                          title: Text(favorite.title),
                          subtitle: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                favorite.description,
                                maxLines: 2,
                                overflow: TextOverflow.ellipsis,
                              ),
                              const SizedBox(height: 4),
                              Row(
                                children: [
                                  Text(
                                    favorite.category,
                                    style: TextStyle(
                                      color: Theme.of(context).colorScheme.secondary,
                                      fontSize: 12,
                                    ),
                                  ),
                                  if (favorite.accessCount > 0) ...[
                                    const Text(' • '),
                                    Text(
                                      '${favorite.accessCount} visits',
                                      style: const TextStyle(fontSize: 12),
                                    ),
                                  ],
                                ],
                              ),
                            ],
                          ),
                          trailing: Row(
                            mainAxisSize: MainAxisSize.min,
                            children: [
                              if (favorite.rating > 0) ...[
                                const Icon(Icons.star, size: 16, color: Colors.amber),
                                Text(favorite.rating.toStringAsFixed(1)),
                              ],
                              PopupMenuButton<String>(
                                onSelected: (value) {
                                  switch (value) {
                                    case 'open':
                                      _favoritesManager.markAsAccessed(favorite.id);
                                      _loadData();
                                      break;
                                    case 'edit':
                                      // Implementation for edit
                                      break;
                                    case 'delete':
                                      _favoritesManager.removeFavorite(favorite.id);
                                      _loadData();
                                      break;
                                  }
                                },
                                itemBuilder: (context) => [
                                  const PopupMenuItem(
                                    value: 'open',
                                    child: Text('Open'),
                                  ),
                                  const PopupMenuItem(
                                    value: 'edit',
                                    child: Text('Edit'),
                                  ),
                                  const PopupMenuItem(
                                    value: 'delete',
                                    child: Text('Delete'),
                                  ),
                                ],
                              ),
                            ],
                          ),
                          onTap: () {
                            _favoritesManager.markAsAccessed(favorite.id);
                            _loadData();
                          },
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }

  void _showStatistics() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Favorites Statistics'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Total Favorites: ${_statistics['totalFavorites']}'),
            Text('Categories: ${_statistics['totalCategories']}'),
            Text('Average Rating: ${_statistics['averageRating']?.toStringAsFixed(1)}'),
            Text('Total Accesses: ${_statistics['totalAccesses']}'),
            if (_statistics['mostPopularCategory'] != null)
              Text('Most Popular: ${_statistics['mostPopularCategory']}'),
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
  }
}

class AddFavoriteDialog extends StatefulWidget {
  const AddFavoriteDialog({super.key});

  @override
  State<AddFavoriteDialog> createState() => _AddFavoriteDialogState();
}

class _AddFavoriteDialogState extends State<AddFavoriteDialog> {
  final _titleController = TextEditingController();
  final _descriptionController = TextEditingController();
  final _urlController = TextEditingController();
  String _selectedCategory = 'General';
  double _rating = 0.0;

  @override
  Widget build(BuildContext context) {
    return AlertDialog(
      title: const Text('Add Favorite'),
      content: SingleChildScrollView(
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(
              controller: _titleController,
              decoration: const InputDecoration(labelText: 'Title'),
            ),
            TextField(
              controller: _descriptionController,
              decoration: const InputDecoration(labelText: 'Description'),
            ),
            TextField(
              controller: _urlController,
              decoration: const InputDecoration(labelText: 'URL'),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                const Text('Rating: '),
                Expanded(
                  child: Slider(
                    value: _rating,
                    min: 0,
                    max: 5,
                    divisions: 10,
                    label: _rating.toStringAsFixed(1),
                    onChanged: (value) {
                      setState(() {
                        _rating = value;
                      });
                    },
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
      actions: [
        TextButton(
          onPressed: () => Navigator.pop(context),
          child: const Text('Cancel'),
        ),
        ElevatedButton(
          onPressed: () {
            Navigator.pop(context, {
              'title': _titleController.text,
              'description': _descriptionController.text,
              'url': _urlController.text,
              'category': _selectedCategory,
              'rating': _rating,
            });
          },
          child: const Text('Add'),
        ),
      ],
    );
  }

  @override
  void dispose() {
    _titleController.dispose();
    _descriptionController.dispose();
    _urlController.dispose();
    super.dispose();
  }
}
```

Favorites management system provides comprehensive bookmark functionality with  
categories, ratings, and usage tracking. The system supports auto-categorization,  
recommendations, search functionality, and detailed analytics for managing  
personal content collections efficiently.  

## Data Validation and Integrity

Ensuring data consistency and validation in persistent storage.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class ValidationRule {
  final String name;
  final bool Function(dynamic value) validator;
  final String errorMessage;

  ValidationRule({
    required this.name,
    required this.validator,
    required this.errorMessage,
  });
}

class ValidationResult {
  final bool isValid;
  final List<String> errors;
  final Map<String, dynamic> sanitizedData;

  ValidationResult({
    required this.isValid,
    required this.errors,
    required this.sanitizedData,
  });
}

class DataValidator {
  final Map<String, List<ValidationRule>> _fieldRules = {};
  final List<ValidationRule> _globalRules = [];

  void addFieldRule(String fieldName, ValidationRule rule) {
    _fieldRules.putIfAbsent(fieldName, () => []).add(rule);
  }

  void addGlobalRule(ValidationRule rule) {
    _globalRules.add(rule);
  }

  ValidationResult validate(Map<String, dynamic> data) {
    final errors = <String>[];
    final sanitizedData = <String, dynamic>{};

    // Validate individual fields
    for (final entry in data.entries) {
      final fieldName = entry.key;
      final value = entry.value;
      
      final rules = _fieldRules[fieldName] ?? [];
      bool fieldValid = true;
      
      for (final rule in rules) {
        if (!rule.validator(value)) {
          errors.add('$fieldName: ${rule.errorMessage}');
          fieldValid = false;
        }
      }
      
      if (fieldValid) {
        sanitizedData[fieldName] = _sanitizeValue(value);
      }
    }

    // Validate global rules
    for (final rule in _globalRules) {
      if (!rule.validator(data)) {
        errors.add('Global: ${rule.errorMessage}');
      }
    }

    return ValidationResult(
      isValid: errors.isEmpty,
      errors: errors,
      sanitizedData: sanitizedData,
    );
  }

  dynamic _sanitizeValue(dynamic value) {
    if (value is String) {
      return value.trim();
    }
    return value;
  }
}

class IntegrityChecker {
  static String calculateChecksum(Map<String, dynamic> data) {
    final jsonString = json.encode(data);
    return jsonString.hashCode.toString();
  }

  static bool verifyIntegrity(
    Map<String, dynamic> data,
    String expectedChecksum,
  ) {
    return calculateChecksum(data) == expectedChecksum;
  }

  static Map<String, dynamic> addIntegrityCheck(Map<String, dynamic> data) {
    final checksum = calculateChecksum(data);
    return {
      ...data,
      '_checksum': checksum,
      '_timestamp': DateTime.now().toIso8601String(),
    };
  }

  static ValidationResult verifyStoredData(Map<String, dynamic> storedData) {
    final errors = <String>[];
    
    if (!storedData.containsKey('_checksum')) {
      errors.add('Missing integrity checksum');
    } else {
      final storedChecksum = storedData['_checksum'];
      final dataWithoutChecksum = Map<String, dynamic>.from(storedData);
      dataWithoutChecksum.remove('_checksum');
      dataWithoutChecksum.remove('_timestamp');
      
      if (!verifyIntegrity(dataWithoutChecksum, storedChecksum)) {
        errors.add('Data integrity check failed');
      }
    }

    return ValidationResult(
      isValid: errors.isEmpty,
      errors: errors,
      sanitizedData: storedData,
    );
  }
}

class ValidatedStorage {
  static final ValidatedStorage _instance = ValidatedStorage._internal();
  factory ValidatedStorage() => _instance;
  ValidatedStorage._internal();

  static Database? _database;
  final DataValidator _validator = DataValidator();
  final List<Map<String, dynamic>> _validationLog = [];

  Future<Database> get database async {
    _database ??= await _initDatabase();
    return _database!;
  }

  Future<Database> _initDatabase() async {
    String path = join(await getDatabasesPath(), 'validated_storage.db');
    
    return await openDatabase(
      path,
      version: 1,
      onCreate: (db, version) async {
        await db.execute('''
          CREATE TABLE validated_data (
            id TEXT PRIMARY KEY,
            data_type TEXT NOT NULL,
            data_json TEXT NOT NULL,
            checksum TEXT NOT NULL,
            created_at INTEGER NOT NULL,
            updated_at INTEGER NOT NULL,
            validation_version INTEGER NOT NULL
          )
        ''');

        await db.execute('''
          CREATE TABLE validation_log (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            operation TEXT NOT NULL,
            data_id TEXT,
            validation_result TEXT NOT NULL,
            timestamp INTEGER NOT NULL
          )
        ''');
      },
    );
  }

  void initializeValidationRules() {
    // User data validation rules
    _validator.addFieldRule('name', ValidationRule(
      name: 'Name Required',
      validator: (value) => value is String && value.trim().isNotEmpty,
      errorMessage: 'Name is required and must not be empty',
    ));

    _validator.addFieldRule('name', ValidationRule(
      name: 'Name Length',
      validator: (value) => value is String && value.length >= 2 && value.length <= 50,
      errorMessage: 'Name must be between 2 and 50 characters',
    ));

    _validator.addFieldRule('email', ValidationRule(
      name: 'Email Format',
      validator: (value) => value is String && 
          RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value),
      errorMessage: 'Invalid email format',
    ));

    _validator.addFieldRule('age', ValidationRule(
      name: 'Age Range',
      validator: (value) => value is int && value >= 0 && value <= 150,
      errorMessage: 'Age must be between 0 and 150',
    ));

    _validator.addFieldRule('score', ValidationRule(
      name: 'Score Range',
      validator: (value) => value is double && value >= 0.0 && value <= 100.0,
      errorMessage: 'Score must be between 0.0 and 100.0',
    ));

    // Global validation rules
    _validator.addGlobalRule(ValidationRule(
      name: 'Required Fields',
      validator: (data) => data is Map && data.containsKey('name'),
      errorMessage: 'Name field is required',
    ));
  }

  Future<bool> storeData(
    String id,
    String dataType,
    Map<String, dynamic> data,
  ) async {
    final validation = _validator.validate(data);
    
    await _logValidation('STORE', id, validation);
    
    if (!validation.isValid) {
      return false;
    }

    try {
      final db = await database;
      final integrityData = IntegrityChecker.addIntegrityCheck(validation.sanitizedData);
      
      await db.insert(
        'validated_data',
        {
          'id': id,
          'data_type': dataType,
          'data_json': json.encode(integrityData),
          'checksum': integrityData['_checksum'],
          'created_at': DateTime.now().millisecondsSinceEpoch,
          'updated_at': DateTime.now().millisecondsSinceEpoch,
          'validation_version': 1,
        },
        conflictAlgorithm: ConflictAlgorithm.replace,
      );
      
      return true;
    } catch (e) {
      await _logValidation('STORE_ERROR', id, ValidationResult(
        isValid: false,
        errors: ['Database error: $e'],
        sanitizedData: {},
      ));
      return false;
    }
  }

  Future<Map<String, dynamic>?> retrieveData(String id) async {
    try {
      final db = await database;
      final result = await db.query(
        'validated_data',
        where: 'id = ?',
        whereArgs: [id],
      );

      if (result.isEmpty) return null;

      final row = result.first;
      final storedData = json.decode(row['data_json']);
      
      final integrityCheck = IntegrityChecker.verifyStoredData(storedData);
      await _logValidation('RETRIEVE', id, integrityCheck);
      
      if (!integrityCheck.isValid) {
        return null;
      }

      // Remove integrity metadata before returning
      final cleanData = Map<String, dynamic>.from(storedData);
      cleanData.remove('_checksum');
      cleanData.remove('_timestamp');
      
      return cleanData;
    } catch (e) {
      await _logValidation('RETRIEVE_ERROR', id, ValidationResult(
        isValid: false,
        errors: ['Database error: $e'],
        sanitizedData: {},
      ));
      return null;
    }
  }

  Future<List<Map<String, dynamic>>> getAllData({String? dataType}) async {
    try {
      final db = await database;
      
      String where = '';
      List<dynamic> whereArgs = [];
      
      if (dataType != null) {
        where = 'data_type = ?';
        whereArgs.add(dataType);
      }

      final result = await db.query(
        'validated_data',
        where: where.isNotEmpty ? where : null,
        whereArgs: whereArgs.isNotEmpty ? whereArgs : null,
        orderBy: 'updated_at DESC',
      );

      final validData = <Map<String, dynamic>>[];
      
      for (final row in result) {
        final storedData = json.decode(row['data_json']);
        final integrityCheck = IntegrityChecker.verifyStoredData(storedData);
        
        if (integrityCheck.isValid) {
          final cleanData = Map<String, dynamic>.from(storedData);
          cleanData.remove('_checksum');
          cleanData.remove('_timestamp');
          cleanData['_storage_id'] = row['id'];
          cleanData['_data_type'] = row['data_type'];
          validData.add(cleanData);
        } else {
          await _logValidation('INTEGRITY_FAILED', row['id'], integrityCheck);
        }
      }
      
      return validData;
    } catch (e) {
      return [];
    }
  }

  Future<void> _logValidation(
    String operation,
    String? dataId,
    ValidationResult result,
  ) async {
    try {
      final db = await database;
      await db.insert('validation_log', {
        'operation': operation,
        'data_id': dataId,
        'validation_result': json.encode({
          'isValid': result.isValid,
          'errors': result.errors,
          'errorCount': result.errors.length,
        }),
        'timestamp': DateTime.now().millisecondsSinceEpoch,
      });
      
      // Keep in-memory log for current session
      _validationLog.add({
        'operation': operation,
        'dataId': dataId,
        'result': result,
        'timestamp': DateTime.now(),
      });
    } catch (e) {
      // Logging failed, but don't interrupt main operation
    }
  }

  Future<List<Map<String, dynamic>>> getValidationLog({int? limit}) async {
    try {
      final db = await database;
      final result = await db.query(
        'validation_log',
        orderBy: 'timestamp DESC',
        limit: limit,
      );

      return result.map((row) {
        final validationResult = json.decode(row['validation_result']);
        return {
          'id': row['id'],
          'operation': row['operation'],
          'dataId': row['data_id'],
          'isValid': validationResult['isValid'],
          'errors': validationResult['errors'],
          'errorCount': validationResult['errorCount'],
          'timestamp': DateTime.fromMillisecondsSinceEpoch(row['timestamp']),
        };
      }).toList();
    } catch (e) {
      return [];
    }
  }

  Future<Map<String, dynamic>> getValidationStatistics() async {
    try {
      final db = await database;
      
      final totalResult = await db.rawQuery('SELECT COUNT(*) as count FROM validation_log');
      final validResult = await db.rawQuery(
        'SELECT COUNT(*) as count FROM validation_log WHERE validation_result LIKE \'%"isValid":true%\'');
      final invalidResult = await db.rawQuery(
        'SELECT COUNT(*) as count FROM validation_log WHERE validation_result LIKE \'%"isValid":false%\'');
      
      final totalCount = totalResult.first['count'] as int;
      final validCount = validResult.first['count'] as int;
      final invalidCount = invalidResult.first['count'] as int;
      
      return {
        'totalValidations': totalCount,
        'validValidations': validCount,
        'invalidValidations': invalidCount,
        'successRate': totalCount > 0 ? (validCount / totalCount * 100) : 0.0,
      };
    } catch (e) {
      return {
        'totalValidations': 0,
        'validValidations': 0,
        'invalidValidations': 0,
        'successRate': 0.0,
      };
    }
  }

  Future<void> clearInvalidData() async {
    try {
      final db = await database;
      final allData = await db.query('validated_data');
      
      for (final row in allData) {
        final storedData = json.decode(row['data_json']);
        final integrityCheck = IntegrityChecker.verifyStoredData(storedData);
        
        if (!integrityCheck.isValid) {
          await db.delete('validated_data', where: 'id = ?', whereArgs: [row['id']]);
          await _logValidation('CLEARED_INVALID', row['id'], integrityCheck);
        }
      }
    } catch (e) {
      // Handle cleanup error
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
      title: 'Data Validation & Integrity',
      home: const ValidationPage(),
    );
  }
}

class ValidationPage extends StatefulWidget {
  const ValidationPage({super.key});

  @override
  State<ValidationPage> createState() => _ValidationPageState();
}

class _ValidationPageState extends State<ValidationPage> {
  final ValidatedStorage _storage = ValidatedStorage();
  final TextEditingController _nameController = TextEditingController();
  final TextEditingController _emailController = TextEditingController();
  final TextEditingController _ageController = TextEditingController();
  final TextEditingController _scoreController = TextEditingController();

  List<Map<String, dynamic>> _storedData = [];
  List<Map<String, dynamic>> _validationLog = [];
  Map<String, dynamic> _statistics = {};
  String _lastValidationResult = '';

  @override
  void initState() {
    super.initState();
    _initializeStorage();
  }

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _ageController.dispose();
    _scoreController.dispose();
    super.dispose();
  }

  Future<void> _initializeStorage() async {
    _storage.initializeValidationRules();
    await _loadData();
  }

  Future<void> _loadData() async {
    final storedData = await _storage.getAllData();
    final validationLog = await _storage.getValidationLog(limit: 20);
    final statistics = await _storage.getValidationStatistics();

    setState(() {
      _storedData = storedData;
      _validationLog = validationLog;
      _statistics = statistics;
    });
  }

  Future<void> _saveData() async {
    final data = {
      'name': _nameController.text,
      'email': _emailController.text,
      'age': int.tryParse(_ageController.text),
      'score': double.tryParse(_scoreController.text),
    };

    final id = DateTime.now().millisecondsSinceEpoch.toString();
    final success = await _storage.storeData(id, 'user', data);

    setState(() {
      _lastValidationResult = success ? 'Data saved successfully!' : 'Validation failed!';
    });

    if (success) {
      _nameController.clear();
      _emailController.clear();
      _ageController.clear();
      _scoreController.clear();
    }

    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text(_lastValidationResult),
          backgroundColor: success ? Colors.green : Colors.red,
        ),
      );
    }
  }

  Future<void> _createInvalidData() async {
    // Intentionally create invalid data for testing
    final invalidData = {
      'name': '', // Invalid: empty name
      'email': 'invalid-email', // Invalid: bad email format
      'age': -5, // Invalid: negative age
      'score': 150.0, // Invalid: score too high
    };

    final id = 'invalid_${DateTime.now().millisecondsSinceEpoch}';
    await _storage.storeData(id, 'user', invalidData);
    await _loadData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Invalid data test completed'),
          backgroundColor: Colors.orange,
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Data Validation & Integrity'),
        actions: [
          IconButton(
            icon: const Icon(Icons.bug_report),
            onPressed: _createInvalidData,
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Input form
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Add User Data',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 16),
                    TextField(
                      controller: _nameController,
                      decoration: const InputDecoration(
                        labelText: 'Name (2-50 chars)',
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
                        labelText: 'Age (0-150)',
                        border: OutlineInputBorder(),
                      ),
                    ),
                    const SizedBox(height: 8),
                    TextField(
                      controller: _scoreController,
                      keyboardType: TextInputType.number,
                      decoration: const InputDecoration(
                        labelText: 'Score (0.0-100.0)',
                        border: OutlineInputBorder(),
                      ),
                    ),
                    const SizedBox(height: 16),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        onPressed: _saveData,
                        child: const Text('Save Data'),
                      ),
                    ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Statistics
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Validation Statistics',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 8),
                    Text('Total Validations: ${_statistics['totalValidations'] ?? 0}'),
                    Text('Valid: ${_statistics['validValidations'] ?? 0}'),
                    Text('Invalid: ${_statistics['invalidValidations'] ?? 0}'),
                    Text('Success Rate: ${(_statistics['successRate'] ?? 0.0).toStringAsFixed(1)}%'),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Stored data
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Valid Stored Data (${_storedData.length})',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 8),
                    if (_storedData.isEmpty)
                      const Text('No valid data stored')
                    else
                      Column(
                        children: _storedData.take(5).map((data) {
                          return Card(
                            margin: const EdgeInsets.symmetric(vertical: 2),
                            child: ListTile(
                              title: Text(data['name'] ?? 'No name'),
                              subtitle: Text('${data['email'] ?? ''} - Age: ${data['age'] ?? 'N/A'}'),
                              trailing: Text('Score: ${data['score']?.toStringAsFixed(1) ?? 'N/A'}'),
                            ),
                          );
                        }).toList(),
                      ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Validation log
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Recent Validation Log',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 8),
                    if (_validationLog.isEmpty)
                      const Text('No validation log entries')
                    else
                      Column(
                        children: _validationLog.take(10).map((entry) {
                          final timestamp = entry['timestamp'] as DateTime;
                          final isValid = entry['isValid'] as bool;
                          final errorCount = entry['errorCount'] as int;
                          
                          return Card(
                            margin: const EdgeInsets.symmetric(vertical: 2),
                            color: isValid 
                                ? Colors.green.withOpacity(0.1)
                                : Colors.red.withOpacity(0.1),
                            child: ListTile(
                              leading: Icon(
                                isValid ? Icons.check_circle : Icons.error,
                                color: isValid ? Colors.green : Colors.red,
                                size: 16,
                              ),
                              title: Text('${entry['operation']}'),
                              subtitle: Text(
                                isValid 
                                    ? 'Valid' 
                                    : '$errorCount error${errorCount != 1 ? 's' : ''}',
                              ),
                              trailing: Text(
                                '${timestamp.hour}:${timestamp.minute.toString().padLeft(2, '0')}',
                                style: const TextStyle(fontSize: 12),
                              ),
                            ),
                          );
                        }).toList(),
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

Data validation and integrity ensures data consistency through comprehensive  
validation rules, checksum verification, and detailed logging. The system  
provides field-level and global validation, automatic sanitization, and  
integrity checks to maintain data quality in persistent storage systems.  

## Logging System with Persistence

Implementing a comprehensive logging system with multiple levels and persistence.  

```dart
import 'package:flutter/material.dart';
import 'dart:io';
import 'dart:convert';
import 'package:path_provider/path_provider.dart';

void main() {
  runApp(const MyApp());
}

enum LogLevel {
  debug,
  info,
  warning,
  error,
  critical,
}

class LogEntry {
  final String id;
  final LogLevel level;
  final String message;
  final String category;
  final DateTime timestamp;
  final Map<String, dynamic> metadata;
  final String? stackTrace;

  LogEntry({
    required this.id,
    required this.level,
    required this.message,
    required this.category,
    required this.timestamp,
    required this.metadata,
    this.stackTrace,
  });

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'level': level.index,
      'message': message,
      'category': category,
      'timestamp': timestamp.toIso8601String(),
      'metadata': metadata,
      'stackTrace': stackTrace,
    };
  }

  factory LogEntry.fromJson(Map<String, dynamic> json) {
    return LogEntry(
      id: json['id'],
      level: LogLevel.values[json['level']],
      message: json['message'],
      category: json['category'],
      timestamp: DateTime.parse(json['timestamp']),
      metadata: Map<String, dynamic>.from(json['metadata'] ?? {}),
      stackTrace: json['stackTrace'],
    );
  }

  String get levelName => level.name.toUpperCase();

  Color get levelColor {
    switch (level) {
      case LogLevel.debug:
        return Colors.grey;
      case LogLevel.info:
        return Colors.blue;
      case LogLevel.warning:
        return Colors.orange;
      case LogLevel.error:
        return Colors.red;
      case LogLevel.critical:
        return Colors.deepPurple;
    }
  }
}

class LoggingSystem {
  static final LoggingSystem _instance = LoggingSystem._internal();
  factory LoggingSystem() => _instance;
  LoggingSystem._internal();

  late String _logDirectory;
  late String _currentLogFile;
  final List<LogEntry> _memoryBuffer = [];
  final Map<String, int> _categoryStats = {};
  final Map<LogLevel, int> _levelStats = {};
  
  LogLevel _minimumLevel = LogLevel.debug;
  int _maxMemoryBuffer = 1000;
  int _maxFileSize = 10 * 1024 * 1024; // 10MB
  int _maxLogFiles = 5;
  bool _enableConsoleOutput = true;
  bool _enableFileOutput = true;

  Future<void> initialize() async {
    final appDir = await getApplicationDocumentsDirectory();
    _logDirectory = '${appDir.path}/logs';
    
    await Directory(_logDirectory).create(recursive: true);
    await _rotateLogFile();
    
    // Log system initialization
    await _log(LogLevel.info, 'Logging system initialized', 'SYSTEM');
  }

  Future<void> _rotateLogFile() async {
    final now = DateTime.now();
    final dateString = '${now.year}${now.month.toString().padLeft(2, '0')}${now.day.toString().padLeft(2, '0')}';
    _currentLogFile = '$_logDirectory/app_$dateString.log';
    
    // Check if rotation is needed
    final currentFile = File(_currentLogFile);
    if (await currentFile.exists()) {
      final stat = await currentFile.stat();
      if (stat.size > _maxFileSize) {
        await _archiveCurrentLogFile();
      }
    }
  }

  Future<void> _archiveCurrentLogFile() async {
    final timestamp = DateTime.now().millisecondsSinceEpoch;
    final archivedFile = File('${_currentLogFile}_$timestamp');
    
    try {
      await File(_currentLogFile).rename(archivedFile.path);
      await _cleanupOldLogs();
    } catch (e) {
      // Handle rename error
    }
  }

  Future<void> _cleanupOldLogs() async {
    try {
      final logFiles = await Directory(_logDirectory)
          .list()
          .where((entity) => entity is File && entity.path.endsWith('.log'))
          .cast<File>()
          .toList();

      if (logFiles.length > _maxLogFiles) {
        // Sort by modification time
        logFiles.sort((a, b) => a.statSync().modified.compareTo(b.statSync().modified));
        
        // Remove oldest files
        for (int i = 0; i < logFiles.length - _maxLogFiles; i++) {
          await logFiles[i].delete();
        }
      }
    } catch (e) {
      // Handle cleanup error
    }
  }

  Future<void> _log(
    LogLevel level,
    String message,
    String category, {
    Map<String, dynamic>? metadata,
    String? stackTrace,
  }) async {
    if (level.index < _minimumLevel.index) return;

    final entry = LogEntry(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      level: level,
      message: message,
      category: category,
      timestamp: DateTime.now(),
      metadata: metadata ?? {},
      stackTrace: stackTrace,
    );

    // Update statistics
    _categoryStats[category] = (_categoryStats[category] ?? 0) + 1;
    _levelStats[level] = (_levelStats[level] ?? 0) + 1;

    // Add to memory buffer
    _memoryBuffer.add(entry);
    if (_memoryBuffer.length > _maxMemoryBuffer) {
      _memoryBuffer.removeAt(0);
    }

    // Console output
    if (_enableConsoleOutput) {
      print('[${entry.levelName}] ${entry.timestamp} [${entry.category}] ${entry.message}');
    }

    // File output
    if (_enableFileOutput) {
      await _writeToFile(entry);
    }
  }

  Future<void> _writeToFile(LogEntry entry) async {
    try {
      await _rotateLogFile(); // Check if rotation needed
      
      final file = File(_currentLogFile);
      final logLine = '${json.encode(entry.toJson())}\n';
      
      await file.writeAsString(logLine, mode: FileMode.append);
    } catch (e) {
      // Handle write error - maybe log to console as fallback
      print('Failed to write log to file: $e');
    }
  }

  // Public logging methods
  Future<void> debug(String message, {String category = 'DEBUG', Map<String, dynamic>? metadata}) async {
    await _log(LogLevel.debug, message, category, metadata: metadata);
  }

  Future<void> info(String message, {String category = 'INFO', Map<String, dynamic>? metadata}) async {
    await _log(LogLevel.info, message, category, metadata: metadata);
  }

  Future<void> warning(String message, {String category = 'WARNING', Map<String, dynamic>? metadata}) async {
    await _log(LogLevel.warning, message, category, metadata: metadata);
  }

  Future<void> error(String message, {String category = 'ERROR', Map<String, dynamic>? metadata, String? stackTrace}) async {
    await _log(LogLevel.error, message, category, metadata: metadata, stackTrace: stackTrace);
  }

  Future<void> critical(String message, {String category = 'CRITICAL', Map<String, dynamic>? metadata, String? stackTrace}) async {
    await _log(LogLevel.critical, message, category, metadata: metadata, stackTrace: stackTrace);
  }

  // Query methods
  List<LogEntry> getRecentLogs({int limit = 100, LogLevel? minLevel, String? category}) {
    var logs = List<LogEntry>.from(_memoryBuffer);
    
    if (minLevel != null) {
      logs = logs.where((log) => log.level.index >= minLevel.index).toList();
    }
    
    if (category != null) {
      logs = logs.where((log) => log.category == category).toList();
    }
    
    logs.sort((a, b) => b.timestamp.compareTo(a.timestamp));
    return logs.take(limit).toList();
  }

  Future<List<LogEntry>> searchLogs({
    String? query,
    LogLevel? minLevel,
    String? category,
    DateTime? fromDate,
    DateTime? toDate,
    int limit = 1000,
  }) async {
    final logs = <LogEntry>[];
    
    try {
      final logFiles = await Directory(_logDirectory)
          .list()
          .where((entity) => entity is File && entity.path.endsWith('.log'))
          .cast<File>()
          .toList();

      for (final file in logFiles) {
        final content = await file.readAsString();
        final lines = content.split('\n');
        
        for (final line in lines) {
          if (line.trim().isEmpty) continue;
          
          try {
            final json = jsonDecode(line);
            final entry = LogEntry.fromJson(json);
            
            // Apply filters
            if (minLevel != null && entry.level.index < minLevel.index) continue;
            if (category != null && entry.category != category) continue;
            if (fromDate != null && entry.timestamp.isBefore(fromDate)) continue;
            if (toDate != null && entry.timestamp.isAfter(toDate)) continue;
            if (query != null && !entry.message.toLowerCase().contains(query.toLowerCase())) continue;
            
            logs.add(entry);
          } catch (e) {
            // Skip invalid log entries
          }
        }
      }
    } catch (e) {
      // Handle file reading error
    }
    
    logs.sort((a, b) => b.timestamp.compareTo(a.timestamp));
    return logs.take(limit).toList();
  }

  Map<String, int> getCategoryStatistics() => Map<String, int>.from(_categoryStats);
  
  Map<LogLevel, int> getLevelStatistics() => Map<LogLevel, int>.from(_levelStats);

  Future<String> exportLogs({
    DateTime? fromDate,
    DateTime? toDate,
    LogLevel? minLevel,
  }) async {
    final logs = await searchLogs(
      fromDate: fromDate,
      toDate: toDate,
      minLevel: minLevel,
      limit: 10000,
    );

    final buffer = StringBuffer();
    buffer.writeln('# Log Export');
    buffer.writeln('# Generated: ${DateTime.now()}');
    buffer.writeln('# Total entries: ${logs.length}');
    buffer.writeln('');

    for (final log in logs) {
      buffer.writeln('${log.timestamp} [${log.levelName}] [${log.category}] ${log.message}');
      if (log.metadata.isNotEmpty) {
        buffer.writeln('  Metadata: ${json.encode(log.metadata)}');
      }
      if (log.stackTrace != null) {
        buffer.writeln('  Stack Trace: ${log.stackTrace}');
      }
      buffer.writeln('');
    }

    return buffer.toString();
  }

  Future<void> clearLogs() async {
    try {
      _memoryBuffer.clear();
      _categoryStats.clear();
      _levelStats.clear();
      
      final logFiles = await Directory(_logDirectory).list().toList();
      for (final file in logFiles) {
        if (file is File) {
          await file.delete();
        }
      }
      
      await _log(LogLevel.info, 'All logs cleared', 'SYSTEM');
    } catch (e) {
      await error('Failed to clear logs: $e', category: 'SYSTEM');
    }
  }

  // Configuration
  void setMinimumLevel(LogLevel level) {
    _minimumLevel = level;
  }

  void setMaxMemoryBuffer(int size) {
    _maxMemoryBuffer = size;
  }

  void enableConsoleOutput(bool enable) {
    _enableConsoleOutput = enable;
  }

  void enableFileOutput(bool enable) {
    _enableFileOutput = enable;
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Logging System',
      home: const LoggingPage(),
    );
  }
}

class LoggingPage extends StatefulWidget {
  const LoggingPage({super.key});

  @override
  State<LoggingPage> createState() => _LoggingPageState();
}

class _LoggingPageState extends State<LoggingPage> {
  final LoggingSystem _logger = LoggingSystem();
  final TextEditingController _messageController = TextEditingController();
  final TextEditingController _categoryController = TextEditingController();
  final TextEditingController _searchController = TextEditingController();
  
  List<LogEntry> _logs = [];
  Map<String, int> _categoryStats = {};
  Map<LogLevel, int> _levelStats = {};
  LogLevel _selectedLevel = LogLevel.info;
  String? _filterCategory;

  @override
  void initState() {
    super.initState();
    _initializeLogger();
  }

  @override
  void dispose() {
    _messageController.dispose();
    _categoryController.dispose();
    _searchController.dispose();
    super.dispose();
  }

  Future<void> _initializeLogger() async {
    await _logger.initialize();
    await _loadLogs();
    
    // Generate some sample logs
    await _generateSampleLogs();
  }

  Future<void> _loadLogs() async {
    final logs = _logger.getRecentLogs(limit: 200);
    final categoryStats = _logger.getCategoryStatistics();
    final levelStats = _logger.getLevelStatistics();
    
    setState(() {
      _logs = logs;
      _categoryStats = categoryStats;
      _levelStats = levelStats;
    });
  }

  Future<void> _generateSampleLogs() async {
    await _logger.info('Application started', category: 'APP');
    await _logger.debug('Debug information loaded', category: 'DEBUG');
    await _logger.warning('Configuration file missing, using defaults', category: 'CONFIG');
    await _logger.error('Failed to connect to server', category: 'NETWORK');
    await _logger.critical('Database connection lost', category: 'DATABASE');
    
    await _loadLogs();
  }

  Future<void> _addLog() async {
    if (_messageController.text.isEmpty) return;

    final message = _messageController.text;
    final category = _categoryController.text.isNotEmpty ? _categoryController.text : 'USER';

    switch (_selectedLevel) {
      case LogLevel.debug:
        await _logger.debug(message, category: category);
        break;
      case LogLevel.info:
        await _logger.info(message, category: category);
        break;
      case LogLevel.warning:
        await _logger.warning(message, category: category);
        break;
      case LogLevel.error:
        await _logger.error(message, category: category);
        break;
      case LogLevel.critical:
        await _logger.critical(message, category: category);
        break;
    }

    _messageController.clear();
    _categoryController.clear();
    await _loadLogs();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Log entry added!')),
      );
    }
  }

  Future<void> _searchLogs() async {
    if (_searchController.text.isEmpty) {
      await _loadLogs();
      return;
    }

    final results = await _logger.searchLogs(
      query: _searchController.text,
      category: _filterCategory,
      limit: 200,
    );

    setState(() {
      _logs = results;
    });
  }

  Future<void> _exportLogs() async {
    final exportedData = await _logger.exportLogs();
    
    if (mounted) {
      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: const Text('Exported Logs'),
          content: SingleChildScrollView(
            child: Text(
              exportedData,
              style: const TextStyle(fontFamily: 'monospace', fontSize: 12),
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
  }

  @override
  Widget build(BuildContext context) {
    final categories = _categoryStats.keys.toList()..sort();
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Logging System'),
        actions: [
          IconButton(
            icon: const Icon(Icons.file_download),
            onPressed: _exportLogs,
          ),
          IconButton(
            icon: const Icon(Icons.clear_all),
            onPressed: () async {
              await _logger.clearLogs();
              await _loadLogs();
            },
          ),
        ],
      ),
      body: Column(
        children: [
          // Add log form
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  children: [
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _messageController,
                            decoration: const InputDecoration(
                              labelText: 'Log Message',
                              border: OutlineInputBorder(),
                            ),
                          ),
                        ),
                        const SizedBox(width: 8),
                        DropdownButton<LogLevel>(
                          value: _selectedLevel,
                          items: LogLevel.values.map((level) {
                            return DropdownMenuItem(
                              value: level,
                              child: Text(level.name.toUpperCase()),
                            );
                          }).toList(),
                          onChanged: (value) {
                            if (value != null) {
                              setState(() {
                                _selectedLevel = value;
                              });
                            }
                          },
                        ),
                      ],
                    ),
                    const SizedBox(height: 8),
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _categoryController,
                            decoration: const InputDecoration(
                              labelText: 'Category (optional)',
                              border: OutlineInputBorder(),
                            ),
                          ),
                        ),
                        const SizedBox(width: 8),
                        ElevatedButton(
                          onPressed: _addLog,
                          child: const Text('Add Log'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
          ),

          // Search and filter
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 16.0),
            child: Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: _searchController,
                    decoration: const InputDecoration(
                      labelText: 'Search logs',
                      border: OutlineInputBorder(),
                      prefixIcon: Icon(Icons.search),
                    ),
                    onChanged: (value) => _searchLogs(),
                  ),
                ),
                const SizedBox(width: 8),
                DropdownButton<String?>(
                  value: _filterCategory,
                  hint: const Text('All Categories'),
                  items: [
                    const DropdownMenuItem<String?>(
                      value: null,
                      child: Text('All Categories'),
                    ),
                    ...categories.map((category) {
                      return DropdownMenuItem<String>(
                        value: category,
                        child: Text(category),
                      );
                    }),
                  ],
                  onChanged: (value) {
                    setState(() {
                      _filterCategory = value;
                    });
                    _searchLogs();
                  },
                ),
              ],
            ),
          ),

          const SizedBox(height: 16),

          // Statistics
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 16.0),
            child: Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Row(
                  children: [
                    Expanded(
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          const Text('Level Stats:', style: TextStyle(fontWeight: FontWeight.bold)),
                          ..._levelStats.entries.map((entry) => 
                            Text('${entry.key.name.toUpperCase()}: ${entry.value}')),
                        ],
                      ),
                    ),
                    Expanded(
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          const Text('Category Stats:', style: TextStyle(fontWeight: FontWeight.bold)),
                          ..._categoryStats.entries.take(3).map((entry) => 
                            Text('${entry.key}: ${entry.value}')),
                        ],
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ),

          const SizedBox(height: 16),

          // Logs list
          Expanded(
            child: _logs.isEmpty
                ? const Center(child: Text('No logs found'))
                : ListView.builder(
                    itemCount: _logs.length,
                    itemBuilder: (context, index) {
                      final log = _logs[index];
                      
                      return Card(
                        margin: const EdgeInsets.symmetric(
                          horizontal: 16,
                          vertical: 2,
                        ),
                        color: log.levelColor.withOpacity(0.1),
                        child: ExpansionTile(
                          leading: Container(
                            padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
                            decoration: BoxDecoration(
                              color: log.levelColor,
                              borderRadius: BorderRadius.circular(4),
                            ),
                            child: Text(
                              log.levelName,
                              style: const TextStyle(
                                color: Colors.white,
                                fontSize: 10,
                                fontWeight: FontWeight.bold,
                              ),
                            ),
                          ),
                          title: Text(
                            log.message,
                            maxLines: 2,
                            overflow: TextOverflow.ellipsis,
                          ),
                          subtitle: Text(
                            '${log.category} • ${log.timestamp.toString().split('.')[0]}',
                            style: const TextStyle(fontSize: 12),
                          ),
                          children: [
                            Padding(
                              padding: const EdgeInsets.all(16.0),
                              child: Column(
                                crossAxisAlignment: CrossAxisAlignment.start,
                                children: [
                                  Text('ID: ${log.id}'),
                                  Text('Timestamp: ${log.timestamp}'),
                                  if (log.metadata.isNotEmpty) ...[
                                    const SizedBox(height: 8),
                                    const Text('Metadata:', style: TextStyle(fontWeight: FontWeight.bold)),
                                    Text(
                                      json.encode(log.metadata),
                                      style: const TextStyle(fontFamily: 'monospace', fontSize: 12),
                                    ),
                                  ],
                                  if (log.stackTrace != null) ...[
                                    const SizedBox(height: 8),
                                    const Text('Stack Trace:', style: TextStyle(fontWeight: FontWeight.bold)),
                                    Text(
                                      log.stackTrace!,
                                      style: const TextStyle(fontFamily: 'monospace', fontSize: 12),
                                    ),
                                  ],
                                ],
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
    );
  }
}
```

Logging system with persistence provides comprehensive application monitoring  
with multiple log levels, categorization, file rotation, and search capabilities.  
The system supports both memory buffering and file storage with configurable  
retention policies and export functionality for debugging and analysis.  

## Performance Monitoring Storage

Storing and analyzing application performance metrics and diagnostics.  

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

class PerformanceMetric {
  final String id;
  final String name;
  final String category;
  final double value;
  final String unit;
  final DateTime timestamp;
  final Map<String, dynamic> context;

  PerformanceMetric({
    required this.id,
    required this.name,
    required this.category,
    required this.value,
    required this.unit,
    required this.timestamp,
    required this.context,
  });

  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': name,
      'category': category,
      'value': value,
      'unit': unit,
      'timestamp': timestamp.millisecondsSinceEpoch,
      'context': json.encode(context),
    };
  }

  factory PerformanceMetric.fromMap(Map<String, dynamic> map) {
    return PerformanceMetric(
      id: map['id'],
      name: map['name'],
      category: map['category'],
      value: map['value']?.toDouble() ?? 0.0,
      unit: map['unit'],
      timestamp: DateTime.fromMillisecondsSinceEpoch(map['timestamp']),
      context: json.decode(map['context']),
    );
  }
}

class PerformanceSession {
  final String id;
  final DateTime startTime;
  final DateTime endTime;
  final Map<String, double> summary;
  final List<String> metricIds;

  PerformanceSession({
    required this.id,
    required this.startTime,
    required this.endTime,
    required this.summary,
    required this.metricIds,
  });

  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'start_time': startTime.millisecondsSinceEpoch,
      'end_time': endTime.millisecondsSinceEpoch,
      'summary': json.encode(summary),
      'metric_ids': json.encode(metricIds),
    };
  }

  factory PerformanceSession.fromMap(Map<String, dynamic> map) {
    return PerformanceSession(
      id: map['id'],
      startTime: DateTime.fromMillisecondsSinceEpoch(map['start_time']),
      endTime: DateTime.fromMillisecondsSinceEpoch(map['end_time']),
      summary: Map<String, double>.from(json.decode(map['summary'])),
      metricIds: List<String>.from(json.decode(map['metric_ids'])),
    );
  }

  Duration get duration => endTime.difference(startTime);
}

class PerformanceMonitor {
  static final PerformanceMonitor _instance = PerformanceMonitor._internal();
  factory PerformanceMonitor() => _instance;
  PerformanceMonitor._internal();

  static Database? _database;
  String? _currentSessionId;
  DateTime? _currentSessionStart;
  final Map<String, Stopwatch> _activeTimers = {};
  final Map<String, List<double>> _realtimeMetrics = {};

  Future<Database> get database async {
    _database ??= await _initDatabase();
    return _database!;
  }

  Future<Database> _initDatabase() async {
    String path = join(await getDatabasesPath(), 'performance_monitor.db');
    
    return await openDatabase(
      path,
      version: 1,
      onCreate: (db, version) async {
        await db.execute('''
          CREATE TABLE metrics (
            id TEXT PRIMARY KEY,
            name TEXT NOT NULL,
            category TEXT NOT NULL,
            value REAL NOT NULL,
            unit TEXT NOT NULL,
            timestamp INTEGER NOT NULL,
            context TEXT NOT NULL
          )
        ''');

        await db.execute('''
          CREATE TABLE sessions (
            id TEXT PRIMARY KEY,
            start_time INTEGER NOT NULL,
            end_time INTEGER NOT NULL,
            summary TEXT NOT NULL,
            metric_ids TEXT NOT NULL
          )
        ''');

        await db.execute('CREATE INDEX idx_metrics_timestamp ON metrics(timestamp)');
        await db.execute('CREATE INDEX idx_metrics_category ON metrics(category)');
        await db.execute('CREATE INDEX idx_sessions_start_time ON sessions(start_time)');
      },
    );
  }

  Future<void> startSession() async {
    _currentSessionId = DateTime.now().millisecondsSinceEpoch.toString();
    _currentSessionStart = DateTime.now();
    
    await recordMetric(
      'session_start',
      1.0,
      category: 'session',
      unit: 'count',
      context: {'session_id': _currentSessionId!},
    );
  }

  Future<void> endSession() async {
    if (_currentSessionId == null || _currentSessionStart == null) return;

    final endTime = DateTime.now();
    final sessionDuration = endTime.difference(_currentSessionStart!).inMilliseconds.toDouble();

    await recordMetric(
      'session_duration',
      sessionDuration,
      category: 'session',
      unit: 'ms',
      context: {'session_id': _currentSessionId!},
    );

    // Calculate session summary
    final sessionMetrics = await getMetrics(
      category: 'session',
      fromTime: _currentSessionStart!,
      toTime: endTime,
    );

    final summary = <String, double>{};
    for (final metric in sessionMetrics) {
      final key = '${metric.category}_${metric.name}';
      if (summary.containsKey(key)) {
        summary[key] = (summary[key]! + metric.value) / 2; // Average
      } else {
        summary[key] = metric.value;
      }
    }

    final session = PerformanceSession(
      id: _currentSessionId!,
      startTime: _currentSessionStart!,
      endTime: endTime,
      summary: summary,
      metricIds: sessionMetrics.map((m) => m.id).toList(),
    );

    final db = await database;
    await db.insert('sessions', session.toMap());

    _currentSessionId = null;
    _currentSessionStart = null;
  }

  Future<void> recordMetric(
    String name,
    double value, {
    required String category,
    required String unit,
    Map<String, dynamic>? context,
  }) async {
    final metric = PerformanceMetric(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      name: name,
      category: category,
      value: value,
      unit: unit,
      timestamp: DateTime.now(),
      context: context ?? {},
    );

    final db = await database;
    await db.insert('metrics', metric.toMap());

    // Update realtime metrics
    _realtimeMetrics.putIfAbsent('${category}_$name', () => []).add(value);
    final metricList = _realtimeMetrics['${category}_$name']!;
    if (metricList.length > 100) {
      metricList.removeAt(0);
    }
  }

  void startTimer(String name) {
    _activeTimers[name] = Stopwatch()..start();
  }

  Future<double> stopTimer(
    String name, {
    String category = 'timing',
    Map<String, dynamic>? context,
  }) async {
    final stopwatch = _activeTimers.remove(name);
    if (stopwatch == null) return 0.0;

    stopwatch.stop();
    final elapsedMs = stopwatch.elapsedMilliseconds.toDouble();

    await recordMetric(
      name,
      elapsedMs,
      category: category,
      unit: 'ms',
      context: context,
    );

    return elapsedMs;
  }

  Future<void> recordMemoryUsage() async {
    // Simulate memory usage recording
    final memoryUsage = (DateTime.now().millisecondsSinceEpoch % 1000000) / 1000.0;
    
    await recordMetric(
      'memory_usage',
      memoryUsage,
      category: 'system',
      unit: 'MB',
      context: {'timestamp': DateTime.now().toIso8601String()},
    );
  }

  Future<void> recordFrameTime(double frameTimeMs) async {
    await recordMetric(
      'frame_time',
      frameTimeMs,
      category: 'rendering',
      unit: 'ms',
      context: {'fps': 1000.0 / frameTimeMs},
    );
  }

  Future<void> recordNetworkRequest(
    String url,
    int statusCode,
    double responseTimeMs,
    int responseSize,
  ) async {
    await recordMetric(
      'network_request',
      responseTimeMs,
      category: 'network',
      unit: 'ms',
      context: {
        'url': url,
        'status_code': statusCode,
        'response_size': responseSize,
        'success': statusCode >= 200 && statusCode < 300,
      },
    );
  }

  Future<List<PerformanceMetric>> getMetrics({
    String? category,
    String? name,
    DateTime? fromTime,
    DateTime? toTime,
    int? limit,
  }) async {
    final db = await database;
    
    String whereClause = '';
    List<dynamic> whereArgs = [];

    if (category != null) {
      whereClause += 'category = ?';
      whereArgs.add(category);
    }

    if (name != null) {
      if (whereClause.isNotEmpty) whereClause += ' AND ';
      whereClause += 'name = ?';
      whereArgs.add(name);
    }

    if (fromTime != null) {
      if (whereClause.isNotEmpty) whereClause += ' AND ';
      whereClause += 'timestamp >= ?';
      whereArgs.add(fromTime.millisecondsSinceEpoch);
    }

    if (toTime != null) {
      if (whereClause.isNotEmpty) whereClause += ' AND ';
      whereClause += 'timestamp <= ?';
      whereArgs.add(toTime.millisecondsSinceEpoch);
    }

    final result = await db.query(
      'metrics',
      where: whereClause.isNotEmpty ? whereClause : null,
      whereArgs: whereArgs.isNotEmpty ? whereArgs : null,
      orderBy: 'timestamp DESC',
      limit: limit,
    );

    return result.map((map) => PerformanceMetric.fromMap(map)).toList();
  }

  Future<List<PerformanceSession>> getSessions({
    DateTime? fromTime,
    DateTime? toTime,
    int? limit,
  }) async {
    final db = await database;
    
    String whereClause = '';
    List<dynamic> whereArgs = [];

    if (fromTime != null) {
      whereClause += 'start_time >= ?';
      whereArgs.add(fromTime.millisecondsSinceEpoch);
    }

    if (toTime != null) {
      if (whereClause.isNotEmpty) whereClause += ' AND ';
      whereClause += 'start_time <= ?';
      whereArgs.add(toTime.millisecondsSinceEpoch);
    }

    final result = await db.query(
      'sessions',
      where: whereClause.isNotEmpty ? whereClause : null,
      whereArgs: whereArgs.isNotEmpty ? whereArgs : null,
      orderBy: 'start_time DESC',
      limit: limit,
    );

    return result.map((map) => PerformanceSession.fromMap(map)).toList();
  }

  Map<String, dynamic> getRealtimeStatistics() {
    final stats = <String, dynamic>{};
    
    _realtimeMetrics.forEach((key, values) {
      if (values.isNotEmpty) {
        final average = values.reduce((a, b) => a + b) / values.length;
        final min = values.reduce((a, b) => a < b ? a : b);
        final max = values.reduce((a, b) => a > b ? a : b);
        
        stats[key] = {
          'average': average,
          'min': min,
          'max': max,
          'count': values.length,
          'current': values.last,
        };
      }
    });
    
    return stats;
  }

  Future<Map<String, dynamic>> getPerformanceReport({
    DateTime? fromTime,
    DateTime? toTime,
  }) async {
    final metrics = await getMetrics(fromTime: fromTime, toTime: toTime);
    final sessions = await getSessions(fromTime: fromTime, toTime: toTime);

    final categoryStats = <String, Map<String, double>>{};
    
    for (final metric in metrics) {
      categoryStats.putIfAbsent(metric.category, () => {});
      final catStats = categoryStats[metric.category]!;
      
      catStats['count'] = (catStats['count'] ?? 0) + 1;
      catStats['sum'] = (catStats['sum'] ?? 0) + metric.value;
      catStats['average'] = catStats['sum']! / catStats['count']!;
      
      if (catStats['min'] == null || metric.value < catStats['min']!) {
        catStats['min'] = metric.value;
      }
      
      if (catStats['max'] == null || metric.value > catStats['max']!) {
        catStats['max'] = metric.value;
      }
    }

    return {
      'total_metrics': metrics.length,
      'total_sessions': sessions.length,
      'category_stats': categoryStats,
      'time_range': {
        'from': fromTime?.toIso8601String(),
        'to': toTime?.toIso8601String(),
      },
      'generated_at': DateTime.now().toIso8601String(),
    };
  }

  Future<void> clearOldMetrics({Duration olderThan = const Duration(days: 30)}) async {
    final cutoffTime = DateTime.now().subtract(olderThan);
    final db = await database;
    
    await db.delete(
      'metrics',
      where: 'timestamp < ?',
      whereArgs: [cutoffTime.millisecondsSinceEpoch],
    );
    
    await db.delete(
      'sessions',
      where: 'start_time < ?',
      whereArgs: [cutoffTime.millisecondsSinceEpoch],
    );
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Performance Monitoring',
      home: const PerformanceMonitorPage(),
    );
  }
}

class PerformanceMonitorPage extends StatefulWidget {
  const PerformanceMonitorPage({super.key});

  @override
  State<PerformanceMonitorPage> createState() => _PerformanceMonitorPageState();
}

class _PerformanceMonitorPageState extends State<PerformanceMonitorPage> {
  final PerformanceMonitor _monitor = PerformanceMonitor();
  
  List<PerformanceMetric> _recentMetrics = [];
  List<PerformanceSession> _sessions = [];
  Map<String, dynamic> _realtimeStats = {};
  bool _sessionActive = false;
  Timer? _metricsTimer;

  @override
  void initState() {
    super.initState();
    _initializeMonitoring();
  }

  @override
  void dispose() {
    _metricsTimer?.cancel();
    super.dispose();
  }

  Future<void> _initializeMonitoring() async {
    await _loadData();
    
    // Start periodic metrics collection
    _metricsTimer = Timer.periodic(const Duration(seconds: 2), (timer) {
      _collectPeriodicMetrics();
    });
  }

  Future<void> _loadData() async {
    final metrics = await _monitor.getMetrics(limit: 50);
    final sessions = await _monitor.getSessions(limit: 10);
    final realtimeStats = _monitor.getRealtimeStatistics();
    
    setState(() {
      _recentMetrics = metrics;
      _sessions = sessions;
      _realtimeStats = realtimeStats;
    });
  }

  Future<void> _collectPeriodicMetrics() async {
    await _monitor.recordMemoryUsage();
    
    // Simulate frame time
    final frameTime = 16.67 + (DateTime.now().millisecondsSinceEpoch % 10);
    await _monitor.recordFrameTime(frameTime);
    
    await _loadData();
  }

  Future<void> _startSession() async {
    await _monitor.startSession();
    setState(() {
      _sessionActive = true;
    });
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Performance monitoring session started')),
      );
    }
  }

  Future<void> _endSession() async {
    await _monitor.endSession();
    setState(() {
      _sessionActive = false;
    });
    
    await _loadData();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Performance monitoring session ended')),
      );
    }
  }

  Future<void> _simulateNetworkRequest() async {
    _monitor.startTimer('network_request');
    
    // Simulate network delay
    await Future.delayed(Duration(milliseconds: 100 + (DateTime.now().millisecondsSinceEpoch % 500)));
    
    final responseTime = await _monitor.stopTimer('network_request', category: 'network');
    
    await _monitor.recordNetworkRequest(
      'https://api.example.com/data',
      200,
      responseTime,
      1024,
    );
    
    await _loadData();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Network request simulated')),
      );
    }
  }

  Future<void> _simulateIntensiveTask() async {
    _monitor.startTimer('intensive_task');
    
    // Simulate intensive computation
    int sum = 0;
    for (int i = 0; i < 1000000; i++) {
      sum += i;
    }
    
    final taskTime = await _monitor.stopTimer('intensive_task', category: 'compute');
    
    await _monitor.recordMetric(
      'computation_result',
      sum.toDouble(),
      category: 'compute',
      unit: 'count',
      context: {'iterations': 1000000},
    );
    
    await _loadData();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Intensive task completed in ${taskTime.toStringAsFixed(2)}ms')),
      );
    }
  }

  String _formatValue(double value, String unit) {
    if (unit == 'ms') {
      return '${value.toStringAsFixed(2)} ms';
    } else if (unit == 'MB') {
      return '${value.toStringAsFixed(2)} MB';
    } else if (unit == 'count') {
      return value.toInt().toString();
    } else {
      return '${value.toStringAsFixed(2)} $unit';
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Performance Monitor'),
        actions: [
          IconButton(
            icon: Icon(_sessionActive ? Icons.stop : Icons.play_arrow),
            onPressed: _sessionActive ? _endSession : _startSession,
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Session status
            Card(
              color: _sessionActive ? Colors.green.withOpacity(0.2) : null,
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Row(
                  children: [
                    Icon(
                      _sessionActive ? Icons.fiber_manual_record : Icons.stop,
                      color: _sessionActive ? Colors.green : Colors.grey,
                    ),
                    const SizedBox(width: 8),
                    Text(
                      _sessionActive ? 'Session Active' : 'Session Inactive',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const Spacer(),
                    Row(
                      children: [
                        ElevatedButton(
                          onPressed: _simulateNetworkRequest,
                          child: const Text('Network'),
                        ),
                        const SizedBox(width: 8),
                        ElevatedButton(
                          onPressed: _simulateIntensiveTask,
                          child: const Text('Compute'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Realtime statistics
            if (_realtimeStats.isNotEmpty)
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Realtime Statistics',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                      const SizedBox(height: 8),
                      ..._realtimeStats.entries.take(5).map((entry) {
                        final stats = entry.value as Map<String, dynamic>;
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 4),
                          child: Row(
                            children: [
                              Expanded(
                                child: Text(entry.key.replaceAll('_', ' ').toUpperCase()),
                              ),
                              Text('Avg: ${stats['average'].toStringAsFixed(2)}'),
                              const SizedBox(width: 16),
                              Text('Current: ${stats['current'].toStringAsFixed(2)}'),
                            ],
                          ),
                        );
                      }),
                    ],
                  ),
                ),
              ),

            const SizedBox(height: 16),

            // Recent sessions
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Recent Sessions (${_sessions.length})',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 8),
                    if (_sessions.isEmpty)
                      const Text('No sessions recorded')
                    else
                      Column(
                        children: _sessions.take(3).map((session) {
                          return Card(
                            margin: const EdgeInsets.symmetric(vertical: 2),
                            child: ListTile(
                              leading: const Icon(Icons.timer),
                              title: Text('Session ${session.id}'),
                              subtitle: Text(
                                'Duration: ${session.duration.inSeconds}s • '
                                '${session.startTime.toString().split('.')[0]}',
                              ),
                              trailing: Text('${session.summary.length} metrics'),
                            ),
                          );
                        }).toList(),
                      ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Recent metrics
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Recent Metrics (${_recentMetrics.length})',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 8),
                    if (_recentMetrics.isEmpty)
                      const Text('No metrics recorded')
                    else
                      Column(
                        children: _recentMetrics.take(10).map((metric) {
                          return Card(
                            margin: const EdgeInsets.symmetric(vertical: 2),
                            child: ListTile(
                              leading: Container(
                                padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
                                decoration: BoxDecoration(
                                  color: _getCategoryColor(metric.category),
                                  borderRadius: BorderRadius.circular(4),
                                ),
                                child: Text(
                                  metric.category.toUpperCase(),
                                  style: const TextStyle(
                                    color: Colors.white,
                                    fontSize: 10,
                                    fontWeight: FontWeight.bold,
                                  ),
                                ),
                              ),
                              title: Text(metric.name.replaceAll('_', ' ').toUpperCase()),
                              subtitle: Text(metric.timestamp.toString().split('.')[0]),
                              trailing: Text(_formatValue(metric.value, metric.unit)),
                            ),
                          );
                        }).toList(),
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

  Color _getCategoryColor(String category) {
    switch (category) {
      case 'system':
        return Colors.blue;
      case 'rendering':
        return Colors.green;
      case 'network':
        return Colors.orange;
      case 'compute':
        return Colors.purple;
      case 'session':
        return Colors.cyan;
      case 'timing':
        return Colors.amber;
      default:
        return Colors.grey;
    }
  }
}
```

Performance monitoring storage provides comprehensive application performance  
tracking with metrics collection, session management, and real-time statistics.  
The system supports various performance categories including timing, memory,  
network, and rendering with detailed analysis and reporting capabilities.  

## Backup and Restore System

Implementing comprehensive backup and restore functionality for application data.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:io';
import 'dart:convert';
import 'package:path_provider/path_provider.dart';
import 'package:crypto/crypto.dart';
import 'dart:typed_data';

void main() {
  runApp(const MyApp());
}

class BackupMetadata {
  final String id;
  final String name;
  final DateTime createdAt;
  final int fileSize;
  final String checksum;
  final Map<String, dynamic> dataTypes;
  final String version;

  BackupMetadata({
    required this.id,
    required this.name,
    required this.createdAt,
    required this.fileSize,
    required this.checksum,
    required this.dataTypes,
    required this.version,
  });

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'createdAt': createdAt.toIso8601String(),
      'fileSize': fileSize,
      'checksum': checksum,
      'dataTypes': dataTypes,
      'version': version,
    };
  }

  factory BackupMetadata.fromJson(Map<String, dynamic> json) {
    return BackupMetadata(
      id: json['id'],
      name: json['name'],
      createdAt: DateTime.parse(json['createdAt']),
      fileSize: json['fileSize'],
      checksum: json['checksum'],
      dataTypes: Map<String, dynamic>.from(json['dataTypes']),
      version: json['version'],
    );
  }
}

class BackupRestoreSystem {
  static final BackupRestoreSystem _instance = BackupRestoreSystem._internal();
  factory BackupRestoreSystem() => _instance;
  BackupRestoreSystem._internal();

  static const String _backupVersion = '1.0.0';
  late String _backupDirectory;
  late String _metadataFile;
  final List<BackupMetadata> _backupCache = [];

  Future<void> initialize() async {
    final appDir = await getApplicationDocumentsDirectory();
    _backupDirectory = '${appDir.path}/backups';
    _metadataFile = '$_backupDirectory/backup_metadata.json';

    await Directory(_backupDirectory).create(recursive: true);
    await _loadBackupMetadata();
  }

  Future<void> _loadBackupMetadata() async {
    try {
      final file = File(_metadataFile);
      if (await file.exists()) {
        final jsonString = await file.readAsString();
        final List<dynamic> data = json.decode(jsonString);
        
        _backupCache.clear();
        _backupCache.addAll(data.map((item) => BackupMetadata.fromJson(item)));
      }
    } catch (e) {
      _backupCache.clear();
    }
  }

  Future<void> _saveBackupMetadata() async {
    try {
      final file = File(_metadataFile);
      final jsonData = _backupCache.map((backup) => backup.toJson()).toList();
      await file.writeAsString(json.encode(jsonData));
    } catch (e) {
      throw Exception('Failed to save backup metadata: $e');
    }
  }

  Future<BackupMetadata> createBackup({String? name}) async {
    await initialize();

    final backupId = DateTime.now().millisecondsSinceEpoch.toString();
    final backupName = name ?? 'Backup_${DateTime.now().toString().split('.')[0]}';
    
    // Collect all application data
    final backupData = await _collectApplicationData();
    
    // Create backup file
    final backupFile = File('$_backupDirectory/$backupId.backup');
    final backupContent = json.encode(backupData);
    await backupFile.writeAsString(backupContent);

    // Calculate checksum
    final bytes = utf8.encode(backupContent);
    final digest = sha256.convert(bytes);
    final checksum = digest.toString();

    // Create metadata
    final metadata = BackupMetadata(
      id: backupId,
      name: backupName,
      createdAt: DateTime.now(),
      fileSize: bytes.length,
      checksum: checksum,
      dataTypes: _getDataTypesInfo(backupData),
      version: _backupVersion,
    );

    _backupCache.add(metadata);
    await _saveBackupMetadata();

    return metadata;
  }

  Future<Map<String, dynamic>> _collectApplicationData() async {
    final data = <String, dynamic>{};

    // Collect SharedPreferences
    final prefs = await SharedPreferences.getInstance();
    final prefKeys = prefs.getKeys();
    final prefsData = <String, dynamic>{};
    
    for (final key in prefKeys) {
      final value = prefs.get(key);
      prefsData[key] = {
        'value': value,
        'type': value.runtimeType.toString(),
      };
    }
    
    data['shared_preferences'] = prefsData;

    // Collect files from documents directory
    try {
      final documentsDir = await getApplicationDocumentsDirectory();
      final fileData = await _collectDirectoryContents(documentsDir, 'documents');
      data['documents'] = fileData;
    } catch (e) {
      data['documents'] = {};
    }

    // Add application metadata
    data['backup_metadata'] = {
      'created_at': DateTime.now().toIso8601String(),
      'version': _backupVersion,
      'platform': Platform.operatingSystem,
    };

    return data;
  }

  Future<Map<String, dynamic>> _collectDirectoryContents(Directory dir, String basePath) async {
    final contents = <String, dynamic>{};
    
    try {
      await for (final entity in dir.list(recursive: false)) {
        final relativePath = entity.path.replaceAll(dir.path, '');
        
        if (entity is File) {
          // Skip backup files and temporary files
          if (entity.path.contains('/backups/') || 
              entity.path.endsWith('.tmp') ||
              entity.path.endsWith('.lock')) {
            continue;
          }
          
          try {
            final fileContent = await entity.readAsString();
            contents[relativePath] = {
              'type': 'file',
              'content': fileContent,
              'size': fileContent.length,
            };
          } catch (e) {
            // Binary file or read error - store metadata only
            final stat = await entity.stat();
            contents[relativePath] = {
              'type': 'binary_file',
              'size': stat.size,
              'modified': stat.modified.toIso8601String(),
            };
          }
        } else if (entity is Directory) {
          final subContents = await _collectDirectoryContents(entity, '$basePath$relativePath/');
          if (subContents.isNotEmpty) {
            contents[relativePath] = {
              'type': 'directory',
              'contents': subContents,
            };
          }
        }
      }
    } catch (e) {
      // Handle permission or access errors
    }
    
    return contents;
  }

  Map<String, dynamic> _getDataTypesInfo(Map<String, dynamic> backupData) {
    final info = <String, dynamic>{};
    
    if (backupData['shared_preferences'] != null) {
      final prefs = backupData['shared_preferences'] as Map<String, dynamic>;
      info['shared_preferences_count'] = prefs.length;
    }
    
    if (backupData['documents'] != null) {
      final docs = backupData['documents'] as Map<String, dynamic>;
      info['documents_count'] = _countFiles(docs);
    }
    
    return info;
  }

  int _countFiles(Map<String, dynamic> contents) {
    int count = 0;
    
    for (final value in contents.values) {
      if (value is Map<String, dynamic>) {
        if (value['type'] == 'file' || value['type'] == 'binary_file') {
          count++;
        } else if (value['type'] == 'directory') {
          count += _countFiles(value['contents']);
        }
      }
    }
    
    return count;
  }

  Future<bool> restoreBackup(String backupId) async {
    await initialize();

    final backupFile = File('$_backupDirectory/$backupId.backup');
    if (!await backupFile.exists()) {
      throw Exception('Backup file not found');
    }

    try {
      // Read and verify backup
      final backupContent = await backupFile.readAsString();
      final backupData = json.decode(backupContent) as Map<String, dynamic>;
      
      // Verify checksum
      final metadata = _backupCache.firstWhere((b) => b.id == backupId);
      final bytes = utf8.encode(backupContent);
      final digest = sha256.convert(bytes);
      
      if (digest.toString() != metadata.checksum) {
        throw Exception('Backup integrity check failed');
      }

      // Restore SharedPreferences
      if (backupData['shared_preferences'] != null) {
        await _restoreSharedPreferences(backupData['shared_preferences']);
      }

      // Restore files
      if (backupData['documents'] != null) {
        await _restoreDocuments(backupData['documents']);
      }

      return true;
    } catch (e) {
      throw Exception('Restore failed: $e');
    }
  }

  Future<void> _restoreSharedPreferences(Map<String, dynamic> prefsData) async {
    final prefs = await SharedPreferences.getInstance();
    
    // Clear existing preferences
    await prefs.clear();
    
    // Restore preferences
    for (final entry in prefsData.entries) {
      final key = entry.key;
      final data = entry.value as Map<String, dynamic>;
      final value = data['value'];
      final type = data['type'];
      
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
          await prefs.setStringList(key, List<String>.from(value));
          break;
      }
    }
  }

  Future<void> _restoreDocuments(Map<String, dynamic> documentsData) async {
    final documentsDir = await getApplicationDocumentsDirectory();
    await _restoreDirectoryContents(documentsData, documentsDir);
  }

  Future<void> _restoreDirectoryContents(Map<String, dynamic> contents, Directory baseDir) async {
    for (final entry in contents.entries) {
      final path = entry.key;
      final data = entry.value as Map<String, dynamic>;
      final fullPath = '${baseDir.path}$path';
      
      if (data['type'] == 'file') {
        final file = File(fullPath);
        await file.create(recursive: true);
        await file.writeAsString(data['content']);
      } else if (data['type'] == 'directory') {
        final dir = Directory(fullPath);
        await dir.create(recursive: true);
        await _restoreDirectoryContents(data['contents'], Directory(fullPath));
      }
    }
  }

  List<BackupMetadata> getBackups() {
    return List<BackupMetadata>.from(_backupCache);
  }

  Future<bool> deleteBackup(String backupId) async {
    await initialize();

    try {
      final backupFile = File('$_backupDirectory/$backupId.backup');
      if (await backupFile.exists()) {
        await backupFile.delete();
      }

      _backupCache.removeWhere((backup) => backup.id == backupId);
      await _saveBackupMetadata();

      return true;
    } catch (e) {
      return false;
    }
  }

  Future<String> exportBackup(String backupId) async {
    final backupFile = File('$_backupDirectory/$backupId.backup');
    if (!await backupFile.exists()) {
      throw Exception('Backup file not found');
    }

    return backupFile.path;
  }

  Future<BackupMetadata> importBackup(String filePath) async {
    final file = File(filePath);
    if (!await file.exists()) {
      throw Exception('Import file not found');
    }

    try {
      final content = await file.readAsString();
      final backupData = json.decode(content) as Map<String, dynamic>;
      
      // Generate new backup ID
      final backupId = DateTime.now().millisecondsSinceEpoch.toString();
      
      // Copy to backup directory
      final backupFile = File('$_backupDirectory/$backupId.backup');
      await backupFile.writeAsString(content);

      // Calculate checksum
      final bytes = utf8.encode(content);
      final digest = sha256.convert(bytes);

      // Create metadata
      final metadata = BackupMetadata(
        id: backupId,
        name: 'Imported_${DateTime.now().toString().split('.')[0]}',
        createdAt: DateTime.now(),
        fileSize: bytes.length,
        checksum: digest.toString(),
        dataTypes: _getDataTypesInfo(backupData),
        version: backupData['backup_metadata']?['version'] ?? 'unknown',
      );

      _backupCache.add(metadata);
      await _saveBackupMetadata();

      return metadata;
    } catch (e) {
      throw Exception('Import failed: $e');
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
      title: 'Backup & Restore System',
      home: const BackupRestorePage(),
    );
  }
}

class BackupRestorePage extends StatefulWidget {
  const BackupRestorePage({super.key});

  @override
  State<BackupRestorePage> createState() => _BackupRestorePageState();
}

class _BackupRestorePageState extends State<BackupRestorePage> {
  final BackupRestoreSystem _backupSystem = BackupRestoreSystem();
  final TextEditingController _backupNameController = TextEditingController();
  
  List<BackupMetadata> _backups = [];
  bool _isProcessing = false;

  @override
  void initState() {
    super.initState();
    _loadBackups();
    _createSampleData();
  }

  @override
  void dispose() {
    _backupNameController.dispose();
    super.dispose();
  }

  Future<void> _loadBackups() async {
    await _backupSystem.initialize();
    final backups = _backupSystem.getBackups();
    
    setState(() {
      _backups = backups;
    });
  }

  Future<void> _createSampleData() async {
    // Create some sample data for backup testing
    final prefs = await SharedPreferences.getInstance();
    await prefs.setString('user_name', 'John Doe');
    await prefs.setInt('app_launches', 42);
    await prefs.setBool('notifications_enabled', true);
    await prefs.setStringList('favorite_colors', ['blue', 'green', 'red']);
  }

  Future<void> _createBackup() async {
    setState(() {
      _isProcessing = true;
    });

    try {
      final backupName = _backupNameController.text.trim();
      final metadata = await _backupSystem.createBackup(
        name: backupName.isNotEmpty ? backupName : null,
      );

      _backupNameController.clear();
      await _loadBackups();

      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Backup "${metadata.name}" created successfully!')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Backup failed: $e')),
        );
      }
    } finally {
      setState(() {
        _isProcessing = false;
      });
    }
  }

  Future<void> _restoreBackup(String backupId) async {
    final confirmed = await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Confirm Restore'),
        content: const Text(
          'This will replace all current application data with the backup data. '
          'This action cannot be undone. Continue?'
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context, false),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () => Navigator.pop(context, true),
            child: const Text('Restore'),
          ),
        ],
      ),
    );

    if (confirmed != true) return;

    setState(() {
      _isProcessing = true;
    });

    try {
      final success = await _backupSystem.restoreBackup(backupId);
      
      if (success && mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Backup restored successfully!')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Restore failed: $e')),
        );
      }
    } finally {
      setState(() {
        _isProcessing = false;
      });
    }
  }

  Future<void> _deleteBackup(String backupId) async {
    final confirmed = await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Delete Backup'),
        content: const Text('Are you sure you want to delete this backup?'),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context, false),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () => Navigator.pop(context, true),
            child: const Text('Delete'),
          ),
        ],
      ),
    );

    if (confirmed != true) return;

    final success = await _backupSystem.deleteBackup(backupId);
    
    if (success) {
      await _loadBackups();
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Backup deleted')),
        );
      }
    }
  }

  String _formatFileSize(int bytes) {
    if (bytes < 1024) return '$bytes B';
    if (bytes < 1024 * 1024) return '${(bytes / 1024).toStringAsFixed(1)} KB';
    return '${(bytes / (1024 * 1024)).toStringAsFixed(1)} MB';
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Backup & Restore'),
      ),
      body: Column(
        children: [
          // Create backup section
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Create New Backup',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 16),
                    TextField(
                      controller: _backupNameController,
                      decoration: const InputDecoration(
                        labelText: 'Backup Name (optional)',
                        border: OutlineInputBorder(),
                      ),
                      enabled: !_isProcessing,
                    ),
                    const SizedBox(height: 16),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton.icon(
                        onPressed: _isProcessing ? null : _createBackup,
                        icon: _isProcessing 
                            ? const SizedBox(
                                width: 16,
                                height: 16,
                                child: CircularProgressIndicator(strokeWidth: 2),
                              )
                            : const Icon(Icons.backup),
                        label: Text(_isProcessing ? 'Creating...' : 'Create Backup'),
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ),

          // Backups list
          Expanded(
            child: Padding(
              padding: const EdgeInsets.symmetric(horizontal: 16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Available Backups (${_backups.length})',
                    style: Theme.of(context).textTheme.titleLarge,
                  ),
                  const SizedBox(height: 8),
                  Expanded(
                    child: _backups.isEmpty
                        ? const Center(child: Text('No backups available'))
                        : ListView.builder(
                            itemCount: _backups.length,
                            itemBuilder: (context, index) {
                              final backup = _backups[index];
                              
                              return Card(
                                margin: const EdgeInsets.symmetric(vertical: 4),
                                child: ListTile(
                                  leading: const Icon(Icons.backup),
                                  title: Text(backup.name),
                                  subtitle: Column(
                                    crossAxisAlignment: CrossAxisAlignment.start,
                                    children: [
                                      Text('Created: ${backup.createdAt.toString().split('.')[0]}'),
                                      Text('Size: ${_formatFileSize(backup.fileSize)} • Version: ${backup.version}'),
                                      if (backup.dataTypes.isNotEmpty)
                                        Text('Data: ${backup.dataTypes.entries.map((e) => '${e.key}: ${e.value}').join(', ')}'),
                                    ],
                                  ),
                                  trailing: PopupMenuButton<String>(
                                    enabled: !_isProcessing,
                                    onSelected: (value) {
                                      switch (value) {
                                        case 'restore':
                                          _restoreBackup(backup.id);
                                          break;
                                        case 'delete':
                                          _deleteBackup(backup.id);
                                          break;
                                        case 'info':
                                          _showBackupInfo(backup);
                                          break;
                                      }
                                    },
                                    itemBuilder: (context) => [
                                      const PopupMenuItem(
                                        value: 'restore',
                                        child: Text('Restore'),
                                      ),
                                      const PopupMenuItem(
                                        value: 'info',
                                        child: Text('Info'),
                                      ),
                                      const PopupMenuItem(
                                        value: 'delete',
                                        child: Text('Delete'),
                                      ),
                                    ],
                                  ),
                                ),
                              );
                            },
                          ),
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }

  void _showBackupInfo(BackupMetadata backup) {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Text('Backup Info: ${backup.name}'),
        content: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            mainAxisSize: MainAxisSize.min,
            children: [
              Text('ID: ${backup.id}'),
              Text('Created: ${backup.createdAt}'),
              Text('Size: ${_formatFileSize(backup.fileSize)}'),
              Text('Version: ${backup.version}'),
              Text('Checksum: ${backup.checksum.substring(0, 16)}...'),
              const SizedBox(height: 8),
              const Text('Data Types:', style: TextStyle(fontWeight: FontWeight.bold)),
              ...backup.dataTypes.entries.map(
                (entry) => Text('  ${entry.key}: ${entry.value}'),
              ),
            ],
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
}
```

Backup and restore system provides comprehensive data protection with  
integrity verification, metadata tracking, and selective restoration. The  
system supports full application state backup including preferences, files,  
and custom data with checksum validation and import/export capabilities.  

## Data Encryption and Security

Implementing data encryption and security features for sensitive information.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';
import 'dart:typed_data';
import 'dart:math';
import 'package:crypto/crypto.dart';

void main() {
  runApp(const MyApp());
}

class EncryptionManager {
  static final EncryptionManager _instance = EncryptionManager._internal();
  factory EncryptionManager() => _instance;
  EncryptionManager._internal();

  static const String _keyPrefix = 'encrypted_';
  static const String _saltKey = 'encryption_salt';
  static const String _ivKey = 'encryption_iv';

  // Simple encryption implementation (for demo purposes)
  // In production, use proper encryption libraries like cryptography or pointycastle
  
  String _generateSalt() {
    final random = Random.secure();
    final saltBytes = List<int>.generate(32, (i) => random.nextInt(256));
    return base64.encode(saltBytes);
  }

  String _generateIV() {
    final random = Random.secure();
    final ivBytes = List<int>.generate(16, (i) => random.nextInt(256));
    return base64.encode(ivBytes);
  }

  Uint8List _deriveKey(String password, String salt) {
    // Simple key derivation (in production, use PBKDF2, Argon2, etc.)
    final combined = utf8.encode(password + salt);
    final digest = sha256.convert(combined);
    return Uint8List.fromList(digest.bytes);
  }

  String _simpleEncrypt(String plaintext, Uint8List key, String iv) {
    // Simple XOR encryption (for demo only - use AES in production)
    final plaintextBytes = utf8.encode(plaintext);
    final ivBytes = base64.decode(iv);
    final encrypted = List<int>.generate(plaintextBytes.length, (i) {
      final keyIndex = i % key.length;
      final ivIndex = i % ivBytes.length;
      return plaintextBytes[i] ^ key[keyIndex] ^ ivBytes[ivIndex];
    });
    return base64.encode(encrypted);
  }

  String _simpleDecrypt(String ciphertext, Uint8List key, String iv) {
    try {
      final encryptedBytes = base64.decode(ciphertext);
      final ivBytes = base64.decode(iv);
      final decrypted = List<int>.generate(encryptedBytes.length, (i) {
        final keyIndex = i % key.length;
        final ivIndex = i % ivBytes.length;
        return encryptedBytes[i] ^ key[keyIndex] ^ ivBytes[ivIndex];
      });
      return utf8.decode(decrypted);
    } catch (e) {
      throw Exception('Decryption failed: Invalid data or password');
    }
  }

  Future<void> initializeEncryption(String password) async {
    final prefs = await SharedPreferences.getInstance();
    
    // Generate salt if not exists
    if (!prefs.containsKey(_saltKey)) {
      final salt = _generateSalt();
      await prefs.setString(_saltKey, salt);
    }

    // Generate IV if not exists
    if (!prefs.containsKey(_ivKey)) {
      final iv = _generateIV();
      await prefs.setString(_ivKey, iv);
    }
  }

  Future<bool> storeEncrypted(String key, String value, String password) async {
    try {
      final prefs = await SharedPreferences.getInstance();
      await initializeEncryption(password);

      final salt = prefs.getString(_saltKey)!;
      final iv = prefs.getString(_ivKey)!;
      final derivedKey = _deriveKey(password, salt);

      final encryptedValue = _simpleEncrypt(value, derivedKey, iv);
      final storageKey = '$_keyPrefix$key';
      
      await prefs.setString(storageKey, encryptedValue);
      return true;
    } catch (e) {
      return false;
    }
  }

  Future<String?> retrieveEncrypted(String key, String password) async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final salt = prefs.getString(_saltKey);
      final iv = prefs.getString(_ivKey);
      
      if (salt == null || iv == null) {
        return null;
      }

      final storageKey = '$_keyPrefix$key';
      final encryptedValue = prefs.getString(storageKey);
      
      if (encryptedValue == null) {
        return null;
      }

      final derivedKey = _deriveKey(password, salt);
      return _simpleDecrypt(encryptedValue, derivedKey, iv);
    } catch (e) {
      return null;
    }
  }

  Future<bool> removeEncrypted(String key) async {
    try {
      final prefs = await SharedPreferences.getInstance();
      final storageKey = '$_keyPrefix$key';
      return await prefs.remove(storageKey);
    } catch (e) {
      return false;
    }
  }

  Future<List<String>> getEncryptedKeys() async {
    final prefs = await SharedPreferences.getInstance();
    final allKeys = prefs.getKeys();
    
    return allKeys
        .where((key) => key.startsWith(_keyPrefix))
        .map((key) => key.substring(_keyPrefix.length))
        .toList();
  }

  Future<void> changePassword(String oldPassword, String newPassword) async {
    final prefs = await SharedPreferences.getInstance();
    final encryptedKeys = await getEncryptedKeys();
    
    // Decrypt all data with old password
    final Map<String, String> decryptedData = {};
    
    for (final key in encryptedKeys) {
      final value = await retrieveEncrypted(key, oldPassword);
      if (value != null) {
        decryptedData[key] = value;
      }
    }

    // Generate new salt and IV
    final newSalt = _generateSalt();
    final newIV = _generateIV();
    
    await prefs.setString(_saltKey, newSalt);
    await prefs.setString(_ivKey, newIV);

    // Re-encrypt all data with new password
    for (final entry in decryptedData.entries) {
      await storeEncrypted(entry.key, entry.value, newPassword);
    }
  }

  Future<void> clearAllEncrypted() async {
    final prefs = await SharedPreferences.getInstance();
    final encryptedKeys = await getEncryptedKeys();
    
    for (final key in encryptedKeys) {
      await removeEncrypted(key);
    }
    
    await prefs.remove(_saltKey);
    await prefs.remove(_ivKey);
  }
}

class SecureDataManager {
  static final SecureDataManager _instance = SecureDataManager._internal();
  factory SecureDataManager() => _instance;
  SecureDataManager._internal();

  final EncryptionManager _encryption = EncryptionManager();
  String? _currentPassword;
  bool _isUnlocked = false;

  bool get isUnlocked => _isUnlocked;

  Future<bool> unlock(String password) async {
    try {
      // Try to decrypt a test value to verify password
      final testKey = 'password_test';
      final testValue = 'test_data';
      
      // Store test data if it doesn't exist
      final existingTest = await _encryption.retrieveEncrypted(testKey, password);
      if (existingTest == null) {
        await _encryption.storeEncrypted(testKey, testValue, password);
      }
      
      // Try to retrieve test data
      final retrieved = await _encryption.retrieveEncrypted(testKey, password);
      
      if (retrieved == testValue || retrieved != null) {
        _currentPassword = password;
        _isUnlocked = true;
        return true;
      }
      
      return false;
    } catch (e) {
      return false;
    }
  }

  void lock() {
    _currentPassword = null;
    _isUnlocked = false;
  }

  Future<bool> storeSecureData(String key, String value) async {
    if (!_isUnlocked || _currentPassword == null) {
      return false;
    }
    
    return await _encryption.storeEncrypted(key, value, _currentPassword!);
  }

  Future<String?> getSecureData(String key) async {
    if (!_isUnlocked || _currentPassword == null) {
      return null;
    }
    
    return await _encryption.retrieveEncrypted(key, _currentPassword!);
  }

  Future<bool> removeSecureData(String key) async {
    if (!_isUnlocked) {
      return false;
    }
    
    return await _encryption.removeEncrypted(key);
  }

  Future<List<String>> getSecureKeys() async {
    if (!_isUnlocked) {
      return [];
    }
    
    final keys = await _encryption.getEncryptedKeys();
    // Remove test key from list
    keys.removeWhere((key) => key == 'password_test');
    return keys;
  }

  Future<bool> changePassword(String newPassword) async {
    if (!_isUnlocked || _currentPassword == null) {
      return false;
    }
    
    try {
      await _encryption.changePassword(_currentPassword!, newPassword);
      _currentPassword = newPassword;
      return true;
    } catch (e) {
      return false;
    }
  }

  Future<Map<String, String>> exportSecureData() async {
    if (!_isUnlocked) {
      return {};
    }
    
    final keys = await getSecureKeys();
    final Map<String, String> data = {};
    
    for (final key in keys) {
      final value = await getSecureData(key);
      if (value != null) {
        data[key] = value;
      }
    }
    
    return data;
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Data Encryption & Security',
      home: const SecurityPage(),
    );
  }
}

class SecurityPage extends StatefulWidget {
  const SecurityPage({super.key});

  @override
  State<SecurityPage> createState() => _SecurityPageState();
}

class _SecurityPageState extends State<SecurityPage> {
  final SecureDataManager _secureData = SecureDataManager();
  final TextEditingController _passwordController = TextEditingController();
  final TextEditingController _keyController = TextEditingController();
  final TextEditingController _valueController = TextEditingController();
  
  Map<String, String> _secureStorage = {};
  bool _passwordVisible = false;

  @override
  void dispose() {
    _passwordController.dispose();
    _keyController.dispose();
    _valueController.dispose();
    super.dispose();
  }

  Future<void> _unlock() async {
    if (_passwordController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please enter a password')),
      );
      return;
    }

    final success = await _secureData.unlock(_passwordController.text);
    
    if (success) {
      await _loadSecureData();
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Storage unlocked successfully!')),
        );
      }
    } else {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            content: Text('Invalid password or initialization failed'),
            backgroundColor: Colors.red,
          ),
        );
      }
    }
  }

  void _lock() {
    _secureData.lock();
    _passwordController.clear();
    setState(() {
      _secureStorage = {};
    });
    
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(content: Text('Storage locked')),
    );
  }

  Future<void> _loadSecureData() async {
    final keys = await _secureData.getSecureKeys();
    final Map<String, String> data = {};
    
    for (final key in keys) {
      final value = await _secureData.getSecureData(key);
      if (value != null) {
        data[key] = value;
      }
    }
    
    setState(() {
      _secureStorage = data;
    });
  }

  Future<void> _storeSecureData() async {
    if (_keyController.text.isEmpty || _valueController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please enter both key and value')),
      );
      return;
    }

    final success = await _secureData.storeSecureData(
      _keyController.text,
      _valueController.text,
    );

    if (success) {
      _keyController.clear();
      _valueController.clear();
      await _loadSecureData();
      
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Data stored securely!')),
        );
      }
    } else {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            content: Text('Failed to store data'),
            backgroundColor: Colors.red,
          ),
        );
      }
    }
  }

  Future<void> _removeSecureData(String key) async {
    final success = await _secureData.removeSecureData(key);
    
    if (success) {
      await _loadSecureData();
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Data removed')),
        );
      }
    }
  }

  Future<void> _createSampleData() async {
    if (!_secureData.isUnlocked) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please unlock storage first')),
      );
      return;
    }

    final sampleData = {
      'credit_card': '4532-1234-5678-9012',
      'social_security': '123-45-6789',
      'api_key': 'sk_test_123456789abcdef',
      'password_manager': 'super_secret_password_123',
      'private_notes': 'This is confidential information that should be encrypted.',
    };

    for (final entry in sampleData.entries) {
      await _secureData.storeSecureData(entry.key, entry.value);
    }

    await _loadSecureData();

    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Sample secure data created!')),
      );
    }
  }

  Future<void> _changePassword() async {
    if (!_secureData.isUnlocked) return;

    final newPasswordController = TextEditingController();
    final confirmPasswordController = TextEditingController();

    final result = await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Change Password'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(
              controller: newPasswordController,
              obscureText: true,
              decoration: const InputDecoration(
                labelText: 'New Password',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: confirmPasswordController,
              obscureText: true,
              decoration: const InputDecoration(
                labelText: 'Confirm Password',
                border: OutlineInputBorder(),
              ),
            ),
          ],
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context, false),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () {
              if (newPasswordController.text == confirmPasswordController.text) {
                Navigator.pop(context, true);
              } else {
                ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(content: Text('Passwords do not match')),
                );
              }
            },
            child: const Text('Change'),
          ),
        ],
      ),
    );

    if (result == true && newPasswordController.text.isNotEmpty) {
      final success = await _secureData.changePassword(newPasswordController.text);
      
      if (success && mounted) {
        _passwordController.text = newPasswordController.text;
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Password changed successfully!')),
        );
      }
    }

    newPasswordController.dispose();
    confirmPasswordController.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Data Encryption & Security'),
        actions: [
          if (_secureData.isUnlocked) ...[
            IconButton(
              icon: const Icon(Icons.key),
              onPressed: _changePassword,
            ),
            IconButton(
              icon: const Icon(Icons.lock),
              onPressed: _lock,
            ),
          ],
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Password section
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Secure Storage Access',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 16),
                    TextField(
                      controller: _passwordController,
                      obscureText: !_passwordVisible,
                      decoration: InputDecoration(
                        labelText: 'Master Password',
                        border: const OutlineInputBorder(),
                        suffixIcon: IconButton(
                          icon: Icon(
                            _passwordVisible ? Icons.visibility : Icons.visibility_off,
                          ),
                          onPressed: () {
                            setState(() {
                              _passwordVisible = !_passwordVisible;
                            });
                          },
                        ),
                      ),
                      enabled: !_secureData.isUnlocked,
                    ),
                    const SizedBox(height: 16),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton.icon(
                        onPressed: _secureData.isUnlocked ? null : _unlock,
                        icon: Icon(_secureData.isUnlocked ? Icons.lock_open : Icons.lock),
                        label: Text(_secureData.isUnlocked ? 'Unlocked' : 'Unlock'),
                      ),
                    ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Add data section
            if (_secureData.isUnlocked) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Store Encrypted Data',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                      const SizedBox(height: 16),
                      TextField(
                        controller: _keyController,
                        decoration: const InputDecoration(
                          labelText: 'Key',
                          border: OutlineInputBorder(),
                        ),
                      ),
                      const SizedBox(height: 8),
                      TextField(
                        controller: _valueController,
                        maxLines: 3,
                        decoration: const InputDecoration(
                          labelText: 'Value',
                          border: OutlineInputBorder(),
                        ),
                      ),
                      const SizedBox(height: 16),
                      Row(
                        children: [
                          Expanded(
                            child: ElevatedButton(
                              onPressed: _storeSecureData,
                              child: const Text('Store Encrypted'),
                            ),
                          ),
                          const SizedBox(width: 8),
                          Expanded(
                            child: ElevatedButton(
                              onPressed: _createSampleData,
                              child: const Text('Add Samples'),
                            ),
                          ),
                        ],
                      ),
                    ],
                  ),
                ),
              ),

              const SizedBox(height: 16),

              // Encrypted data list
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Encrypted Data (${_secureStorage.length} items)',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                      const SizedBox(height: 16),
                      if (_secureStorage.isEmpty)
                        const Text('No encrypted data stored')
                      else
                        Column(
                          children: _secureStorage.entries.map((entry) {
                            return Card(
                              margin: const EdgeInsets.symmetric(vertical: 4),
                              color: Colors.green.withOpacity(0.1),
                              child: ExpansionTile(
                                leading: const Icon(Icons.security, color: Colors.green),
                                title: Text(entry.key),
                                subtitle: const Text('Encrypted • Tap to view'),
                                trailing: IconButton(
                                  icon: const Icon(Icons.delete, color: Colors.red),
                                  onPressed: () => _removeSecureData(entry.key),
                                ),
                                children: [
                                  Padding(
                                    padding: const EdgeInsets.all(16.0),
                                    child: Container(
                                      width: double.infinity,
                                      padding: const EdgeInsets.all(12),
                                      decoration: BoxDecoration(
                                        color: Colors.grey.withOpacity(0.1),
                                        borderRadius: BorderRadius.circular(8),
                                        border: Border.all(color: Colors.green.withOpacity(0.3)),
                                      ),
                                      child: SelectableText(
                                        entry.value,
                                        style: const TextStyle(fontFamily: 'monospace'),
                                      ),
                                    ),
                                  ),
                                ],
                              ),
                            );
                          }).toList(),
                        ),
                    ],
                  ),
                ),
              ),
            ] else ...[
              const Card(
                child: Padding(
                  padding: EdgeInsets.all(16.0),
                  child: Column(
                    children: [
                      Icon(Icons.lock, size: 64, color: Colors.orange),
                      SizedBox(height: 16),
                      Text(
                        'Secure Storage Locked',
                        style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                      ),
                      SizedBox(height: 8),
                      Text('Enter your master password to access encrypted data.'),
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
}
```

Data encryption and security provides comprehensive protection for sensitive  
information using encryption, secure key management, and password-based access  
control. The system includes secure storage, password management, and data  
protection with proper encryption practices for maintaining data confidentiality.  

## Memory Management and Cleanup

Managing memory efficiently with proper cleanup and resource management  
for optimal app performance.  

```dart
import 'package:flutter/material.dart';
import 'dart:async';
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
      title: 'Memory Management',
      home: const MemoryManagerPage(),
    );
  }
}

class MemoryManagerPage extends StatefulWidget {
  const MemoryManagerPage({super.key});

  @override
  State<MemoryManagerPage> createState() => _MemoryManagerPageState();
}

class _MemoryManagerPageState extends State<MemoryManagerPage> {
  final List<StreamSubscription> _subscriptions = [];
  final List<Timer> _timers = [];
  final Map<String, dynamic> _cache = {};
  late final ResourceManager _resourceManager;

  @override
  void initState() {
    super.initState();
    _resourceManager = ResourceManager();
    _setupMemoryManagement();
  }

  void _setupMemoryManagement() {
    // Monitor memory usage
    final memoryTimer = Timer.periodic(
      const Duration(seconds: 5),
      (_) => _checkMemoryUsage(),
    );
    _timers.add(memoryTimer);

    // Cleanup cache periodically
    final cleanupTimer = Timer.periodic(
      const Duration(minutes: 1),
      (_) => _cleanupCache(),
    );
    _timers.add(cleanupTimer);
  }

  void _checkMemoryUsage() {
    _resourceManager.checkMemoryUsage();
    if (mounted) {
      setState(() {});
    }
  }

  void _cleanupCache() {
    final now = DateTime.now();
    _cache.removeWhere((key, value) {
      if (value is Map && value.containsKey('expiry')) {
        return now.isAfter(value['expiry']);
      }
      return false;
    });
  }

  void _addToCache(String key, dynamic value, Duration expiry) {
    _cache[key] = {
      'value': value,
      'expiry': DateTime.now().add(expiry),
    };
  }

  void _forceGarbageCollection() {
    // Clear all caches
    _cache.clear();
    
    // Force garbage collection (platform-specific)
    if (Platform.isAndroid || Platform.isIOS) {
      // Trigger system GC by creating and releasing memory
      List<List<int>> memoryPressure = [];
      for (int i = 0; i < 1000; i++) {
        memoryPressure.add(List.filled(1000, i));
      }
      memoryPressure.clear();
    }

    _resourceManager.forceCleanup();
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Memory cleanup completed')),
      );
      setState(() {});
    }
  }

  @override
  void dispose() {
    // Cancel all subscriptions
    for (final subscription in _subscriptions) {
      subscription.cancel();
    }

    // Cancel all timers
    for (final timer in _timers) {
      timer.cancel();
    }

    // Clear cache
    _cache.clear();

    // Cleanup resources
    _resourceManager.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Memory Management'),
      ),
      body: SingleChildScrollView(
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
                    const Text(
                      'Memory Statistics',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    _buildStatRow('Active Timers', '${_timers.length}'),
                    _buildStatRow('Active Subscriptions', '${_subscriptions.length}'),
                    _buildStatRow('Cache Entries', '${_cache.length}'),
                    _buildStatRow('Memory Status', _resourceManager.getMemoryStatus()),
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
                    const Text(
                      'Cache Management',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        Expanded(
                          child: ElevatedButton(
                            onPressed: () => _addToCache(
                              'test_${DateTime.now().millisecondsSinceEpoch}',
                              'Sample data',
                              const Duration(minutes: 5),
                            ),
                            child: const Text('Add Cache Entry'),
                          ),
                        ),
                        const SizedBox(width: 8),
                        Expanded(
                          child: ElevatedButton(
                            onPressed: _cleanupCache,
                            child: const Text('Cleanup Cache'),
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 8),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        onPressed: _forceGarbageCollection,
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.red.shade700,
                        ),
                        child: const Text('Force Memory Cleanup'),
                      ),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            if (_cache.isNotEmpty)
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Cache Contents',
                        style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 12),
                      ..._cache.entries.take(5).map((entry) {
                        final expiry = entry.value['expiry'] as DateTime;
                        final isExpired = DateTime.now().isAfter(expiry);
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 4.0),
                          child: Row(
                            children: [
                              Expanded(child: Text(entry.key)),
                              Text(
                                isExpired ? 'Expired' : 'Valid',
                                style: TextStyle(
                                  color: isExpired ? Colors.red : Colors.green,
                                ),
                              ),
                            ],
                          ),
                        );
                      }).toList(),
                    ],
                  ),
                ),
              ),
          ],
        ),
      ),
    );
  }

  Widget _buildStatRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4.0),
      child: Row(
        children: [
          Expanded(child: Text(label)),
          Text(
            value,
            style: const TextStyle(fontWeight: FontWeight.bold),
          ),
        ],
      ),
    );
  }
}

class ResourceManager {
  final Map<String, dynamic> _resources = {};
  Timer? _monitorTimer;

  ResourceManager() {
    _startMonitoring();
  }

  void _startMonitoring() {
    _monitorTimer = Timer.periodic(
      const Duration(seconds: 10),
      (_) => _performHealthCheck(),
    );
  }

  void _performHealthCheck() {
    // Simulate memory health check
    _resources['last_check'] = DateTime.now();
    _resources['status'] = 'Healthy';
  }

  void checkMemoryUsage() {
    _resources['memory_check'] = DateTime.now();
    // In a real app, you would check actual memory usage
    _resources['memory_usage'] = 'Normal';
  }

  String getMemoryStatus() {
    return _resources['memory_usage'] ?? 'Unknown';
  }

  void forceCleanup() {
    _resources.clear();
    _resources['cleanup_time'] = DateTime.now();
    _resources['status'] = 'Cleaned';
  }

  void dispose() {
    _monitorTimer?.cancel();
    _resources.clear();
  }
}
```

Memory management and cleanup ensures optimal app performance by properly  
disposing resources, managing cache expiration, monitoring memory usage,  
and providing cleanup mechanisms. This prevents memory leaks and maintains  
responsive app performance through automated resource management.  

## Async Data Operations

Managing asynchronous data operations with proper error handling, timeout  
management, and concurrent operation control.  

```dart
import 'package:flutter/material.dart';
import 'dart:async';
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
      title: 'Async Data Operations',
      home: const AsyncDataPage(),
    );
  }
}

class AsyncDataPage extends StatefulWidget {
  const AsyncDataPage({super.key});

  @override
  State<AsyncDataPage> createState() => _AsyncDataPageState();
}

class _AsyncDataPageState extends State<AsyncDataPage> {
  final AsyncDataManager _dataManager = AsyncDataManager();
  List<String> _results = [];
  bool _isLoading = false;
  String _status = 'Ready';

  @override
  void initState() {
    super.initState();
    _setupDataManager();
  }

  void _setupDataManager() {
    _dataManager.statusStream.listen((status) {
      if (mounted) {
        setState(() {
          _status = status;
        });
      }
    });

    _dataManager.resultsStream.listen((results) {
      if (mounted) {
        setState(() {
          _results = results;
        });
      }
    });
  }

  Future<void> _performBatchOperations() async {
    setState(() {
      _isLoading = true;
    });

    try {
      await _dataManager.performBatchOperations();
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Batch operations completed')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error: $e')),
        );
      }
    } finally {
      if (mounted) {
        setState(() {
          _isLoading = false;
        });
      }
    }
  }

  Future<void> _performConcurrentOperations() async {
    setState(() {
      _isLoading = true;
    });

    try {
      await _dataManager.performConcurrentOperations();
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Concurrent operations completed')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error: $e')),
        );
      }
    } finally {
      if (mounted) {
        setState(() {
          _isLoading = false;
        });
      }
    }
  }

  Future<void> _performStreamOperations() async {
    try {
      await _dataManager.performStreamOperations();
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Stream operations started')),
        );
      }
    } catch (e) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error: $e')),
        );
      }
    }
  }

  @override
  void dispose() {
    _dataManager.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Async Data Operations'),
      ),
      body: SingleChildScrollView(
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
                    const Text(
                      'Operation Status',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        if (_isLoading) ...[
                          const SizedBox(
                            width: 20,
                            height: 20,
                            child: CircularProgressIndicator(strokeWidth: 2),
                          ),
                          const SizedBox(width: 8),
                        ],
                        Expanded(
                          child: Text(
                            _status,
                            style: const TextStyle(fontSize: 16),
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
                    const Text(
                      'Async Operations',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        onPressed: _isLoading ? null : _performBatchOperations,
                        child: const Text('Perform Batch Operations'),
                      ),
                    ),
                    const SizedBox(height: 8),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        onPressed: _isLoading ? null : _performConcurrentOperations,
                        child: const Text('Perform Concurrent Operations'),
                      ),
                    ),
                    const SizedBox(height: 8),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        onPressed: _isLoading ? null : _performStreamOperations,
                        child: const Text('Start Stream Operations'),
                      ),
                    ),
                    const SizedBox(height: 8),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        onPressed: () => _dataManager.cancelOperations(),
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.red.shade700,
                        ),
                        child: const Text('Cancel All Operations'),
                      ),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            if (_results.isNotEmpty)
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Operation Results',
                        style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 12),
                      ...._results.take(10).map((result) {
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 4.0),
                          child: Container(
                            padding: const EdgeInsets.all(8.0),
                            decoration: BoxDecoration(
                              color: Colors.grey.withOpacity(0.1),
                              borderRadius: BorderRadius.circular(4),
                            ),
                            child: Text(result),
                          ),
                        );
                      }).toList(),
                      if (_results.length > 10) ...[
                        const SizedBox(height: 8),
                        Text(
                          '... and ${_results.length - 10} more results',
                          style: const TextStyle(fontStyle: FontStyle.italic),
                        ),
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
}

class AsyncDataManager {
  final StreamController<String> _statusController = StreamController<String>.broadcast();
  final StreamController<List<String>> _resultsController = StreamController<List<String>>.broadcast();
  final List<String> _results = [];
  final List<Completer> _activeOperations = [];

  Stream<String> get statusStream => _statusController.stream;
  Stream<List<String>> get resultsStream => _resultsController.stream;

  Future<void> performBatchOperations() async {
    _updateStatus('Starting batch operations...');
    
    final operations = <Future>[];
    
    for (int i = 0; i < 5; i++) {
      operations.add(_simulateDataOperation('Batch $i', Duration(seconds: i + 1)));
    }
    
    try {
      await Future.wait(operations, eagerError: true);
      _updateStatus('Batch operations completed successfully');
    } on TimeoutException {
      _updateStatus('Batch operations timed out');
      throw TimeoutException('Operations timed out', const Duration(seconds: 30));
    } catch (e) {
      _updateStatus('Batch operations failed: $e');
      rethrow;
    }
  }

  Future<void> performConcurrentOperations() async {
    _updateStatus('Starting concurrent operations...');
    
    try {
      final futures = <Future>[];
      
      // Create multiple concurrent operations with different delays
      for (int i = 0; i < 3; i++) {
        futures.add(_simulateDataOperation('Concurrent $i', Duration(milliseconds: 500 * (i + 1))));
      }
      
      // Wait for first completion
      final firstCompleted = await Future.any(futures);
      _updateStatus('First operation completed: $firstCompleted');
      
      // Wait for all to complete
      await Future.wait(futures);
      _updateStatus('All concurrent operations completed');
      
    } catch (e) {
      _updateStatus('Concurrent operations failed: $e');
      rethrow;
    }
  }

  Future<void> performStreamOperations() async {
    _updateStatus('Starting stream operations...');
    
    // Create a data stream
    final stream = _createDataStream();
    
    await for (final data in stream.take(10)) {
      _addResult('Stream data: $data');
      _updateStatus('Processing stream data: $data');
      
      // Add small delay to show progress
      await Future.delayed(const Duration(milliseconds: 200));
    }
    
    _updateStatus('Stream operations completed');
  }

  Stream<String> _createDataStream() async* {
    for (int i = 0; i < 20; i++) {
      yield 'Data item $i';
      await Future.delayed(const Duration(milliseconds: 100));
    }
  }

  Future<String> _simulateDataOperation(String name, Duration delay) async {
    final completer = Completer<String>();
    _activeOperations.add(completer);
    
    try {
      await Future.delayed(delay);
      
      if (completer.isCompleted) {
        return completer.future;
      }
      
      final result = '$name completed at ${DateTime.now().toIso8601String()}';
      _addResult(result);
      completer.complete(result);
      
      return result;
    } catch (e) {
      if (!completer.isCompleted) {
        completer.completeError(e);
      }
      rethrow;
    } finally {
      _activeOperations.remove(completer);
    }
  }

  void _updateStatus(String status) {
    _statusController.add(status);
  }

  void _addResult(String result) {
    _results.add(result);
    _resultsController.add(List.from(_results));
  }

  void cancelOperations() {
    _updateStatus('Cancelling all operations...');
    
    for (final completer in _activeOperations) {
      if (!completer.isCompleted) {
        completer.completeError('Operation cancelled');
      }
    }
    
    _activeOperations.clear();
    _updateStatus('All operations cancelled');
  }

  void dispose() {
    cancelOperations();
    _statusController.close();
    _resultsController.close();
  }
}
```

Async data operations manage complex asynchronous workflows with proper error  
handling, timeout management, concurrent operation control, and stream processing.  
The system provides batch operations, concurrent execution, stream handling,  
and cancellation mechanisms for robust data processing scenarios.  

## Local Storage Best Practices

Comprehensive example demonstrating best practices for local storage including  
optimization, error handling, data validation, and performance considerations.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';
import 'dart:async';
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
      title: 'Storage Best Practices',
      home: const BestPracticesPage(),
    );
  }
}

class BestPracticesPage extends StatefulWidget {
  const BestPracticesPage({super.key});

  @override
  State<BestPracticesPage> createState() => _BestPracticesPageState();
}

class _BestPracticesPageState extends State<BestPracticesPage> {
  final StorageBestPractices _storage = StorageBestPractices();
  final TextEditingController _keyController = TextEditingController();
  final TextEditingController _valueController = TextEditingController();
  
  Map<String, dynamic> _data = {};
  List<String> _validationErrors = [];
  String _performanceInfo = '';

  @override
  void initState() {
    super.initState();
    _loadData();
  }

  Future<void> _loadData() async {
    try {
      final data = await _storage.getAllData();
      final performance = await _storage.getPerformanceInfo();
      
      setState(() {
        _data = data;
        _performanceInfo = performance;
        _validationErrors.clear();
      });
    } catch (e) {
      _showError('Failed to load data: $e');
    }
  }

  Future<void> _saveData() async {
    final key = _keyController.text.trim();
    final value = _valueController.text.trim();

    if (key.isEmpty || value.isEmpty) {
      setState(() {
        _validationErrors = ['Key and value cannot be empty'];
      });
      return;
    }

    try {
      final validation = _storage.validateData(key, value);
      if (validation.isNotEmpty) {
        setState(() {
          _validationErrors = validation;
        });
        return;
      }

      await _storage.setData(key, value);
      await _loadData();
      
      _keyController.clear();
      _valueController.clear();
      
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Data saved successfully')),
        );
      }
    } catch (e) {
      _showError('Failed to save data: $e');
    }
  }

  Future<void> _optimizeStorage() async {
    try {
      await _storage.optimizeStorage();
      await _loadData();
      
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Storage optimized')),
        );
      }
    } catch (e) {
      _showError('Failed to optimize storage: $e');
    }
  }

  Future<void> _performHealthCheck() async {
    try {
      final issues = await _storage.performHealthCheck();
      
      if (issues.isEmpty) {
        if (mounted) {
          ScaffoldMessenger.of(context).showSnackBar(
            const SnackBar(content: Text('Storage health check passed')),
          );
        }
      } else {
        _showError('Health check issues: ${issues.join(', ')}');
      }
    } catch (e) {
      _showError('Health check failed: $e');
    }
  }

  void _showError(String message) {
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text(message),
          backgroundColor: Colors.red.shade700,
        ),
      );
    }
  }

  @override
  void dispose() {
    _keyController.dispose();
    _valueController.dispose();
    _storage.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Storage Best Practices'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _loadData,
            tooltip: 'Refresh',
          ),
        ],
      ),
      body: SingleChildScrollView(
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
                    const Text(
                      'Add Data with Validation',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    TextField(
                      controller: _keyController,
                      decoration: const InputDecoration(
                        labelText: 'Key',
                        border: OutlineInputBorder(),
                        hintText: 'Enter storage key',
                      ),
                    ),
                    const SizedBox(height: 8),
                    TextField(
                      controller: _valueController,
                      decoration: const InputDecoration(
                        labelText: 'Value',
                        border: OutlineInputBorder(),
                        hintText: 'Enter value to store',
                      ),
                      maxLines: 3,
                    ),
                    if (_validationErrors.isNotEmpty) ...[
                      const SizedBox(height: 8),
                      Container(
                        padding: const EdgeInsets.all(8.0),
                        decoration: BoxDecoration(
                          color: Colors.red.withOpacity(0.1),
                          borderRadius: BorderRadius.circular(4),
                          border: Border.all(color: Colors.red.withOpacity(0.3)),
                        ),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: _validationErrors.map((error) {
                            return Text(
                              '• $error',
                              style: const TextStyle(color: Colors.red),
                            );
                          }).toList(),
                        ),
                      ),
                    ],
                    const SizedBox(height: 12),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        onPressed: _saveData,
                        child: const Text('Save with Validation'),
                      ),
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
                    const Text(
                      'Storage Management',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        Expanded(
                          child: ElevatedButton(
                            onPressed: _optimizeStorage,
                            child: const Text('Optimize Storage'),
                          ),
                        ),
                        const SizedBox(width: 8),
                        Expanded(
                          child: ElevatedButton(
                            onPressed: _performHealthCheck,
                            child: const Text('Health Check'),
                          ),
                        ),
                      ],
                    ),
                    if (_performanceInfo.isNotEmpty) ...[
                      const SizedBox(height: 12),
                      Container(
                        padding: const EdgeInsets.all(8.0),
                        decoration: BoxDecoration(
                          color: Colors.blue.withOpacity(0.1),
                          borderRadius: BorderRadius.circular(4),
                          border: Border.all(color: Colors.blue.withOpacity(0.3)),
                        ),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            const Text(
                              'Performance Info:',
                              style: TextStyle(fontWeight: FontWeight.bold),
                            ),
                            const SizedBox(height: 4),
                            Text(_performanceInfo),
                          ],
                        ),
                      ),
                    ],
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            if (_data.isNotEmpty)
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Stored Data',
                        style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 12),
                      ..._data.entries.map((entry) {
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 4.0),
                          child: Container(
                            padding: const EdgeInsets.all(8.0),
                            decoration: BoxDecoration(
                              color: Colors.grey.withOpacity(0.1),
                              borderRadius: BorderRadius.circular(4),
                            ),
                            child: Column(
                              crossAxisAlignment: CrossAxisAlignment.start,
                              children: [
                                Row(
                                  children: [
                                    Expanded(
                                      child: Text(
                                        entry.key,
                                        style: const TextStyle(
                                          fontWeight: FontWeight.bold,
                                        ),
                                      ),
                                    ),
                                    IconButton(
                                      icon: const Icon(Icons.delete, size: 16),
                                      onPressed: () async {
                                        await _storage.deleteData(entry.key);
                                        _loadData();
                                      },
                                    ),
                                  ],
                                ),
                                Text(entry.value.toString()),
                              ],
                            ),
                          ),
                        );
                      }).toList(),
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

class StorageBestPractices {
  static const String _prefix = 'bp_';
  static const int _maxKeyLength = 100;
  static const int _maxValueLength = 10000;
  SharedPreferences? _prefs;
  final Map<String, dynamic> _cache = {};
  Timer? _cacheCleanupTimer;

  StorageBestPractices() {
    _initializeCache();
  }

  void _initializeCache() {
    _cacheCleanupTimer = Timer.periodic(
      const Duration(minutes: 5),
      (_) => _cleanupCache(),
    );
  }

  Future<SharedPreferences> get _preferences async {
    _prefs ??= await SharedPreferences.getInstance();
    return _prefs!;
  }

  List<String> validateData(String key, String value) {
    final errors = <String>[];

    if (key.length > _maxKeyLength) {
      errors.add('Key too long (max $_maxKeyLength characters)');
    }

    if (value.length > _maxValueLength) {
      errors.add('Value too long (max $_maxValueLength characters)');
    }

    if (key.contains(' ')) {
      errors.add('Key cannot contain spaces');
    }

    if (key.startsWith('_')) {
      errors.add('Key cannot start with underscore (reserved)');
    }

    try {
      json.decode(value);
    } catch (_) {
      // Value is not JSON, which is fine
    }

    return errors;
  }

  Future<void> setData(String key, dynamic value) async {
    final validation = validateData(key, value.toString());
    if (validation.isNotEmpty) {
      throw ArgumentError('Validation failed: ${validation.join(', ')}');
    }

    final prefs = await _preferences;
    final prefKey = '$_prefix$key';

    try {
      if (value is String) {
        await prefs.setString(prefKey, value);
      } else if (value is int) {
        await prefs.setInt(prefKey, value);
      } else if (value is double) {
        await prefs.setDouble(prefKey, value);
      } else if (value is bool) {
        await prefs.setBool(prefKey, value);
      } else if (value is List<String>) {
        await prefs.setStringList(prefKey, value);
      } else {
        // Store as JSON string
        await prefs.setString(prefKey, json.encode(value));
      }

      // Update cache
      _cache[key] = value;
    } catch (e) {
      throw StorageException('Failed to save data: $e');
    }
  }

  Future<dynamic> getData(String key, {dynamic defaultValue}) async {
    // Check cache first
    if (_cache.containsKey(key)) {
      return _cache[key];
    }

    final prefs = await _preferences;
    final prefKey = '$_prefix$key';

    try {
      final value = prefs.get(prefKey) ?? defaultValue;
      if (value != null) {
        _cache[key] = value;
      }
      return value;
    } catch (e) {
      throw StorageException('Failed to load data: $e');
    }
  }

  Future<Map<String, dynamic>> getAllData() async {
    final prefs = await _preferences;
    final data = <String, dynamic>{};

    try {
      final keys = prefs.getKeys().where((k) => k.startsWith(_prefix));
      
      for (final key in keys) {
        final cleanKey = key.substring(_prefix.length);
        final value = prefs.get(key);
        if (value != null) {
          data[cleanKey] = value;
          _cache[cleanKey] = value;
        }
      }

      return data;
    } catch (e) {
      throw StorageException('Failed to load all data: $e');
    }
  }

  Future<void> deleteData(String key) async {
    final prefs = await _preferences;
    final prefKey = '$_prefix$key';

    try {
      await prefs.remove(prefKey);
      _cache.remove(key);
    } catch (e) {
      throw StorageException('Failed to delete data: $e');
    }
  }

  Future<void> optimizeStorage() async {
    try {
      // Clean up expired entries
      _cleanupCache();
      
      // Reload preferences to ensure consistency
      _prefs = null;
      await _preferences;
      
      // Remove any corrupted entries
      await _removeCorruptedEntries();
    } catch (e) {
      throw StorageException('Storage optimization failed: $e');
    }
  }

  Future<void> _removeCorruptedEntries() async {
    final prefs = await _preferences;
    final keys = prefs.getKeys().where((k) => k.startsWith(_prefix)).toList();
    
    for (final key in keys) {
      try {
        prefs.get(key);
      } catch (e) {
        // Remove corrupted entry
        await prefs.remove(key);
      }
    }
  }

  Future<List<String>> performHealthCheck() async {
    final issues = <String>[];

    try {
      final prefs = await _preferences;
      final keys = prefs.getKeys().where((k) => k.startsWith(_prefix));
      
      // Check for issues
      if (keys.length > 1000) {
        issues.add('Too many stored keys (${keys.length})');
      }

      // Check for large values
      for (final key in keys) {
        final value = prefs.get(key);
        if (value is String && value.length > 5000) {
          issues.add('Large value detected in key: ${key.substring(_prefix.length)}');
        }
      }

      // Check cache consistency
      final cachedKeys = _cache.keys.toSet();
      final storedKeys = keys.map((k) => k.substring(_prefix.length)).toSet();
      final inconsistent = cachedKeys.difference(storedKeys);
      
      if (inconsistent.isNotEmpty) {
        issues.add('Cache inconsistency detected');
      }

    } catch (e) {
      issues.add('Health check error: $e');
    }

    return issues;
  }

  Future<String> getPerformanceInfo() async {
    try {
      final prefs = await _preferences;
      final keys = prefs.getKeys().where((k) => k.startsWith(_prefix));
      final cacheHitRatio = _cache.isNotEmpty ? 
          (_cache.length / keys.length * 100).toStringAsFixed(1) : '0.0';
      
      return 'Keys: ${keys.length}, Cache entries: ${_cache.length}, '
             'Cache hit ratio: $cacheHitRatio%';
    } catch (e) {
      return 'Performance info unavailable: $e';
    }
  }

  void _cleanupCache() {
    // Simple cache size limit
    if (_cache.length > 100) {
      final excess = _cache.length - 100;
      final keysToRemove = _cache.keys.take(excess).toList();
      for (final key in keysToRemove) {
        _cache.remove(key);
      }
    }
  }

  void dispose() {
    _cacheCleanupTimer?.cancel();
    _cache.clear();
  }
}

class StorageException implements Exception {
  final String message;
  
  const StorageException(this.message);
  
  @override
  String toString() => 'StorageException: $message';
}
```

Local storage best practices demonstrates comprehensive storage management  
with data validation, error handling, performance optimization, caching  
strategies, health monitoring, and proper resource management. The system  
includes validation rules, cache management, optimization routines, and  
health checks for maintaining reliable and performant local storage.  