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
