# Flutter Navigation & Routing - 30 Essential Examples

This collection provides 30 navigation and routing examples covering basic  
to advanced navigation patterns in Flutter. Each example demonstrates  
different navigation concepts from simple page transitions to complex  
routing architectures.  

## Basic Navigation

Simple navigation between screens using Navigator push and pop.  

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
      title: 'Basic Navigation',
      home: const FirstPage(),
    );
  }
}

class FirstPage extends StatelessWidget {
  const FirstPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('First Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const SecondPage()),
            );
          },
          child: const Text('Go to Second Page'),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  const SecondPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: const Text('Go Back'),
        ),
      ),
    );
  }
}
```

Navigator.push adds a new route to the navigation stack using  
MaterialPageRoute. Navigator.pop removes the current route, returning  
to the previous screen. The AppBar automatically shows a back button.  

## Named Routes

Using named routes for organized navigation structure.  

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
      title: 'Named Routes',
      initialRoute: '/',
      routes: {
        '/': (context) => const HomeScreen(),
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
      appBar: AppBar(title: const Text('Home')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/profile');
              },
              child: const Text('Go to Profile'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/settings');
              },
              child: const Text('Go to Settings'),
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
      appBar: AppBar(title: const Text('Profile')),
      body: const Center(
        child: Text('Profile Screen', style: TextStyle(fontSize: 24)),
      ),
    );
  }
}

class SettingsScreen extends StatelessWidget {
  const SettingsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Settings')),
      body: const Center(
        child: Text('Settings Screen', style: TextStyle(fontSize: 24)),
      ),
    );
  }
}
```

Named routes provide a clean way to organize navigation. The routes  
map defines all available routes with their corresponding widgets.  
Navigator.pushNamed navigates using route names instead of widgets.  

## Passing Data Between Screens

Sending and receiving data during navigation.  

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
      title: 'Data Passing',
      home: const UserListScreen(),
    );
  }
}

class User {
  final String name;
  final String email;
  final int age;

  User(this.name, this.email, this.age);
}

class UserListScreen extends StatelessWidget {
  const UserListScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final users = [
      User('Alice Johnson', 'alice@email.com', 28),
      User('Bob Smith', 'bob@email.com', 34),
      User('Carol Wilson', 'carol@email.com', 25),
    ];

    return Scaffold(
      appBar: AppBar(title: const Text('Users')),
      body: ListView.builder(
        itemCount: users.length,
        itemBuilder: (context, index) {
          final user = users[index];
          return ListTile(
            title: Text(user.name),
            subtitle: Text(user.email),
            onTap: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => UserDetailsScreen(user: user),
                ),
              );
            },
          );
        },
      ),
    );
  }
}

class UserDetailsScreen extends StatelessWidget {
  final User user;

  const UserDetailsScreen({super.key, required this.user});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(user.name)),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Name: ${user.name}', style: const TextStyle(fontSize: 20)),
            const SizedBox(height: 10),
            Text('Email: ${user.email}', style: const TextStyle(fontSize: 18)),
            const SizedBox(height: 10),
            Text('Age: ${user.age}', style: const TextStyle(fontSize: 18)),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: const Text('Back to List'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Data can be passed to screens through constructor parameters. The  
receiving widget accepts the data and uses it to display content.  
This pattern works well for passing objects between screens.  

## Returning Data from Screens

Receiving data back from navigated screens.  

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
      title: 'Return Data',
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
  String _selectedOption = 'None';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Main Screen')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Selected: $_selectedOption',
              style: const TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () async {
                final result = await Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const SelectionScreen(),
                  ),
                );
                
                if (result != null) {
                  setState(() {
                    _selectedOption = result;
                  });
                }
              },
              child: const Text('Make Selection'),
            ),
          ],
        ),
      ),
    );
  }
}

class SelectionScreen extends StatelessWidget {
  const SelectionScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Make Selection')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context, 'Option A');
              },
              child: const Text('Choose Option A'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context, 'Option B');
              },
              child: const Text('Choose Option B'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context, 'Option C');
              },
              child: const Text('Choose Option C'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Navigator.pop can return data to the previous screen. The calling  
screen receives this data by awaiting the Navigator.push call.  
This pattern is useful for selection dialogs and forms.  

## Bottom Navigation

Implementing bottom navigation bar for main app sections.  

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
      title: 'Bottom Navigation',
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

  final List<Widget> _screens = [
    const HomeTab(),
    const SearchTab(),
    const ProfileTab(),
  ];

  void _onItemTapped(int index) {
    setState(() {
      _selectedIndex = index;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: _screens[_selectedIndex],
      bottomNavigationBar: BottomNavigationBar(
        items: const [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.search),
            label: 'Search',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.person),
            label: 'Profile',
          ),
        ],
        currentIndex: _selectedIndex,
        onTap: _onItemTapped,
      ),
    );
  }
}

class HomeTab extends StatelessWidget {
  const HomeTab({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Home')),
      body: const Center(
        child: Text('Home Tab Content', style: TextStyle(fontSize: 24)),
      ),
    );
  }
}

class SearchTab extends StatelessWidget {
  const SearchTab({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Search')),
      body: const Center(
        child: Text('Search Tab Content', style: TextStyle(fontSize: 24)),
      ),
    );
  }
}

class ProfileTab extends StatelessWidget {
  const ProfileTab({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Profile')),
      body: const Center(
        child: Text('Profile Tab Content', style: TextStyle(fontSize: 24)),
      ),
    );
  }
}
```

BottomNavigationBar provides tab-based navigation at the bottom of  
the screen. Each tab displays a different widget. The currentIndex  
tracks the active tab and onTap handles tab switches.  

## Tab Navigation

Creating tabbed interfaces with TabBar and TabBarView.  

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
      title: 'Tab Navigation',
      home: const TabScreen(),
    );
  }
}

class TabScreen extends StatelessWidget {
  const TabScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: 3,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('Tab Navigation'),
          bottom: const TabBar(
            tabs: [
              Tab(icon: Icon(Icons.camera_alt), text: 'Camera'),
              Tab(icon: Icon(Icons.chat), text: 'Chats'),
              Tab(icon: Icon(Icons.call), text: 'Calls'),
            ],
          ),
        ),
        body: const TabBarView(
          children: [
            CameraTab(),
            ChatsTab(),
            CallsTab(),
          ],
        ),
      ),
    );
  }
}

class CameraTab extends StatelessWidget {
  const CameraTab({super.key});

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.blue[900],
      child: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.camera_alt, size: 80, color: Colors.white),
            SizedBox(height: 20),
            Text(
              'Camera Tab',
              style: TextStyle(fontSize: 24, color: Colors.white),
            ),
          ],
        ),
      ),
    );
  }
}

class ChatsTab extends StatelessWidget {
  const ChatsTab({super.key});

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: 10,
      itemBuilder: (context, index) {
        return ListTile(
          leading: CircleAvatar(
            child: Text('${index + 1}'),
          ),
          title: Text('Contact ${index + 1}'),
          subtitle: Text('Last message from contact ${index + 1}'),
          onTap: () {
            ScaffoldMessenger.of(context).showSnackBar(
              SnackBar(content: Text('Tapped Contact ${index + 1}')),
            );
          },
        );
      },
    );
  }
}

class CallsTab extends StatelessWidget {
  const CallsTab({super.key});

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: 5,
      itemBuilder: (context, index) {
        return ListTile(
          leading: const Icon(Icons.call),
          title: Text('Call ${index + 1}'),
          subtitle: Text('Yesterday, ${10 + index}:30 AM'),
          trailing: IconButton(
            icon: const Icon(Icons.info),
            onPressed: () {
              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  title: Text('Call ${index + 1}'),
                  content: const Text('Call details would be shown here'),
                  actions: [
                    TextButton(
                      onPressed: () => Navigator.pop(context),
                      child: const Text('OK'),
                    ),
                  ],
                ),
              );
            },
          ),
        );
      },
    );
  }
}
```

DefaultTabController manages tab state automatically. TabBar displays  
the tab headers with icons and text, while TabBarView contains the  
content for each tab. Tabs sync automatically with swipe gestures.  

## Drawer Navigation

Implementing side navigation drawer for app sections.  

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
      title: 'Drawer Navigation',
      home: const DrawerScreen(),
    );
  }
}

class DrawerScreen extends StatefulWidget {
  const DrawerScreen({super.key});

  @override
  State<DrawerScreen> createState() => _DrawerScreenState();
}

class _DrawerScreenState extends State<DrawerScreen> {
  String _currentPage = 'Home';

  void _selectPage(String page) {
    setState(() {
      _currentPage = page;
    });
    Navigator.pop(context);
  }

  Widget _getCurrentPageWidget() {
    switch (_currentPage) {
      case 'Home':
        return const Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Icon(Icons.home, size: 80),
              SizedBox(height: 20),
              Text('Home Page', style: TextStyle(fontSize: 24)),
            ],
          ),
        );
      case 'Messages':
        return const Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Icon(Icons.message, size: 80),
              SizedBox(height: 20),
              Text('Messages Page', style: TextStyle(fontSize: 24)),
            ],
          ),
        );
      case 'Settings':
        return const Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Icon(Icons.settings, size: 80),
              SizedBox(height: 20),
              Text('Settings Page', style: TextStyle(fontSize: 24)),
            ],
          ),
        );
      default:
        return const Center(child: Text('Unknown Page'));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(_currentPage),
      ),
      drawer: Drawer(
        child: ListView(
          padding: EdgeInsets.zero,
          children: [
            const DrawerHeader(
              decoration: BoxDecoration(
                color: Colors.blue,
              ),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  CircleAvatar(
                    radius: 30,
                    child: Icon(Icons.person, size: 40),
                  ),
                  SizedBox(height: 10),
                  Text(
                    'John Doe',
                    style: TextStyle(
                      color: Colors.white,
                      fontSize: 18,
                    ),
                  ),
                  Text(
                    'john.doe@email.com',
                    style: TextStyle(
                      color: Colors.white70,
                      fontSize: 14,
                    ),
                  ),
                ],
              ),
            ),
            ListTile(
              leading: const Icon(Icons.home),
              title: const Text('Home'),
              selected: _currentPage == 'Home',
              onTap: () => _selectPage('Home'),
            ),
            ListTile(
              leading: const Icon(Icons.message),
              title: const Text('Messages'),
              selected: _currentPage == 'Messages',
              onTap: () => _selectPage('Messages'),
            ),
            ListTile(
              leading: const Icon(Icons.settings),
              title: const Text('Settings'),
              selected: _currentPage == 'Settings',
              onTap: () => _selectPage('Settings'),
            ),
            const Divider(),
            ListTile(
              leading: const Icon(Icons.logout),
              title: const Text('Logout'),
              onTap: () {
                Navigator.pop(context);
                ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(content: Text('Logged out')),
                );
              },
            ),
          ],
        ),
      ),
      body: _getCurrentPageWidget(),
    );
  }
}
```

Drawer provides side navigation accessible by swiping from the left  
edge or tapping the hamburger menu icon. DrawerHeader displays user  
information, while ListTile items handle page navigation.  

## Modal Navigation

Using modal screens for temporary content and dialogs.  

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
      title: 'Modal Navigation',
      home: const ModalScreen(),
    );
  }
}

class ModalScreen extends StatefulWidget {
  const ModalScreen({super.key});

  @override
  State<ModalScreen> createState() => _ModalScreenState();
}

class _ModalScreenState extends State<ModalScreen> {
  String _result = 'No action taken';

  void _showAlertDialog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Confirm Action'),
        content: const Text('Are you sure you want to proceed?'),
        actions: [
          TextButton(
            onPressed: () {
              Navigator.pop(context);
              setState(() {
                _result = 'Action cancelled';
              });
            },
            child: const Text('Cancel'),
          ),
          TextButton(
            onPressed: () {
              Navigator.pop(context);
              setState(() {
                _result = 'Action confirmed';
              });
            },
            child: const Text('Confirm'),
          ),
        ],
      ),
    );
  }

  void _showBottomSheet() {
    showModalBottomSheet(
      context: context,
      builder: (context) => Container(
        height: 300,
        padding: const EdgeInsets.all(20),
        child: Column(
          children: [
            const Text(
              'Choose an Option',
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),
            ListTile(
              leading: const Icon(Icons.edit),
              title: const Text('Edit'),
              onTap: () {
                Navigator.pop(context);
                setState(() {
                  _result = 'Edit selected';
                });
              },
            ),
            ListTile(
              leading: const Icon(Icons.share),
              title: const Text('Share'),
              onTap: () {
                Navigator.pop(context);
                setState(() {
                  _result = 'Share selected';
                });
              },
            ),
            ListTile(
              leading: const Icon(Icons.delete),
              title: const Text('Delete'),
              onTap: () {
                Navigator.pop(context);
                setState(() {
                  _result = 'Delete selected';
                });
              },
            ),
          ],
        ),
      ),
    );
  }

  void _showFullScreenModal() async {
    final result = await Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => const FullScreenModal(),
        fullscreenDialog: true,
      ),
    );
    
    if (result != null) {
      setState(() {
        _result = result;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Modal Navigation')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Result: $_result',
              style: const TextStyle(fontSize: 18),
              textAlign: TextAlign.center,
            ),
            const SizedBox(height: 40),
            ElevatedButton(
              onPressed: _showAlertDialog,
              child: const Text('Show Alert Dialog'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _showBottomSheet,
              child: const Text('Show Bottom Sheet'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _showFullScreenModal,
              child: const Text('Show Full Screen Modal'),
            ),
          ],
        ),
      ),
    );
  }
}

class FullScreenModal extends StatelessWidget {
  const FullScreenModal({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Full Screen Modal'),
        leading: IconButton(
          icon: const Icon(Icons.close),
          onPressed: () => Navigator.pop(context, 'Modal closed'),
        ),
      ),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.fullscreen, size: 80),
            SizedBox(height: 20),
            Text(
              'This is a full screen modal',
              style: TextStyle(fontSize: 24),
            ),
            SizedBox(height: 20),
            Text(
              'Use the close button to dismiss',
              style: TextStyle(fontSize: 16),
            ),
          ],
        ),
      ),
    );
  }
}
```

Modal navigation includes dialogs, bottom sheets, and full-screen  
modals. showDialog displays alert dialogs, showModalBottomSheet  
shows bottom sheets, and fullscreenDialog creates modal pages.  

## Custom Page Transitions

Creating custom animations for page transitions.  

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
      title: 'Custom Transitions',
      home: const TransitionScreen(),
    );
  }
}

class TransitionScreen extends StatelessWidget {
  const TransitionScreen({super.key});

  void _navigateWithSlide(BuildContext context) {
    Navigator.push(
      context,
      PageRouteBuilder(
        pageBuilder: (context, animation, secondaryAnimation) =>
            const DestinationScreen(title: 'Slide Transition'),
        transitionsBuilder: (context, animation, secondaryAnimation, child) {
          const begin = Offset(1.0, 0.0);
          const end = Offset.zero;
          const curve = Curves.ease;

          var tween = Tween(begin: begin, end: end).chain(
            CurveTween(curve: curve),
          );

          return SlideTransition(
            position: animation.drive(tween),
            child: child,
          );
        },
      ),
    );
  }

  void _navigateWithFade(BuildContext context) {
    Navigator.push(
      context,
      PageRouteBuilder(
        pageBuilder: (context, animation, secondaryAnimation) =>
            const DestinationScreen(title: 'Fade Transition'),
        transitionsBuilder: (context, animation, secondaryAnimation, child) {
          return FadeTransition(
            opacity: animation,
            child: child,
          );
        },
      ),
    );
  }

  void _navigateWithScale(BuildContext context) {
    Navigator.push(
      context,
      PageRouteBuilder(
        pageBuilder: (context, animation, secondaryAnimation) =>
            const DestinationScreen(title: 'Scale Transition'),
        transitionsBuilder: (context, animation, secondaryAnimation, child) {
          return ScaleTransition(
            scale: animation,
            child: child,
          );
        },
      ),
    );
  }

  void _navigateWithRotation(BuildContext context) {
    Navigator.push(
      context,
      PageRouteBuilder(
        pageBuilder: (context, animation, secondaryAnimation) =>
            const DestinationScreen(title: 'Rotation Transition'),
        transitionsBuilder: (context, animation, secondaryAnimation, child) {
          return RotationTransition(
            turns: animation,
            child: child,
          );
        },
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Custom Transitions')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () => _navigateWithSlide(context),
              child: const Text('Slide Transition'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () => _navigateWithFade(context),
              child: const Text('Fade Transition'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () => _navigateWithScale(context),
              child: const Text('Scale Transition'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () => _navigateWithRotation(context),
              child: const Text('Rotation Transition'),
            ),
          ],
        ),
      ),
    );
  }
}

class DestinationScreen extends StatelessWidget {
  final String title;

  const DestinationScreen({super.key, required this.title});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(title)),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              title,
              style: const TextStyle(fontSize: 24),
              textAlign: TextAlign.center,
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () => Navigator.pop(context),
              child: const Text('Go Back'),
            ),
          ],
        ),
      ),
    );
  }
}
```

PageRouteBuilder allows custom page transitions by defining the  
transitionsBuilder. Different transition widgets like SlideTransition,  
FadeTransition, ScaleTransition create various animation effects.  

## Navigation with Arguments

Using route arguments for flexible navigation.  

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
      title: 'Navigation Arguments',
      initialRoute: '/',
      routes: {
        '/': (context) => const ProductListScreen(),
        '/product': (context) => const ProductDetailsScreen(),
      },
    );
  }
}

class Product {
  final int id;
  final String name;
  final double price;
  final String description;
  final String imageUrl;

  Product({
    required this.id,
    required this.name,
    required this.price,
    required this.description,
    required this.imageUrl,
  });
}

class ProductListScreen extends StatelessWidget {
  const ProductListScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final products = [
      Product(
        id: 1,
        name: 'Laptop',
        price: 999.99,
        description: 'High-performance laptop for work and gaming',
        imageUrl: 'https://via.placeholder.com/300x200',
      ),
      Product(
        id: 2,
        name: 'Smartphone',
        price: 599.99,
        description: 'Latest smartphone with advanced features',
        imageUrl: 'https://via.placeholder.com/300x200',
      ),
      Product(
        id: 3,
        name: 'Tablet',
        price: 399.99,
        description: 'Portable tablet for entertainment and work',
        imageUrl: 'https://via.placeholder.com/300x200',
      ),
    ];

    return Scaffold(
      appBar: AppBar(title: const Text('Products')),
      body: ListView.builder(
        itemCount: products.length,
        itemBuilder: (context, index) {
          final product = products[index];
          return Card(
            margin: const EdgeInsets.all(8),
            child: ListTile(
              leading: Container(
                width: 60,
                height: 60,
                color: Colors.grey[300],
                child: const Icon(Icons.image),
              ),
              title: Text(product.name),
              subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
              onTap: () {
                Navigator.pushNamed(
                  context,
                  '/product',
                  arguments: product,
                );
              },
            ),
          );
        },
      ),
    );
  }
}

class ProductDetailsScreen extends StatelessWidget {
  const ProductDetailsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final product = ModalRoute.of(context)!.settings.arguments as Product;

    return Scaffold(
      appBar: AppBar(title: Text(product.name)),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Container(
              width: double.infinity,
              height: 200,
              color: Colors.grey[300],
              child: const Icon(Icons.image, size: 80),
            ),
            const SizedBox(height: 20),
            Text(
              product.name,
              style: const TextStyle(fontSize: 28, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            Text(
              '\$${product.price.toStringAsFixed(2)}',
              style: const TextStyle(
                fontSize: 24,
                color: Colors.green,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 20),
            const Text(
              'Description:',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            Text(
              product.description,
              style: const TextStyle(fontSize: 16),
            ),
            const Spacer(),
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                onPressed: () {
                  ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(
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
```

Route arguments allow passing data to named routes. Use  
Navigator.pushNamed with arguments parameter, then retrieve  
arguments using ModalRoute.of(context).settings.arguments.  

## Deep Linking

Implementing deep links for direct navigation to specific screens.  

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
      title: 'Deep Linking',
      initialRoute: '/',
      onGenerateRoute: (settings) {
        final uri = Uri.parse(settings.name!);
        
        if (uri.pathSegments.isEmpty) {
          return MaterialPageRoute(
            builder: (context) => const HomeScreen(),
          );
        }
        
        if (uri.pathSegments.first == 'user') {
          final userId = uri.pathSegments.length > 1 
              ? uri.pathSegments[1] 
              : null;
          
          if (userId != null) {
            return MaterialPageRoute(
              builder: (context) => UserProfileScreen(userId: userId),
            );
          }
        }
        
        if (uri.pathSegments.first == 'article') {
          final articleId = uri.pathSegments.length > 1 
              ? uri.pathSegments[1] 
              : null;
          final category = uri.queryParameters['category'];
          
          if (articleId != null) {
            return MaterialPageRoute(
              builder: (context) => ArticleScreen(
                articleId: articleId,
                category: category,
              ),
            );
          }
        }
        
        return MaterialPageRoute(
          builder: (context) => const NotFoundScreen(),
        );
      },
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Deep Linking Demo')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Deep Linking Examples',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/user/123');
              },
              child: const Text('Go to User Profile (ID: 123)'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(
                  context,
                  '/article/456?category=technology',
                );
              },
              child: const Text('Go to Article (ID: 456, Tech)'),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/nonexistent');
              },
              child: const Text('Go to Non-existent Route'),
            ),
          ],
        ),
      ),
    );
  }
}

class UserProfileScreen extends StatelessWidget {
  final String userId;

  const UserProfileScreen({super.key, required this.userId});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('User Profile: $userId')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const CircleAvatar(
              radius: 50,
              child: Icon(Icons.person, size: 60),
            ),
            const SizedBox(height: 20),
            Text(
              'User ID: $userId',
              style: const TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 10),
            const Text(
              'This screen was accessed via deep link',
              style: TextStyle(fontSize: 16, color: Colors.grey),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () => Navigator.pop(context),
              child: const Text('Go Back'),
            ),
          ],
        ),
      ),
    );
  }
}

class ArticleScreen extends StatelessWidget {
  final String articleId;
  final String? category;

  const ArticleScreen({
    super.key,
    required this.articleId,
    this.category,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Article: $articleId')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Article ID: $articleId',
              style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            if (category != null) ...[
              const SizedBox(height: 10),
              Text(
                'Category: $category',
                style: const TextStyle(fontSize: 18, color: Colors.blue),
              ),
            ],
            const SizedBox(height: 20),
            const Text(
              'This article was accessed via deep link with query parameters.',
              style: TextStyle(fontSize: 16),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () => Navigator.pop(context),
              child: const Text('Go Back'),
            ),
          ],
        ),
      ),
    );
  }
}

class NotFoundScreen extends StatelessWidget {
  const NotFoundScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Page Not Found')),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.error_outline, size: 80, color: Colors.red),
            SizedBox(height: 20),
            Text(
              '404 - Page Not Found',
              style: TextStyle(fontSize: 24),
            ),
            SizedBox(height: 10),
            Text(
              'The requested page could not be found.',
              style: TextStyle(fontSize: 16, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }
}
```

Deep linking uses onGenerateRoute to parse URLs and navigate to  
specific screens. URI parsing extracts path segments and query  
parameters to determine the appropriate route and data.  

## Nested Navigation

Implementing nested navigators for complex app structures.  

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
      title: 'Nested Navigation',
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
  final List<GlobalKey<NavigatorState>> _navigatorKeys = [
    GlobalKey<NavigatorState>(),
    GlobalKey<NavigatorState>(),
    GlobalKey<NavigatorState>(),
  ];

  void _onItemTapped(int index) {
    setState(() {
      _selectedIndex = index;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: IndexedStack(
        index: _selectedIndex,
        children: [
          Navigator(
            key: _navigatorKeys[0],
            onGenerateRoute: (settings) {
              return MaterialPageRoute(
                builder: (context) => const HomeFlow(),
              );
            },
          ),
          Navigator(
            key: _navigatorKeys[1],
            onGenerateRoute: (settings) {
              return MaterialPageRoute(
                builder: (context) => const ShopFlow(),
              );
            },
          ),
          Navigator(
            key: _navigatorKeys[2],
            onGenerateRoute: (settings) {
              return MaterialPageRoute(
                builder: (context) => const ProfileFlow(),
              );
            },
          ),
        ],
      ),
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _selectedIndex,
        onTap: _onItemTapped,
        items: const [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.shopping_cart),
            label: 'Shop',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.person),
            label: 'Profile',
          ),
        ],
      ),
    );
  }
}

class HomeFlow extends StatelessWidget {
  const HomeFlow({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Home')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Home Tab Navigation',
              style: TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const HomeDetailScreen(),
                  ),
                );
              },
              child: const Text('Go to Home Details'),
            ),
          ],
        ),
      ),
    );
  }
}

class HomeDetailScreen extends StatelessWidget {
  const HomeDetailScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Home Details')),
      body: const Center(
        child: Text(
          'Home Details Screen',
          style: TextStyle(fontSize: 20),
        ),
      ),
    );
  }
}

class ShopFlow extends StatelessWidget {
  const ShopFlow({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Shop')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Shop Tab Navigation',
              style: TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const ProductScreen(),
                  ),
                );
              },
              child: const Text('Browse Products'),
            ),
          ],
        ),
      ),
    );
  }
}

class ProductScreen extends StatelessWidget {
  const ProductScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Products')),
      body: ListView.builder(
        itemCount: 10,
        itemBuilder: (context, index) {
          return ListTile(
            title: Text('Product ${index + 1}'),
            subtitle: Text('Price: \$${(index + 1) * 10}.99'),
            onTap: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => ProductDetailScreen(
                    productId: index + 1,
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

class ProductDetailScreen extends StatelessWidget {
  final int productId;

  const ProductDetailScreen({super.key, required this.productId});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Product $productId')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Product $productId Details',
              style: const TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 20),
            Text(
              'Price: \$${productId * 10}.99',
              style: const TextStyle(fontSize: 18),
            ),
          ],
        ),
      ),
    );
  }
}

class ProfileFlow extends StatelessWidget {
  const ProfileFlow({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Profile')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const CircleAvatar(
              radius: 50,
              child: Icon(Icons.person, size: 60),
            ),
            const SizedBox(height: 20),
            const Text(
              'John Doe',
              style: TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const SettingsScreen(),
                  ),
                );
              },
              child: const Text('Settings'),
            ),
          ],
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
      appBar: AppBar(title: const Text('Settings')),
      body: const Center(
        child: Text(
          'Settings Screen',
          style: TextStyle(fontSize: 20),
        ),
      ),
    );
  }
}
```

Nested navigation uses multiple Navigator widgets with IndexedStack  
to maintain separate navigation stacks for each tab. Each tab has  
its own navigation history and can navigate independently.  

## Navigation Guards

Implementing navigation guards to control route access.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class AuthService {
  static bool _isLoggedIn = false;
  static String? _userRole;

  static bool get isLoggedIn => _isLoggedIn;
  static String? get userRole => _userRole;

  static void login(String role) {
    _isLoggedIn = true;
    _userRole = role;
  }

  static void logout() {
    _isLoggedIn = false;
    _userRole = null;
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Navigation Guards',
      initialRoute: '/',
      onGenerateRoute: (settings) {
        switch (settings.name) {
          case '/':
            return MaterialPageRoute(
              builder: (context) => const LoginScreen(),
            );
          case '/dashboard':
            if (!AuthService.isLoggedIn) {
              return MaterialPageRoute(
                builder: (context) => const UnauthorizedScreen(),
              );
            }
            return MaterialPageRoute(
              builder: (context) => const DashboardScreen(),
            );
          case '/admin':
            if (!AuthService.isLoggedIn) {
              return MaterialPageRoute(
                builder: (context) => const UnauthorizedScreen(),
              );
            }
            if (AuthService.userRole != 'admin') {
              return MaterialPageRoute(
                builder: (context) => const ForbiddenScreen(),
              );
            }
            return MaterialPageRoute(
              builder: (context) => const AdminScreen(),
            );
          default:
            return MaterialPageRoute(
              builder: (context) => const NotFoundScreen(),
            );
        }
      },
    );
  }
}

class LoginScreen extends StatefulWidget {
  const LoginScreen({super.key});

  @override
  State<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  String _selectedRole = 'user';

  void _login() {
    AuthService.login(_selectedRole);
    Navigator.pushReplacementNamed(context, '/dashboard');
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Login')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Login Screen',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            DropdownButton<String>(
              value: _selectedRole,
              items: const [
                DropdownMenuItem(value: 'user', child: Text('User')),
                DropdownMenuItem(value: 'admin', child: Text('Admin')),
              ],
              onChanged: (value) {
                setState(() {
                  _selectedRole = value!;
                });
              },
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: _login,
              child: const Text('Login'),
            ),
          ],
        ),
      ),
    );
  }
}

class DashboardScreen extends StatelessWidget {
  const DashboardScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Dashboard'),
        actions: [
          TextButton(
            onPressed: () {
              AuthService.logout();
              Navigator.pushReplacementNamed(context, '/');
            },
            child: const Text('Logout'),
          ),
        ],
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Welcome, ${AuthService.userRole}!',
              style: const TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/admin');
              },
              child: const Text('Go to Admin Panel'),
            ),
          ],
        ),
      ),
    );
  }
}

class AdminScreen extends StatelessWidget {
  const AdminScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Admin Panel')),
      body: const Center(
        child: Text(
          'Admin Panel - Restricted Access',
          style: TextStyle(fontSize: 20),
        ),
      ),
    );
  }
}

class UnauthorizedScreen extends StatelessWidget {
  const UnauthorizedScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Unauthorized')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Icon(Icons.lock, size: 80, color: Colors.red),
            const SizedBox(height: 20),
            const Text(
              'Access Denied',
              style: TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 10),
            const Text('Please log in to access this page.'),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushReplacementNamed(context, '/');
              },
              child: const Text('Go to Login'),
            ),
          ],
        ),
      ),
    );
  }
}

class ForbiddenScreen extends StatelessWidget {
  const ForbiddenScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Forbidden')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Icon(Icons.block, size: 80, color: Colors.red),
            const SizedBox(height: 20),
            const Text(
              'Access Forbidden',
              style: TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 10),
            const Text('You do not have permission to access this page.'),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: const Text('Go Back'),
            ),
          ],
        ),
      ),
    );
  }
}

class NotFoundScreen extends StatelessWidget {
  const NotFoundScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Page Not Found')),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.error_outline, size: 80, color: Colors.orange),
            SizedBox(height: 20),
            Text(
              '404 - Page Not Found',
              style: TextStyle(fontSize: 24),
            ),
          ],
        ),
      ),
    );
  }
}
```

Navigation guards use onGenerateRoute to check authentication and  
authorization before allowing access to protected routes. Different  
screens handle unauthorized access and permission errors.  

## Navigation State Management

Managing navigation state with providers and state management.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class NavigationState extends ChangeNotifier {
  String _currentRoute = '/';
  final List<String> _history = ['/'];

  String get currentRoute => _currentRoute;
  List<String> get history => List.unmodifiable(_history);
  bool get canGoBack => _history.length > 1;

  void navigateTo(String route) {
    _currentRoute = route;
    _history.add(route);
    notifyListeners();
  }

  void goBack() {
    if (canGoBack) {
      _history.removeLast();
      _currentRoute = _history.last;
      notifyListeners();
    }
  }

  void clearHistory() {
    _history.clear();
    _history.add('/');
    _currentRoute = '/';
    notifyListeners();
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => NavigationState(),
      child: MaterialApp(
        darkTheme: ThemeData.dark(),
        themeMode: ThemeMode.dark,
        title: 'Navigation State Management',
        home: const NavigationDemo(),
      ),
    );
  }
}

class ChangeNotifierProvider<T extends ChangeNotifier> extends InheritedNotifier<T> {
  const ChangeNotifierProvider({
    super.key,
    required T create,
    required Widget child,
  }) : _create = create, super(notifier: create, child: child);

  final T _create;

  static T of<T extends ChangeNotifier>(BuildContext context) {
    final provider = context.dependOnInheritedWidgetOfExactType<ChangeNotifierProvider<T>>();
    return provider!.notifier!;
  }

  @override
  bool updateShouldNotify(ChangeNotifierProvider<T> oldWidget) {
    return false;
  }
}

class NavigationDemo extends StatelessWidget {
  const NavigationDemo({super.key});

  @override
  Widget build(BuildContext context) {
    final navState = ChangeNotifierProvider.of<NavigationState>(context);
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Navigation State Demo'),
        actions: [
          IconButton(
            icon: const Icon(Icons.history),
            onPressed: () {
              _showHistoryDialog(context, navState);
            },
          ),
        ],
      ),
      body: AnimatedBuilder(
        animation: navState,
        builder: (context, child) {
          return _buildCurrentScreen(navState.currentRoute);
        },
      ),
      bottomNavigationBar: AnimatedBuilder(
        animation: navState,
        builder: (context, child) {
          return Container(
            padding: const EdgeInsets.all(16),
            child: Row(
              children: [
                Expanded(
                  child: Text(
                    'Current: ${navState.currentRoute}',
                    style: const TextStyle(fontSize: 16),
                  ),
                ),
                ElevatedButton(
                  onPressed: navState.canGoBack ? () => navState.goBack() : null,
                  child: const Text('Back'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () => navState.clearHistory(),
                  child: const Text('Clear'),
                ),
              ],
            ),
          );
        },
      ),
    );
  }

  Widget _buildCurrentScreen(String route) {
    switch (route) {
      case '/':
        return const HomeScreen();
      case '/page1':
        return const PageOne();
      case '/page2':
        return const PageTwo();
      case '/page3':
        return const PageThree();
      default:
        return const HomeScreen();
    }
  }

  void _showHistoryDialog(BuildContext context, NavigationState navState) {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Navigation History'),
        content: SizedBox(
          width: double.maxFinite,
          height: 200,
          child: ListView.builder(
            itemCount: navState.history.length,
            itemBuilder: (context, index) {
              final route = navState.history[index];
              final isActive = route == navState.currentRoute;
              return ListTile(
                title: Text(
                  route,
                  style: TextStyle(
                    fontWeight: isActive ? FontWeight.bold : FontWeight.normal,
                    color: isActive ? Colors.blue : null,
                  ),
                ),
                leading: Text('${index + 1}.'),
                onTap: () {
                  Navigator.pop(context);
                  navState.navigateTo(route);
                },
              );
            },
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

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final navState = ChangeNotifierProvider.of<NavigationState>(context);
    
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const Text(
            'Home Screen',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          const SizedBox(height: 30),
          ElevatedButton(
            onPressed: () => navState.navigateTo('/page1'),
            child: const Text('Go to Page 1'),
          ),
          const SizedBox(height: 15),
          ElevatedButton(
            onPressed: () => navState.navigateTo('/page2'),
            child: const Text('Go to Page 2'),
          ),
          const SizedBox(height: 15),
          ElevatedButton(
            onPressed: () => navState.navigateTo('/page3'),
            child: const Text('Go to Page 3'),
          ),
        ],
      ),
    );
  }
}

class PageOne extends StatelessWidget {
  const PageOne({super.key});

  @override
  Widget build(BuildContext context) {
    final navState = ChangeNotifierProvider.of<NavigationState>(context);
    
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const Text(
            'Page One',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          const SizedBox(height: 30),
          ElevatedButton(
            onPressed: () => navState.navigateTo('/page2'),
            child: const Text('Go to Page 2'),
          ),
          const SizedBox(height: 15),
          ElevatedButton(
            onPressed: () => navState.navigateTo('/'),
            child: const Text('Go to Home'),
          ),
        ],
      ),
    );
  }
}

class PageTwo extends StatelessWidget {
  const PageTwo({super.key});

  @override
  Widget build(BuildContext context) {
    final navState = ChangeNotifierProvider.of<NavigationState>(context);
    
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const Text(
            'Page Two',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          const SizedBox(height: 30),
          ElevatedButton(
            onPressed: () => navState.navigateTo('/page3'),
            child: const Text('Go to Page 3'),
          ),
          const SizedBox(height: 15),
          ElevatedButton(
            onPressed: () => navState.navigateTo('/page1'),
            child: const Text('Go to Page 1'),
          ),
        ],
      ),
    );
  }
}

class PageThree extends StatelessWidget {
  const PageThree({super.key});

  @override
  Widget build(BuildContext context) {
    final navState = ChangeNotifierProvider.of<NavigationState>(context);
    
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const Text(
            'Page Three',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          const SizedBox(height: 30),
          ElevatedButton(
            onPressed: () => navState.navigateTo('/'),
            child: const Text('Go to Home'),
          ),
          const SizedBox(height: 15),
          ElevatedButton(
            onPressed: () => navState.navigateTo('/page1'),
            child: const Text('Go to Page 1'),
          ),
        ],
      ),
    );
  }
}
```

Navigation state management uses ChangeNotifier to track the current  
route and navigation history. This provides centralized control over  
navigation state with the ability to go back and view history.  

## Hero Animations

Implementing hero animations for smooth transitions between screens.  

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
      title: 'Hero Animations',
      home: const HeroListScreen(),
    );
  }
}

class HeroItem {
  final int id;
  final String title;
  final String subtitle;
  final Color color;

  HeroItem(this.id, this.title, this.subtitle, this.color);
}

class HeroListScreen extends StatelessWidget {
  const HeroListScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final items = [
      HeroItem(1, 'Red Item', 'This is a red item', Colors.red),
      HeroItem(2, 'Blue Item', 'This is a blue item', Colors.blue),
      HeroItem(3, 'Green Item', 'This is a green item', Colors.green),
      HeroItem(4, 'Orange Item', 'This is an orange item', Colors.orange),
      HeroItem(5, 'Purple Item', 'This is a purple item', Colors.purple),
    ];

    return Scaffold(
      appBar: AppBar(title: const Text('Hero Animations')),
      body: GridView.builder(
        padding: const EdgeInsets.all(16),
        gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 2,
          childAspectRatio: 1,
          crossAxisSpacing: 16,
          mainAxisSpacing: 16,
        ),
        itemCount: items.length,
        itemBuilder: (context, index) {
          final item = items[index];
          return Hero(
            tag: 'hero-${item.id}',
            child: GestureDetector(
              onTap: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => HeroDetailScreen(item: item),
                  ),
                );
              },
              child: Card(
                color: item.color.withOpacity(0.7),
                child: Center(
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      Icon(
                        Icons.star,
                        size: 50,
                        color: Colors.white,
                      ),
                      const SizedBox(height: 10),
                      Text(
                        item.title,
                        style: const TextStyle(
                          color: Colors.white,
                          fontSize: 18,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ],
                  ),
                ),
              ),
            ),
          );
        },
      ),
    );
  }
}

class HeroDetailScreen extends StatelessWidget {
  final HeroItem item;

  const HeroDetailScreen({super.key, required this.item});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          Hero(
            tag: 'hero-${item.id}',
            child: Container(
              width: double.infinity,
              height: 300,
              color: item.color.withOpacity(0.7),
              child: const Center(
                child: Icon(
                  Icons.star,
                  size: 100,
                  color: Colors.white,
                ),
              ),
            ),
          ),
          Expanded(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  const SizedBox(height: 20),
                  Text(
                    item.title,
                    style: const TextStyle(
                      fontSize: 28,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  const SizedBox(height: 10),
                  Text(
                    item.subtitle,
                    style: const TextStyle(fontSize: 18),
                  ),
                  const SizedBox(height: 30),
                  const Text(
                    'This screen demonstrates hero animations. The card from the previous screen smoothly transitions to become the header of this screen.',
                    style: TextStyle(fontSize: 16),
                  ),
                  const Spacer(),
                  SizedBox(
                    width: double.infinity,
                    child: ElevatedButton(
                      onPressed: () => Navigator.pop(context),
                      child: const Text('Go Back'),
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
}
```

Hero animations create smooth transitions between screens by animating  
shared elements. The Hero widget with matching tags automatically  
animates the element from one screen position to another.  

## Navigation Replacement

Using pushReplacement and pushNamedAndRemoveUntil for stack management.  

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
      title: 'Navigation Replacement',
      home: const SplashScreen(),
    );
  }
}

class SplashScreen extends StatefulWidget {
  const SplashScreen({super.key});

  @override
  State<SplashScreen> createState() => _SplashScreenState();
}

class _SplashScreenState extends State<SplashScreen> {
  @override
  void initState() {
    super.initState();
    _navigateToLogin();
  }

  Future<void> _navigateToLogin() async {
    await Future.delayed(const Duration(seconds: 2));
    if (mounted) {
      Navigator.pushReplacement(
        context,
        MaterialPageRoute(builder: (context) => const LoginScreen()),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return const Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            CircularProgressIndicator(),
            SizedBox(height: 20),
            Text(
              'Loading...',
              style: TextStyle(fontSize: 18),
            ),
          ],
        ),
      ),
    );
  }
}

class LoginScreen extends StatelessWidget {
  const LoginScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Login')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Login Screen',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(builder: (context) => const OnboardingScreen()),
                );
              },
              child: const Text('Login & Start Onboarding'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.pushAndRemoveUntil(
                  context,
                  MaterialPageRoute(builder: (context) => const HomeScreen()),
                  (Route<dynamic> route) => false,
                );
              },
              child: const Text('Skip to Home (Clear Stack)'),
            ),
          ],
        ),
      ),
    );
  }
}

class OnboardingScreen extends StatefulWidget {
  const OnboardingScreen({super.key});

  @override
  State<OnboardingScreen> createState() => _OnboardingScreenState();
}

class _OnboardingScreenState extends State<OnboardingScreen> {
  int _currentStep = 0;
  final PageController _pageController = PageController();

  final List<OnboardingPage> _pages = [
    OnboardingPage(
      title: 'Welcome',
      description: 'Welcome to our amazing app!',
      icon: Icons.waving_hand,
    ),
    OnboardingPage(
      title: 'Features',
      description: 'Discover all the great features we offer.',
      icon: Icons.star,
    ),
    OnboardingPage(
      title: 'Get Started',
      description: 'Ready to begin your journey?',
      icon: Icons.rocket_launch,
    ),
  ];

  void _nextPage() {
    if (_currentStep < _pages.length - 1) {
      _currentStep++;
      _pageController.nextPage(
        duration: const Duration(milliseconds: 300),
        curve: Curves.ease,
      );
    } else {
      Navigator.pushNamedAndRemoveUntil(
        context,
        '/home',
        (Route<dynamic> route) => false,
      );
    }
  }

  void _skipOnboarding() {
    Navigator.pushAndRemoveUntil(
      context,
      MaterialPageRoute(builder: (context) => const HomeScreen()),
      (Route<dynamic> route) => false,
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          Expanded(
            child: PageView.builder(
              controller: _pageController,
              onPageChanged: (index) {
                setState(() {
                  _currentStep = index;
                });
              },
              itemCount: _pages.length,
              itemBuilder: (context, index) {
                final page = _pages[index];
                return Padding(
                  padding: const EdgeInsets.all(32),
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      Icon(
                        page.icon,
                        size: 100,
                        color: Colors.blue,
                      ),
                      const SizedBox(height: 30),
                      Text(
                        page.title,
                        style: const TextStyle(
                          fontSize: 28,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      const SizedBox(height: 20),
                      Text(
                        page.description,
                        textAlign: TextAlign.center,
                        style: const TextStyle(fontSize: 18),
                      ),
                    ],
                  ),
                );
              },
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(32),
            child: Column(
              children: [
                Row(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: List.generate(
                    _pages.length,
                    (index) => Container(
                      margin: const EdgeInsets.symmetric(horizontal: 4),
                      width: 10,
                      height: 10,
                      decoration: BoxDecoration(
                        shape: BoxShape.circle,
                        color: index == _currentStep
                            ? Colors.blue
                            : Colors.grey,
                      ),
                    ),
                  ),
                ),
                const SizedBox(height: 30),
                Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: [
                    TextButton(
                      onPressed: _skipOnboarding,
                      child: const Text('Skip'),
                    ),
                    ElevatedButton(
                      onPressed: _nextPage,
                      child: Text(
                        _currentStep == _pages.length - 1
                            ? 'Get Started'
                            : 'Next',
                      ),
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
}

class OnboardingPage {
  final String title;
  final String description;
  final IconData icon;

  OnboardingPage({
    required this.title,
    required this.description,
    required this.icon,
  });
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home'),
        automaticallyImplyLeading: false,
      ),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Welcome to the Home Screen!',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 20),
            Text(
              'Navigation stack has been cleared.',
              style: TextStyle(fontSize: 16),
            ),
          ],
        ),
      ),
    );
  }
}
```

Navigation replacement uses pushReplacement to replace the current  
screen and pushAndRemoveUntil to clear the entire navigation stack.  
This is useful for login flows and onboarding sequences.  

## Pop Until Route

Using popUntil to navigate back to specific routes in the stack.  

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
      title: 'Pop Until Route',
      initialRoute: '/',
      routes: {
        '/': (context) => const HomeScreen(),
        '/level1': (context) => const LevelOneScreen(),
        '/level2': (context) => const LevelTwoScreen(),
        '/level3': (context) => const LevelThreeScreen(),
        '/level4': (context) => const LevelFourScreen(),
      },
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Home')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Home Screen (Level 0)',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/level1');
              },
              child: const Text('Go to Level 1'),
            ),
          ],
        ),
      ),
    );
  }
}

class LevelOneScreen extends StatelessWidget {
  const LevelOneScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Level 1')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Level 1 Screen',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/level2');
              },
              child: const Text('Go to Level 2'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/'));
              },
              child: const Text('Pop to Home'),
            ),
          ],
        ),
      ),
    );
  }
}

class LevelTwoScreen extends StatelessWidget {
  const LevelTwoScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Level 2')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Level 2 Screen',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/level3');
              },
              child: const Text('Go to Level 3'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/level1'));
              },
              child: const Text('Pop to Level 1'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/'));
              },
              child: const Text('Pop to Home'),
            ),
          ],
        ),
      ),
    );
  }
}

class LevelThreeScreen extends StatelessWidget {
  const LevelThreeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Level 3')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Level 3 Screen',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/level4');
              },
              child: const Text('Go to Level 4'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/level1'));
              },
              child: const Text('Pop to Level 1'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/'));
              },
              child: const Text('Pop to Home'),
            ),
          ],
        ),
      ),
    );
  }
}

class LevelFourScreen extends StatelessWidget {
  const LevelFourScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Level 4')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Level 4 Screen (Deepest)',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/level2'));
              },
              child: const Text('Pop to Level 2'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/level1'));
              },
              child: const Text('Pop to Level 1'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/'));
              },
              child: const Text('Pop to Home'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Navigator.popUntil removes routes from the stack until it reaches  
a specific route. This is useful for navigating back to a particular  
screen in a deep navigation hierarchy.  

## Route Observers

Monitoring navigation events with RouteObserver.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class NavigationLogger extends RouteObserver<PageRoute<dynamic>> {
  void _log(String message) {
    print('Navigation: $message');
  }

  @override
  void didPush(Route<dynamic> route, Route<dynamic>? previousRoute) {
    super.didPush(route, previousRoute);
    if (route is PageRoute) {
      _log('Pushed: ${route.settings.name ?? route.runtimeType}');
    }
  }

  @override
  void didPop(Route<dynamic> route, Route<dynamic>? previousRoute) {
    super.didPop(route, previousRoute);
    if (route is PageRoute) {
      _log('Popped: ${route.settings.name ?? route.runtimeType}');
    }
  }

  @override
  void didReplace({Route<dynamic>? newRoute, Route<dynamic>? oldRoute}) {
    super.didReplace(newRoute: newRoute, oldRoute: oldRoute);
    if (newRoute is PageRoute && oldRoute is PageRoute) {
      _log('Replaced ${oldRoute.settings.name ?? oldRoute.runtimeType} with ${newRoute.settings.name ?? newRoute.runtimeType}');
    }
  }

  @override
  void didRemove(Route<dynamic> route, Route<dynamic>? previousRoute) {
    super.didRemove(route, previousRoute);
    if (route is PageRoute) {
      _log('Removed: ${route.settings.name ?? route.runtimeType}');
    }
  }
}

class MyApp extends StatelessWidget {
  final NavigationLogger _navigationLogger = NavigationLogger();

  MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Route Observers',
      navigatorObservers: [_navigationLogger],
      initialRoute: '/',
      routes: {
        '/': (context) => const ObserverHomeScreen(),
        '/page1': (context) => const ObserverPageOne(),
        '/page2': (context) => const ObserverPageTwo(),
        '/page3': (context) => const ObserverPageThree(),
      },
    );
  }
}

class ObserverHomeScreen extends StatefulWidget {
  const ObserverHomeScreen({super.key});

  @override
  State<ObserverHomeScreen> createState() => _ObserverHomeScreenState();
}

class _ObserverHomeScreenState extends State<ObserverHomeScreen> with RouteAware {
  final RouteObserver<PageRoute<dynamic>> _routeObserver = RouteObserver<PageRoute<dynamic>>();

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    _routeObserver.subscribe(this, ModalRoute.of(context)! as PageRoute<dynamic>);
  }

  @override
  void dispose() {
    _routeObserver.unsubscribe(this);
    super.dispose();
  }

  @override
  void didPopNext() {
    print('Home: Returned to this screen');
  }

  @override
  void didPushNext() {
    print('Home: Navigated away from this screen');
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Route Observer Demo'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Route Observer Home',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),
            const Text(
              'Check console for navigation logs',
              style: TextStyle(fontSize: 16, color: Colors.grey),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/page1');
              },
              child: const Text('Go to Page 1'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/page2');
              },
              child: const Text('Go to Page 2'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/page3');
              },
              child: const Text('Go to Page 3'),
            ),
          ],
        ),
      ),
    );
  }
}

class ObserverPageOne extends StatelessWidget {
  const ObserverPageOne({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Page 1')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Observer Page 1',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/page2');
              },
              child: const Text('Go to Page 2'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.pushReplacementNamed(context, '/page3');
              },
              child: const Text('Replace with Page 3'),
            ),
          ],
        ),
      ),
    );
  }
}

class ObserverPageTwo extends StatelessWidget {
  const ObserverPageTwo({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Page 2')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Observer Page 2',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/page3');
              },
              child: const Text('Go to Page 3'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.popUntil(context, ModalRoute.withName('/'));
              },
              child: const Text('Pop to Home'),
            ),
          ],
        ),
      ),
    );
  }
}

class ObserverPageThree extends StatelessWidget {
  const ObserverPageThree({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Page 3')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Observer Page 3',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: const Text('Go Back'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamedAndRemoveUntil(
                  context,
                  '/',
                  (Route<dynamic> route) => false,
                );
              },
              child: const Text('Clear Stack to Home'),
            ),
          ],
        ),
      ),
    );
  }
}
```

RouteObserver monitors navigation events like push, pop, replace,  
and remove. RouteAware widgets can subscribe to receive notifications  
when they become active or inactive in the navigation stack.  

## Route Information Parser

Advanced routing with Router and RouteInformationParser.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class AppRoute {
  final String name;
  final String? id;

  AppRoute(this.name, {this.id});

  @override
  bool operator ==(Object other) {
    if (other is! AppRoute) return false;
    return name == other.name && id == other.id;
  }

  @override
  int get hashCode => Object.hash(name, id);
}

class AppRouteInformationParser extends RouteInformationParser<AppRoute> {
  @override
  Future<AppRoute> parseRouteInformation(RouteInformation routeInformation) async {
    final location = routeInformation.location ?? '/';
    final uri = Uri.parse(location);

    if (uri.pathSegments.isEmpty) {
      return AppRoute('home');
    }

    final path = uri.pathSegments.first;
    switch (path) {
      case 'products':
        if (uri.pathSegments.length > 1) {
          return AppRoute('product', id: uri.pathSegments[1]);
        }
        return AppRoute('products');
      case 'settings':
        return AppRoute('settings');
      default:
        return AppRoute('unknown');
    }
  }

  @override
  RouteInformation restoreRouteInformation(AppRoute configuration) {
    switch (configuration.name) {
      case 'home':
        return const RouteInformation(location: '/');
      case 'products':
        return const RouteInformation(location: '/products');
      case 'product':
        return RouteInformation(location: '/products/${configuration.id}');
      case 'settings':
        return const RouteInformation(location: '/settings');
      default:
        return const RouteInformation(location: '/');
    }
  }
}

class AppRouterDelegate extends RouterDelegate<AppRoute>
    with ChangeNotifier, PopNavigatorRouterDelegateMixin<AppRoute> {
  
  @override
  final GlobalKey<NavigatorState> navigatorKey = GlobalKey<NavigatorState>();

  AppRoute _currentRoute = AppRoute('home');

  AppRoute get currentRoute => _currentRoute;

  void _navigate(AppRoute route) {
    _currentRoute = route;
    notifyListeners();
  }

  @override
  AppRoute get currentConfiguration => _currentRoute;

  @override
  Widget build(BuildContext context) {
    return Navigator(
      key: navigatorKey,
      pages: _buildPages(),
      onPopPage: (route, result) {
        if (!route.didPop(result)) return false;
        _navigate(AppRoute('home'));
        return true;
      },
    );
  }

  List<Page> _buildPages() {
    final List<Page> pages = [];

    // Always include home page
    pages.add(
      const MaterialPage(
        key: ValueKey('home'),
        child: RouterHomeScreen(),
      ),
    );

    // Add additional pages based on current route
    switch (_currentRoute.name) {
      case 'products':
        pages.add(
          const MaterialPage(
            key: ValueKey('products'),
            child: ProductsScreen(),
          ),
        );
        break;
      case 'product':
        pages.add(
          const MaterialPage(
            key: ValueKey('products'),
            child: ProductsScreen(),
          ),
        );
        pages.add(
          MaterialPage(
            key: ValueKey('product-${_currentRoute.id}'),
            child: ProductDetailScreen(productId: _currentRoute.id!),
          ),
        );
        break;
      case 'settings':
        pages.add(
          const MaterialPage(
            key: ValueKey('settings'),
            child: RouterSettingsScreen(),
          ),
        );
        break;
    }

    return pages;
  }

  @override
  Future<void> setNewRoutePath(AppRoute configuration) async {
    _currentRoute = configuration;
    notifyListeners();
  }

  void goToProducts() => _navigate(AppRoute('products'));
  void goToProduct(String id) => _navigate(AppRoute('product', id: id));
  void goToSettings() => _navigate(AppRoute('settings'));
  void goHome() => _navigate(AppRoute('home'));
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp.router(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Route Information Parser',
      routeInformationParser: AppRouteInformationParser(),
      routerDelegate: AppRouterDelegate(),
    );
  }
}

class RouterHomeScreen extends StatelessWidget {
  const RouterHomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final delegate = Router.of(context).routerDelegate as AppRouterDelegate;

    return Scaffold(
      appBar: AppBar(title: const Text('Router Demo Home')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Advanced Router Example',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () => delegate.goToProducts(),
              child: const Text('View Products'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () => delegate.goToSettings(),
              child: const Text('Settings'),
            ),
          ],
        ),
      ),
    );
  }
}

class ProductsScreen extends StatelessWidget {
  const ProductsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final delegate = Router.of(context).routerDelegate as AppRouterDelegate;

    return Scaffold(
      appBar: AppBar(title: const Text('Products')),
      body: ListView.builder(
        itemCount: 5,
        itemBuilder: (context, index) {
          final productId = (index + 1).toString();
          return ListTile(
            title: Text('Product $productId'),
            subtitle: Text('Description for product $productId'),
            onTap: () => delegate.goToProduct(productId),
          );
        },
      ),
    );
  }
}

class ProductDetailScreen extends StatelessWidget {
  final String productId;

  const ProductDetailScreen({super.key, required this.productId});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Product $productId')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Product $productId Details',
              style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),
            const Text(
              'This is a detailed view of the selected product.',
              style: TextStyle(fontSize: 16),
            ),
          ],
        ),
      ),
    );
  }
}

class RouterSettingsScreen extends StatelessWidget {
  const RouterSettingsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Settings')),
      body: const Center(
        child: Text(
          'Settings Screen',
          style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
        ),
      ),
    );
  }
}
```

Router with RouteInformationParser provides advanced routing  
capabilities with URL synchronization and complex navigation patterns.  
This enables declarative routing with proper browser support.  

## Navigation Restoration

Implementing state restoration for navigation history.  

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
      title: 'Navigation Restoration',
      restorationScopeId: 'app',
      home: const RestorationHomeScreen(),
    );
  }
}

class RestorationHomeScreen extends StatefulWidget {
  const RestorationHomeScreen({super.key});

  @override
  State<RestorationHomeScreen> createState() => _RestorationHomeScreenState();
}

class _RestorationHomeScreenState extends State<RestorationHomeScreen>
    with RestorationMixin {
  
  final RestorableInt _counter = RestorableInt(0);
  final RestorableString _selectedPage = RestorableString('home');

  @override
  String? get restorationId => 'home_screen';

  @override
  void restoreState(RestorationBucket? oldBucket, bool initialRestore) {
    registerForRestoration(_counter, 'counter');
    registerForRestoration(_selectedPage, 'selected_page');
  }

  void _incrementCounter() {
    setState(() {
      _counter.value++;
    });
  }

  void _navigateToDetails() {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => RestorationDetailsScreen(
          counter: _counter.value,
        ),
        settings: const RouteSettings(name: '/details'),
      ),
    );
  }

  void _changePage(String page) {
    setState(() {
      _selectedPage.value = page;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Restoration Demo'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Selected Page: ${_selectedPage.value}',
              style: const TextStyle(fontSize: 20),
            ),
            const SizedBox(height: 20),
            Text(
              'Counter: ${_counter.value}',
              style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: _incrementCounter,
              child: const Text('Increment Counter'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: _navigateToDetails,
              child: const Text('Go to Details'),
            ),
            const SizedBox(height: 30),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                ElevatedButton(
                  onPressed: () => _changePage('home'),
                  child: const Text('Home'),
                ),
                ElevatedButton(
                  onPressed: () => _changePage('profile'),
                  child: const Text('Profile'),
                ),
                ElevatedButton(
                  onPressed: () => _changePage('settings'),
                  child: const Text('Settings'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  @override
  void dispose() {
    _counter.dispose();
    _selectedPage.dispose();
    super.dispose();
  }
}

class RestorationDetailsScreen extends StatefulWidget {
  final int counter;

  const RestorationDetailsScreen({super.key, required this.counter});

  @override
  State<RestorationDetailsScreen> createState() => _RestorationDetailsScreenState();
}

class _RestorationDetailsScreenState extends State<RestorationDetailsScreen>
    with RestorationMixin {
  
  final RestorableString _notes = RestorableString('');

  @override
  String? get restorationId => 'details_screen';

  @override
  void restoreState(RestorationBucket? oldBucket, bool initialRestore) {
    registerForRestoration(_notes, 'notes');
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Details')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Counter from previous screen: ${widget.counter}',
              style: const TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            const Text(
              'Notes (will be restored):',
              style: TextStyle(fontSize: 18),
            ),
            const SizedBox(height: 10),
            TextField(
              controller: TextEditingController(text: _notes.value),
              onChanged: (value) {
                _notes.value = value;
              },
              decoration: const InputDecoration(
                hintText: 'Enter your notes here...',
                border: OutlineInputBorder(),
              ),
              maxLines: 5,
            ),
            const SizedBox(height: 30),
            const Text(
              'State restoration preserves data across app restarts.',
              style: TextStyle(fontSize: 16, color: Colors.grey),
            ),
          ],
        ),
      ),
    );
  }

  @override
  void dispose() {
    _notes.dispose();
    super.dispose();
  }
}
```

Navigation restoration preserves app state including navigation  
history and screen data across app lifecycle events using  
RestorationMixin and Restorable values.  

## WillPopScope Navigation

Controlling back button behavior with WillPopScope.  

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
      title: 'WillPopScope Demo',
      home: const WillPopHomeScreen(),
    );
  }
}

class WillPopHomeScreen extends StatelessWidget {
  const WillPopHomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('WillPopScope Demo')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'WillPopScope Examples',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const ConfirmExitScreen(),
                  ),
                );
              },
              child: const Text('Confirm Exit Screen'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const UnsavedFormScreen(),
                  ),
                );
              },
              child: const Text('Unsaved Form Screen'),
            ),
            const SizedBox(height: 15),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const TimerScreen(),
                  ),
                );
              },
              child: const Text('Timer Screen'),
            ),
          ],
        ),
      ),
    );
  }
}

class ConfirmExitScreen extends StatelessWidget {
  const ConfirmExitScreen({super.key});

  Future<bool> _onWillPop(BuildContext context) async {
    final shouldPop = await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Confirm Exit'),
        content: const Text('Are you sure you want to leave this screen?'),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context, false),
            child: const Text('Cancel'),
          ),
          TextButton(
            onPressed: () => Navigator.pop(context, true),
            child: const Text('Exit'),
          ),
        ],
      ),
    );
    return shouldPop ?? false;
  }

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: () => _onWillPop(context),
      child: Scaffold(
        appBar: AppBar(title: const Text('Confirm Exit')),
        body: const Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Icon(Icons.warning, size: 80, color: Colors.orange),
              SizedBox(height: 20),
              Text(
                'This screen requires confirmation to exit',
                style: TextStyle(fontSize: 18),
                textAlign: TextAlign.center,
              ),
              SizedBox(height: 20),
              Text(
                'Try pressing the back button or using the back arrow',
                style: TextStyle(fontSize: 14, color: Colors.grey),
                textAlign: TextAlign.center,
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class UnsavedFormScreen extends StatefulWidget {
  const UnsavedFormScreen({super.key});

  @override
  State<UnsavedFormScreen> createState() => _UnsavedFormScreenState();
}

class _UnsavedFormScreenState extends State<UnsavedFormScreen> {
  final TextEditingController _nameController = TextEditingController();
  final TextEditingController _emailController = TextEditingController();
  bool _hasUnsavedChanges = false;

  void _onTextChanged() {
    if (!_hasUnsavedChanges) {
      setState(() {
        _hasUnsavedChanges = true;
      });
    }
  }

  Future<bool> _onWillPop() async {
    if (!_hasUnsavedChanges) return true;

    final shouldPop = await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Unsaved Changes'),
        content: const Text(
          'You have unsaved changes. Are you sure you want to leave without saving?'
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context, false),
            child: const Text('Cancel'),
          ),
          TextButton(
            onPressed: () => Navigator.pop(context, true),
            child: const Text('Discard'),
          ),
        ],
      ),
    );
    return shouldPop ?? false;
  }

  void _saveForm() {
    setState(() {
      _hasUnsavedChanges = false;
    });
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(content: Text('Form saved!')),
    );
  }

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: _onWillPop,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('Form with Unsaved Changes'),
          actions: [
            if (_hasUnsavedChanges)
              const Icon(Icons.circle, color: Colors.red, size: 12),
          ],
        ),
        body: Padding(
          padding: const EdgeInsets.all(16),
          child: Column(
            children: [
              TextField(
                controller: _nameController,
                onChanged: (_) => _onTextChanged(),
                decoration: const InputDecoration(
                  labelText: 'Name',
                  border: OutlineInputBorder(),
                ),
              ),
              const SizedBox(height: 16),
              TextField(
                controller: _emailController,
                onChanged: (_) => _onTextChanged(),
                decoration: const InputDecoration(
                  labelText: 'Email',
                  border: OutlineInputBorder(),
                ),
              ),
              const SizedBox(height: 30),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: _saveForm,
                  child: const Text('Save'),
                ),
              ),
              const SizedBox(height: 20),
              if (_hasUnsavedChanges)
                const Text(
                  'You have unsaved changes',
                  style: TextStyle(color: Colors.red),
                ),
            ],
          ),
        ),
      ),
    );
  }
}

class TimerScreen extends StatefulWidget {
  const TimerScreen({super.key});

  @override
  State<TimerScreen> createState() => _TimerScreenState();
}

class _TimerScreenState extends State<TimerScreen> {
  int _seconds = 0;
  bool _isRunning = false;
  
  void _startTimer() {
    setState(() {
      _isRunning = true;
    });
    Future.doWhile(() async {
      await Future.delayed(const Duration(seconds: 1));
      if (mounted && _isRunning) {
        setState(() {
          _seconds++;
        });
        return true;
      }
      return false;
    });
  }

  void _stopTimer() {
    setState(() {
      _isRunning = false;
    });
  }

  Future<bool> _onWillPop() async {
    if (!_isRunning) return true;

    final shouldPop = await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Timer Running'),
        content: const Text(
          'The timer is still running. Are you sure you want to exit?'
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context, false),
            child: const Text('Continue Timer'),
          ),
          TextButton(
            onPressed: () => Navigator.pop(context, true),
            child: const Text('Exit'),
          ),
        ],
      ),
    );
    return shouldPop ?? false;
  }

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: _onWillPop,
      child: Scaffold(
        appBar: AppBar(title: const Text('Timer')),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text(
                '$_seconds seconds',
                style: const TextStyle(fontSize: 48, fontWeight: FontWeight.bold),
              ),
              const SizedBox(height: 30),
              Row(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  ElevatedButton(
                    onPressed: _isRunning ? null : _startTimer,
                    child: const Text('Start'),
                  ),
                  const SizedBox(width: 20),
                  ElevatedButton(
                    onPressed: _isRunning ? _stopTimer : null,
                    child: const Text('Stop'),
                  ),
                ],
              ),
              const SizedBox(height: 30),
              if (_isRunning)
                const Text(
                  'Timer is running - exit will be confirmed',
                  style: TextStyle(color: Colors.orange),
                ),
            ],
          ),
        ),
      ),
    );
  }
}
```

WillPopScope intercepts back navigation to show confirmation dialogs  
or prevent navigation when there are unsaved changes or running  
processes that shouldn't be interrupted.  

## Dynamic Route Generation

Generating routes dynamically based on app state and user permissions.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class UserRole {
  static const String guest = 'guest';
  static const String user = 'user';
  static const String admin = 'admin';
}

class AppState {
  static String currentUserRole = UserRole.guest;
  static bool isDarkMode = true;
  static List<String> availableFeatures = [];

  static void setUserRole(String role) {
    currentUserRole = role;
    _updateAvailableFeatures();
  }

  static void _updateAvailableFeatures() {
    switch (currentUserRole) {
      case UserRole.admin:
        availableFeatures = ['dashboard', 'users', 'analytics', 'settings'];
        break;
      case UserRole.user:
        availableFeatures = ['dashboard', 'profile', 'settings'];
        break;
      case UserRole.guest:
        availableFeatures = ['login', 'about'];
        break;
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
      title: 'Dynamic Route Generation',
      initialRoute: '/',
      onGenerateRoute: _generateRoute,
    );
  }

  Route<dynamic>? _generateRoute(RouteSettings settings) {
    final args = settings.arguments as Map<String, dynamic>? ?? {};

    switch (settings.name) {
      case '/':
        return MaterialPageRoute(
          builder: (context) => const DynamicHomeScreen(),
        );
      
      case '/login':
        if (AppState.availableFeatures.contains('login')) {
          return MaterialPageRoute(
            builder: (context) => const LoginScreen(),
          );
        }
        break;
      
      case '/dashboard':
        if (AppState.availableFeatures.contains('dashboard')) {
          return MaterialPageRoute(
            builder: (context) => const DashboardScreen(),
          );
        }
        break;
      
      case '/users':
        if (AppState.availableFeatures.contains('users')) {
          return MaterialPageRoute(
            builder: (context) => const UsersScreen(),
          );
        }
        break;
      
      case '/analytics':
        if (AppState.availableFeatures.contains('analytics')) {
          return MaterialPageRoute(
            builder: (context) => AnalyticsScreen(
              chartType: args['chartType'] ?? 'bar',
            ),
          );
        }
        break;
      
      case '/profile':
        if (AppState.availableFeatures.contains('profile')) {
          return MaterialPageRoute(
            builder: (context) => const ProfileScreen(),
          );
        }
        break;
      
      case '/settings':
        if (AppState.availableFeatures.contains('settings')) {
          return MaterialPageRoute(
            builder: (context) => const SettingsScreen(),
          );
        }
        break;
      
      case '/about':
        if (AppState.availableFeatures.contains('about')) {
          return MaterialPageRoute(
            builder: (context) => const AboutScreen(),
          );
        }
        break;
    }

    // Default to access denied screen
    return MaterialPageRoute(
      builder: (context) => const AccessDeniedScreen(),
    );
  }
}

class DynamicHomeScreen extends StatefulWidget {
  const DynamicHomeScreen({super.key});

  @override
  State<DynamicHomeScreen> createState() => _DynamicHomeScreenState();
}

class _DynamicHomeScreenState extends State<DynamicHomeScreen> {
  @override
  void initState() {
    super.initState();
    AppState._updateAvailableFeatures();
  }

  void _changeRole(String role) {
    setState(() {
      AppState.setUserRole(role);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Dynamic Routes'),
        actions: [
          PopupMenuButton<String>(
            onSelected: _changeRole,
            itemBuilder: (context) => [
              const PopupMenuItem(
                value: UserRole.guest,
                child: Text('Guest'),
              ),
              const PopupMenuItem(
                value: UserRole.user,
                child: Text('User'),
              ),
              const PopupMenuItem(
                value: UserRole.admin,
                child: Text('Admin'),
              ),
            ],
          ),
        ],
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Current Role: ${AppState.currentUserRole}',
                  style: const TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 10),
                Text(
                  'Available Features: ${AppState.availableFeatures.join(', ')}',
                  style: const TextStyle(fontSize: 14, color: Colors.grey),
                ),
              ],
            ),
          ),
          Expanded(
            child: GridView.builder(
              padding: const EdgeInsets.all(16),
              gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 2,
                childAspectRatio: 1.5,
                crossAxisSpacing: 16,
                mainAxisSpacing: 16,
              ),
              itemCount: _getRouteItems().length,
              itemBuilder: (context, index) {
                final item = _getRouteItems()[index];
                return Card(
                  child: InkWell(
                    onTap: () {
                      Navigator.pushNamed(
                        context,
                        item.route,
                        arguments: item.arguments,
                      );
                    },
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Icon(item.icon, size: 40),
                        const SizedBox(height: 8),
                        Text(
                          item.title,
                          style: const TextStyle(fontSize: 16),
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
    );
  }

  List<RouteItem> _getRouteItems() {
    final items = <RouteItem>[
      RouteItem('Login', '/login', Icons.login, {}),
      RouteItem('Dashboard', '/dashboard', Icons.dashboard, {}),
      RouteItem('Users', '/users', Icons.people, {}),
      RouteItem('Analytics', '/analytics', Icons.analytics, {'chartType': 'pie'}),
      RouteItem('Profile', '/profile', Icons.person, {}),
      RouteItem('Settings', '/settings', Icons.settings, {}),
      RouteItem('About', '/about', Icons.info, {}),
    ];

    return items.where((item) {
      final feature = item.route.substring(1); // Remove leading '/'
      return AppState.availableFeatures.contains(feature);
    }).toList();
  }
}

class RouteItem {
  final String title;
  final String route;
  final IconData icon;
  final Map<String, dynamic> arguments;

  RouteItem(this.title, this.route, this.icon, this.arguments);
}

class LoginScreen extends StatelessWidget {
  const LoginScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Login')),
      body: const Center(
        child: Text(
          'Login Screen',
          style: TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}

class DashboardScreen extends StatelessWidget {
  const DashboardScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Dashboard')),
      body: const Center(
        child: Text(
          'Dashboard Screen',
          style: TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}

class UsersScreen extends StatelessWidget {
  const UsersScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Users')),
      body: const Center(
        child: Text(
          'Users Management Screen',
          style: TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}

class AnalyticsScreen extends StatelessWidget {
  final String chartType;

  const AnalyticsScreen({super.key, required this.chartType});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Analytics')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Analytics Screen',
              style: TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 10),
            Text(
              'Chart Type: $chartType',
              style: const TextStyle(fontSize: 16, color: Colors.grey),
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
      appBar: AppBar(title: const Text('Profile')),
      body: const Center(
        child: Text(
          'Profile Screen',
          style: TextStyle(fontSize: 24),
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
      appBar: AppBar(title: const Text('Settings')),
      body: const Center(
        child: Text(
          'Settings Screen',
          style: TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}

class AboutScreen extends StatelessWidget {
  const AboutScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('About')),
      body: const Center(
        child: Text(
          'About Screen',
          style: TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}

class AccessDeniedScreen extends StatelessWidget {
  const AccessDeniedScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Access Denied')),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.block, size: 80, color: Colors.red),
            SizedBox(height: 20),
            Text(
              'Access Denied',
              style: TextStyle(fontSize: 24),
            ),
            SizedBox(height: 10),
            Text(
              'You do not have permission to access this feature.',
              style: TextStyle(fontSize: 16, color: Colors.grey),
              textAlign: TextAlign.center,
            ),
          ],
        ),
      ),
    );
  }
}
```

Dynamic route generation creates routes based on user roles and app  
state. The onGenerateRoute callback checks permissions and available  
features before allowing navigation to specific screens.  

## Animated Route Transitions

Creating custom animated transitions between routes.  

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
      title: 'Animated Route Transitions',
      home: const AnimatedTransitionsHome(),
    );
  }
}

class AnimatedTransitionsHome extends StatelessWidget {
  const AnimatedTransitionsHome({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Animated Transitions')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Choose a Transition Style',
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 30),
            _buildTransitionButton(
              context,
              'Slide from Right',
              () => _navigateWithSlideTransition(context, SlideDirection.right),
            ),
            _buildTransitionButton(
              context,
              'Slide from Bottom',
              () => _navigateWithSlideTransition(context, SlideDirection.bottom),
            ),
            _buildTransitionButton(
              context,
              'Fade Transition',
              () => _navigateWithFadeTransition(context),
            ),
            _buildTransitionButton(
              context,
              'Scale Transition',
              () => _navigateWithScaleTransition(context),
            ),
            _buildTransitionButton(
              context,
              'Rotation Transition',
              () => _navigateWithRotationTransition(context),
            ),
            _buildTransitionButton(
              context,
              'Custom Combined',
              () => _navigateWithCombinedTransition(context),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildTransitionButton(
    BuildContext context,
    String title,
    VoidCallback onPressed,
  ) {
    return Container(
      width: 250,
      margin: const EdgeInsets.symmetric(vertical: 8),
      child: ElevatedButton(
        onPressed: onPressed,
        child: Text(title),
      ),
    );
  }

  void _navigateWithSlideTransition(BuildContext context, SlideDirection direction) {
    Navigator.push(
      context,
      PageRouteBuilder(
        pageBuilder: (context, animation, secondaryAnimation) =>
            TransitionDestinationScreen(title: 'Slide Transition'),
        transitionsBuilder: (context, animation, secondaryAnimation, child) {
          Offset begin;
          switch (direction) {
            case SlideDirection.right:
              begin = const Offset(1.0, 0.0);
              break;
            case SlideDirection.left:
              begin = const Offset(-1.0, 0.0);
              break;
            case SlideDirection.bottom:
              begin = const Offset(0.0, 1.0);
              break;
            case SlideDirection.top:
              begin = const Offset(0.0, -1.0);
              break;
          }

          const end = Offset.zero;
          const curve = Curves.easeInOut;

          var tween = Tween(begin: begin, end: end).chain(
            CurveTween(curve: curve),
          );

          return SlideTransition(
            position: animation.drive(tween),
            child: child,
          );
        },
        transitionDuration: const Duration(milliseconds: 500),
      ),
    );
  }

  void _navigateWithFadeTransition(BuildContext context) {
    Navigator.push(
      context,
      PageRouteBuilder(
        pageBuilder: (context, animation, secondaryAnimation) =>
            const TransitionDestinationScreen(title: 'Fade Transition'),
        transitionsBuilder: (context, animation, secondaryAnimation, child) {
          return FadeTransition(
            opacity: CurvedAnimation(parent: animation, curve: Curves.easeInOut),
            child: child,
          );
        },
        transitionDuration: const Duration(milliseconds: 800),
      ),
    );
  }

  void _navigateWithScaleTransition(BuildContext context) {
    Navigator.push(
      context,
      PageRouteBuilder(
        pageBuilder: (context, animation, secondaryAnimation) =>
            const TransitionDestinationScreen(title: 'Scale Transition'),
        transitionsBuilder: (context, animation, secondaryAnimation, child) {
          return ScaleTransition(
            scale: CurvedAnimation(parent: animation, curve: Curves.elasticOut),
            child: child,
          );
        },
        transitionDuration: const Duration(milliseconds: 1000),
      ),
    );
  }

  void _navigateWithRotationTransition(BuildContext context) {
    Navigator.push(
      context,
      PageRouteBuilder(
        pageBuilder: (context, animation, secondaryAnimation) =>
            const TransitionDestinationScreen(title: 'Rotation Transition'),
        transitionsBuilder: (context, animation, secondaryAnimation, child) {
          return RotationTransition(
            turns: CurvedAnimation(parent: animation, curve: Curves.elasticOut),
            child: child,
          );
        },
        transitionDuration: const Duration(milliseconds: 1200),
      ),
    );
  }

  void _navigateWithCombinedTransition(BuildContext context) {
    Navigator.push(
      context,
      PageRouteBuilder(
        pageBuilder: (context, animation, secondaryAnimation) =>
            const TransitionDestinationScreen(title: 'Combined Transition'),
        transitionsBuilder: (context, animation, secondaryAnimation, child) {
          return SlideTransition(
            position: Tween(
              begin: const Offset(0.0, 1.0),
              end: Offset.zero,
            ).animate(CurvedAnimation(parent: animation, curve: Curves.easeOut)),
            child: ScaleTransition(
              scale: Tween(begin: 0.8, end: 1.0).animate(
                CurvedAnimation(parent: animation, curve: Curves.easeOut),
              ),
              child: FadeTransition(
                opacity: animation,
                child: child,
              ),
            ),
          );
        },
        transitionDuration: const Duration(milliseconds: 600),
      ),
    );
  }
}

enum SlideDirection { left, right, top, bottom }

class TransitionDestinationScreen extends StatelessWidget {
  final String title;

  const TransitionDestinationScreen({super.key, required this.title});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(title)),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Icon(Icons.star, size: 80, color: Colors.amber),
            const SizedBox(height: 20),
            Text(
              title,
              style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),
            const Text(
              'This screen was animated in with a custom transition',
              textAlign: TextAlign.center,
              style: TextStyle(fontSize: 16),
            ),
            const SizedBox(height: 40),
            ElevatedButton(
              onPressed: () => Navigator.pop(context),
              child: const Text('Go Back'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Animated route transitions use PageRouteBuilder with custom  
transitionsBuilder to create smooth, engaging transitions between  
screens with various animation effects and timing curves.  

## Navigation with Streams

Managing navigation state using streams and reactive programming.  

```dart
import 'package:flutter/material.dart';
import 'dart:async';

void main() {
  runApp(const MyApp());
}

class NavigationEvent {
  final String route;
  final Map<String, dynamic>? data;
  final DateTime timestamp;

  NavigationEvent(this.route, {this.data}) : timestamp = DateTime.now();

  @override
  String toString() => 'NavigationEvent(route: $route, data: $data, time: $timestamp)';
}

class StreamNavigationController {
  static final StreamNavigationController _instance = StreamNavigationController._internal();
  factory StreamNavigationController() => _instance;
  StreamNavigationController._internal();

  final StreamController<NavigationEvent> _navigationController = 
      StreamController<NavigationEvent>.broadcast();
  
  final StreamController<String> _routeChangeController = 
      StreamController<String>.broadcast();

  final List<NavigationEvent> _history = [];
  String _currentRoute = '/';

  Stream<NavigationEvent> get navigationStream => _navigationController.stream;
  Stream<String> get routeChangeStream => _routeChangeController.stream;
  List<NavigationEvent> get history => List.unmodifiable(_history);
  String get currentRoute => _currentRoute;

  void navigateTo(String route, {Map<String, dynamic>? data}) {
    final event = NavigationEvent(route, data: data);
    _history.add(event);
    _currentRoute = route;
    _navigationController.add(event);
    _routeChangeController.add(route);
  }

  void clearHistory() {
    _history.clear();
  }

  void dispose() {
    _navigationController.close();
    _routeChangeController.close();
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Navigation with Streams',
      home: const StreamNavigationHome(),
    );
  }
}

class StreamNavigationHome extends StatefulWidget {
  const StreamNavigationHome({super.key});

  @override
  State<StreamNavigationHome> createState() => _StreamNavigationHomeState();
}

class _StreamNavigationHomeState extends State<StreamNavigationHome> {
  final StreamNavigationController _navController = StreamNavigationController();
  late StreamSubscription<NavigationEvent> _navigationSubscription;
  late StreamSubscription<String> _routeSubscription;
  
  String _currentDisplayRoute = '/';
  List<NavigationEvent> _recentEvents = [];

  @override
  void initState() {
    super.initState();
    _listenToNavigationEvents();
  }

  void _listenToNavigationEvents() {
    _navigationSubscription = _navController.navigationStream.listen((event) {
      setState(() {
        _recentEvents.insert(0, event);
        if (_recentEvents.length > 5) {
          _recentEvents.removeLast();
        }
      });
      
      // Simulate navigation based on the event
      _handleNavigationEvent(event);
    });

    _routeSubscription = _navController.routeChangeStream.listen((route) {
      setState(() {
        _currentDisplayRoute = route;
      });
    });
  }

  void _handleNavigationEvent(NavigationEvent event) {
    // In a real app, you might use Navigator here based on the event
    print('Handling navigation: ${event.route}');
    
    // Simulate delayed operations that might affect navigation
    if (event.route == '/async-page') {
      Future.delayed(const Duration(seconds: 2), () {
        if (mounted) {
          _navController.navigateTo('/async-result', 
            data: {'result': 'Async operation completed'});
        }
      });
    }
  }

  Widget _buildRouteContent(String route) {
    switch (route) {
      case '/':
        return _buildHomeContent();
      case '/page1':
        return _buildPageContent('Page 1', Icons.looks_one);
      case '/page2':
        return _buildPageContent('Page 2', Icons.looks_two);
      case '/async-page':
        return _buildPageContent('Loading...', Icons.hourglass_empty);
      case '/async-result':
        return _buildPageContent('Async Result', Icons.check_circle);
      default:
        return _buildPageContent('Unknown Page', Icons.error);
    }
  }

  Widget _buildHomeContent() {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        const Text(
          'Stream Navigation Demo',
          style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
        ),
        const SizedBox(height: 30),
        ElevatedButton(
          onPressed: () => _navController.navigateTo('/page1', 
            data: {'source': 'home_button'}),
          child: const Text('Go to Page 1'),
        ),
        const SizedBox(height: 15),
        ElevatedButton(
          onPressed: () => _navController.navigateTo('/page2',
            data: {'timestamp': DateTime.now().millisecondsSinceEpoch}),
          child: const Text('Go to Page 2'),
        ),
        const SizedBox(height: 15),
        ElevatedButton(
          onPressed: () => _navController.navigateTo('/async-page'),
          child: const Text('Start Async Operation'),
        ),
        const SizedBox(height: 15),
        ElevatedButton(
          onPressed: () => _navController.clearHistory(),
          child: const Text('Clear History'),
        ),
      ],
    );
  }

  Widget _buildPageContent(String title, IconData icon) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Icon(icon, size: 80),
        const SizedBox(height: 20),
        Text(
          title,
          style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
        ),
        const SizedBox(height: 30),
        ElevatedButton(
          onPressed: () => _navController.navigateTo('/'),
          child: const Text('Go Home'),
        ),
      ],
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Current: $_currentDisplayRoute'),
        actions: [
          IconButton(
            icon: const Icon(Icons.history),
            onPressed: _showNavigationHistory,
          ),
        ],
      ),
      body: Column(
        children: [
          // Current route content
          Expanded(
            flex: 2,
            child: _buildRouteContent(_currentDisplayRoute),
          ),
          
          // Recent events stream
          Expanded(
            flex: 1,
            child: Container(
              color: Colors.grey[800],
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  const Text(
                    'Recent Navigation Events:',
                    style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                  ),
                  const SizedBox(height: 10),
                  Expanded(
                    child: ListView.builder(
                      itemCount: _recentEvents.length,
                      itemBuilder: (context, index) {
                        final event = _recentEvents[index];
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 2),
                          child: Text(
                            '${event.route} ${event.data != null ? '(${event.data})' : ''}',
                            style: const TextStyle(fontSize: 12, color: Colors.white70),
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

  void _showNavigationHistory() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Navigation History'),
        content: SizedBox(
          width: double.maxFinite,
          height: 300,
          child: ListView.builder(
            itemCount: _navController.history.length,
            itemBuilder: (context, index) {
              final event = _navController.history[index];
              return ListTile(
                title: Text(event.route),
                subtitle: Text(
                  'Time: ${event.timestamp.toString().substring(11, 19)}',
                ),
                trailing: event.data != null 
                  ? const Icon(Icons.data_object, size: 16)
                  : null,
              );
            },
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

  @override
  void dispose() {
    _navigationSubscription.cancel();
    _routeSubscription.cancel();
    super.dispose();
  }
}
```

Stream-based navigation uses reactive programming to manage navigation  
state, providing real-time updates and event tracking through streams  
for complex navigation scenarios.  

## Navigator 2.0 Router

Implementing Navigator 2.0 with Router for declarative navigation.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class AppRouterState extends ChangeNotifier {
  String _selectedBook = '';
  bool _showBookDetails = false;

  String get selectedBook => _selectedBook;
  bool get showBookDetails => _showBookDetails;

  void selectBook(String book) {
    _selectedBook = book;
    _showBookDetails = true;
    notifyListeners();
  }

  void clearSelection() {
    _selectedBook = '';
    _showBookDetails = false;
    notifyListeners();
  }
}

class BookRouterDelegate extends RouterDelegate<BookRouterConfiguration>
    with ChangeNotifier, PopNavigatorRouterDelegateMixin<BookRouterConfiguration> {
  
  @override
  final GlobalKey<NavigatorState> navigatorKey = GlobalKey<NavigatorState>();

  final AppRouterState _routerState = AppRouterState();

  BookRouterDelegate() {
    _routerState.addListener(notifyListeners);
  }

  @override
  BookRouterConfiguration? get currentConfiguration {
    if (_routerState.showBookDetails) {
      return BookRouterConfiguration.details(_routerState.selectedBook);
    } else {
      return BookRouterConfiguration.list();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Navigator(
      key: navigatorKey,
      pages: [
        const MaterialPage(
          key: ValueKey('BooksListPage'),
          child: BooksListScreen(),
        ),
        if (_routerState.showBookDetails)
          MaterialPage(
            key: ValueKey('BookDetailsPage-${_routerState.selectedBook}'),
            child: BookDetailsScreen(book: _routerState.selectedBook),
          ),
      ],
      onPopPage: (route, result) {
        if (!route.didPop(result)) {
          return false;
        }
        
        _routerState.clearSelection();
        return true;
      },
    );
  }

  @override
  Future<void> setNewRoutePath(BookRouterConfiguration configuration) async {
    if (configuration.isListPage) {
      _routerState.clearSelection();
    } else if (configuration.isDetailsPage) {
      _routerState.selectBook(configuration.bookId!);
    }
  }

  void selectBook(String book) {
    _routerState.selectBook(book);
  }

  @override
  void dispose() {
    _routerState.dispose();
    super.dispose();
  }
}

class BookRouterConfiguration {
  final String? bookId;
  final bool isListPage;
  final bool isDetailsPage;

  BookRouterConfiguration.list()
      : bookId = null,
        isListPage = true,
        isDetailsPage = false;

  BookRouterConfiguration.details(this.bookId)
      : isListPage = false,
        isDetailsPage = true;

  @override
  bool operator ==(Object other) {
    return other is BookRouterConfiguration &&
        other.bookId == bookId &&
        other.isListPage == isListPage &&
        other.isDetailsPage == isDetailsPage;
  }

  @override
  int get hashCode => Object.hash(bookId, isListPage, isDetailsPage);
}

class BookRouteInformationParser extends RouteInformationParser<BookRouterConfiguration> {
  @override
  Future<BookRouterConfiguration> parseRouteInformation(
    RouteInformation routeInformation,
  ) async {
    final location = routeInformation.location;
    
    if (location == null || location == '/') {
      return BookRouterConfiguration.list();
    }

    final uri = Uri.parse(location);
    if (uri.pathSegments.length == 2 && 
        uri.pathSegments.first == 'book') {
      return BookRouterConfiguration.details(uri.pathSegments[1]);
    }

    return BookRouterConfiguration.list();
  }

  @override
  RouteInformation? restoreRouteInformation(BookRouterConfiguration configuration) {
    if (configuration.isListPage) {
      return const RouteInformation(location: '/');
    }
    if (configuration.isDetailsPage) {
      return RouteInformation(location: '/book/${configuration.bookId}');
    }
    return null;
  }
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  final BookRouterDelegate _routerDelegate = BookRouterDelegate();
  final BookRouteInformationParser _routeInformationParser = BookRouteInformationParser();

  @override
  Widget build(BuildContext context) {
    return MaterialApp.router(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Navigator 2.0 Demo',
      routerDelegate: _routerDelegate,
      routeInformationParser: _routeInformationParser,
    );
  }

  @override
  void dispose() {
    _routerDelegate.dispose();
    super.dispose();
  }
}

class BooksListScreen extends StatelessWidget {
  const BooksListScreen({super.key});

  static final List<Book> books = [
    Book('1', 'The Flutter Guide', 'Learn Flutter development from basics to advanced'),
    Book('2', 'Dart Programming', 'Master the Dart programming language'),
    Book('3', 'Mobile UI Design', 'Create beautiful mobile user interfaces'),
    Book('4', 'State Management', 'Manage app state effectively in Flutter'),
    Book('5', 'Flutter Architecture', 'Build scalable Flutter applications'),
  ];

  @override
  Widget build(BuildContext context) {
    final delegate = Router.of(context).routerDelegate as BookRouterDelegate;

    return Scaffold(
      appBar: AppBar(title: const Text('Books Library')),
      body: ListView.builder(
        itemCount: books.length,
        itemBuilder: (context, index) {
          final book = books[index];
          return Card(
            margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
            child: ListTile(
              leading: CircleAvatar(
                child: Text(book.id),
              ),
              title: Text(book.title),
              subtitle: Text(book.description),
              onTap: () => delegate.selectBook(book.id),
            ),
          );
        },
      ),
    );
  }
}

class BookDetailsScreen extends StatelessWidget {
  final String book;

  const BookDetailsScreen({super.key, required this.book});

  @override
  Widget build(BuildContext context) {
    final bookData = BooksListScreen.books.firstWhere((b) => b.id == book);

    return Scaffold(
      appBar: AppBar(title: Text(bookData.title)),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Center(
              child: Container(
                width: 200,
                height: 250,
                decoration: BoxDecoration(
                  color: Colors.grey[700],
                  borderRadius: BorderRadius.circular(8),
                ),
                child: const Icon(Icons.book, size: 80),
              ),
            ),
            const SizedBox(height: 30),
            Text(
              bookData.title,
              style: const TextStyle(fontSize: 28, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            Text(
              'Book ID: ${bookData.id}',
              style: const TextStyle(fontSize: 16, color: Colors.grey),
            ),
            const SizedBox(height: 20),
            const Text(
              'Description:',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            Text(
              bookData.description,
              style: const TextStyle(fontSize: 16),
            ),
            const Spacer(),
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                onPressed: () {
                  ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(content: Text('Added "${bookData.title}" to reading list')),
                  );
                },
                child: const Text('Add to Reading List'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class Book {
  final String id;
  final String title;
  final String description;

  Book(this.id, this.title, this.description);
}
```

Navigator 2.0 with Router provides declarative navigation management  
with proper URL synchronization, browser history support, and  
state-driven navigation architecture for complex apps.  
