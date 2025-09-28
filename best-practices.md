# Flutter Best Practices 

This comprehensive guide covers Flutter best practices with 25 practical  
examples that demonstrate professional development standards. Following  
these practices ensures maintainable, scalable, and high-performance  
Flutter applications while improving development efficiency and code  
quality across teams and projects.  

## In-Depth Description of Flutter Best Practices

Flutter best practices encompass a comprehensive set of development  
standards that promote code maintainability, performance optimization,  
and team collaboration. These practices evolved from collective  
experience of Flutter developers worldwide and Google's official  
recommendations for professional Flutter development.  

**Project Organization**: Proper project structure with clear separation  
of concerns, organized folders for features, shared components, and  
utilities. This includes logical grouping of related files, consistent  
naming conventions, and modular architecture that supports scalability  
and team collaboration.  

**Code Quality**: Writing clean, readable code with meaningful variable  
names, proper documentation, consistent formatting, and adherence to  
Dart language conventions. This encompasses effective use of linting  
rules, code analysis tools, and maintaining consistent code style  
across the project.  

**Performance Optimization**: Implementing efficient rendering patterns,  
memory management, and optimizing widget rebuilds. This includes proper  
use of const constructors, avoiding unnecessary computations in build  
methods, implementing lazy loading, and utilizing Flutter's performance  
profiling tools.  

**UI/UX Standards**: Creating consistent user interfaces that follow  
platform design guidelines while maintaining accessibility standards.  
This includes responsive design principles, proper color contrast,  
keyboard navigation support, and screen reader compatibility.  

**Security Practices**: Implementing secure data handling, authentication  
patterns, and protecting user privacy. This encompasses secure storage  
of sensitive data, proper certificate validation, and following privacy  
regulations like GDPR.  

**Testing Strategy**: Comprehensive testing approach including unit tests,  
widget tests, and integration tests with proper mocking strategies.  
This ensures code reliability, easier debugging, and confidence in  
application stability across different scenarios.  

## Best Practices Summary Table

| Category | Practice | Description | Impact |
|----------|----------|-------------|--------|
| **Project Structure** | Feature-based organization | Group related files by feature | High maintainability |
| | Consistent naming conventions | Use clear, descriptive names | Improved readability |
| | Separation of concerns | Isolate business logic from UI | Better testability |
| | Modular architecture | Create reusable components | Enhanced scalability |
| | Configuration management | Centralize app settings | Easier maintenance |
| **Code Quality** | Const constructors | Use const for immutable widgets | Better performance |
| | Meaningful names | Choose descriptive variable names | Improved readability |
| | Documentation | Add comments and API docs | Better collaboration |
| | Linting rules | Follow Dart linting standards | Consistent code style |
| | Error handling | Implement proper exception handling | Robust applications |
| **Performance** | Widget optimization | Minimize unnecessary rebuilds | Faster UI updates |
| | Memory management | Dispose resources properly | Prevent memory leaks |
| | Lazy loading | Load data when needed | Reduced initial load time |
| | Image optimization | Use appropriate image formats | Smaller app size |
| | Build method efficiency | Keep build methods pure | Consistent rendering |
| **UI/UX** | Responsive design | Support multiple screen sizes | Better user experience |
| | Platform consistency | Follow platform design guidelines | Native feel |
| | Accessibility | Support screen readers and navigation | Inclusive design |
| | Loading states | Show progress indicators | User feedback |
| | Error states | Handle and display errors gracefully | Better user experience |
| **Security** | Secure storage | Encrypt sensitive data | Data protection |
| | Certificate validation | Verify SSL certificates | Secure communication |
| | Input validation | Validate user inputs | Prevent security vulnerabilities |
| **Testing** | Unit testing | Test individual functions/classes | Code reliability |
| | Widget testing | Test UI components | Interface stability |

## Project Structure Organization

Implementing a feature-based folder structure that promotes maintainability  
and team collaboration.  

```dart
import 'package:flutter/material.dart';

// Example of well-organized project structure:
// lib/
//   core/
//     constants/
//       app_constants.dart
//       colors.dart  
//       strings.dart
//     services/
//       api_service.dart
//       storage_service.dart
//     utils/
//       validators.dart
//       formatters.dart
//   features/
//     authentication/
//       models/
//       screens/
//       services/
//       widgets/
//     home/
//       models/
//       screens/
//       services/
//       widgets/
//   shared/
//     widgets/
//     models/
//   main.dart

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
      title: 'Best Practices - Project Structure',
      home: const ProjectStructureDemo(),
    );
  }
}

// Example of feature-based organization
class ProjectStructureDemo extends StatelessWidget {
  const ProjectStructureDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Project Structure'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildStructureSection('Core Layer', [
              'constants/ - App-wide constants and configuration',
              'services/ - Business logic and external services',  
              'utils/ - Helper functions and utilities',
              'models/ - Data models and entities',
            ]),
            const SizedBox(height: 20),
            _buildStructureSection('Feature Layer', [
              'authentication/ - Login, registration, user management',
              'home/ - Main dashboard and navigation',
              'profile/ - User profile management',
              'settings/ - App configuration and preferences',
            ]),
            const SizedBox(height: 20),
            _buildStructureSection('Shared Layer', [
              'widgets/ - Reusable UI components',
              'themes/ - App theming and styling',
              'extensions/ - Dart extensions and helpers',
            ]),
            const SizedBox(height: 20),
            const Card(
              child: Padding(
                padding: EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Benefits of This Structure:',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    SizedBox(height: 8),
                    Text('• Clear separation of concerns'),
                    Text('• Easy navigation for team members'),
                    Text('• Simplified testing and debugging'),
                    Text('• Reduced coupling between features'),
                    Text('• Scalable as the app grows'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildStructureSection(String title, List<String> items) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              title,
              style: const TextStyle(
                fontSize: 18,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 8),
            ...items.map((item) => Padding(
              padding: const EdgeInsets.symmetric(vertical: 2),
              child: Text('• $item'),
            )),
          ],
        ),
      ),
    );
  }
}
```

Feature-based project organization groups related files together, making  
it easier to navigate, maintain, and scale applications. The core layer  
contains shared functionality, features are self-contained modules, and  
shared components promote reusability across the application.  

## Consistent Naming Conventions

Using clear, descriptive names that follow Dart conventions for better  
code readability and maintenance.  

```dart
import 'package:flutter/material.dart';

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
      title: 'Best Practices - Naming Conventions',
      home: const NamingConventionsDemo(),
    );
  }
}

// Good: Descriptive class name with clear purpose
class UserProfileManager {
  static const int maxUserNameLength = 50;
  static const String defaultAvatarPath = 'assets/images/default_avatar.png';
  
  // Good: Clear method names that describe actions
  Future<UserProfile> loadUserProfile(String userId) async {
    // Implementation
    return const UserProfile(
      id: '123', 
      displayName: 'John Doe',
      email: 'john@example.com',
    );
  }
  
  bool isValidEmailAddress(String email) {
    return email.contains('@') && email.contains('.');
  }
  
  String formatUserDisplayName(String firstName, String lastName) {
    return '$firstName $lastName'.trim();
  }
}

// Good: Clear data model with descriptive properties
class UserProfile {
  const UserProfile({
    required this.id,
    required this.displayName,
    required this.email,
    this.avatarUrl,
    this.isEmailVerified = false,
  });
  
  final String id;
  final String displayName;
  final String email;
  final String? avatarUrl;
  final bool isEmailVerified;
  
  // Good: Factory method with clear intent
  factory UserProfile.fromJson(Map<String, dynamic> json) {
    return UserProfile(
      id: json['id'] as String,
      displayName: json['display_name'] as String,
      email: json['email'] as String,
      avatarUrl: json['avatar_url'] as String?,
      isEmailVerified: json['email_verified'] as bool? ?? false,
    );
  }
}

class NamingConventionsDemo extends StatefulWidget {
  const NamingConventionsDemo({super.key});

  @override
  State<NamingConventionsDemo> createState() => _NamingConventionsDemoState();
}

class _NamingConventionsDemoState extends State<NamingConventionsDemo> {
  final TextEditingController _emailController = TextEditingController();
  final UserProfileManager _userProfileManager = UserProfileManager();
  bool _isLoadingUserData = false;
  String _validationErrorMessage = '';
  
  @override
  void dispose() {
    _emailController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Naming Conventions'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildNamingRulesSection(),
            const SizedBox(height: 20),
            _buildExampleSection(),
            const SizedBox(height: 20),
            _buildInteractiveDemo(),
          ],
        ),
      ),
    );
  }
  
  Widget _buildNamingRulesSection() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Naming Convention Rules',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 12),
            Text('• Classes: PascalCase (e.g., UserProfileManager)'),
            Text('• Methods: camelCase (e.g., loadUserProfile)'),
            Text('• Variables: camelCase (e.g., isLoadingUserData)'),
            Text('• Constants: camelCase with const (e.g., maxUserNameLength)'),
            Text('• Private members: prefix with _ (e.g., _userProfileManager)'),
            Text('• Files: snake_case (e.g., user_profile_manager.dart)'),
            Text('• Use descriptive names over abbreviations'),
            Text('• Boolean variables should be questions (e.g., isVisible)'),
          ],
        ),
      ),
    );
  }
  
  Widget _buildExampleSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Good vs Bad Examples',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            _buildComparisonRow('Class Names', 
              'UserProfileManager', 'UPM or Manager1'),
            _buildComparisonRow('Method Names', 
              'loadUserProfile()', 'load() or getUserStuff()'),
            _buildComparisonRow('Variable Names', 
              'isLoadingUserData', 'loading or flag'),
            _buildComparisonRow('Constants', 
              'maxUserNameLength', 'MAX_LEN or limit'),
          ],
        ),
      ),
    );
  }
  
  Widget _buildComparisonRow(String category, String good, String bad) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        children: [
          SizedBox(
            width: 120,
            child: Text(
              '$category:',
              style: const TextStyle(fontWeight: FontWeight.w500),
            ),
          ),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text('✓ $good', style: const TextStyle(color: Colors.green)),
                Text('✗ $bad', style: const TextStyle(color: Colors.red)),
              ],
            ),
          ),
        ],
      ),
    );
  }
  
  Widget _buildInteractiveDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Interactive Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            TextField(
              controller: _emailController,
              decoration: InputDecoration(
                labelText: 'Email Address',
                hintText: 'Enter your email',
                errorText: _validationErrorMessage.isEmpty 
                  ? null 
                  : _validationErrorMessage,
                border: const OutlineInputBorder(),
              ),
              onChanged: _validateEmailInput,
            ),
            const SizedBox(height: 16),
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                onPressed: _isLoadingUserData ? null : _handleLoadUserProfile,
                child: _isLoadingUserData
                  ? const CircularProgressIndicator()
                  : const Text('Load User Profile'),
              ),
            ),
          ],
        ),
      ),
    );
  }
  
  // Good: Method name clearly indicates what validation is performed
  void _validateEmailInput(String email) {
    setState(() {
      if (email.isEmpty) {
        _validationErrorMessage = '';
      } else if (!_userProfileManager.isValidEmailAddress(email)) {
        _validationErrorMessage = 'Please enter a valid email address';
      } else {
        _validationErrorMessage = '';
      }
    });
  }
  
  // Good: Method name describes the action and expected outcome
  Future<void> _handleLoadUserProfile() async {
    if (_emailController.text.isEmpty) return;
    
    setState(() {
      _isLoadingUserData = true;
    });
    
    try {
      await _userProfileManager.loadUserProfile(_emailController.text);
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            content: Text('User profile loaded successfully'),
            backgroundColor: Colors.green,
          ),
        );
      }
    } catch (error) {
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(
            content: Text('Error loading profile: $error'),
            backgroundColor: Colors.red,
          ),
        );
      }
    } finally {
      if (mounted) {
        setState(() {
          _isLoadingUserData = false;
        });
      }
    }
  }
}
```

Consistent naming conventions improve code readability and make it easier  
for team members to understand and maintain the codebase. Using descriptive  
names reduces the need for comments and makes the code self-documenting.  
Following Dart conventions ensures consistency across the Flutter ecosystem.  

## Const Constructors for Performance

Using const constructors to improve performance by preventing unnecessary  
widget rebuilds and reducing memory allocation.  

```dart
import 'package:flutter/material.dart';

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
      title: 'Best Practices - Const Constructors',
      home: const ConstConstructorDemo(),
    );
  }
}

// Good: Const constructor for immutable widget
class CustomButton extends StatelessWidget {
  const CustomButton({
    super.key,
    required this.text,
    required this.onPressed,
    this.backgroundColor = Colors.blue,
    this.textColor = Colors.white,
  });

  final String text;
  final VoidCallback onPressed;
  final Color backgroundColor;
  final Color textColor;

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      style: ElevatedButton.styleFrom(
        backgroundColor: backgroundColor,
        foregroundColor: textColor,
        padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(8),
        ),
      ),
      child: Text(text),
    );
  }
}

// Good: Const constructor for reusable card widget
class InfoCard extends StatelessWidget {
  const InfoCard({
    super.key,
    required this.title,
    required this.description,
    this.icon,
  });

  final String title;
  final String description;
  final IconData? icon;

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                if (icon != null) ...[
                  Icon(icon, size: 24),
                  const SizedBox(width: 8),
                ],
                Expanded(
                  child: Text(
                    title,
                    style: const TextStyle(
                      fontSize: 18,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            Text(
              description,
              style: const TextStyle(fontSize: 14),
            ),
          ],
        ),
      ),
    );
  }
}

// Good: Performance-optimized list item with const constructor
class PerformanceListItem extends StatelessWidget {
  const PerformanceListItem({
    super.key,
    required this.index,
    required this.title,
  });

  final int index;
  final String title;

  @override
  Widget build(BuildContext context) {
    return ListTile(
      leading: CircleAvatar(
        backgroundColor: Colors.blue,
        child: Text('${index + 1}'),
      ),
      title: Text(title),
      subtitle: const Text('Optimized with const constructor'),
      trailing: const Icon(Icons.arrow_forward_ios),
    );
  }
}

class ConstConstructorDemo extends StatefulWidget {
  const ConstConstructorDemo({super.key});

  @override
  State<ConstConstructorDemo> createState() => _ConstConstructorDemoState();
}

class _ConstConstructorDemoState extends State<ConstConstructorDemo> {
  int _rebuildCounter = 0;
  
  // Good: Static const widgets that never change
  static const List<String> performanceTips = [
    'Use const constructors for immutable widgets',
    'Apply const to widget parameters when possible',
    'Create const instances of commonly used widgets',
    'Use const with collections that don\'t change',
    'Prefer const over final for compile-time constants',
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Const Constructors'),
        actions: [
          // Good: Const widget in app bar
          IconButton(
            onPressed: _triggerRebuild,
            icon: const Icon(Icons.refresh),
            tooltip: 'Trigger Rebuild',
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Good: Const info card that won't rebuild
            const InfoCard(
              title: 'Performance Benefits',
              description: 'Const constructors prevent unnecessary rebuilds '
                          'and reduce memory allocation, improving app performance.',
              icon: Icons.speed,
            ),
            const SizedBox(height: 16),
            
            // Dynamic content that shows rebuild counter
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Rebuild Counter Demo',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 8),
                    Text('Rebuilds: $_rebuildCounter'),
                    const SizedBox(height: 12),
                    // Good: Const button that won't rebuild
                    CustomButton(
                      text: 'Trigger Rebuild',
                      onPressed: _triggerRebuild,
                      backgroundColor: Colors.green,
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Good: Const card with performance tips
            const InfoCard(
              title: 'Best Practices',
              description: 'Always use const constructors when widget '
                          'properties are known at compile time.',
              icon: Icons.lightbulb,
            ),
            const SizedBox(height: 16),
            
            // Performance-optimized list
            Card(
              child: Column(
                children: [
                  const Padding(
                    padding: EdgeInsets.all(16),
                    child: Text(
                      'Performance Tips',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ),
                  // Good: Using const constructor for list items
                  ...performanceTips.asMap().entries.map(
                    (entry) => PerformanceListItem(
                      index: entry.key,
                      title: entry.value,
                    ),
                  ),
                ],
              ),
            ),
            const SizedBox(height: 16),
            
            // Example of const vs non-const comparison
            _buildComparisonSection(),
          ],
        ),
      ),
    );
  }
  
  Widget _buildComparisonSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Const vs Non-Const Comparison',
              style: TextStyle(
                fontSize: 18,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 12),
            
            // Good: Const text widgets
            const Text(
              '✓ Const Constructor (Optimized):',
              style: TextStyle(
                color: Colors.green,
                fontWeight: FontWeight.w500,
              ),
            ),
            const SizedBox(height: 4),
            const Text(
              'const Text("Hello World")',
              style: TextStyle(
                fontFamily: 'monospace',
                backgroundColor: Colors.black12,
              ),
            ),
            const SizedBox(height: 12),
            
            // Bad example (but necessary to show difference)
            Text(
              '✗ Non-Const Constructor (Less Efficient):',
              style: TextStyle(
                color: Colors.red.shade300,
                fontWeight: FontWeight.w500,
              ),
            ),
            const SizedBox(height: 4),
            const Text(
              'Text("Hello World")',
              style: TextStyle(
                fontFamily: 'monospace',
                backgroundColor: Colors.black12,
              ),
            ),
            const SizedBox(height: 12),
            
            // Good: Const explanation
            const Text(
              'The const version creates the widget once at compile time. '
              'The non-const version creates a new widget instance on every build.',
              style: TextStyle(fontSize: 14),
            ),
          ],
        ),
      ),
    );
  }

  void _triggerRebuild() {
    setState(() {
      _rebuildCounter++;
    });
  }
}
```

Const constructors significantly improve Flutter app performance by creating  
widget instances at compile time rather than runtime. This prevents  
unnecessary object allocation and enables Flutter's widget tree optimization.  
Use const whenever widget properties are known at compile time and won't  
change during the widget's lifetime.  

## Separation of Business Logic

Separating business logic from UI components to improve testability,  
maintainability, and code organization.  

```dart
import 'package:flutter/material.dart';

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
      title: 'Best Practices - Business Logic Separation',
      home: const BusinessLogicDemo(),
    );
  }
}

// Good: Business logic separated into its own class
class TaskManager {
  TaskManager() {
    _loadInitialTasks();
  }

  final List<Task> _tasks = [];
  final List<VoidCallback> _listeners = [];

  List<Task> get tasks => List.unmodifiable(_tasks);
  int get totalTasks => _tasks.length;
  int get completedTasks => _tasks.where((task) => task.isCompleted).length;
  int get pendingTasks => totalTasks - completedTasks;
  double get completionPercentage => 
      totalTasks == 0 ? 0.0 : (completedTasks / totalTasks) * 100;

  void addListener(VoidCallback listener) {
    _listeners.add(listener);
  }

  void removeListener(VoidCallback listener) {
    _listeners.remove(listener);
  }

  void _notifyListeners() {
    for (final listener in _listeners) {
      listener();
    }
  }

  void _loadInitialTasks() {
    // Simulate loading tasks from storage
    _tasks.addAll([
      Task(id: '1', title: 'Complete project setup', isCompleted: true),
      Task(id: '2', title: 'Implement user authentication', isCompleted: false),
      Task(id: '3', title: 'Design main dashboard', isCompleted: false),
      Task(id: '4', title: 'Write unit tests', isCompleted: true),
    ]);
  }

  void addTask(String title) {
    if (title.trim().isEmpty) {
      throw ArgumentError('Task title cannot be empty');
    }
    
    final task = Task(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      title: title.trim(),
      isCompleted: false,
    );
    
    _tasks.add(task);
    _notifyListeners();
  }

  void toggleTaskCompletion(String taskId) {
    final taskIndex = _tasks.indexWhere((task) => task.id == taskId);
    if (taskIndex != -1) {
      _tasks[taskIndex] = _tasks[taskIndex].copyWith(
        isCompleted: !_tasks[taskIndex].isCompleted,
      );
      _notifyListeners();
    }
  }

  void removeTask(String taskId) {
    _tasks.removeWhere((task) => task.id == taskId);
    _notifyListeners();
  }

  List<Task> getTasksByStatus({required bool isCompleted}) {
    return _tasks.where((task) => task.isCompleted == isCompleted).toList();
  }

  void clearCompletedTasks() {
    _tasks.removeWhere((task) => task.isCompleted);
    _notifyListeners();
  }

  bool validateTaskTitle(String title) {
    return title.trim().isNotEmpty && title.trim().length >= 3;
  }
}

// Good: Data model with clear structure
class Task {
  const Task({
    required this.id,
    required this.title,
    required this.isCompleted,
    this.createdAt,
  });

  final String id;
  final String title;
  final bool isCompleted;
  final DateTime? createdAt;

  Task copyWith({
    String? id,
    String? title,
    bool? isCompleted,
    DateTime? createdAt,
  }) {
    return Task(
      id: id ?? this.id,
      title: title ?? this.title,
      isCompleted: isCompleted ?? this.isCompleted,
      createdAt: createdAt ?? this.createdAt,
    );
  }

  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    return other is Task && other.id == id;
  }

  @override
  int get hashCode => id.hashCode;
}

// Good: UI component focused only on presentation
class TaskStatistics extends StatelessWidget {
  const TaskStatistics({
    super.key,
    required this.taskManager,
  });

  final TaskManager taskManager;

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Task Statistics',
              style: TextStyle(
                fontSize: 18,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 12),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: [
                _buildStatItem(
                  'Total',
                  taskManager.totalTasks.toString(),
                  Colors.blue,
                ),
                _buildStatItem(
                  'Completed',
                  taskManager.completedTasks.toString(),
                  Colors.green,
                ),
                _buildStatItem(
                  'Pending',
                  taskManager.pendingTasks.toString(),
                  Colors.orange,
                ),
              ],
            ),
            const SizedBox(height: 12),
            LinearProgressIndicator(
              value: taskManager.completionPercentage / 100,
              backgroundColor: Colors.grey.shade300,
              valueColor: AlwaysStoppedAnimation<Color>(
                taskManager.completionPercentage > 50 
                  ? Colors.green 
                  : Colors.orange,
              ),
            ),
            const SizedBox(height: 8),
            Text(
              'Completion: ${taskManager.completionPercentage.toStringAsFixed(1)}%',
              style: const TextStyle(fontSize: 14),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildStatItem(String label, String value, Color color) {
    return Column(
      children: [
        Text(
          value,
          style: TextStyle(
            fontSize: 24,
            fontWeight: FontWeight.bold,
            color: color,
          ),
        ),
        Text(
          label,
          style: const TextStyle(fontSize: 12),
        ),
      ],
    );
  }
}

// Good: Reusable task item widget
class TaskItem extends StatelessWidget {
  const TaskItem({
    super.key,
    required this.task,
    required this.onToggle,
    required this.onDelete,
  });

  final Task task;
  final VoidCallback onToggle;
  final VoidCallback onDelete;

  @override
  Widget build(BuildContext context) {
    return Card(
      child: ListTile(
        leading: Checkbox(
          value: task.isCompleted,
          onChanged: (_) => onToggle(),
        ),
        title: Text(
          task.title,
          style: TextStyle(
            decoration: task.isCompleted 
              ? TextDecoration.lineThrough 
              : TextDecoration.none,
            color: task.isCompleted 
              ? Colors.grey 
              : null,
          ),
        ),
        trailing: IconButton(
          icon: const Icon(Icons.delete, color: Colors.red),
          onPressed: onDelete,
        ),
      ),
    );
  }
}

class BusinessLogicDemo extends StatefulWidget {
  const BusinessLogicDemo({super.key});

  @override
  State<BusinessLogicDemo> createState() => _BusinessLogicDemoState();
}

class _BusinessLogicDemoState extends State<BusinessLogicDemo> {
  late final TaskManager _taskManager;
  final TextEditingController _taskController = TextEditingController();
  String _validationError = '';

  @override
  void initState() {
    super.initState();
    _taskManager = TaskManager();
    _taskManager.addListener(_onTasksChanged);
  }

  @override
  void dispose() {
    _taskManager.removeListener(_onTasksChanged);
    _taskController.dispose();
    super.dispose();
  }

  void _onTasksChanged() {
    if (mounted) {
      setState(() {}); // UI updates when business logic changes
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Business Logic Separation'),
        actions: [
          if (_taskManager.completedTasks > 0)
            IconButton(
              icon: const Icon(Icons.clear_all),
              onPressed: _clearCompletedTasks,
              tooltip: 'Clear Completed Tasks',
            ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Business logic benefits explanation
            const Card(
              child: Padding(
                padding: EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Business Logic Separation Benefits',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    SizedBox(height: 8),
                    Text('• Improved testability - business logic can be unit tested'),
                    Text('• Better maintainability - logic is centralized'),
                    Text('• Code reusability - logic can be shared across widgets'),
                    Text('• Clear separation of concerns - UI vs business rules'),
                    Text('• Easier debugging - isolate logic from UI issues'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Task statistics (uses business logic)
            TaskStatistics(taskManager: _taskManager),
            const SizedBox(height: 16),
            
            // Task input
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Add New Task',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 12),
                    TextField(
                      controller: _taskController,
                      decoration: InputDecoration(
                        labelText: 'Task Title',
                        hintText: 'Enter task description',
                        errorText: _validationError.isEmpty 
                          ? null 
                          : _validationError,
                        border: const OutlineInputBorder(),
                      ),
                      onChanged: _validateTaskInput,
                      onSubmitted: (_) => _addTask(),
                    ),
                    const SizedBox(height: 12),
                    SizedBox(
                      width: double.infinity,
                      child: ElevatedButton(
                        onPressed: _validationError.isEmpty 
                          ? _addTask 
                          : null,
                        child: const Text('Add Task'),
                      ),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Task list
            if (_taskManager.tasks.isNotEmpty) ...[
              const Text(
                'Tasks',
                style: TextStyle(
                  fontSize: 18,
                  fontWeight: FontWeight.bold,
                ),
              ),
              const SizedBox(height: 8),
              ..._taskManager.tasks.map(
                (task) => TaskItem(
                  key: ValueKey(task.id),
                  task: task,
                  onToggle: () => _taskManager.toggleTaskCompletion(task.id),
                  onDelete: () => _taskManager.removeTask(task.id),
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }

  void _validateTaskInput(String input) {
    setState(() {
      if (input.isEmpty) {
        _validationError = '';
      } else if (!_taskManager.validateTaskTitle(input)) {
        _validationError = 'Task title must be at least 3 characters';
      } else {
        _validationError = '';
      }
    });
  }

  void _addTask() {
    if (_taskController.text.trim().isEmpty) return;
    
    try {
      _taskManager.addTask(_taskController.text);
      _taskController.clear();
      setState(() {
        _validationError = '';
      });
      
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Task added successfully'),
          backgroundColor: Colors.green,
        ),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text('Error adding task: $e'),
          backgroundColor: Colors.red,
        ),
      );
    }
  }

  void _clearCompletedTasks() {
    _taskManager.clearCompletedTasks();
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('Completed tasks cleared'),
      ),
    );
  }
}
```

Separating business logic from UI components creates more maintainable  
and testable applications. Business logic classes can be unit tested  
independently, UI components focus on presentation, and the separation  
enables code reuse across different parts of the application.  

## Proper Error Handling

Implementing comprehensive error handling strategies to create robust  
applications that gracefully handle unexpected situations.  

```dart
import 'package:flutter/material.dart';
import 'dart:async';
import 'dart:math';

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
      title: 'Best Practices - Error Handling',
      home: const ErrorHandlingDemo(),
    );
  }
}

// Good: Custom exception classes for different error types
class NetworkException implements Exception {
  const NetworkException(this.message, this.statusCode);
  
  final String message;
  final int statusCode;
  
  @override
  String toString() => 'NetworkException: $message (Status: $statusCode)';
}

class ValidationException implements Exception {
  const ValidationException(this.message, this.field);
  
  final String message;
  final String field;
  
  @override
  String toString() => 'ValidationException: $message (Field: $field)';
}

class DataNotFoundException implements Exception {
  const DataNotFoundException(this.message);
  
  final String message;
  
  @override
  String toString() => 'DataNotFoundException: $message';
}

// Good: Service class with proper error handling
class UserService {
  final Random _random = Random();
  
  Future<Map<String, dynamic>> fetchUserData(String userId) async {
    // Simulate network delay
    await Future.delayed(const Duration(seconds: 2));
    
    // Simulate various error scenarios
    final errorType = _random.nextInt(5);
    
    switch (errorType) {
      case 0:
        // Success case
        return {
          'id': userId,
          'name': 'John Doe',
          'email': 'john@example.com',
          'avatar': 'https://example.com/avatar.jpg',
        };
      case 1:
        throw const NetworkException('Failed to connect to server', 500);
      case 2:
        throw const NetworkException('User not found', 404);
      case 3:
        throw const DataNotFoundException('User data is corrupted');
      case 4:
        throw const ValidationException('Invalid user ID format', 'userId');
      default:
        throw Exception('Unknown error occurred');
    }
  }
  
  Future<bool> updateUserProfile(Map<String, dynamic> userData) async {
    // Validate input data
    if (userData['name']?.toString().trim().isEmpty ?? true) {
      throw const ValidationException('Name cannot be empty', 'name');
    }
    
    if (userData['email']?.toString().isEmpty ?? true) {
      throw const ValidationException('Email cannot be empty', 'email');
    }
    
    // Simulate update operation
    await Future.delayed(const Duration(seconds: 1));
    
    if (_random.nextBool()) {
      return true; // Success
    } else {
      throw const NetworkException('Failed to update profile', 500);
    }
  }
}

// Good: Error state management
enum LoadingState { idle, loading, success, error }

class ErrorResult {
  const ErrorResult({
    required this.message,
    required this.type,
    this.canRetry = true,
  });
  
  final String message;
  final String type;
  final bool canRetry;
}

class ErrorHandlingDemo extends StatefulWidget {
  const ErrorHandlingDemo({super.key});

  @override
  State<ErrorHandlingDemo> createState() => _ErrorHandlingDemoState();
}

class _ErrorHandlingDemoState extends State<ErrorHandlingDemo> {
  final UserService _userService = UserService();
  final TextEditingController _userIdController = TextEditingController();
  
  LoadingState _loadingState = LoadingState.idle;
  Map<String, dynamic>? _userData;
  ErrorResult? _currentError;
  
  @override
  void dispose() {
    _userIdController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Error Handling'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildErrorHandlingInfo(),
            const SizedBox(height: 16),
            _buildUserDataSection(),
            const SizedBox(height: 16),
            _buildErrorTypesDemo(),
          ],
        ),
      ),
    );
  }
  
  Widget _buildErrorHandlingInfo() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Error Handling Best Practices',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text('• Use specific exception types for different errors'),
            Text('• Provide meaningful error messages to users'),
            Text('• Implement retry mechanisms for transient errors'),
            Text('• Log errors for debugging purposes'),
            Text('• Show appropriate UI states (loading, error, empty)'),
            Text('• Handle network connectivity issues gracefully'),
            Text('• Validate input data before processing'),
            Text('• Use try-catch blocks around risky operations'),
          ],
        ),
      ),
    );
  }
  
  Widget _buildUserDataSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'User Data Fetching Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            // Input field
            TextField(
              controller: _userIdController,
              decoration: const InputDecoration(
                labelText: 'User ID',
                hintText: 'Enter user ID',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 12),
            
            // Fetch button
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                onPressed: _loadingState == LoadingState.loading 
                  ? null 
                  : _fetchUserData,
                child: _loadingState == LoadingState.loading
                  ? const CircularProgressIndicator()
                  : const Text('Fetch User Data'),
              ),
            ),
            const SizedBox(height: 16),
            
            // Content based on state
            _buildContentBasedOnState(),
          ],
        ),
      ),
    );
  }
  
  Widget _buildContentBasedOnState() {
    switch (_loadingState) {
      case LoadingState.idle:
        return const Card(
          color: Colors.blueGrey,
          child: Padding(
            padding: EdgeInsets.all(16),
            child: Row(
              children: [
                Icon(Icons.info, color: Colors.blue),
                SizedBox(width: 8),
                Text('Enter a User ID and click Fetch to begin'),
              ],
            ),
          ),
        );
        
      case LoadingState.loading:
        return const Card(
          child: Padding(
            padding: EdgeInsets.all(16),
            child: Row(
              children: [
                CircularProgressIndicator(),
                SizedBox(width: 16),
                Text('Fetching user data...'),
              ],
            ),
          ),
        );
        
      case LoadingState.success:
        return _buildSuccessContent();
        
      case LoadingState.error:
        return _buildErrorContent();
    }
  }
  
  Widget _buildSuccessContent() {
    if (_userData == null) return const SizedBox();
    
    return Card(
      color: Colors.green.shade50,
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Row(
              children: [
                Icon(Icons.check_circle, color: Colors.green),
                SizedBox(width: 8),
                Text(
                  'Success!',
                  style: TextStyle(
                    fontWeight: FontWeight.bold,
                    color: Colors.green,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            Text('ID: ${_userData!['id']}'),
            Text('Name: ${_userData!['name']}'),
            Text('Email: ${_userData!['email']}'),
            const SizedBox(height: 12),
            ElevatedButton(
              onPressed: _updateProfile,
              child: const Text('Update Profile'),
            ),
          ],
        ),
      ),
    );
  }
  
  Widget _buildErrorContent() {
    if (_currentError == null) return const SizedBox();
    
    return Card(
      color: Colors.red.shade50,
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const Icon(Icons.error, color: Colors.red),
                const SizedBox(width: 8),
                Expanded(
                  child: Text(
                    'Error: ${_currentError!.type}',
                    style: const TextStyle(
                      fontWeight: FontWeight.bold,
                      color: Colors.red,
                    ),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            Text(_currentError!.message),
            if (_currentError!.canRetry) ...[
              const SizedBox(height: 12),
              ElevatedButton.icon(
                onPressed: _fetchUserData,
                icon: const Icon(Icons.refresh),
                label: const Text('Retry'),
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.orange,
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }
  
  Widget _buildErrorTypesDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Error Types Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            const Text(
              'This demo simulates different types of errors randomly:',
              style: TextStyle(fontSize: 14),
            ),
            const SizedBox(height: 12),
            _buildErrorTypeItem('Network Error (500)', 'Server unavailable', Colors.red),
            _buildErrorTypeItem('Not Found (404)', 'User does not exist', Colors.orange),
            _buildErrorTypeItem('Data Corruption', 'Invalid data format', Colors.purple),
            _buildErrorTypeItem('Validation Error', 'Invalid input format', Colors.blue),
            _buildErrorTypeItem('Success', 'Data loaded successfully', Colors.green),
            const SizedBox(height: 12),
            const Text(
              'Try clicking "Fetch User Data" multiple times to see different error types.',
              style: TextStyle(fontSize: 12, fontStyle: FontStyle.italic),
            ),
          ],
        ),
      ),
    );
  }
  
  Widget _buildErrorTypeItem(String title, String description, Color color) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        children: [
          Container(
            width: 12,
            height: 12,
            decoration: BoxDecoration(
              color: color,
              shape: BoxShape.circle,
            ),
          ),
          const SizedBox(width: 8),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  title,
                  style: const TextStyle(fontWeight: FontWeight.w500),
                ),
                Text(
                  description,
                  style: const TextStyle(fontSize: 12, color: Colors.grey),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
  
  Future<void> _fetchUserData() async {
    if (_userIdController.text.trim().isEmpty) {
      _showErrorSnackBar('Please enter a User ID');
      return;
    }
    
    setState(() {
      _loadingState = LoadingState.loading;
      _currentError = null;
      _userData = null;
    });
    
    try {
      final userData = await _userService.fetchUserData(_userIdController.text);
      
      setState(() {
        _loadingState = LoadingState.success;
        _userData = userData;
      });
      
    } on NetworkException catch (e) {
      setState(() {
        _loadingState = LoadingState.error;
        _currentError = ErrorResult(
          message: e.message,
          type: 'Network Error',
          canRetry: e.statusCode >= 500, // Retry on server errors
        );
      });
    } on ValidationException catch (e) {
      setState(() {
        _loadingState = LoadingState.error;
        _currentError = ErrorResult(
          message: e.message,
          type: 'Validation Error',
          canRetry: false, // Don't retry validation errors
        );
      });
    } on DataNotFoundException catch (e) {
      setState(() {
        _loadingState = LoadingState.error;
        _currentError = ErrorResult(
          message: e.message,
          type: 'Data Not Found',
          canRetry: true,
        );
      });
    } catch (e) {
      setState(() {
        _loadingState = LoadingState.error;
        _currentError = ErrorResult(
          message: 'An unexpected error occurred: $e',
          type: 'Unknown Error',
          canRetry: true,
        );
      });
    }
  }
  
  Future<void> _updateProfile() async {
    if (_userData == null) return;
    
    try {
      await _userService.updateUserProfile(_userData!);
      _showSuccessSnackBar('Profile updated successfully');
    } on ValidationException catch (e) {
      _showErrorSnackBar('Validation Error: ${e.message}');
    } on NetworkException catch (e) {
      _showErrorSnackBar('Network Error: ${e.message}');
    } catch (e) {
      _showErrorSnackBar('Failed to update profile: $e');
    }
  }
  
  void _showErrorSnackBar(String message) {
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(message),
        backgroundColor: Colors.red,
        action: SnackBarAction(
          label: 'Dismiss',
          textColor: Colors.white,
          onPressed: () {
            ScaffoldMessenger.of(context).hideCurrentSnackBar();
          },
        ),
      ),
    );
  }
  
  void _showSuccessSnackBar(String message) {
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(message),
        backgroundColor: Colors.green,
      ),
    );
  }
}
```

Proper error handling improves application robustness and user experience.  
Custom exception types enable specific error handling strategies, loading  
states provide user feedback, and retry mechanisms handle transient errors.  
Always validate inputs, handle network issues gracefully, and provide  
meaningful error messages to guide users toward resolution.  

## Widget Lifecycle Management

Managing widget lifecycles properly to prevent memory leaks and ensure  
optimal performance through proper resource disposal.  

```dart
import 'package:flutter/material.dart';
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
      title: 'Best Practices - Lifecycle Management',
      home: const LifecycleDemo(),
    );
  }
}

// Good: Proper lifecycle management in StatefulWidget
class LifecycleDemo extends StatefulWidget {
  const LifecycleDemo({super.key});

  @override
  State<LifecycleDemo> createState() => _LifecycleDemoState();
}

class _LifecycleDemoState extends State<LifecycleDemo>
    with SingleTickerProviderStateMixin {
  late final AnimationController _animationController;
  late final Animation<double> _fadeAnimation;
  
  Timer? _periodicTimer;
  StreamSubscription<int>? _streamSubscription;
  final List<TextEditingController> _controllers = [];
  
  int _counter = 0;
  bool _isSubscribed = false;

  @override
  void initState() {
    super.initState();
    _initializeResources();
  }

  @override
  void dispose() {
    _disposeResources();
    super.dispose();
  }

  void _initializeResources() {
    // Good: Initialize animation controller
    _animationController = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );
    
    _fadeAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: Curves.easeInOut,
    ));
    
    // Good: Initialize controllers
    for (int i = 0; i < 3; i++) {
      _controllers.add(TextEditingController());
    }
    
    // Good: Start periodic timer
    _startPeriodicTimer();
    
    // Good: Subscribe to stream
    _subscribeToCounterStream();
  }

  void _disposeResources() {
    // Good: Dispose animation controller
    _animationController.dispose();
    
    // Good: Cancel timer
    _periodicTimer?.cancel();
    
    // Good: Cancel stream subscription
    _streamSubscription?.cancel();
    
    // Good: Dispose all text controllers
    for (final controller in _controllers) {
      controller.dispose();
    }
    _controllers.clear();
  }

  void _startPeriodicTimer() {
    _periodicTimer = Timer.periodic(
      const Duration(seconds: 1),
      (timer) {
        if (mounted) {
          setState(() {
            _counter++;
          });
        }
      },
    );
  }

  void _subscribeToCounterStream() {
    // Simulate a stream of data
    final stream = Stream.periodic(
      const Duration(seconds: 2),
      (count) => count,
    );
    
    _streamSubscription = stream.listen(
      (data) {
        if (mounted) {
          // Good: Check if widget is still mounted
          setState(() {
            _isSubscribed = true;
          });
        }
      },
      onError: (error) {
        if (mounted) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              content: Text('Stream error: $error'),
              backgroundColor: Colors.red,
            ),
          );
        }
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Lifecycle Management'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildLifecycleInfo(),
            const SizedBox(height: 16),
            _buildTimerDemo(),
            const SizedBox(height: 16),
            _buildAnimationDemo(),
            const SizedBox(height: 16),
            _buildControllerDemo(),
            const SizedBox(height: 16),
            _buildStreamDemo(),
          ],
        ),
      ),
    );
  }

  Widget _buildLifecycleInfo() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Lifecycle Management Best Practices',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text('• Dispose controllers in dispose() method'),
            Text('• Cancel timers and subscriptions'),
            Text('• Check mounted before setState()'),
            Text('• Use SingleTickerProviderStateMixin for animations'),
            Text('• Clean up resources to prevent memory leaks'),
            Text('• Handle async operations properly'),
          ],
        ),
      ),
    );
  }

  Widget _buildTimerDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Periodic Timer Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Text('Counter: $_counter'),
            const SizedBox(height: 8),
            const Text(
              'This timer is properly cancelled in dispose() to prevent memory leaks.',
              style: TextStyle(fontSize: 12, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildAnimationDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Animation Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            Row(
              children: [
                ElevatedButton(
                  onPressed: () {
                    _animationController.forward();
                  },
                  child: const Text('Start Animation'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {
                    _animationController.reverse();
                  },
                  child: const Text('Reverse Animation'),
                ),
              ],
            ),
            const SizedBox(height: 12),
            FadeTransition(
              opacity: _fadeAnimation,
              child: Container(
                width: 100,
                height: 100,
                decoration: BoxDecoration(
                  color: Colors.blue,
                  borderRadius: BorderRadius.circular(8),
                ),
                child: const Center(
                  child: Text('Animated'),
                ),
              ),
            ),
            const SizedBox(height: 8),
            const Text(
              'Animation controller is properly disposed in dispose() method.',
              style: TextStyle(fontSize: 12, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildControllerDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Text Controllers Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            ..._controllers.asMap().entries.map(
              (entry) => Padding(
                padding: const EdgeInsets.only(bottom: 8),
                child: TextField(
                  controller: entry.value,
                  decoration: InputDecoration(
                    labelText: 'Field ${entry.key + 1}',
                    border: const OutlineInputBorder(),
                  ),
                ),
              ),
            ),
            const SizedBox(height: 8),
            const Text(
              'All text controllers are properly disposed in dispose() method.',
              style: TextStyle(fontSize: 12, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildStreamDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Stream Subscription Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                const Text('Stream Status: '),
                Text(
                  _isSubscribed ? 'Active' : 'Inactive',
                  style: TextStyle(
                    color: _isSubscribed ? Colors.green : Colors.red,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            const Text(
              'Stream subscription is properly cancelled in dispose() method.',
              style: TextStyle(fontSize: 12, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }
}
```

Proper widget lifecycle management prevents memory leaks and ensures optimal  
performance. Always dispose controllers, cancel timers and subscriptions,  
and check if the widget is mounted before calling setState(). Use proper  
mixin classes for animation controllers and handle async operations carefully.  

## Responsive Design Implementation

Creating responsive layouts that adapt to different screen sizes and  
orientations while maintaining usability across devices.  

```dart
import 'package:flutter/material.dart';

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
      title: 'Best Practices - Responsive Design',
      home: const ResponsiveDemo(),
    );
  }
}

// Good: Responsive breakpoint constants
class Breakpoints {
  static const double mobile = 600;
  static const double tablet = 900;
  static const double desktop = 1200;
}

// Good: Extension for media query convenience
extension MediaQueryExtension on BuildContext {
  Size get screenSize => MediaQuery.of(this).size;
  bool get isMobile => screenSize.width < Breakpoints.mobile;
  bool get isTablet => screenSize.width >= Breakpoints.mobile && 
                       screenSize.width < Breakpoints.desktop;
  bool get isDesktop => screenSize.width >= Breakpoints.desktop;
  bool get isPortrait => screenSize.height > screenSize.width;
}

// Good: Responsive layout widget
class ResponsiveLayout extends StatelessWidget {
  const ResponsiveLayout({
    super.key,
    required this.mobile,
    this.tablet,
    this.desktop,
  });

  final Widget mobile;
  final Widget? tablet;
  final Widget? desktop;

  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
      builder: (context, constraints) {
        if (constraints.maxWidth >= Breakpoints.desktop) {
          return desktop ?? tablet ?? mobile;
        } else if (constraints.maxWidth >= Breakpoints.mobile) {
          return tablet ?? mobile;
        } else {
          return mobile;
        }
      },
    );
  }
}

// Good: Responsive grid widget
class ResponsiveGrid extends StatelessWidget {
  const ResponsiveGrid({
    super.key,
    required this.children,
    this.spacing = 8.0,
  });

  final List<Widget> children;
  final double spacing;

  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
      builder: (context, constraints) {
        int columns = _getColumnCount(constraints.maxWidth);
        
        return GridView.builder(
          shrinkWrap: true,
          physics: const NeverScrollableScrollPhysics(),
          gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
            crossAxisCount: columns,
            crossAxisSpacing: spacing,
            mainAxisSpacing: spacing,
            childAspectRatio: 1.2,
          ),
          itemCount: children.length,
          itemBuilder: (context, index) => children[index],
        );
      },
    );
  }

  int _getColumnCount(double width) {
    if (width >= Breakpoints.desktop) return 4;
    if (width >= Breakpoints.tablet) return 3;
    if (width >= Breakpoints.mobile) return 2;
    return 1;
  }
}

class ResponsiveDemo extends StatelessWidget {
  const ResponsiveDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Responsive Design'),
      ),
      body: ResponsiveLayout(
        mobile: _buildMobileLayout(context),
        tablet: _buildTabletLayout(context),
        desktop: _buildDesktopLayout(context),
      ),
    );
  }

  Widget _buildMobileLayout(BuildContext context) {
    return SingleChildScrollView(
      padding: const EdgeInsets.all(16),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          _buildResponsiveInfo(context),
          const SizedBox(height: 16),
          _buildFeatureCards(context),
          const SizedBox(height: 16),
          _buildResponsiveContent(context),
        ],
      ),
    );
  }

  Widget _buildTabletLayout(BuildContext context) {
    return SingleChildScrollView(
      padding: const EdgeInsets.all(24),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Row(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Expanded(
                flex: 2,
                child: _buildResponsiveInfo(context),
              ),
              const SizedBox(width: 24),
              Expanded(
                child: _buildDeviceInfo(context),
              ),
            ],
          ),
          const SizedBox(height: 24),
          _buildFeatureCards(context),
          const SizedBox(height: 24),
          _buildResponsiveContent(context),
        ],
      ),
    );
  }

  Widget _buildDesktopLayout(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(32),
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          // Sidebar
          SizedBox(
            width: 300,
            child: Column(
              children: [
                _buildResponsiveInfo(context),
                const SizedBox(height: 24),
                _buildDeviceInfo(context),
              ],
            ),
          ),
          const SizedBox(width: 32),
          
          // Main content
          Expanded(
            child: SingleChildScrollView(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  _buildFeatureCards(context),
                  const SizedBox(height: 32),
                  _buildResponsiveContent(context),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildResponsiveInfo(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Responsive Design Principles',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            const Text('• Use flexible layouts with Flexible and Expanded'),
            const Text('• Define clear breakpoints for different screen sizes'),
            const Text('• Test on various devices and orientations'),
            const Text('• Use MediaQuery for screen-specific logic'),
            const Text('• Implement adaptive navigation patterns'),
            const Text('• Optimize touch targets for different devices'),
            const SizedBox(height: 12),
            Text(
              'Current Layout: ${_getCurrentLayoutType(context)}',
              style: const TextStyle(
                fontWeight: FontWeight.bold,
                color: Colors.blue,
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildDeviceInfo(BuildContext context) {
    final screenSize = context.screenSize;
    
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Device Information',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Text('Width: ${screenSize.width.toInt()}px'),
            Text('Height: ${screenSize.height.toInt()}px'),
            Text('Orientation: ${context.isPortrait ? "Portrait" : "Landscape"}'),
            Text('Device Type: ${_getDeviceType(context)}'),
            const SizedBox(height: 8),
            LinearProgressIndicator(
              value: (screenSize.width / Breakpoints.desktop).clamp(0.0, 1.0),
              backgroundColor: Colors.grey.shade300,
              valueColor: AlwaysStoppedAnimation<Color>(
                _getProgressColor(context),
              ),
            ),
            const SizedBox(height: 4),
            Text(
              'Screen utilization: ${((screenSize.width / Breakpoints.desktop) * 100).clamp(0, 100).toInt()}%',
              style: const TextStyle(fontSize: 12),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildFeatureCards(BuildContext context) {
    final features = [
      {'title': 'Flexible Layouts', 'icon': Icons.view_module, 'color': Colors.blue},
      {'title': 'Breakpoints', 'icon': Icons.devices, 'color': Colors.green},
      {'title': 'Adaptive UI', 'icon': Icons.adaptive_audio_controls, 'color': Colors.orange},
      {'title': 'Touch Targets', 'icon': Icons.touch_app, 'color': Colors.purple},
    ];

    return ResponsiveGrid(
      children: features.map((feature) => _buildFeatureCard(
        title: feature['title'] as String,
        icon: feature['icon'] as IconData,
        color: feature['color'] as Color,
      )).toList(),
    );
  }

  Widget _buildFeatureCard({
    required String title,
    required IconData icon,
    required Color color,
  }) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(icon, size: 32, color: color),
            const SizedBox(height: 8),
            Text(
              title,
              textAlign: TextAlign.center,
              style: const TextStyle(
                fontSize: 14,
                fontWeight: FontWeight.bold,
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildResponsiveContent(BuildContext context) {
    return Card(
      child: Padding(
        padding: EdgeInsets.all(context.isMobile ? 16 : 24),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Responsive Content',
              style: TextStyle(
                fontSize: context.isMobile ? 18 : 20,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 12),
            
            // Responsive button layout
            if (context.isMobile) ...[
              // Stack buttons vertically on mobile
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: () {},
                  child: const Text('Primary Action'),
                ),
              ),
              const SizedBox(height: 8),
              SizedBox(
                width: double.infinity,
                child: OutlinedButton(
                  onPressed: () {},
                  child: const Text('Secondary Action'),
                ),
              ),
            ] else ...[
              // Row layout for larger screens
              Row(
                children: [
                  ElevatedButton(
                    onPressed: () {},
                    child: const Text('Primary Action'),
                  ),
                  const SizedBox(width: 12),
                  OutlinedButton(
                    onPressed: () {},
                    child: const Text('Secondary Action'),
                  ),
                  const Spacer(),
                  TextButton(
                    onPressed: () {},
                    child: const Text('Learn More'),
                  ),
                ],
              ),
            ],
            
            const SizedBox(height: 16),
            
            // Responsive text
            Text(
              'This content adapts to different screen sizes. On mobile devices, '
              'buttons stack vertically for easier touch interaction. On larger '
              'screens, elements are arranged horizontally to make better use '
              'of available space.',
              style: TextStyle(
                fontSize: context.isMobile ? 14 : 16,
                height: 1.4,
              ),
            ),
            
            const SizedBox(height: 16),
            
            // Responsive form fields
            if (context.isDesktop) ...[
              // Two-column layout for desktop
              Row(
                children: [
                  const Expanded(
                    child: TextField(
                      decoration: InputDecoration(
                        labelText: 'First Name',
                        border: OutlineInputBorder(),
                      ),
                    ),
                  ),
                  const SizedBox(width: 16),
                  const Expanded(
                    child: TextField(
                      decoration: InputDecoration(
                        labelText: 'Last Name',
                        border: OutlineInputBorder(),
                      ),
                    ),
                  ),
                ],
              ),
            ] else ...[
              // Single column for mobile/tablet
              const TextField(
                decoration: InputDecoration(
                  labelText: 'First Name',
                  border: OutlineInputBorder(),
                ),
              ),
              const SizedBox(height: 12),
              const TextField(
                decoration: InputDecoration(
                  labelText: 'Last Name',
                  border: OutlineInputBorder(),
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }

  String _getCurrentLayoutType(BuildContext context) {
    if (context.isDesktop) return 'Desktop';
    if (context.isTablet) return 'Tablet';
    return 'Mobile';
  }

  String _getDeviceType(BuildContext context) {
    if (context.isDesktop) return 'Desktop/Large Tablet';
    if (context.isTablet) return 'Tablet';
    return 'Mobile Phone';
  }

  Color _getProgressColor(BuildContext context) {
    if (context.isDesktop) return Colors.green;
    if (context.isTablet) return Colors.orange;
    return Colors.blue;
  }
}
```

Responsive design ensures applications work well across all devices and  
screen sizes. Use flexible layouts, define clear breakpoints, and test  
on various devices. Implement adaptive navigation patterns and optimize  
touch targets for different screen sizes to provide the best user experience.  

## Performance Optimization Techniques

Implementing performance optimization strategies to create fast, smooth  
Flutter applications with efficient resource usage.  

```dart
import 'package:flutter/material.dart';

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
      title: 'Best Practices - Performance Optimization',
      home: const PerformanceDemo(),
    );
  }
}

// Good: Optimized list item with const constructor
class OptimizedListItem extends StatelessWidget {
  const OptimizedListItem({
    super.key,
    required this.index,
    required this.title,
    required this.subtitle,
  });

  final int index;
  final String title;
  final String subtitle;

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 4),
      child: ListTile(
        // Good: Using const constructor for performance
        leading: CircleAvatar(
          backgroundColor: Colors.primaries[index % Colors.primaries.length],
          child: Text('${index + 1}'),
        ),
        title: Text(title),
        subtitle: Text(subtitle),
        trailing: const Icon(Icons.arrow_forward_ios, size: 16),
        onTap: () {
          // Handle tap
        },
      ),
    );
  }
}

// Good: Efficient custom widget with RepaintBoundary
class PerformanceChart extends StatelessWidget {
  const PerformanceChart({
    super.key,
    required this.data,
  });

  final List<double> data;

  @override
  Widget build(BuildContext context) {
    // Good: RepaintBoundary prevents unnecessary repaints
    return RepaintBoundary(
      child: Container(
        height: 200,
        padding: const EdgeInsets.all(16),
        child: CustomPaint(
          size: const Size.fromHeight(200),
          painter: ChartPainter(data),
        ),
      ),
    );
  }
}

class ChartPainter extends CustomPainter {
  ChartPainter(this.data);
  
  final List<double> data;

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..color = Colors.blue
      ..strokeWidth = 2
      ..style = PaintingStyle.stroke;

    final path = Path();
    
    for (int i = 0; i < data.length; i++) {
      final x = (i / (data.length - 1)) * size.width;
      final y = size.height - (data[i] * size.height);
      
      if (i == 0) {
        path.moveTo(x, y);
      } else {
        path.lineTo(x, y);
      }
    }
    
    canvas.drawPath(path, paint);
  }

  @override
  bool shouldRepaint(ChartPainter oldDelegate) {
    // Good: Only repaint when data actually changes
    return oldDelegate.data != data;
  }
}

// Good: Memoized expensive computation
class ExpensiveComputation {
  static final Map<String, int> _cache = {};
  
  static int computeExpensiveValue(String input) {
    if (_cache.containsKey(input)) {
      return _cache[input]!;
    }
    
    // Simulate expensive computation
    int result = 0;
    for (int i = 0; i < input.length * 1000; i++) {
      result += i.hashCode;
    }
    
    _cache[input] = result;
    return result;
  }
  
  static void clearCache() {
    _cache.clear();
  }
}

class PerformanceDemo extends StatefulWidget {
  const PerformanceDemo({super.key});

  @override
  State<PerformanceDemo> createState() => _PerformanceDemoState();
}

class _PerformanceDemoState extends State<PerformanceDemo>
    with SingleTickerProviderStateMixin {
  late TabController _tabController;
  
  // Good: Static const data that doesn't change
  static const List<String> performanceTips = [
    'Use const constructors when possible',
    'Implement shouldRepaint in CustomPainter',
    'Use RepaintBoundary for expensive widgets',
    'Cache expensive computations',
    'Use ListView.builder for long lists',
    'Minimize widget rebuilds',
    'Use keys for list items',
    'Implement proper shouldRebuild logic',
  ];
  
  final List<double> _chartData = List.generate(
    20,
    (index) => (index * 0.05).clamp(0.0, 1.0),
  );

  @override
  void initState() {
    super.initState();
    _tabController = TabController(length: 3, vsync: this);
  }

  @override
  void dispose() {
    _tabController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Performance Optimization'),
        bottom: TabBar(
          controller: _tabController,
          tabs: const [
            Tab(text: 'Optimization', icon: Icon(Icons.speed)),
            Tab(text: 'Lists', icon: Icon(Icons.list)),
            Tab(text: 'Graphics', icon: Icon(Icons.show_chart)),
          ],
        ),
      ),
      body: TabBarView(
        controller: _tabController,
        children: [
          _buildOptimizationTab(),
          _buildListsTab(),
          _buildGraphicsTab(),
        ],
      ),
    );
  }

  Widget _buildOptimizationTab() {
    return SingleChildScrollView(
      padding: const EdgeInsets.all(16),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          const Card(
            child: Padding(
              padding: EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Performance Optimization Best Practices',
                    style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                  ),
                  SizedBox(height: 8),
                  Text('• Use const constructors to prevent unnecessary rebuilds'),
                  Text('• Implement RepaintBoundary for expensive custom widgets'),
                  Text('• Cache expensive computations and API responses'),
                  Text('• Use ListView.builder instead of ListView for long lists'),
                  Text('• Minimize the number of widgets in the widget tree'),
                  Text('• Use keys for stateful widgets in lists'),
                  Text('• Avoid rebuilding widgets unnecessarily'),
                  Text('• Profile your app regularly to identify bottlenecks'),
                ],
              ),
            ),
          ),
          const SizedBox(height: 16),
          
          // Performance metrics demo
          _buildPerformanceMetrics(),
          const SizedBox(height: 16),
          
          // Expensive computation demo
          _buildComputationDemo(),
        ],
      ),
    );
  }

  Widget _buildListsTab() {
    return Column(
      children: [
        const Padding(
          padding: EdgeInsets.all(16),
          child: Card(
            child: Padding(
              padding: EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Optimized List Performance',
                    style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                  ),
                  SizedBox(height: 8),
                  Text('• This list uses ListView.builder for optimal performance'),
                  Text('• Each item has a const constructor'),
                  Text('• Items are cached and reused efficiently'),
                  Text('• Scrolling performance remains smooth even with many items'),
                ],
              ),
            ),
          ),
        ),
        
        // Good: Efficient list with ListView.builder
        Expanded(
          child: ListView.builder(
            // Good: Using itemExtent for better performance
            itemExtent: 72,
            itemCount: 1000, // Large list for performance testing
            itemBuilder: (context, index) {
              return OptimizedListItem(
                key: ValueKey(index), // Good: Using keys for list items
                index: index,
                title: 'Optimized Item ${index + 1}',
                subtitle: 'This is a performance-optimized list item',
              );
            },
          ),
        ),
      ],
    );
  }

  Widget _buildGraphicsTab() {
    return SingleChildScrollView(
      padding: const EdgeInsets.all(16),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          const Card(
            child: Padding(
              padding: EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Graphics Performance',
                    style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                  ),
                  SizedBox(height: 8),
                  Text('• Use RepaintBoundary to isolate expensive custom painting'),
                  Text('• Implement proper shouldRepaint logic in CustomPainter'),
                  Text('• Cache complex graphics operations'),
                  Text('• Use appropriate image formats and sizes'),
                  Text('• Minimize overdraw in complex layouts'),
                ],
              ),
            ),
          ),
          const SizedBox(height: 16),
          
          // Performance chart with RepaintBoundary
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  const Text(
                    'Performance Chart (Optimized)',
                    style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                  ),
                  const SizedBox(height: 8),
                  PerformanceChart(data: _chartData),
                  const SizedBox(height: 8),
                  const Text(
                    'This chart uses RepaintBoundary and efficient shouldRepaint logic.',
                    style: TextStyle(fontSize: 12, color: Colors.grey),
                  ),
                ],
              ),
            ),
          ),
          const SizedBox(height: 16),
          
          // Animation performance demo
          _buildAnimationDemo(),
        ],
      ),
    );
  }

  Widget _buildPerformanceMetrics() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Performance Metrics',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            // Memory usage indicator
            _buildMetricRow('Memory Usage', '${_getRandomMetric()}MB', Colors.blue),
            _buildMetricRow('CPU Usage', '${_getRandomMetric()}%', Colors.green),
            _buildMetricRow('GPU Usage', '${_getRandomMetric()}%', Colors.orange),
            _buildMetricRow('Frame Rate', '${60 - _getRandomMetric()}fps', Colors.purple),
            
            const SizedBox(height: 12),
            const Text(
              'Monitor these metrics during development to identify performance issues.',
              style: TextStyle(fontSize: 12, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildMetricRow(String label, String value, Color color) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(label),
          Container(
            padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 2),
            decoration: BoxDecoration(
              color: color.withOpacity(0.2),
              borderRadius: BorderRadius.circular(12),
            ),
            child: Text(
              value,
              style: TextStyle(
                color: color,
                fontWeight: FontWeight.bold,
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildComputationDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Expensive Computation Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            const Text(
              'This demo shows caching of expensive computations. Try clicking '
              'the same input multiple times to see caching in action.',
            ),
            const SizedBox(height: 12),
            
            Wrap(
              spacing: 8,
              children: ['Flutter', 'Dart', 'Performance', 'Cache'].map(
                (input) => ElevatedButton(
                  onPressed: () => _performExpensiveComputation(input),
                  child: Text(input),
                ),
              ).toList(),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: () {
                ExpensiveComputation.clearCache();
                ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(content: Text('Cache cleared')),
                );
              },
              style: ElevatedButton.styleFrom(backgroundColor: Colors.red),
              child: const Text('Clear Cache'),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildAnimationDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Animation Performance',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            const Text(
              'Use AnimatedBuilder and RepaintBoundary for smooth animations.',
            ),
            const SizedBox(height: 12),
            
            // Good: Optimized animation with RepaintBoundary
            RepaintBoundary(
              child: TweenAnimationBuilder<double>(
                duration: const Duration(seconds: 2),
                tween: Tween<double>(begin: 0, end: 1),
                builder: (context, value, child) {
                  return LinearProgressIndicator(
                    value: value,
                    backgroundColor: Colors.grey.shade300,
                    valueColor: AlwaysStoppedAnimation<Color>(
                      Color.lerp(Colors.red, Colors.green, value)!,
                    ),
                  );
                },
              ),
            ),
            const SizedBox(height: 8),
            const Text(
              'This animation is optimized with RepaintBoundary.',
              style: TextStyle(fontSize: 12, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }

  void _performExpensiveComputation(String input) {
    final stopwatch = Stopwatch()..start();
    final result = ExpensiveComputation.computeExpensiveValue(input);
    stopwatch.stop();
    
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(
          'Computation for "$input": $result\n'
          'Time: ${stopwatch.elapsedMilliseconds}ms',
        ),
        duration: const Duration(seconds: 2),
      ),
    );
  }

  int _getRandomMetric() {
    return DateTime.now().millisecondsSinceEpoch % 50 + 10;
  }
}
```

Performance optimization is crucial for creating smooth Flutter applications.  
Use const constructors, implement RepaintBoundary for expensive widgets,  
cache computations, and use ListView.builder for long lists. Regular  
profiling helps identify performance bottlenecks and optimization opportunities.

## State Management Best Practices

Implementing effective state management patterns to handle application  
state efficiently and maintainably.  

```dart
import 'package:flutter/material.dart';

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
      title: 'Best Practices - State Management',
      home: const StateManagementDemo(),
    );
  }
}

// Good: Clean state management with ChangeNotifier
class CounterModel extends ChangeNotifier {
  int _count = 0;
  bool _isIncrementing = true;
  
  int get count => _count;
  bool get isIncrementing => _isIncrementing;
  
  void increment() {
    _count++;
    notifyListeners();
  }
  
  void decrement() {
    _count--;
    notifyListeners();
  }
  
  void toggleDirection() {
    _isIncrementing = !_isIncrementing;
    notifyListeners();
  }
  
  void reset() {
    _count = 0;
    _isIncrementing = true;
    notifyListeners();
  }
  
  void updateCount(int newCount) {
    if (newCount != _count) {
      _count = newCount;
      notifyListeners();
    }
  }
}

// Good: Immutable state class
class AppSettings {
  const AppSettings({
    required this.theme,
    required this.language,
    required this.notificationsEnabled,
    required this.autoSave,
  });
  
  final ThemeMode theme;
  final String language;
  final bool notificationsEnabled;
  final bool autoSave;
  
  AppSettings copyWith({
    ThemeMode? theme,
    String? language,
    bool? notificationsEnabled,
    bool? autoSave,
  }) {
    return AppSettings(
      theme: theme ?? this.theme,
      language: language ?? this.language,
      notificationsEnabled: notificationsEnabled ?? this.notificationsEnabled,
      autoSave: autoSave ?? this.autoSave,
    );
  }
  
  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    return other is AppSettings &&
        other.theme == theme &&
        other.language == language &&
        other.notificationsEnabled == notificationsEnabled &&
        other.autoSave == autoSave;
  }
  
  @override
  int get hashCode {
    return theme.hashCode ^
        language.hashCode ^
        notificationsEnabled.hashCode ^
        autoSave.hashCode;
  }
}

// Good: Settings manager with proper state management
class SettingsManager extends ChangeNotifier {
  AppSettings _settings = const AppSettings(
    theme: ThemeMode.system,
    language: 'English',
    notificationsEnabled: true,
    autoSave: true,
  );
  
  AppSettings get settings => _settings;
  
  void updateSettings(AppSettings newSettings) {
    if (newSettings != _settings) {
      _settings = newSettings;
      notifyListeners();
      _saveSettings();
    }
  }
  
  void updateTheme(ThemeMode theme) {
    updateSettings(_settings.copyWith(theme: theme));
  }
  
  void updateLanguage(String language) {
    updateSettings(_settings.copyWith(language: language));
  }
  
  void toggleNotifications() {
    updateSettings(_settings.copyWith(
      notificationsEnabled: !_settings.notificationsEnabled,
    ));
  }
  
  void toggleAutoSave() {
    updateSettings(_settings.copyWith(
      autoSave: !_settings.autoSave,
    ));
  }
  
  Future<void> _saveSettings() async {
    // Simulate saving to persistent storage
    await Future.delayed(const Duration(milliseconds: 100));
  }
  
  Future<void> loadSettings() async {
    // Simulate loading from persistent storage
    await Future.delayed(const Duration(milliseconds: 200));
    notifyListeners();
  }
}

// Good: State provider widget
class StateProvider<T extends ChangeNotifier> extends InheritedNotifier<T> {
  const StateProvider({
    super.key,
    required super.notifier,
    required super.child,
  });
  
  static T of<T extends ChangeNotifier>(BuildContext context) {
    final provider = context.dependOnInheritedWidgetOfExactType<StateProvider<T>>();
    if (provider == null) {
      throw StateError('No StateProvider<$T> found in context');
    }
    return provider.notifier!;
  }
  
  static T? maybeOf<T extends ChangeNotifier>(BuildContext context) {
    final provider = context.dependOnInheritedWidgetOfExactType<StateProvider<T>>();
    return provider?.notifier;
  }
}

// Good: Consumer widget for efficient rebuilds
class StateConsumer<T extends ChangeNotifier> extends StatelessWidget {
  const StateConsumer({
    super.key,
    required this.builder,
    this.child,
  });
  
  final Widget Function(BuildContext context, T state, Widget? child) builder;
  final Widget? child;
  
  @override
  Widget build(BuildContext context) {
    final state = StateProvider.of<T>(context);
    return builder(context, state, child);
  }
}

class StateManagementDemo extends StatelessWidget {
  const StateManagementDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return StateProvider<CounterModel>(
      notifier: CounterModel(),
      child: StateProvider<SettingsManager>(
        notifier: SettingsManager(),
        child: const StateManagementScreen(),
      ),
    );
  }
}

class StateManagementScreen extends StatefulWidget {
  const StateManagementScreen({super.key});

  @override
  State<StateManagementScreen> createState() => _StateManagementScreenState();
}

class _StateManagementScreenState extends State<StateManagementScreen> {
  @override
  void initState() {
    super.initState();
    // Load settings on initialization
    WidgetsBinding.instance.addPostFrameCallback((_) {
      StateProvider.of<SettingsManager>(context).loadSettings();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('State Management'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildStateManagementInfo(),
            const SizedBox(height: 16),
            _buildCounterDemo(),
            const SizedBox(height: 16),
            _buildSettingsDemo(),
            const SizedBox(height: 16),
            _buildStateAnalysis(),
          ],
        ),
      ),
    );
  }

  Widget _buildStateManagementInfo() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'State Management Best Practices',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text('• Use ChangeNotifier for simple state management'),
            Text('• Implement immutable state classes with copyWith'),
            Text('• Separate business logic from UI components'),
            Text('• Use InheritedWidget for dependency injection'),
            Text('• Optimize rebuilds with specific consumer widgets'),
            Text('• Handle loading and error states appropriately'),
            Text('• Implement proper state persistence when needed'),
            Text('• Use proper naming conventions for state classes'),
          ],
        ),
      ),
    );
  }

  Widget _buildCounterDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Counter State Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            // Good: Using StateConsumer for efficient rebuilds
            StateConsumer<CounterModel>(
              builder: (context, counter, child) {
                return Column(
                  children: [
                    Text(
                      'Count: ${counter.count}',
                      style: const TextStyle(
                        fontSize: 32,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                      children: [
                        ElevatedButton(
                          onPressed: counter.decrement,
                          child: const Text('-'),
                        ),
                        ElevatedButton(
                          onPressed: counter.increment,
                          child: const Text('+'),
                        ),
                        ElevatedButton(
                          onPressed: counter.reset,
                          child: const Text('Reset'),
                        ),
                      ],
                    ),
                    const SizedBox(height: 8),
                    Row(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        const Text('Auto: '),
                        Text(
                          counter.isIncrementing ? 'Incrementing' : 'Decrementing',
                          style: TextStyle(
                            color: counter.isIncrementing ? Colors.green : Colors.red,
                          ),
                        ),
                        IconButton(
                          onPressed: counter.toggleDirection,
                          icon: const Icon(Icons.swap_horiz),
                        ),
                      ],
                    ),
                  ],
                );
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSettingsDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Settings State Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            StateConsumer<SettingsManager>(
              builder: (context, settingsManager, child) {
                final settings = settingsManager.settings;
                
                return Column(
                  children: [
                    _buildSettingTile(
                      'Theme',
                      _getThemeName(settings.theme),
                      Icons.palette,
                      () => _showThemeSelector(context, settingsManager),
                    ),
                    _buildSettingTile(
                      'Language',
                      settings.language,
                      Icons.language,
                      () => _showLanguageSelector(context, settingsManager),
                    ),
                    SwitchListTile(
                      title: const Text('Notifications'),
                      subtitle: const Text('Receive push notifications'),
                      value: settings.notificationsEnabled,
                      onChanged: (_) => settingsManager.toggleNotifications(),
                      secondary: const Icon(Icons.notifications),
                    ),
                    SwitchListTile(
                      title: const Text('Auto Save'),
                      subtitle: const Text('Automatically save changes'),
                      value: settings.autoSave,
                      onChanged: (_) => settingsManager.toggleAutoSave(),
                      secondary: const Icon(Icons.save),
                    ),
                  ],
                );
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSettingTile(
    String title,
    String value,
    IconData icon,
    VoidCallback onTap,
  ) {
    return ListTile(
      leading: Icon(icon),
      title: Text(title),
      subtitle: Text(value),
      trailing: const Icon(Icons.arrow_forward_ios),
      onTap: onTap,
    );
  }

  Widget _buildStateAnalysis() {
    return StateConsumer<CounterModel>(
      builder: (context, counter, child) {
        return StateConsumer<SettingsManager>(
          builder: (context, settingsManager, child) {
            return Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'State Analysis',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    _buildAnalysisRow('Counter Value', '${counter.count}'),
                    _buildAnalysisRow('Counter Direction', 
                      counter.isIncrementing ? 'Incrementing' : 'Decrementing'),
                    _buildAnalysisRow('Theme Mode', 
                      _getThemeName(settingsManager.settings.theme)),
                    _buildAnalysisRow('Notifications', 
                      settingsManager.settings.notificationsEnabled ? 'Enabled' : 'Disabled'),
                    _buildAnalysisRow('Auto Save', 
                      settingsManager.settings.autoSave ? 'Enabled' : 'Disabled'),
                    const SizedBox(height: 12),
                    const Text(
                      'Both state objects update independently and efficiently '
                      'notify only the widgets that depend on them.',
                      style: TextStyle(fontSize: 12, color: Colors.grey),
                    ),
                  ],
                ),
              ),
            );
          },
        );
      },
    );
  }

  Widget _buildAnalysisRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 2),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(label),
          Text(
            value,
            style: const TextStyle(fontWeight: FontWeight.bold),
          ),
        ],
      ),
    );
  }

  String _getThemeName(ThemeMode theme) {
    switch (theme) {
      case ThemeMode.light:
        return 'Light';
      case ThemeMode.dark:
        return 'Dark';
      case ThemeMode.system:
        return 'System';
    }
  }

  void _showThemeSelector(BuildContext context, SettingsManager settingsManager) {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Select Theme'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            _buildThemeOption('Light', ThemeMode.light, settingsManager),
            _buildThemeOption('Dark', ThemeMode.dark, settingsManager),
            _buildThemeOption('System', ThemeMode.system, settingsManager),
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

  Widget _buildThemeOption(String name, ThemeMode mode, SettingsManager settingsManager) {
    return ListTile(
      title: Text(name),
      leading: Radio<ThemeMode>(
        value: mode,
        groupValue: settingsManager.settings.theme,
        onChanged: (value) {
          if (value != null) {
            settingsManager.updateTheme(value);
            Navigator.pop(context);
          }
        },
      ),
      onTap: () {
        settingsManager.updateTheme(mode);
        Navigator.pop(context);
      },
    );
  }

  void _showLanguageSelector(BuildContext context, SettingsManager settingsManager) {
    final languages = ['English', 'Spanish', 'French', 'German', 'Chinese'];
    
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Select Language'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: languages.map(
            (language) => ListTile(
              title: Text(language),
              leading: Radio<String>(
                value: language,
                groupValue: settingsManager.settings.language,
                onChanged: (value) {
                  if (value != null) {
                    settingsManager.updateLanguage(value);
                    Navigator.pop(context);
                  }
                },
              ),
              onTap: () {
                settingsManager.updateLanguage(language);
                Navigator.pop(context);
              },
            ),
          ).toList(),
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

Effective state management is fundamental to building maintainable Flutter  
applications. Use ChangeNotifier for simple state, implement immutable  
state classes, separate business logic from UI, and optimize rebuilds  
with consumer widgets. Proper state management improves performance  
and makes applications easier to debug and maintain.  

## Security and Data Protection

Implementing security best practices to protect user data and ensure  
application security across different platforms.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'dart:convert';
import 'dart:math';

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
      title: 'Best Practices - Security',
      home: const SecurityDemo(),
    );
  }
}

// Good: Secure data handling utility
class SecurityUtils {
  static const String _chars = 
      'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
  static final Random _random = Random.secure();
  
  // Good: Generate secure random strings
  static String generateSecureToken(int length) {
    return List.generate(
      length,
      (index) => _chars[_random.nextInt(_chars.length)],
    ).join();
  }
  
  // Good: Basic password strength validation
  static PasswordStrength validatePasswordStrength(String password) {
    if (password.length < 8) {
      return PasswordStrength.weak;
    }
    
    bool hasUppercase = password.contains(RegExp(r'[A-Z]'));
    bool hasLowercase = password.contains(RegExp(r'[a-z]'));
    bool hasNumbers = password.contains(RegExp(r'[0-9]'));
    bool hasSpecialChars = password.contains(RegExp(r'[!@#$%^&*(),.?":{}|<>]'));
    
    int strength = 0;
    if (hasUppercase) strength++;
    if (hasLowercase) strength++;
    if (hasNumbers) strength++;
    if (hasSpecialChars) strength++;
    if (password.length >= 12) strength++;
    
    if (strength >= 4) return PasswordStrength.strong;
    if (strength >= 3) return PasswordStrength.medium;
    return PasswordStrength.weak;
  }
  
  // Good: Input sanitization
  static String sanitizeInput(String input) {
    return input
        .replaceAll(RegExp(r'[<>"\'\&]'), '')
        .trim();
  }
  
  // Good: Email validation
  static bool isValidEmail(String email) {
    return RegExp(r'^[^@]+@[^@]+\.[^@]+').hasMatch(email);
  }
  
  // Good: Secure storage simulation (in real app, use flutter_secure_storage)
  static Map<String, String> _secureStorage = {};
  
  static Future<void> storeSecurely(String key, String value) async {
    // Simulate encryption and secure storage
    await Future.delayed(const Duration(milliseconds: 100));
    _secureStorage[key] = base64Encode(utf8.encode(value));
  }
  
  static Future<String?> retrieveSecurely(String key) async {
    await Future.delayed(const Duration(milliseconds: 100));
    final encodedValue = _secureStorage[key];
    if (encodedValue != null) {
      return utf8.decode(base64Decode(encodedValue));
    }
    return null;
  }
  
  static Future<void> deleteSecurely(String key) async {
    await Future.delayed(const Duration(milliseconds: 50));
    _secureStorage.remove(key);
  }
}

enum PasswordStrength { weak, medium, strong }

// Good: Secure user authentication model
class UserAuthentication {
  UserAuthentication() {
    _loadStoredCredentials();
  }
  
  bool _isAuthenticated = false;
  String? _currentUser;
  DateTime? _lastLoginTime;
  int _failedAttempts = 0;
  static const int maxFailedAttempts = 3;
  
  bool get isAuthenticated => _isAuthenticated;
  String? get currentUser => _currentUser;
  bool get isLocked => _failedAttempts >= maxFailedAttempts;
  int get remainingAttempts => maxFailedAttempts - _failedAttempts;
  
  Future<AuthResult> authenticate(String username, String password) async {
    if (isLocked) {
      return AuthResult.accountLocked;
    }
    
    // Simulate authentication delay (prevents brute force)
    await Future.delayed(const Duration(seconds: 1));
    
    // Good: Validate input
    if (username.trim().isEmpty || password.isEmpty) {
      return AuthResult.invalidCredentials;
    }
    
    // Simulate authentication logic
    if (_validateCredentials(username, password)) {
      _isAuthenticated = true;
      _currentUser = username;
      _lastLoginTime = DateTime.now();
      _failedAttempts = 0;
      
      // Good: Store session securely
      await SecurityUtils.storeSecurely('session_token', 
          SecurityUtils.generateSecureToken(32));
      await SecurityUtils.storeSecurely('last_login', 
          DateTime.now().toIso8601String());
      
      return AuthResult.success;
    } else {
      _failedAttempts++;
      return AuthResult.invalidCredentials;
    }
  }
  
  Future<void> logout() async {
    _isAuthenticated = false;
    _currentUser = null;
    _lastLoginTime = null;
    
    // Good: Clear sensitive data
    await SecurityUtils.deleteSecurely('session_token');
  }
  
  void resetFailedAttempts() {
    _failedAttempts = 0;
  }
  
  bool _validateCredentials(String username, String password) {
    // Simulate credential validation
    return username == 'demo' && password == 'SecurePass123!';
  }
  
  Future<void> _loadStoredCredentials() async {
    final sessionToken = await SecurityUtils.retrieveSecurely('session_token');
    if (sessionToken != null) {
      // Validate session and restore if valid
      _isAuthenticated = true;
      _currentUser = 'demo';
    }
  }
}

enum AuthResult { success, invalidCredentials, accountLocked, networkError }

class SecurityDemo extends StatefulWidget {
  const SecurityDemo({super.key});

  @override
  State<SecurityDemo> createState() => _SecurityDemoState();
}

class _SecurityDemoState extends State<SecurityDemo> {
  final UserAuthentication _auth = UserAuthentication();
  final TextEditingController _usernameController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();
  final TextEditingController _inputController = TextEditingController();
  
  bool _passwordVisible = false;
  PasswordStrength _passwordStrength = PasswordStrength.weak;

  @override
  void dispose() {
    _usernameController.dispose();
    _passwordController.dispose();
    _inputController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Security Best Practices'),
        actions: [
          if (_auth.isAuthenticated)
            IconButton(
              onPressed: _logout,
              icon: const Icon(Icons.logout),
              tooltip: 'Logout',
            ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildSecurityInfo(),
            const SizedBox(height: 16),
            if (_auth.isAuthenticated) ...[
              _buildAuthenticatedContent(),
            ] else ...[
              _buildAuthenticationSection(),
            ],
            const SizedBox(height: 16),
            _buildPasswordStrengthDemo(),
            const SizedBox(height: 16),
            _buildInputSanitizationDemo(),
            const SizedBox(height: 16),
            _buildSecureStorageDemo(),
          ],
        ),
      ),
    );
  }

  Widget _buildSecurityInfo() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Security Best Practices',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text('• Never store sensitive data in plain text'),
            Text('• Use secure storage for credentials and tokens'),
            Text('• Implement proper authentication and authorization'),
            Text('• Validate and sanitize all user inputs'),
            Text('• Use HTTPS for all network communications'),
            Text('• Implement rate limiting for login attempts'),
            Text('• Follow principle of least privilege'),
            Text('• Regularly update dependencies for security patches'),
          ],
        ),
      ),
    );
  }

  Widget _buildAuthenticationSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Secure Authentication',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            if (_auth.isLocked) ...[
              Container(
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: Colors.red.shade100,
                  borderRadius: BorderRadius.circular(8),
                  border: Border.all(color: Colors.red),
                ),
                child: Row(
                  children: [
                    const Icon(Icons.lock, color: Colors.red),
                    const SizedBox(width: 8),
                    const Expanded(
                      child: Text(
                        'Account locked due to multiple failed attempts. '
                        'Please try again later.',
                        style: TextStyle(color: Colors.red),
                      ),
                    ),
                  ],
                ),
              ),
              const SizedBox(height: 12),
              ElevatedButton(
                onPressed: () {
                  setState(() {
                    _auth.resetFailedAttempts();
                  });
                },
                child: const Text('Reset Attempts'),
              ),
            ] else ...[
              TextField(
                controller: _usernameController,
                decoration: const InputDecoration(
                  labelText: 'Username',
                  prefixIcon: Icon(Icons.person),
                  border: OutlineInputBorder(),
                ),
                textInputAction: TextInputAction.next,
              ),
              const SizedBox(height: 12),
              TextField(
                controller: _passwordController,
                obscureText: !_passwordVisible,
                decoration: InputDecoration(
                  labelText: 'Password',
                  prefixIcon: const Icon(Icons.lock),
                  suffixIcon: IconButton(
                    icon: Icon(_passwordVisible 
                        ? Icons.visibility 
                        : Icons.visibility_off),
                    onPressed: () {
                      setState(() {
                        _passwordVisible = !_passwordVisible;
                      });
                    },
                  ),
                  border: const OutlineInputBorder(),
                ),
                onSubmitted: (_) => _authenticate(),
              ),
              const SizedBox(height: 8),
              if (_auth.remainingAttempts < 3)
                Text(
                  'Remaining attempts: ${_auth.remainingAttempts}',
                  style: const TextStyle(color: Colors.orange),
                ),
              const SizedBox(height: 12),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: _authenticate,
                  child: const Text('Login'),
                ),
              ),
              const SizedBox(height: 8),
              const Text(
                'Demo credentials: username="demo", password="SecurePass123!"',
                style: TextStyle(fontSize: 12, color: Colors.grey),
              ),
            ],
          ],
        ),
      ),
    );
  }

  Widget _buildAuthenticatedContent() {
    return Card(
      color: Colors.green.shade50,
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const Icon(Icons.check_circle, color: Colors.green),
                const SizedBox(width: 8),
                const Text(
                  'Authentication Successful',
                  style: TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                    color: Colors.green,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            Text('Welcome, ${_auth.currentUser}!'),
            const SizedBox(height: 12),
            const Text(
              'Secure Features:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const Text('• Session token generated and stored securely'),
            const Text('• Failed attempts counter reset'),
            const Text('• Login time recorded for audit'),
            const Text('• Sensitive operations now available'),
          ],
        ),
      ),
    );
  }

  Widget _buildPasswordStrengthDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Password Strength Validator',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            TextField(
              onChanged: (value) {
                setState(() {
                  _passwordStrength = SecurityUtils.validatePasswordStrength(value);
                });
              },
              decoration: const InputDecoration(
                labelText: 'Test Password Strength',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 8),
            LinearProgressIndicator(
              value: _getStrengthValue(_passwordStrength),
              backgroundColor: Colors.grey.shade300,
              valueColor: AlwaysStoppedAnimation<Color>(
                _getStrengthColor(_passwordStrength),
              ),
            ),
            const SizedBox(height: 4),
            Text(
              'Strength: ${_getStrengthText(_passwordStrength)}',
              style: TextStyle(
                color: _getStrengthColor(_passwordStrength),
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 8),
            const Text(
              'Strong passwords should have:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const Text('• At least 8 characters (12+ recommended)'),
            const Text('• Uppercase and lowercase letters'),
            const Text('• Numbers and special characters'),
            const Text('• No common words or patterns'),
          ],
        ),
      ),
    );
  }

  Widget _buildInputSanitizationDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Input Sanitization',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            TextField(
              controller: _inputController,
              decoration: const InputDecoration(
                labelText: 'Test Input (try entering <script> or special chars)',
                border: OutlineInputBorder(),
              ),
              onChanged: (value) => setState(() {}),
            ),
            const SizedBox(height: 12),
            if (_inputController.text.isNotEmpty) ...[
              Container(
                width: double.infinity,
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: Colors.grey.shade100,
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Original Input:',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    Text(_inputController.text),
                    const SizedBox(height: 8),
                    const Text(
                      'Sanitized Input:',
                      style: TextStyle(fontWeight: FontWeight.bold, color: Colors.green),
                    ),
                    Text(
                      SecurityUtils.sanitizeInput(_inputController.text),
                      style: const TextStyle(color: Colors.green),
                    ),
                  ],
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }

  Widget _buildSecureStorageDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Secure Storage',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            const Text(
              'In production apps, use flutter_secure_storage package '
              'for storing sensitive data like tokens, passwords, and API keys.',
            ),
            const SizedBox(height: 12),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _storeSecureData,
                    child: const Text('Store Secure Token'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _retrieveSecureData,
                    child: const Text('Retrieve Token'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            const Text(
              'Security features:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const Text('• Data encrypted at rest'),
            const Text('• Platform-specific secure storage'),
            const Text('• Automatic key management'),
            const Text('• Protection against unauthorized access'),
          ],
        ),
      ),
    );
  }

  double _getStrengthValue(PasswordStrength strength) {
    switch (strength) {
      case PasswordStrength.weak:
        return 0.33;
      case PasswordStrength.medium:
        return 0.66;
      case PasswordStrength.strong:
        return 1.0;
    }
  }

  Color _getStrengthColor(PasswordStrength strength) {
    switch (strength) {
      case PasswordStrength.weak:
        return Colors.red;
      case PasswordStrength.medium:
        return Colors.orange;
      case PasswordStrength.strong:
        return Colors.green;
    }
  }

  String _getStrengthText(PasswordStrength strength) {
    switch (strength) {
      case PasswordStrength.weak:
        return 'Weak';
      case PasswordStrength.medium:
        return 'Medium';
      case PasswordStrength.strong:
        return 'Strong';
    }
  }

  Future<void> _authenticate() async {
    final result = await _auth.authenticate(
      _usernameController.text,
      _passwordController.text,
    );
    
    if (mounted) {
      switch (result) {
        case AuthResult.success:
          setState(() {});
          ScaffoldMessenger.of(context).showSnackBar(
            const SnackBar(
              content: Text('Login successful!'),
              backgroundColor: Colors.green,
            ),
          );
          break;
        case AuthResult.invalidCredentials:
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              content: Text('Invalid credentials. ${_auth.remainingAttempts} attempts remaining.'),
              backgroundColor: Colors.red,
            ),
          );
          setState(() {});
          break;
        case AuthResult.accountLocked:
          ScaffoldMessenger.of(context).showSnackBar(
            const SnackBar(
              content: Text('Account locked. Too many failed attempts.'),
              backgroundColor: Colors.red,
            ),
          );
          setState(() {});
          break;
        case AuthResult.networkError:
          ScaffoldMessenger.of(context).showSnackBar(
            const SnackBar(
              content: Text('Network error. Please try again.'),
              backgroundColor: Colors.orange,
            ),
          );
          break;
      }
    }
  }

  Future<void> _logout() async {
    await _auth.logout();
    setState(() {
      _usernameController.clear();
      _passwordController.clear();
    });
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Logged out successfully'),
        ),
      );
    }
  }

  Future<void> _storeSecureData() async {
    final token = SecurityUtils.generateSecureToken(32);
    await SecurityUtils.storeSecurely('demo_token', token);
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Secure token stored successfully'),
          backgroundColor: Colors.green,
        ),
      );
    }
  }

  Future<void> _retrieveSecureData() async {
    final token = await SecurityUtils.retrieveSecurely('demo_token');
    
    if (mounted) {
      if (token != null) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(
            content: Text('Token retrieved: ${token.substring(0, 8)}...'),
            backgroundColor: Colors.green,
          ),
        );
      } else {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            content: Text('No token found. Store one first.'),
            backgroundColor: Colors.orange,
          ),
        );
      }
    }
  }
}
```

Security is paramount in Flutter applications. Implement proper authentication,  
use secure storage for sensitive data, validate and sanitize inputs, and  
follow security best practices. Never store sensitive data in plain text,  
implement rate limiting, and regularly update dependencies for security patches.

## Testing Integration and Quality Assurance

Implementing comprehensive testing strategies to ensure code quality,  
reliability, and maintainability across the application lifecycle.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

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
      title: 'Best Practices - Testing',
      home: const TestingDemo(),
    );
  }
}

// Good: Testable business logic separated from UI
class CalculatorService {
  double add(double a, double b) => a + b;
  double subtract(double a, double b) => a - b;
  double multiply(double a, double b) => a * b;
  
  double divide(double a, double b) {
    if (b == 0) {
      throw ArgumentError('Cannot divide by zero');
    }
    return a / b;
  }
  
  bool isEven(int number) => number % 2 == 0;
  bool isPrime(int number) {
    if (number < 2) return false;
    for (int i = 2; i <= number / 2; i++) {
      if (number % i == 0) return false;
    }
    return true;
  }
}

// Good: Testable data model with validation
class UserProfile {
  UserProfile({
    required this.name,
    required this.email,
    required this.age,
  }) {
    _validate();
  }
  
  final String name;
  final String email;
  final int age;
  
  void _validate() {
    if (name.trim().isEmpty) {
      throw ArgumentError('Name cannot be empty');
    }
    if (!email.contains('@')) {
      throw ArgumentError('Invalid email format');
    }
    if (age < 0 || age > 150) {
      throw ArgumentError('Invalid age range');
    }
  }
  
  bool get isAdult => age >= 18;
  bool get isSenior => age >= 65;
  
  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    return other is UserProfile &&
        other.name == name &&
        other.email == email &&
        other.age == age;
  }
  
  @override
  int get hashCode => name.hashCode ^ email.hashCode ^ age.hashCode;
}

// Good: Widget that's easy to test
class CalculatorWidget extends StatefulWidget {
  const CalculatorWidget({super.key, required this.calculator});
  
  final CalculatorService calculator;
  
  @override
  State<CalculatorWidget> createState() => _CalculatorWidgetState();
}

class _CalculatorWidgetState extends State<CalculatorWidget> {
  final TextEditingController _aController = TextEditingController();
  final TextEditingController _bController = TextEditingController();
  String _result = '';
  String _operation = 'add';

  @override
  void dispose() {
    _aController.dispose();
    _bController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Testable Calculator',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            // Input fields with test keys
            TextField(
              key: const Key('calculator_input_a'),
              controller: _aController,
              decoration: const InputDecoration(
                labelText: 'Number A',
                border: OutlineInputBorder(),
              ),
              keyboardType: TextInputType.number,
            ),
            const SizedBox(height: 8),
            
            TextField(
              key: const Key('calculator_input_b'),
              controller: _bController,
              decoration: const InputDecoration(
                labelText: 'Number B',
                border: OutlineInputBorder(),
              ),
              keyboardType: TextInputType.number,
            ),
            const SizedBox(height: 12),
            
            // Operation selector
            DropdownButtonFormField<String>(
              key: const Key('calculator_operation'),
              value: _operation,
              decoration: const InputDecoration(
                labelText: 'Operation',
                border: OutlineInputBorder(),
              ),
              items: const [
                DropdownMenuItem(value: 'add', child: Text('Add')),
                DropdownMenuItem(value: 'subtract', child: Text('Subtract')),
                DropdownMenuItem(value: 'multiply', child: Text('Multiply')),
                DropdownMenuItem(value: 'divide', child: Text('Divide')),
              ],
              onChanged: (value) {
                setState(() {
                  _operation = value!;
                });
              },
            ),
            const SizedBox(height: 12),
            
            // Calculate button
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                key: const Key('calculator_calculate'),
                onPressed: _calculate,
                child: const Text('Calculate'),
              ),
            ),
            const SizedBox(height: 12),
            
            // Result display
            Container(
              key: const Key('calculator_result'),
              width: double.infinity,
              padding: const EdgeInsets.all(12),
              decoration: BoxDecoration(
                border: Border.all(color: Colors.grey),
                borderRadius: BorderRadius.circular(4),
              ),
              child: Text(
                'Result: $_result',
                style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
              ),
            ),
          ],
        ),
      ),
    );
  }
  
  void _calculate() {
    try {
      final a = double.parse(_aController.text);
      final b = double.parse(_bController.text);
      
      double result;
      switch (_operation) {
        case 'add':
          result = widget.calculator.add(a, b);
          break;
        case 'subtract':
          result = widget.calculator.subtract(a, b);
          break;
        case 'multiply':
          result = widget.calculator.multiply(a, b);
          break;
        case 'divide':
          result = widget.calculator.divide(a, b);
          break;
        default:
          throw ArgumentError('Unknown operation');
      }
      
      setState(() {
        _result = result.toStringAsFixed(2);
      });
    } catch (e) {
      setState(() {
        _result = 'Error: $e';
      });
    }
  }
}

class TestingDemo extends StatelessWidget {
  const TestingDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Testing Best Practices'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildTestingInfo(),
            const SizedBox(height: 16),
            CalculatorWidget(calculator: CalculatorService()),
            const SizedBox(height: 16),
            _buildTestExamples(),
            const SizedBox(height: 16),
            _buildTestingTips(),
          ],
        ),
      ),
    );
  }

  Widget _buildTestingInfo() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Testing Best Practices',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text('• Write unit tests for business logic'),
            Text('• Use widget tests for UI components'),
            Text('• Implement integration tests for user flows'),
            Text('• Use meaningful test descriptions'),
            Text('• Test both success and error scenarios'),
            Text('• Mock external dependencies'),
            Text('• Use test keys for widget identification'),
            Text('• Maintain good test coverage'),
          ],
        ),
      ),
    );
  }

  Widget _buildTestExamples() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Example Test Cases',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            ExpansionTile(
              title: const Text('Unit Test Example'),
              children: [
                Container(
                  width: double.infinity,
                  padding: const EdgeInsets.all(12),
                  margin: const EdgeInsets.all(8),
                  decoration: BoxDecoration(
                    color: Colors.grey.shade900,
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: const Text(
                    '''// Unit test for CalculatorService
void main() {
  group('CalculatorService', () {
    late CalculatorService calculator;
    
    setUp(() {
      calculator = CalculatorService();
    });
    
    test('should add two numbers correctly', () {
      // Arrange
      const a = 5.0;
      const b = 3.0;
      
      // Act
      final result = calculator.add(a, b);
      
      // Assert
      expect(result, equals(8.0));
    });
    
    test('should throw error when dividing by zero', () {
      // Arrange
      const a = 10.0;
      const b = 0.0;
      
      // Act & Assert
      expect(
        () => calculator.divide(a, b),
        throwsA(isA<ArgumentError>()),
      );
    });
  });
}''',
                    style: TextStyle(
                      fontFamily: 'monospace',
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ),
            
            ExpansionTile(
              title: const Text('Widget Test Example'),
              children: [
                Container(
                  width: double.infinity,
                  padding: const EdgeInsets.all(12),
                  margin: const EdgeInsets.all(8),
                  decoration: BoxDecoration(
                    color: Colors.grey.shade900,
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: const Text(
                    '''// Widget test for CalculatorWidget
void main() {
  testWidgets('Calculator performs addition correctly',
      (WidgetTester tester) async {
    // Arrange
    final calculator = CalculatorService();
    await tester.pumpWidget(
      MaterialApp(
        home: Scaffold(
          body: CalculatorWidget(calculator: calculator),
        ),
      ),
    );
    
    // Act
    await tester.enterText(
      find.byKey(const Key('calculator_input_a')),
      '5',
    );
    await tester.enterText(
      find.byKey(const Key('calculator_input_b')),
      '3',
    );
    await tester.tap(
      find.byKey(const Key('calculator_calculate')),
    );
    await tester.pump();
    
    // Assert
    expect(
      find.textContaining('Result: 8.00'),
      findsOneWidget,
    );
  });
}''',
                    style: TextStyle(
                      fontFamily: 'monospace',
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ),
            
            ExpansionTile(
              title: const Text('Model Test Example'),
              children: [
                Container(
                  width: double.infinity,
                  padding: const EdgeInsets.all(12),
                  margin: const EdgeInsets.all(8),
                  decoration: BoxDecoration(
                    color: Colors.grey.shade900,
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: const Text(
                    '''// Test for UserProfile model
void main() {
  group('UserProfile', () {
    test('should create valid user profile', () {
      // Act
      final profile = UserProfile(
        name: 'John Doe',
        email: 'john@example.com',
        age: 25,
      );
      
      // Assert
      expect(profile.name, equals('John Doe'));
      expect(profile.isAdult, isTrue);
      expect(profile.isSenior, isFalse);
    });
    
    test('should throw error for invalid email', () {
      // Act & Assert
      expect(
        () => UserProfile(
          name: 'John',
          email: 'invalid-email',
          age: 25,
        ),
        throwsA(isA<ArgumentError>()),
      );
    });
    
    test('should identify senior users correctly', () {
      // Act
      final senior = UserProfile(
        name: 'Jane',
        email: 'jane@example.com',
        age: 70,
      );
      
      // Assert
      expect(senior.isSenior, isTrue);
      expect(senior.isAdult, isTrue);
    });
  });
}''',
                    style: TextStyle(
                      fontFamily: 'monospace',
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildTestingTips() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Testing Tips and Tools',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 12),
            
            Text(
              'Testing Pyramid:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            Text('• Unit Tests (70%): Fast, isolated, test business logic'),
            Text('• Widget Tests (20%): Test UI components and interactions'),
            Text('• Integration Tests (10%): Test complete user workflows'),
            SizedBox(height: 12),
            
            Text(
              'Testing Tools:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            Text('• flutter_test: Built-in testing framework'),
            Text('• mockito: Mock dependencies for testing'),
            Text('• integration_test: End-to-end testing'),
            Text('• golden_toolkit: Visual regression testing'),
            SizedBox(height: 12),
            
            Text(
              'Best Practices:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            Text('• Write tests before or during development (TDD)'),
            Text('• Use descriptive test names and organize with groups'),
            Text('• Test edge cases and error conditions'),
            Text('• Keep tests simple, focused, and independent'),
            Text('• Use setUp and tearDown for test preparation'),
            Text('• Aim for high test coverage but focus on quality'),
            Text('• Run tests frequently during development'),
            Text('• Include tests in CI/CD pipeline'),
          ],
        ),
      ),
    );
  }
}

// Example test file structure (would be in test/ directory)
/*
test/
├── unit/
│   ├── services/
│   │   └── calculator_service_test.dart
│   └── models/
│       └── user_profile_test.dart
├── widget/
│   └── calculator_widget_test.dart
└── integration/
    └── app_test.dart
*/
```

Comprehensive testing ensures code quality and reliability. Implement unit  
tests for business logic, widget tests for UI components, and integration  
tests for user flows. Use meaningful test descriptions, mock dependencies,  
and maintain good test coverage. Testing should be integrated into the  
development workflow from the beginning of the project.  

## Accessibility and Inclusive Design

Implementing accessibility features to ensure applications are usable  
by people with disabilities and support inclusive design principles.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter/semantics.dart';

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
      title: 'Best Practices - Accessibility',
      home: const AccessibilityDemo(),
    );
  }
}

// Good: Accessible custom button with proper semantics
class AccessibleButton extends StatelessWidget {
  const AccessibleButton({
    super.key,
    required this.onPressed,
    required this.child,
    this.semanticsLabel,
    this.tooltip,
    this.backgroundColor,
  });

  final VoidCallback? onPressed;
  final Widget child;
  final String? semanticsLabel;
  final String? tooltip;
  final Color? backgroundColor;

  @override
  Widget build(BuildContext context) {
    return Semantics(
      label: semanticsLabel,
      button: true,
      enabled: onPressed != null,
      child: Tooltip(
        message: tooltip ?? '',
        child: ElevatedButton(
          onPressed: onPressed,
          style: ElevatedButton.styleFrom(
            backgroundColor: backgroundColor,
            minimumSize: const Size(88, 36), // Minimum touch target size
            padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
          ),
          child: child,
        ),
      ),
    );
  }
}

// Good: Accessible form with proper labels and error handling
class AccessibleForm extends StatefulWidget {
  const AccessibleForm({super.key});

  @override
  State<AccessibleForm> createState() => _AccessibleFormState();
}

class _AccessibleFormState extends State<AccessibleForm> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  
  bool _agreeToTerms = false;
  String? _selectedCountry;
  
  final List<String> _countries = [
    'United States',
    'Canada',
    'United Kingdom',
    'Australia',
    'Germany',
  ];

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              const Text(
                'Accessible Contact Form',
                style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
              ),
              const SizedBox(height: 16),
              
              // Good: Proper text field with semantic labels
              Semantics(
                label: 'Full name input field',
                textField: true,
                child: TextFormField(
                  controller: _nameController,
                  decoration: const InputDecoration(
                    labelText: 'Full Name *',
                    hintText: 'Enter your full name',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.person),
                  ),
                  validator: (value) {
                    if (value == null || value.trim().isEmpty) {
                      return 'Name is required';
                    }
                    return null;
                  },
                  textInputAction: TextInputAction.next,
                ),
              ),
              const SizedBox(height: 16),
              
              // Good: Email field with proper validation
              Semantics(
                label: 'Email address input field',
                textField: true,
                child: TextFormField(
                  controller: _emailController,
                  decoration: const InputDecoration(
                    labelText: 'Email Address *',
                    hintText: 'Enter your email address',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.email),
                  ),
                  keyboardType: TextInputType.emailAddress,
                  validator: (value) {
                    if (value == null || value.trim().isEmpty) {
                      return 'Email is required';
                    }
                    if (!value.contains('@')) {
                      return 'Please enter a valid email address';
                    }
                    return null;
                  },
                  textInputAction: TextInputAction.next,
                ),
              ),
              const SizedBox(height: 16),
              
              // Good: Phone field with input formatting hint
              Semantics(
                label: 'Phone number input field, optional',
                textField: true,
                child: TextFormField(
                  controller: _phoneController,
                  decoration: const InputDecoration(
                    labelText: 'Phone Number (Optional)',
                    hintText: '(555) 123-4567',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.phone),
                  ),
                  keyboardType: TextInputType.phone,
                  textInputAction: TextInputAction.next,
                ),
              ),
              const SizedBox(height: 16),
              
              // Good: Accessible dropdown with proper labeling
              Semantics(
                label: 'Country selection dropdown',
                child: DropdownButtonFormField<String>(
                  value: _selectedCountry,
                  decoration: const InputDecoration(
                    labelText: 'Country',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.flag),
                  ),
                  hint: const Text('Select your country'),
                  items: _countries.map((country) {
                    return DropdownMenuItem<String>(
                      value: country,
                      child: Text(country),
                    );
                  }).toList(),
                  onChanged: (value) {
                    setState(() {
                      _selectedCountry = value;
                    });
                  },
                ),
              ),
              const SizedBox(height: 16),
              
              // Good: Accessible checkbox with clear labeling
              Semantics(
                label: 'Agreement to terms and conditions checkbox',
                child: CheckboxListTile(
                  value: _agreeToTerms,
                  onChanged: (value) {
                    setState(() {
                      _agreeToTerms = value ?? false;
                    });
                  },
                  title: const Text('I agree to the Terms and Conditions *'),
                  subtitle: const Text('Required to submit the form'),
                  controlAffinity: ListTileControlAffinity.leading,
                ),
              ),
              const SizedBox(height: 24),
              
              // Good: Accessible submit button
              SizedBox(
                width: double.infinity,
                child: AccessibleButton(
                  onPressed: _agreeToTerms ? _submitForm : null,
                  semanticsLabel: 'Submit contact form',
                  tooltip: _agreeToTerms 
                    ? 'Submit the contact form' 
                    : 'Accept terms and conditions to enable submit',
                  backgroundColor: _agreeToTerms ? null : Colors.grey,
                  child: const Text('Submit Form'),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  void _submitForm() {
    if (_formKey.currentState?.validate() ?? false) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: const Text('Form submitted successfully!'),
          backgroundColor: Colors.green,
          action: SnackBarAction(
            label: 'Dismiss',
            onPressed: () {
              ScaffoldMessenger.of(context).hideCurrentSnackBar();
            },
          ),
        ),
      );
    } else {
      // Good: Announce form errors to screen readers
      SemanticsService.announce(
        'Please fix the errors in the form',
        TextDirection.ltr,
      );
    }
  }
}

// Good: Accessible navigation drawer
class AccessibleDrawer extends StatelessWidget {
  const AccessibleDrawer({super.key});

  @override
  Widget build(BuildContext context) {
    return Drawer(
      child: ListView(
        padding: EdgeInsets.zero,
        children: [
          // Good: Drawer header with semantic information
          const DrawerHeader(
            decoration: BoxDecoration(color: Colors.blue),
            child: Semantics(
              label: 'Navigation menu header',
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  CircleAvatar(
                    radius: 30,
                    child: Icon(Icons.person, size: 30),
                  ),
                  SizedBox(height: 8),
                  Text(
                    'John Doe',
                    style: TextStyle(color: Colors.white, fontSize: 18),
                  ),
                  Text(
                    'john@example.com',
                    style: TextStyle(color: Colors.white70, fontSize: 14),
                  ),
                ],
              ),
            ),
          ),
          
          // Good: Accessible navigation items
          _buildDrawerItem(
            icon: Icons.home,
            title: 'Home',
            onTap: () => Navigator.pop(context),
          ),
          _buildDrawerItem(
            icon: Icons.person,
            title: 'Profile',
            onTap: () => Navigator.pop(context),
          ),
          _buildDrawerItem(
            icon: Icons.settings,
            title: 'Settings',
            onTap: () => Navigator.pop(context),
          ),
          _buildDrawerItem(
            icon: Icons.help,
            title: 'Help',
            onTap: () => Navigator.pop(context),
          ),
          
          const Divider(),
          
          _buildDrawerItem(
            icon: Icons.logout,
            title: 'Logout',
            onTap: () => Navigator.pop(context),
          ),
        ],
      ),
    );
  }

  Widget _buildDrawerItem({
    required IconData icon,
    required String title,
    required VoidCallback onTap,
  }) {
    return Semantics(
      label: '$title navigation item',
      button: true,
      child: ListTile(
        leading: Icon(icon),
        title: Text(title),
        onTap: onTap,
        trailing: const Icon(Icons.arrow_forward_ios, size: 16),
      ),
    );
  }
}

class AccessibilityDemo extends StatefulWidget {
  const AccessibilityDemo({super.key});

  @override
  State<AccessibilityDemo> createState() => _AccessibilityDemoState();
}

class _AccessibilityDemoState extends State<AccessibilityDemo> {
  double _fontSize = 16.0;
  bool _highContrast = false;
  bool _reduceAnimations = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Accessibility Best Practices'),
      ),
      drawer: const AccessibleDrawer(),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildAccessibilityInfo(),
            const SizedBox(height: 16),
            _buildAccessibilitySettings(),
            const SizedBox(height: 16),
            const AccessibleForm(),
            const SizedBox(height: 16),
            _buildColorContrastDemo(),
            const SizedBox(height: 16),
            _buildTouchTargetDemo(),
          ],
        ),
      ),
      // Good: Accessible floating action button
      floatingActionButton: Semantics(
        label: 'Add new item',
        child: FloatingActionButton(
          onPressed: () {
            SemanticsService.announce(
              'Add button pressed',
              TextDirection.ltr,
            );
          },
          tooltip: 'Add new item',
          child: const Icon(Icons.add),
        ),
      ),
    );
  }

  Widget _buildAccessibilityInfo() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Accessibility Best Practices',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text('• Provide semantic labels for interactive elements'),
            Text('• Ensure sufficient color contrast (4.5:1 for normal text)'),
            Text('• Make touch targets at least 44x44 points'),
            Text('• Support screen readers and assistive technologies'),
            Text('• Provide alternative text for images'),
            Text('• Use proper heading hierarchy'),
            Text('• Enable keyboard navigation'),
            Text('• Test with accessibility tools and real users'),
          ],
        ),
      ),
    );
  }

  Widget _buildAccessibilitySettings() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Accessibility Settings',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            
            // Font size adjustment
            Semantics(
              label: 'Font size adjustment slider',
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text('Font Size: ${_fontSize.toInt()}px'),
                  Slider(
                    value: _fontSize,
                    min: 12.0,
                    max: 24.0,
                    divisions: 12,
                    label: '${_fontSize.toInt()}px',
                    onChanged: (value) {
                      setState(() {
                        _fontSize = value;
                      });
                    },
                  ),
                ],
              ),
            ),
            
            // High contrast toggle
            Semantics(
              label: 'High contrast mode toggle',
              child: SwitchListTile(
                title: const Text('High Contrast Mode'),
                subtitle: const Text('Increases color contrast for better visibility'),
                value: _highContrast,
                onChanged: (value) {
                  setState(() {
                    _highContrast = value;
                  });
                },
              ),
            ),
            
            // Reduce animations toggle
            Semantics(
              label: 'Reduce animations toggle',
              child: SwitchListTile(
                title: const Text('Reduce Animations'),
                subtitle: const Text('Minimizes motion for users sensitive to movement'),
                value: _reduceAnimations,
                onChanged: (value) {
                  setState(() {
                    _reduceAnimations = value;
                  });
                },
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorContrastDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Color Contrast Examples',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            
            // Good contrast examples
            Container(
              width: double.infinity,
              padding: const EdgeInsets.all(12),
              decoration: BoxDecoration(
                color: _highContrast ? Colors.black : Colors.blue,
                borderRadius: BorderRadius.circular(8),
              ),
              child: Text(
                'Good Contrast: White text on blue background',
                style: TextStyle(
                  color: Colors.white,
                  fontSize: _fontSize,
                ),
              ),
            ),
            const SizedBox(height: 8),
            
            // Warning about poor contrast
            Container(
              width: double.infinity,
              padding: const EdgeInsets.all(12),
              decoration: BoxDecoration(
                color: Colors.yellow.shade100,
                borderRadius: BorderRadius.circular(8),
                border: Border.all(color: Colors.orange),
              ),
              child: Text(
                'Poor Contrast Example: Light yellow text would be hard to read',
                style: TextStyle(
                  color: Colors.orange.shade800,
                  fontSize: _fontSize,
                ),
              ),
            ),
            const SizedBox(height: 12),
            
            const Text(
              'Color Contrast Guidelines:',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            Text('• Normal text: 4.5:1 contrast ratio', style: TextStyle(fontSize: _fontSize)),
            Text('• Large text (18pt+): 3:1 contrast ratio', style: TextStyle(fontSize: _fontSize)),
            Text('• Don\'t rely on color alone to convey information', style: TextStyle(fontSize: _fontSize)),
          ],
        ),
      ),
    );
  }

  Widget _buildTouchTargetDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Touch Target Sizes',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                // Small touch target (bad)
                Semantics(
                  label: 'Small button, difficult to tap',
                  child: GestureDetector(
                    onTap: () {
                      ScaffoldMessenger.of(context).showSnackBar(
                        const SnackBar(
                          content: Text('Small button tapped (not recommended)'),
                        ),
                      );
                    },
                    child: Container(
                      width: 32,
                      height: 32,
                      decoration: const BoxDecoration(
                        color: Colors.red,
                        shape: BoxShape.circle,
                      ),
                      child: const Icon(Icons.close, color: Colors.white, size: 16),
                    ),
                  ),
                ),
                
                // Good touch target size
                Semantics(
                  label: 'Appropriately sized button, easy to tap',
                  child: GestureDetector(
                    onTap: () {
                      ScaffoldMessenger.of(context).showSnackBar(
                        const SnackBar(
                          content: Text('Good sized button tapped!'),
                          backgroundColor: Colors.green,
                        ),
                      );
                    },
                    child: Container(
                      width: 44,
                      height: 44,
                      decoration: const BoxDecoration(
                        color: Colors.green,
                        shape: BoxShape.circle,
                      ),
                      child: const Icon(Icons.check, color: Colors.white, size: 20),
                    ),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            
            Text(
              'Minimum touch target size: 44x44 points',
              style: TextStyle(
                fontWeight: FontWeight.bold,
                fontSize: _fontSize,
              ),
            ),
            Text(
              'Left: 32x32 (too small) | Right: 44x44 (good)',
              style: TextStyle(fontSize: _fontSize),
            ),
          ],
        ),
      ),
    );
  }
}
```

Accessibility ensures applications are usable by everyone, including people  
with disabilities. Provide semantic labels, ensure sufficient color contrast,  
use proper touch target sizes, and support assistive technologies. Test  
with accessibility tools and real users to create truly inclusive applications  
that work for all users regardless of their abilities.

## Documentation and Code Comments

Writing comprehensive documentation and meaningful code comments to improve  
code maintainability, team collaboration, and knowledge transfer.  

```dart
import 'package:flutter/material.dart';

/// Main application entry point.
/// 
/// This app demonstrates best practices for documentation and code comments
/// in Flutter applications. It showcases proper documentation techniques
/// including class documentation, method documentation, parameter descriptions,
/// and inline comments for complex logic.
void main() {
  runApp(const MyApp());
}

/// Root application widget that configures the MaterialApp.
/// 
/// This widget sets up the app's theme, title, and initial route.
/// The app uses a dark theme by default to demonstrate theming practices.
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Best Practices - Documentation',
      home: const DocumentationDemo(),
    );
  }
}

/// Service class that handles user profile operations.
/// 
/// This class demonstrates proper documentation for service classes,
/// including comprehensive method documentation with examples and
/// parameter descriptions.
/// 
/// Example usage:
/// ```dart
/// final service = UserProfileService();
/// final profile = await service.fetchUserProfile('user123');
/// ```
class UserProfileService {
  /// Internal cache for storing fetched user profiles.
  /// 
  /// This Map prevents redundant API calls by caching previously
  /// fetched user profiles. The key is the user ID and the value
  /// is the UserProfile object.
  final Map<String, UserProfile> _profileCache = {};

  /// Fetches a user profile by ID with caching support.
  /// 
  /// This method first checks the internal cache before making an API call.
  /// If the profile is found in cache, it returns immediately. Otherwise,
  /// it simulates an API call and caches the result.
  /// 
  /// Parameters:
  /// - [userId]: The unique identifier for the user. Must not be empty.
  /// 
  /// Returns:
  /// A [Future<UserProfile>] containing the user's profile information.
  /// 
  /// Throws:
  /// - [ArgumentError] if [userId] is empty or null.
  /// - [NetworkException] if the API call fails.
  /// 
  /// Example:
  /// ```dart
  /// try {
  ///   final profile = await service.fetchUserProfile('user123');
  ///   print('User: ${profile.name}');
  /// } catch (e) {
  ///   print('Error: $e');
  /// }
  /// ```
  Future<UserProfile> fetchUserProfile(String userId) async {
    // Validate input parameters
    if (userId.trim().isEmpty) {
      throw ArgumentError('User ID cannot be empty');
    }

    // Check cache first to avoid unnecessary API calls
    if (_profileCache.containsKey(userId)) {
      return _profileCache[userId]!;
    }

    // Simulate API call delay
    await Future.delayed(const Duration(seconds: 1));

    // Create mock user profile
    // In a real app, this would be an actual API call
    final profile = UserProfile(
      id: userId,
      name: 'User $userId',
      email: 'user$userId@example.com',
      joinDate: DateTime.now().subtract(const Duration(days: 30)),
      isVerified: userId.hashCode % 2 == 0, // Mock verification status
    );

    // Cache the profile for future requests
    _profileCache[userId] = profile;

    return profile;
  }

  /// Updates a user profile with new information.
  /// 
  /// This method validates the provided profile data and simulates
  /// updating the user's information on the server. It also updates
  /// the local cache with the new profile data.
  /// 
  /// Parameters:
  /// - [profile]: The updated user profile information.
  /// 
  /// Returns:
  /// A [Future<bool>] indicating whether the update was successful.
  /// 
  /// Throws:
  /// - [ArgumentError] if the profile contains invalid data.
  /// - [NetworkException] if the update operation fails.
  Future<bool> updateUserProfile(UserProfile profile) async {
    // Validate profile data
    if (profile.name.trim().isEmpty) {
      throw ArgumentError('User name cannot be empty');
    }

    // Simulate network operation
    await Future.delayed(const Duration(milliseconds: 500));

    // Update cache with new profile data
    _profileCache[profile.id] = profile;

    return true; // Success indicator
  }

  /// Clears the internal profile cache.
  /// 
  /// This method removes all cached profiles, forcing fresh data
  /// to be fetched on the next request. Useful for logout operations
  /// or when user data needs to be refreshed.
  void clearCache() {
    _profileCache.clear();
  }
}

/// Represents a user profile with personal information.
/// 
/// This immutable data class contains all the information associated
/// with a user account. It includes validation logic and utility methods
/// for working with user data.
/// 
/// All fields are final to ensure immutability, which helps prevent
/// bugs and makes the class thread-safe.
class UserProfile {
  /// Creates a new UserProfile instance.
  /// 
  /// All required parameters must be provided. The constructor
  /// validates the input data to ensure data integrity.
  const UserProfile({
    required this.id,
    required this.name,
    required this.email,
    required this.joinDate,
    this.isVerified = false,
    this.avatarUrl,
  });

  /// Unique identifier for the user.
  final String id;

  /// User's display name.
  /// 
  /// This is the name shown throughout the application UI.
  /// Must not be empty and should be properly formatted.
  final String name;

  /// User's email address.
  /// 
  /// Used for notifications and account recovery.
  /// Must be a valid email format.
  final String email;

  /// Date when the user joined the platform.
  /// 
  /// Used to calculate account age and for analytics purposes.
  final DateTime joinDate;

  /// Whether the user's email has been verified.
  /// 
  /// Defaults to false. Users with verified emails have access
  /// to additional features and higher trust levels.
  final bool isVerified;

  /// Optional URL to the user's avatar image.
  /// 
  /// If null, the application should display a default avatar.
  final String? avatarUrl;

  /// Calculates how many days the user has been registered.
  /// 
  /// Returns the number of days between the join date and today.
  /// Useful for displaying account age or for user segmentation.
  int get daysSinceJoined {
    final now = DateTime.now();
    return now.difference(joinDate).inDays;
  }

  /// Determines if the user is considered active.
  /// 
  /// An active user is defined as someone who joined within
  /// the last 30 days. This can be used for onboarding flows
  /// or feature recommendations.
  bool get isActiveUser {
    return daysSinceJoined <= 30;
  }

  /// Creates a copy of this profile with optional field updates.
  /// 
  /// This method enables immutable updates by creating a new
  /// instance with modified fields while preserving unchanged values.
  /// 
  /// Parameters:
  /// - [id]: New user ID (optional)
  /// - [name]: New display name (optional)
  /// - [email]: New email address (optional)
  /// - [joinDate]: New join date (optional)
  /// - [isVerified]: New verification status (optional)
  /// - [avatarUrl]: New avatar URL (optional)
  /// 
  /// Returns:
  /// A new [UserProfile] instance with updated fields.
  /// 
  /// Example:
  /// ```dart
  /// final updatedProfile = profile.copyWith(
  ///   name: 'New Name',
  ///   isVerified: true,
  /// );
  /// ```
  UserProfile copyWith({
    String? id,
    String? name,
    String? email,
    DateTime? joinDate,
    bool? isVerified,
    String? avatarUrl,
  }) {
    return UserProfile(
      id: id ?? this.id,
      name: name ?? this.name,
      email: email ?? this.email,
      joinDate: joinDate ?? this.joinDate,
      isVerified: isVerified ?? this.isVerified,
      avatarUrl: avatarUrl ?? this.avatarUrl,
    );
  }

  @override
  String toString() {
    return 'UserProfile(id: $id, name: $name, email: $email, '
           'joinDate: $joinDate, isVerified: $isVerified)';
  }
}

/// Custom exception for network-related errors.
/// 
/// This exception is thrown when network operations fail,
/// providing specific error information and HTTP status codes
/// when available.
class NetworkException implements Exception {
  /// Creates a new NetworkException.
  /// 
  /// Parameters:
  /// - [message]: Human-readable error description
  /// - [statusCode]: HTTP status code (optional)
  const NetworkException(this.message, [this.statusCode]);

  /// Description of the network error.
  final String message;

  /// HTTP status code if applicable.
  /// 
  /// This field is null for non-HTTP network errors
  /// such as connection timeouts or DNS resolution failures.
  final int? statusCode;

  @override
  String toString() {
    if (statusCode != null) {
      return 'NetworkException: $message (Status: $statusCode)';
    }
    return 'NetworkException: $message';
  }
}

/// Main documentation demo screen.
/// 
/// This widget demonstrates various documentation practices and
/// displays examples of well-documented code components.
class DocumentationDemo extends StatefulWidget {
  const DocumentationDemo({super.key});

  @override
  State<DocumentationDemo> createState() => _DocumentationDemoState();
}

/// State class for the documentation demo.
/// 
/// Manages user interaction and demonstrates proper state management
/// documentation practices.
class _DocumentationDemoState extends State<DocumentationDemo> {
  /// Service instance for user profile operations.
  final UserProfileService _userService = UserProfileService();

  /// Currently loaded user profile.
  /// 
  /// This field is null until a profile is successfully loaded.
  UserProfile? _currentProfile;

  /// Indicates whether a profile loading operation is in progress.
  bool _isLoading = false;

  /// Error message from the last failed operation.
  /// 
  /// This field is empty when no error has occurred or when
  /// the error has been cleared.
  String _errorMessage = '';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Documentation Best Practices'),
        // Action button to clear cache - demonstrates inline documentation
        actions: [
          IconButton(
            onPressed: () {
              // Clear the service cache and reset local state
              _userService.clearCache();
              setState(() {
                _currentProfile = null;
                _errorMessage = '';
              });
              
              // Notify user of the action
              ScaffoldMessenger.of(context).showSnackBar(
                const SnackBar(content: Text('Cache cleared')),
              );
            },
            icon: const Icon(Icons.refresh),
            tooltip: 'Clear Cache',
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildDocumentationInfo(),
            const SizedBox(height: 16),
            _buildUserProfileSection(),
            const SizedBox(height: 16),
            _buildDocumentationExamples(),
          ],
        ),
      ),
    );
  }

  /// Builds the documentation information card.
  /// 
  /// This method creates a card explaining documentation best practices.
  /// The card is informational only and doesn't handle user interaction.
  Widget _buildDocumentationInfo() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Documentation Best Practices',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text('• Write clear, comprehensive class and method documentation'),
            Text('• Include parameter descriptions and return value information'),
            Text('• Provide usage examples for complex methods'),
            Text('• Document exceptions that methods might throw'),
            Text('• Use meaningful variable and method names'),
            Text('• Add inline comments for complex logic'),
            Text('• Keep documentation up-to-date with code changes'),
            Text('• Follow consistent documentation style'),
          ],
        ),
      ),
    );
  }

  /// Builds the user profile demonstration section.
  /// 
  /// This method creates UI components that showcase the documented
  /// UserProfileService and UserProfile classes in action.
  Widget _buildUserProfileSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'User Profile Demo',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            // Profile loading controls
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _isLoading ? null : () => _loadUserProfile('user123'),
                    child: _isLoading 
                      ? const CircularProgressIndicator()
                      : const Text('Load User Profile'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _currentProfile == null 
                      ? null 
                      : _updateUserProfile,
                    child: const Text('Update Profile'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            
            // Error display
            if (_errorMessage.isNotEmpty) ...[
              Container(
                width: double.infinity,
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: Colors.red.shade50,
                  border: Border.all(color: Colors.red),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Text(
                  'Error: $_errorMessage',
                  style: const TextStyle(color: Colors.red),
                ),
              ),
              const SizedBox(height: 12),
            ],
            
            // Profile display
            if (_currentProfile != null) ...[
              _buildProfileDisplay(_currentProfile!),
            ] else if (!_isLoading) ...[
              const Text(
                'No profile loaded. Click "Load User Profile" to begin.',
                style: TextStyle(color: Colors.grey),
              ),
            ],
          ],
        ),
      ),
    );
  }

  /// Builds the profile display widget.
  /// 
  /// This method creates a formatted display of user profile information.
  /// 
  /// Parameters:
  /// - [profile]: The UserProfile to display. Must not be null.
  Widget _buildProfileDisplay(UserProfile profile) {
    return Container(
      width: double.infinity,
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: Colors.green.shade50,
        border: Border.all(color: Colors.green),
        borderRadius: BorderRadius.circular(8),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'Profile Loaded Successfully',
            style: TextStyle(
              color: Colors.green.shade800,
              fontWeight: FontWeight.bold,
            ),
          ),
          const SizedBox(height: 8),
          Text('ID: ${profile.id}'),
          Text('Name: ${profile.name}'),
          Text('Email: ${profile.email}'),
          Text('Verified: ${profile.isVerified ? "Yes" : "No"}'),
          Text('Days since joined: ${profile.daysSinceJoined}'),
          Text('Active user: ${profile.isActiveUser ? "Yes" : "No"}'),
        ],
      ),
    );
  }

  /// Builds examples of different documentation styles.
  /// 
  /// This method creates a card showing various documentation examples
  /// to help developers understand proper documentation practices.
  Widget _buildDocumentationExamples() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Documentation Examples',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            ExpansionTile(
              title: const Text('Class Documentation'),
              children: [
                Container(
                  width: double.infinity,
                  padding: const EdgeInsets.all(12),
                  margin: const EdgeInsets.all(8),
                  decoration: BoxDecoration(
                    color: Colors.grey.shade900,
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: const Text(
                    '''/// Service class that handles user profile operations.
/// 
/// This class demonstrates proper documentation for service classes,
/// including comprehensive method documentation with examples and
/// parameter descriptions.
/// 
/// Example usage:
/// ```dart
/// final service = UserProfileService();
/// final profile = await service.fetchUserProfile('user123');
/// ```
class UserProfileService {
  // Implementation details...
}''',
                    style: TextStyle(
                      fontFamily: 'monospace',
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ),
            
            ExpansionTile(
              title: const Text('Method Documentation'),
              children: [
                Container(
                  width: double.infinity,
                  padding: const EdgeInsets.all(12),
                  margin: const EdgeInsets.all(8),
                  decoration: BoxDecoration(
                    color: Colors.grey.shade900,
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: const Text(
                    '''/// Fetches a user profile by ID with caching support.
/// 
/// This method first checks the internal cache before making an API call.
/// If the profile is found in cache, it returns immediately. Otherwise,
/// it simulates an API call and caches the result.
/// 
/// Parameters:
/// - [userId]: The unique identifier for the user. Must not be empty.
/// 
/// Returns:
/// A [Future<UserProfile>] containing the user's profile information.
/// 
/// Throws:
/// - [ArgumentError] if [userId] is empty or null.
/// - [NetworkException] if the API call fails.
/// 
/// Example:
/// ```dart
/// try {
///   final profile = await service.fetchUserProfile('user123');
///   print('User: \${profile.name}');
/// } catch (e) {
///   print('Error: \$e');
/// }
/// ```
Future<UserProfile> fetchUserProfile(String userId) async {
  // Implementation...
}''',
                    style: TextStyle(
                      fontFamily: 'monospace',
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ),
            
            ExpansionTile(
              title: const Text('Inline Comments'),
              children: [
                Container(
                  width: double.infinity,
                  padding: const EdgeInsets.all(12),
                  margin: const EdgeInsets.all(8),
                  decoration: BoxDecoration(
                    color: Colors.grey.shade900,
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: const Text(
                    '''// Validate input parameters
if (userId.trim().isEmpty) {
  throw ArgumentError('User ID cannot be empty');
}

// Check cache first to avoid unnecessary API calls
if (_profileCache.containsKey(userId)) {
  return _profileCache[userId]!;
}

// Simulate API call delay
await Future.delayed(const Duration(seconds: 1));

// Cache the profile for future requests
_profileCache[userId] = profile;''',
                    style: TextStyle(
                      fontFamily: 'monospace',
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  /// Loads a user profile asynchronously.
  /// 
  /// This method demonstrates error handling and state management
  /// during asynchronous operations.
  /// 
  /// Parameters:
  /// - [userId]: The ID of the user to load.
  Future<void> _loadUserProfile(String userId) async {
    // Set loading state to show progress indicator
    setState(() {
      _isLoading = true;
      _errorMessage = '';
    });

    try {
      // Attempt to load the user profile
      final profile = await _userService.fetchUserProfile(userId);
      
      // Update state with successful result
      setState(() {
        _currentProfile = profile;
        _isLoading = false;
      });
    } catch (e) {
      // Handle errors and update state accordingly
      setState(() {
        _errorMessage = e.toString();
        _isLoading = false;
      });
    }
  }

  /// Updates the current user profile with new information.
  /// 
  /// This method demonstrates profile modification and error handling
  /// for update operations.
  Future<void> _updateUserProfile() async {
    if (_currentProfile == null) return;

    try {
      // Create an updated profile with verification status toggled
      final updatedProfile = _currentProfile!.copyWith(
        isVerified: !_currentProfile!.isVerified,
      );

      // Attempt to update the profile
      await _userService.updateUserProfile(updatedProfile);

      // Update local state with the new profile
      setState(() {
        _currentProfile = updatedProfile;
        _errorMessage = '';
      });

      // Notify user of successful update
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            content: Text('Profile updated successfully'),
            backgroundColor: Colors.green,
          ),
        );
      }
    } catch (e) {
      // Handle update errors
      setState(() {
        _errorMessage = e.toString();
      });
    }
  }
}
```

Comprehensive documentation and meaningful comments are essential for  
maintainable code. Write clear class and method documentation, include  
parameter descriptions and examples, document exceptions, and add inline  
comments for complex logic. Good documentation helps team members  
understand code quickly and reduces maintenance overhead.  

## Platform-Specific Optimizations

Implementing platform-specific optimizations and adaptations to provide  
the best user experience across different platforms and devices.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter/foundation.dart';
import 'package:flutter/cupertino.dart';
import 'package:flutter/services.dart';
import 'dart:io';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    // Platform-specific app configuration
    return MaterialApp(
      // Use platform-appropriate theme
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Best Practices - Platform Optimizations',
      home: const PlatformOptimizationsDemo(),
      // Platform-specific scrolling behavior
      scrollBehavior: const MaterialScrollBehavior().copyWith(
        dragDevices: {
          PointerDeviceKind.mouse,
          PointerDeviceKind.touch,
          PointerDeviceKind.stylus,
          PointerDeviceKind.trackpad,
        },
      ),
    );
  }
}

/// Utility class for platform detection and adaptation.
class PlatformUtils {
  /// Returns true if running on a mobile platform (iOS or Android).
  static bool get isMobile => Platform.isIOS || Platform.isAndroid;
  
  /// Returns true if running on a desktop platform.
  static bool get isDesktop => Platform.isMacOS || Platform.isWindows || Platform.isLinux;
  
  /// Returns true if running on iOS.
  static bool get isIOS => Platform.isIOS;
  
  /// Returns true if running on Android.
  static bool get isAndroid => Platform.isAndroid;
  
  /// Returns true if running on web platform.
  static bool get isWeb => kIsWeb;
  
  /// Returns the appropriate keyboard type for the platform.
  static TextInputType getKeyboardType(String inputType) {
    switch (inputType) {
      case 'email':
        return isIOS ? TextInputType.emailAddress : TextInputType.emailAddress;
      case 'phone':
        return isIOS ? TextInputType.phone : TextInputType.phone;
      case 'number':
        return isIOS 
          ? const TextInputType.numberWithOptions(decimal: true)
          : TextInputType.number;
      default:
        return TextInputType.text;
    }
  }
  
  /// Returns platform-specific text selection controls.
  static TextSelectionControls? getTextSelectionControls() {
    if (isIOS) {
      return cupertinoTextSelectionControls;
    } else if (isAndroid) {
      return materialTextSelectionControls;
    }
    return null;
  }
}

/// Platform-adaptive button that changes appearance based on platform.
class PlatformButton extends StatelessWidget {
  const PlatformButton({
    super.key,
    required this.onPressed,
    required this.child,
    this.color,
  });

  final VoidCallback? onPressed;
  final Widget child;
  final Color? color;

  @override
  Widget build(BuildContext context) {
    // Use platform-appropriate button style
    if (PlatformUtils.isIOS) {
      return CupertinoButton.filled(
        onPressed: onPressed,
        color: color ?? CupertinoColors.activeBlue,
        child: child,
      );
    } else {
      return ElevatedButton(
        onPressed: onPressed,
        style: color != null 
          ? ElevatedButton.styleFrom(backgroundColor: color)
          : null,
        child: child,
      );
    }
  }
}

/// Platform-adaptive dialog that uses native dialog styles.
class PlatformDialog extends StatelessWidget {
  const PlatformDialog({
    super.key,
    required this.title,
    required this.content,
    required this.actions,
  });

  final String title;
  final String content;
  final List<Widget> actions;

  @override
  Widget build(BuildContext context) {
    if (PlatformUtils.isIOS) {
      return CupertinoAlertDialog(
        title: Text(title),
        content: Text(content),
        actions: actions,
      );
    } else {
      return AlertDialog(
        title: Text(title),
        content: Text(content),
        actions: actions,
      );
    }
  }

  /// Static method to show platform-appropriate dialog.
  static Future<void> show({
    required BuildContext context,
    required String title,
    required String content,
    List<Widget>? actions,
  }) {
    return showDialog(
      context: context,
      builder: (context) => PlatformDialog(
        title: title,
        content: content,
        actions: actions ?? [
          PlatformButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('OK'),
          ),
        ],
      ),
    );
  }
}

/// Adaptive navigation widget that changes based on platform and screen size.
class AdaptiveNavigation extends StatelessWidget {
  const AdaptiveNavigation({
    super.key,
    required this.selectedIndex,
    required this.onDestinationSelected,
    required this.destinations,
  });

  final int selectedIndex;
  final ValueChanged<int> onDestinationSelected;
  final List<NavigationDestination> destinations;

  @override
  Widget build(BuildContext context) {
    final isLargeScreen = MediaQuery.of(context).size.width > 600;
    
    if (isLargeScreen && PlatformUtils.isDesktop) {
      // Use navigation rail for desktop/large screens
      return NavigationRail(
        selectedIndex: selectedIndex,
        onDestinationSelected: onDestinationSelected,
        labelType: NavigationRailLabelType.selected,
        destinations: destinations.map((dest) {
          return NavigationRailDestination(
            icon: dest.icon,
            selectedIcon: dest.selectedIcon,
            label: Text(dest.label),
          );
        }).toList(),
      );
    } else if (PlatformUtils.isIOS) {
      // Use Cupertino tab bar for iOS
      return CupertinoTabBar(
        currentIndex: selectedIndex,
        onTap: onDestinationSelected,
        items: destinations.map((dest) {
          return BottomNavigationBarItem(
            icon: dest.icon,
            activeIcon: dest.selectedIcon ?? dest.icon,
            label: dest.label,
          );
        }).toList(),
      );
    } else {
      // Use Material bottom navigation bar for Android
      return NavigationBar(
        selectedIndex: selectedIndex,
        onDestinationSelected: onDestinationSelected,
        destinations: destinations,
      );
    }
  }
}

class PlatformOptimizationsDemo extends StatefulWidget {
  const PlatformOptimizationsDemo({super.key});

  @override
  State<PlatformOptimizationsDemo> createState() => _PlatformOptimizationsDemoState();
}

class _PlatformOptimizationsDemoState extends State<PlatformOptimizationsDemo> {
  int _selectedIndex = 0;
  final TextEditingController _textController = TextEditingController();
  
  final List<NavigationDestination> _destinations = [
    const NavigationDestination(
      icon: Icon(Icons.home_outlined),
      selectedIcon: Icon(Icons.home),
      label: 'Home',
    ),
    const NavigationDestination(
      icon: Icon(Icons.settings_outlined),
      selectedIcon: Icon(Icons.settings),
      label: 'Settings',
    ),
    const NavigationDestination(
      icon: Icon(Icons.info_outlined),
      selectedIcon: Icon(Icons.info),
      label: 'About',
    ),
  ];

  @override
  void dispose() {
    _textController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final isLargeScreen = MediaQuery.of(context).size.width > 600;
    
    // Platform-specific app bar
    final appBar = PlatformUtils.isIOS 
      ? CupertinoNavigationBar(
          middle: const Text('Platform Optimizations'),
        ) as PreferredSizeWidget
      : AppBar(
          title: const Text('Platform Optimizations'),
          // Desktop-specific: add window controls space
          toolbarHeight: PlatformUtils.isDesktop ? 56 : 56,
        );

    Widget body = IndexedStack(
      index: _selectedIndex,
      children: [
        _buildHomeTab(),
        _buildSettingsTab(),
        _buildAboutTab(),
      ],
    );

    // Adapt layout based on screen size and platform
    if (isLargeScreen && PlatformUtils.isDesktop) {
      return Scaffold(
        body: Row(
          children: [
            AdaptiveNavigation(
              selectedIndex: _selectedIndex,
              onDestinationSelected: (index) {
                setState(() {
                  _selectedIndex = index;
                });
              },
              destinations: _destinations,
            ),
            Expanded(
              child: Column(
                children: [
                  appBar,
                  Expanded(child: body),
                ],
              ),
            ),
          ],
        ),
      );
    } else if (PlatformUtils.isIOS) {
      return CupertinoTabScaffold(
        tabBar: AdaptiveNavigation(
          selectedIndex: _selectedIndex,
          onDestinationSelected: (index) {
            setState(() {
              _selectedIndex = index;
            });
          },
          destinations: _destinations,
        ) as CupertinoTabBar,
        tabBuilder: (context, index) {
          return CupertinoTabView(
            builder: (context) => CupertinoPageScaffold(
              navigationBar: appBar as CupertinoNavigationBar,
              child: SafeArea(
                child: IndexedStack(
                  index: index,
                  children: [
                    _buildHomeTab(),
                    _buildSettingsTab(),
                    _buildAboutTab(),
                  ],
                ),
              ),
            ),
          );
        },
      );
    } else {
      return Scaffold(
        appBar: appBar,
        body: body,
        bottomNavigationBar: AdaptiveNavigation(
          selectedIndex: _selectedIndex,
          onDestinationSelected: (index) {
            setState(() {
              _selectedIndex = index;
            });
          },
          destinations: _destinations,
        ),
      );
    }
  }

  Widget _buildHomeTab() {
    return SingleChildScrollView(
      padding: EdgeInsets.all(PlatformUtils.isMobile ? 16 : 24),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          _buildPlatformInfo(),
          const SizedBox(height: 16),
          _buildInputDemo(),
          const SizedBox(height: 16),
          _buildButtonDemo(),
          const SizedBox(height: 16),
          _buildScrollingDemo(),
        ],
      ),
    );
  }

  Widget _buildSettingsTab() {
    return SingleChildScrollView(
      padding: EdgeInsets.all(PlatformUtils.isMobile ? 16 : 24),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          const Card(
            child: Padding(
              padding: EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Platform Settings',
                    style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                  ),
                  SizedBox(height: 12),
                  Text('• Adapt UI components to platform conventions'),
                  Text('• Use platform-specific navigation patterns'),
                  Text('• Implement platform-appropriate input methods'),
                  Text('• Follow platform design guidelines'),
                  Text('• Optimize touch targets for different devices'),
                ],
              ),
            ),
          ),
          const SizedBox(height: 16),
          _buildPlatformSpecificSettings(),
        ],
      ),
    );
  }

  Widget _buildAboutTab() {
    return SingleChildScrollView(
      padding: EdgeInsets.all(PlatformUtils.isMobile ? 16 : 24),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          const Card(
            child: Padding(
              padding: EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'About Platform Optimizations',
                    style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                  ),
                  SizedBox(height: 12),
                  Text(
                    'This demo showcases various platform-specific optimizations '
                    'and adaptations that enhance user experience across different '
                    'platforms and screen sizes.',
                  ),
                  SizedBox(height: 12),
                  Text(
                    'Key Features:',
                    style: TextStyle(fontWeight: FontWeight.bold),
                  ),
                  Text('• Platform-adaptive UI components'),
                  Text('• Responsive layout adaptations'),
                  Text('• Platform-specific input handling'),
                  Text('• Native navigation patterns'),
                  Text('• Optimized touch interactions'),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildPlatformInfo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Platform Detection',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            _buildInfoRow('Platform', _getPlatformName()),
            _buildInfoRow('Form Factor', PlatformUtils.isMobile ? 'Mobile' : 'Desktop'),
            _buildInfoRow('Screen Width', '${MediaQuery.of(context).size.width.toInt()}px'),
            _buildInfoRow('Screen Height', '${MediaQuery.of(context).size.height.toInt()}px'),
            _buildInfoRow('Device Pixel Ratio', MediaQuery.of(context).devicePixelRatio.toStringAsFixed(1)),
            _buildInfoRow('Text Scale Factor', MediaQuery.of(context).textScaleFactor.toStringAsFixed(1)),
          ],
        ),
      ),
    );
  }

  Widget _buildInfoRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 2),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(label),
          Text(
            value,
            style: const TextStyle(fontWeight: FontWeight.bold),
          ),
        ],
      ),
    );
  }

  Widget _buildInputDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Platform-Adaptive Input',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            // Platform-adaptive text field
            TextField(
              controller: _textController,
              decoration: InputDecoration(
                labelText: 'Text Input',
                hintText: 'Enter text here',
                border: PlatformUtils.isIOS 
                  ? const UnderlineInputBorder()
                  : const OutlineInputBorder(),
                prefixIcon: const Icon(Icons.edit),
              ),
              keyboardType: PlatformUtils.getKeyboardType('text'),
              textInputAction: TextInputAction.done,
              // Platform-specific text selection
              selectionControls: PlatformUtils.getTextSelectionControls(),
            ),
            const SizedBox(height: 12),
            
            Text(
              'Keyboard Type: ${PlatformUtils.getKeyboardType("text").toString()}',
              style: const TextStyle(fontSize: 12, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildButtonDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Platform-Adaptive Buttons',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            Row(
              children: [
                Expanded(
                  child: PlatformButton(
                    onPressed: () => _showPlatformDialog(),
                    child: const Text('Show Dialog'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: PlatformButton(
                    onPressed: () => _showPlatformSnackBar(),
                    color: Colors.green,
                    child: const Text('Show Message'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 8),
            
            Text(
              PlatformUtils.isIOS 
                ? 'Using Cupertino buttons'
                : 'Using Material buttons',
              style: const TextStyle(fontSize: 12, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildScrollingDemo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Platform Scrolling Behavior',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            Container(
              height: 200,
              decoration: BoxDecoration(
                border: Border.all(color: Colors.grey),
                borderRadius: BorderRadius.circular(8),
              ),
              child: ListView.builder(
                itemCount: 50,
                itemBuilder: (context, index) {
                  return ListTile(
                    leading: CircleAvatar(child: Text('${index + 1}')),
                    title: Text('Platform Item ${index + 1}'),
                    subtitle: Text('Scrolling on ${_getPlatformName()}'),
                  );
                },
              ),
            ),
            const SizedBox(height: 8),
            
            Text(
              PlatformUtils.isIOS 
                ? 'iOS-style scrolling with bounce effect'
                : 'Material-style scrolling with glow effect',
              style: const TextStyle(fontSize: 12, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildPlatformSpecificSettings() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Platform-Specific Features',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            
            if (PlatformUtils.isIOS) ...[
              const ListTile(
                leading: Icon(CupertinoIcons.device_phone_portrait),
                title: Text('iOS Haptic Feedback'),
                subtitle: Text('Native iOS haptic feedback patterns'),
              ),
              const ListTile(
                leading: Icon(CupertinoIcons.slider_horizontal_3),
                title: Text('Cupertino Sliders'),
                subtitle: Text('iOS-style sliders and pickers'),
              ),
            ] else if (PlatformUtils.isAndroid) ...[
              const ListTile(
                leading: Icon(Icons.vibration),
                title: Text('Android Vibration'),
                subtitle: Text('Material haptic feedback'),
              ),
              const ListTile(
                leading: Icon(Icons.palette),
                title: Text('Material You'),
                subtitle: Text('Dynamic color theming'),
              ),
            ] else if (PlatformUtils.isDesktop) ...[
              const ListTile(
                leading: Icon(Icons.keyboard),
                title: Text('Keyboard Shortcuts'),
                subtitle: Text('Desktop keyboard navigation'),
              ),
              const ListTile(
                leading: Icon(Icons.mouse),
                title: Text('Mouse Interactions'),
                subtitle: Text('Right-click context menus'),
              ),
            ],
            
            const SizedBox(height: 12),
            
            PlatformButton(
              onPressed: _triggerHapticFeedback,
              child: const Text('Test Haptic Feedback'),
            ),
          ],
        ),
      ),
    );
  }

  String _getPlatformName() {
    if (PlatformUtils.isWeb) return 'Web';
    if (PlatformUtils.isIOS) return 'iOS';
    if (PlatformUtils.isAndroid) return 'Android';
    if (Platform.isMacOS) return 'macOS';
    if (Platform.isWindows) return 'Windows';
    if (Platform.isLinux) return 'Linux';
    return 'Unknown';
  }

  void _showPlatformDialog() {
    PlatformDialog.show(
      context: context,
      title: 'Platform Dialog',
      content: 'This dialog adapts to the current platform conventions.',
    );
  }

  void _showPlatformSnackBar() {
    final message = 'Platform message on ${_getPlatformName()}';
    
    if (PlatformUtils.isIOS) {
      // iOS doesn't have snackbars, so show a banner or alert
      showDialog(
        context: context,
        barrierDismissible: true,
        builder: (context) => CupertinoAlertDialog(
          content: Text(message),
          actions: [
            CupertinoDialogAction(
              onPressed: () => Navigator.pop(context),
              child: const Text('OK'),
            ),
          ],
        ),
      );
    } else {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text(message),
          action: SnackBarAction(
            label: 'Dismiss',
            onPressed: () {
              ScaffoldMessenger.of(context).hideCurrentSnackBar();
            },
          ),
        ),
      );
    }
  }

  void _triggerHapticFeedback() {
    if (PlatformUtils.isMobile) {
      HapticFeedback.mediumImpact();
      
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text('Haptic feedback triggered on ${_getPlatformName()}'),
          duration: const Duration(seconds: 1),
        ),
      );
    } else {
      // Desktop platforms don't typically support haptic feedback
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Haptic feedback not available on desktop'),
          duration: Duration(seconds: 1),
        ),
      );
    }
  }
}
```

Platform-specific optimizations ensure the best user experience across  
different devices and operating systems. Adapt UI components to platform  
conventions, use appropriate navigation patterns, implement platform-specific  
input methods, and follow platform design guidelines. This creates familiar  
and intuitive experiences that feel native to each platform.

## Summary

This comprehensive guide covers 25 essential Flutter best practices across  
six critical categories:

**Project Structure & Organization (5 examples):**  
- Feature-based project organization with clear separation of concerns  
- Consistent naming conventions following Dart language standards  
- Proper use of const constructors for performance optimization  
- Business logic separation from UI components for better testability  
- Comprehensive error handling strategies with custom exceptions  

**Code Quality & Maintainability (5 examples):**  
- Widget lifecycle management with proper resource disposal  
- Responsive design implementation supporting multiple screen sizes  
- Performance optimization techniques including caching and RepaintBoundary  
- Effective state management with ChangeNotifier and immutable patterns  
- Security best practices including input validation and secure storage  

**Testing & Documentation (5 examples):**  
- Comprehensive testing strategies covering unit, widget, and integration tests  
- Accessibility implementation with semantic labels and inclusive design  
- Thorough documentation with meaningful comments and API descriptions  
- Platform-specific optimizations for native user experiences  
- Code organization patterns that support long-term maintenance  

**Performance & Security (5 examples):**  
- Memory management and resource optimization techniques  
- Secure authentication patterns with proper session management  
- Input sanitization and validation for security vulnerabilities  
- Caching strategies for improved application performance  
- Platform detection and adaptive UI components  

**UI/UX Best Practices (3 examples):**  
- Responsive layouts that work across all device sizes  
- Accessibility features supporting screen readers and assistive technologies  
- Platform-appropriate design patterns and interaction models  

**Advanced Patterns (2 examples):**  
- Complex state management with proper separation of concerns  
- Professional documentation standards with comprehensive examples  

Each example demonstrates real-world implementation patterns with complete,  
runnable code that follows Flutter and Dart best practices. The examples  
progress from fundamental concepts to advanced techniques, building a  
comprehensive foundation for professional Flutter development.  

These practices ensure applications are maintainable, scalable, performant,  
secure, accessible, and provide excellent user experiences across all  
platforms and devices. Following these guidelines will result in  
production-ready Flutter applications that meet professional standards.  
