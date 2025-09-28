# Flutter Themes - 25 Essential Examples

This comprehensive guide covers Flutter theming with 25 practical examples,  
from basic light and dark themes to advanced theming techniques. Each example  
demonstrates different aspects of Flutter's theming system, helping you create  
consistent, beautiful, and accessible apps.

## Basic Light Theme

Creating a simple light theme with custom colors and styling.  

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
      title: 'Basic Light Theme',
      theme: ThemeData(
        brightness: Brightness.light,
        primarySwatch: Colors.blue,
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.blue,
          foregroundColor: Colors.white,
          elevation: 2,
        ),
        cardTheme: const CardTheme(
          elevation: 4,
          margin: EdgeInsets.all(8),
        ),
        textTheme: const TextTheme(
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.bold,
            color: Colors.black87,
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            color: Colors.black87,
          ),
        ),
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Light Theme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Welcome',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'This is a basic light theme example with custom colors.',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
            const SizedBox(height: 20),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Text(
                  'This card uses the theme card styling.',
                  style: Theme.of(context).textTheme.bodyLarge,
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

This example demonstrates basic light theme creation using ThemeData.  
The primarySwatch defines the color palette, while appBarTheme and  
textTheme provide component-specific styling. Theme.of(context) accesses  
these theme properties throughout the widget tree.  

## Basic Dark Theme

Creating a dark theme with appropriate colors and contrast.  

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
      title: 'Basic Dark Theme',
      theme: ThemeData(
        brightness: Brightness.dark,
        primarySwatch: Colors.deepPurple,
        scaffoldBackgroundColor: const Color(0xFF121212),
        appBarTheme: const AppBarTheme(
          backgroundColor: Color(0xFF1F1F1F),
          foregroundColor: Colors.white,
          elevation: 0,
        ),
        cardTheme: const CardTheme(
          color: Color(0xFF1E1E1E),
          elevation: 2,
          margin: EdgeInsets.all(8),
        ),
        textTheme: const TextTheme(
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.bold,
            color: Colors.white,
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            color: Colors.white70,
          ),
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: Colors.deepPurple,
            foregroundColor: Colors.white,
          ),
        ),
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Dark Theme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Dark Mode',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'This dark theme provides better viewing in low-light conditions.',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
            const SizedBox(height: 20),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Dark Theme Card',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 8),
                    Text(
                      'Cards adapt to dark theme colors automatically.',
                      style: Theme.of(context).textTheme.bodyMedium,
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {},
              child: const Text('Dark Theme Button'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Dark themes require careful color selection for proper contrast and  
readability. This example uses darker background colors and lighter text  
colors. The scaffoldBackgroundColor sets the overall background, while  
component themes define specific element styling.  

## Light and Dark Theme Toggle

Implementing dynamic theme switching between light and dark modes.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  bool _isDarkMode = false;

  void _toggleTheme() {
    setState(() {
      _isDarkMode = !_isDarkMode;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Theme Toggle',
      theme: ThemeData(
        brightness: Brightness.light,
        primarySwatch: Colors.teal,
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.teal,
          foregroundColor: Colors.white,
        ),
      ),
      darkTheme: ThemeData(
        brightness: Brightness.dark,
        primarySwatch: Colors.teal,
        appBarTheme: const AppBarTheme(
          backgroundColor: Color(0xFF1F1F1F),
          foregroundColor: Colors.white,
        ),
        scaffoldBackgroundColor: const Color(0xFF121212),
      ),
      themeMode: _isDarkMode ? ThemeMode.dark : ThemeMode.light,
      home: ThemeTogglePage(
        isDarkMode: _isDarkMode,
        onThemeToggle: _toggleTheme,
      ),
    );
  }
}

class ThemeTogglePage extends StatelessWidget {
  final bool isDarkMode;
  final VoidCallback onThemeToggle;

  const ThemeTogglePage({
    super.key,
    required this.isDarkMode,
    required this.onThemeToggle,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Theme Toggle'),
        actions: [
          IconButton(
            icon: Icon(isDarkMode ? Icons.light_mode : Icons.dark_mode),
            onPressed: onThemeToggle,
            tooltip: isDarkMode ? 'Switch to Light Mode' : 'Switch to Dark Mode',
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Current Theme: ${isDarkMode ? 'Dark' : 'Light'}',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            const SizedBox(height: 20),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Icon(
                          isDarkMode ? Icons.nightlight_round : Icons.wb_sunny,
                          size: 32,
                          color: Theme.of(context).primaryColor,
                        ),
                        const SizedBox(width: 12),
                        Expanded(
                          child: Text(
                            'Theme Switching',
                            style: Theme.of(context).textTheme.titleLarge,
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Text(
                      'Toggle between light and dark themes using the icon button '
                      'in the app bar. The entire app adapts to the selected theme.',
                      style: Theme.of(context).textTheme.bodyMedium,
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 20),
            Row(
              children: [
                ElevatedButton(
                  onPressed: () {},
                  child: const Text('Primary Button'),
                ),
                const SizedBox(width: 12),
                OutlinedButton(
                  onPressed: () {},
                  child: const Text('Outlined Button'),
                ),
              ],
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: onThemeToggle,
        tooltip: 'Toggle Theme',
        child: Icon(isDarkMode ? Icons.light_mode : Icons.dark_mode),
      ),
    );
  }
}
```

Theme toggling uses both theme and darkTheme properties in MaterialApp.  
The themeMode property controls which theme is active. State management  
tracks the current theme mode, allowing users to switch themes dynamically  
while maintaining the selection throughout the app session.  

## Custom Color Scheme

Creating themes with custom color schemes and brand colors.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const customColorScheme = ColorScheme(
      brightness: Brightness.light,
      primary: Color(0xFF6750A4),
      onPrimary: Color(0xFFFFFFFF),
      secondary: Color(0xFF625B71),
      onSecondary: Color(0xFFFFFFFF),
      error: Color(0xFFBA1A1A),
      onError: Color(0xFFFFFFFF),
      background: Color(0xFFFFFBFE),
      onBackground: Color(0xFF1C1B1F),
      surface: Color(0xFFFFFBFE),
      onSurface: Color(0xFF1C1B1F),
    );

    return MaterialApp(
      title: 'Custom Color Scheme',
      theme: ThemeData(
        colorScheme: customColorScheme,
        appBarTheme: AppBarTheme(
          backgroundColor: customColorScheme.primary,
          foregroundColor: customColorScheme.onPrimary,
          elevation: 2,
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: customColorScheme.primary,
            foregroundColor: customColorScheme.onPrimary,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
            ),
          ),
        ),
        cardTheme: CardTheme(
          color: customColorScheme.surface,
          elevation: 3,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(12),
          ),
        ),
        textTheme: TextTheme(
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.bold,
            color: customColorScheme.onBackground,
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            color: customColorScheme.onBackground,
          ),
        ),
      ),
      home: const ColorSchemePage(),
    );
  }
}

class ColorSchemePage extends StatelessWidget {
  const ColorSchemePage({super.key});

  @override
  Widget build(BuildContext context) {
    final colorScheme = Theme.of(context).colorScheme;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Color Scheme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Brand Colors',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 20),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Color Palette',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 12),
                    _buildColorSwatch('Primary', colorScheme.primary),
                    _buildColorSwatch('Secondary', colorScheme.secondary),
                    _buildColorSwatch('Error', colorScheme.error),
                    _buildColorSwatch('Background', colorScheme.background),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 20),
            Row(
              children: [
                ElevatedButton(
                  onPressed: () {},
                  child: const Text('Primary Action'),
                ),
                const SizedBox(width: 12),
                FilledButton.tonal(
                  onPressed: () {},
                  child: const Text('Tonal Button'),
                ),
              ],
            ),
            const SizedBox(height: 12),
            OutlinedButton(
              onPressed: () {},
              child: const Text('Outlined Action'),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorSwatch(String name, Color color) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4.0),
      child: Row(
        children: [
          Container(
            width: 24,
            height: 24,
            decoration: BoxDecoration(
              color: color,
              borderRadius: BorderRadius.circular(4),
              border: Border.all(color: Colors.grey.shade300),
            ),
          ),
          const SizedBox(width: 12),
          Text(name),
          const Spacer(),
          Text(
            '#${color.value.toRadixString(16).substring(2).toUpperCase()}',
            style: const TextStyle(fontFamily: 'monospace'),
          ),
        ],
      ),
    );
  }
}
```

Custom color schemes provide consistent branding across your app.  
The ColorScheme class defines all necessary colors including primary,  
secondary, error, and surface colors. Each color has an "on" counterpart  
for text and icons displayed on that color background.  

## Material 3 Theme

Implementing Material 3 design system with dynamic colors and modern styling.  

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
      title: 'Material 3 Theme',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF6750A4),
          brightness: Brightness.light,
        ),
        appBarTheme: const AppBarTheme(
          centerTitle: false,
          elevation: 0,
          scrolledUnderElevation: 3,
        ),
        navigationBarTheme: const NavigationBarThemeData(
          labelBehavior: NavigationDestinationLabelBehavior.onlyShowSelected,
        ),
        cardTheme: const CardTheme(
          elevation: 1,
          margin: EdgeInsets.all(8),
        ),
        filledButtonTheme: FilledButtonThemeData(
          style: FilledButton.styleFrom(
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(20),
            ),
          ),
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            elevation: 1,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(20),
            ),
          ),
        ),
      ),
      home: const Material3HomePage(),
    );
  }
}

class Material3HomePage extends StatefulWidget {
  const Material3HomePage({super.key});

  @override
  State<Material3HomePage> createState() => _Material3HomePageState();
}

class _Material3HomePageState extends State<Material3HomePage> {
  int _selectedIndex = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Material 3 Design'),
        actions: [
          IconButton(
            icon: const Icon(Icons.more_vert),
            onPressed: () {},
          ),
        ],
      ),
      body: IndexedStack(
        index: _selectedIndex,
        children: [
          _buildHomeContent(),
          _buildExploreContent(),
          _buildProfileContent(),
        ],
      ),
      navigationBar: NavigationBar(
        selectedIndex: _selectedIndex,
        onDestinationSelected: (index) {
          setState(() {
            _selectedIndex = index;
          });
        },
        destinations: const [
          NavigationDestination(
            icon: Icon(Icons.home_outlined),
            selectedIcon: Icon(Icons.home),
            label: 'Home',
          ),
          NavigationDestination(
            icon: Icon(Icons.explore_outlined),
            selectedIcon: Icon(Icons.explore),
            label: 'Explore',
          ),
          NavigationDestination(
            icon: Icon(Icons.person_outline),
            selectedIcon: Icon(Icons.person),
            label: 'Profile',
          ),
        ],
      ),
    );
  }

  Widget _buildHomeContent() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'Material 3 Features',
            style: Theme.of(context).textTheme.headlineMedium,
          ),
          const SizedBox(height: 20),
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Dynamic Colors',
                    style: Theme.of(context).textTheme.titleLarge,
                  ),
                  const SizedBox(height: 8),
                  Text(
                    'Material 3 uses dynamic color generation from a seed color '
                    'to create harmonious color palettes.',
                    style: Theme.of(context).textTheme.bodyMedium,
                  ),
                ],
              ),
            ),
          ),
          const SizedBox(height: 16),
          Row(
            children: [
              FilledButton(
                onPressed: () {},
                child: const Text('Filled Button'),
              ),
              const SizedBox(width: 12),
              FilledButton.tonal(
                onPressed: () {},
                child: const Text('Tonal Button'),
              ),
            ],
          ),
          const SizedBox(height: 12),
          Row(
            children: [
              ElevatedButton(
                onPressed: () {},
                child: const Text('Elevated Button'),
              ),
              const SizedBox(width: 12),
              OutlinedButton(
                onPressed: () {},
                child: const Text('Outlined Button'),
              ),
            ],
          ),
        ],
      ),
    );
  }

  Widget _buildExploreContent() {
    return const Center(
      child: Text('Explore content goes here'),
    );
  }

  Widget _buildProfileContent() {
    return const Center(
      child: Text('Profile content goes here'),
    );
  }
}
```

Material 3 introduces modern design principles with dynamic colors,  
improved accessibility, and updated component designs. The useMaterial3  
flag enables new components like NavigationBar and FilledButton.  
ColorScheme.fromSeed generates a complete color palette from a single  
seed color, ensuring visual harmony throughout the app.  

## Custom Text Theme

Customizing typography with font families, sizes, and weights.  

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
      title: 'Custom Text Theme',
      theme: ThemeData(
        textTheme: const TextTheme(
          displayLarge: TextStyle(
            fontSize: 57,
            fontWeight: FontWeight.w400,
            letterSpacing: -0.25,
            color: Color(0xFF1D1B20),
          ),
          displayMedium: TextStyle(
            fontSize: 45,
            fontWeight: FontWeight.w400,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          displaySmall: TextStyle(
            fontSize: 36,
            fontWeight: FontWeight.w400,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.w700,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          headlineMedium: TextStyle(
            fontSize: 28,
            fontWeight: FontWeight.w600,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          headlineSmall: TextStyle(
            fontSize: 24,
            fontWeight: FontWeight.w600,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          titleLarge: TextStyle(
            fontSize: 22,
            fontWeight: FontWeight.w500,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          titleMedium: TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.w600,
            letterSpacing: 0.15,
            color: Color(0xFF1D1B20),
          ),
          titleSmall: TextStyle(
            fontSize: 14,
            fontWeight: FontWeight.w600,
            letterSpacing: 0.1,
            color: Color(0xFF1D1B20),
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.5,
            color: Color(0xFF1D1B20),
          ),
          bodyMedium: TextStyle(
            fontSize: 14,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.25,
            color: Color(0xFF1D1B20),
          ),
          bodySmall: TextStyle(
            fontSize: 12,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.4,
            color: Color(0xFF49454F),
          ),
        ),
      ),
      home: const TextThemePage(),
    );
  }
}

class TextThemePage extends StatelessWidget {
  const TextThemePage({super.key});

  @override
  Widget build(BuildContext context) {
    final textTheme = Theme.of(context).textTheme;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Text Theme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text('Display Large', style: textTheme.displayLarge),
              const SizedBox(height: 16),
              Text('Display Medium', style: textTheme.displayMedium),
              const SizedBox(height: 16),
              Text('Display Small', style: textTheme.displaySmall),
              const SizedBox(height: 20),
              Text('Headline Large', style: textTheme.headlineLarge),
              const SizedBox(height: 12),
              Text('Headline Medium', style: textTheme.headlineMedium),
              const SizedBox(height: 12),
              Text('Headline Small', style: textTheme.headlineSmall),
              const SizedBox(height: 20),
              Text('Title Large', style: textTheme.titleLarge),
              const SizedBox(height: 8),
              Text('Title Medium', style: textTheme.titleMedium),
              const SizedBox(height: 8),
              Text('Title Small', style: textTheme.titleSmall),
              const SizedBox(height: 20),
              Text(
                'Body Large: This is a paragraph using body large text style. '
                'It demonstrates the font size, weight, and spacing for '
                'primary body text content.',
                style: textTheme.bodyLarge,
              ),
              const SizedBox(height: 12),
              Text(
                'Body Medium: This text uses the body medium style, which is '
                'typically used for secondary text content and supporting '
                'information in your application.',
                style: textTheme.bodyMedium,
              ),
              const SizedBox(height: 12),
              Text(
                'Body Small: Used for captions, footnotes, and other small text.',
                style: textTheme.bodySmall,
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

Custom text themes define consistent typography throughout your app.  
Each text style specifies fontSize, fontWeight, letterSpacing, and color.  
The TextTheme class provides semantic names like displayLarge, headlineSmall,  
and bodyMedium for different text purposes and hierarchy levels.  

## Typography Scale Customization

Advanced typography with custom font scales and responsive text sizing.  

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
      title: 'Typography Scale',
      theme: ThemeData(
        useMaterial3: true,
        textTheme: _buildCustomTypographyScale(),
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
      ),
      home: const TypographyScalePage(),
    );
  }

  TextTheme _buildCustomTypographyScale() {
    const double baseSize = 14.0;
    const double ratio = 1.25; // Major third scale

    return TextTheme(
      displayLarge: TextStyle(
        fontSize: baseSize * ratio * ratio * ratio * ratio,
        fontWeight: FontWeight.w300,
        letterSpacing: -0.25,
        height: 1.12,
      ),
      displayMedium: TextStyle(
        fontSize: baseSize * ratio * ratio * ratio,
        fontWeight: FontWeight.w300,
        letterSpacing: 0,
        height: 1.16,
      ),
      displaySmall: TextStyle(
        fontSize: baseSize * ratio * ratio,
        fontWeight: FontWeight.w400,
        letterSpacing: 0,
        height: 1.22,
      ),
      headlineLarge: TextStyle(
        fontSize: baseSize * ratio * ratio,
        fontWeight: FontWeight.w600,
        letterSpacing: 0,
        height: 1.25,
      ),
      headlineMedium: TextStyle(
        fontSize: baseSize * ratio,
        fontWeight: FontWeight.w600,
        letterSpacing: 0,
        height: 1.29,
      ),
      headlineSmall: TextStyle(
        fontSize: baseSize,
        fontWeight: FontWeight.w600,
        letterSpacing: 0,
        height: 1.33,
      ),
      titleLarge: TextStyle(
        fontSize: baseSize,
        fontWeight: FontWeight.w500,
        letterSpacing: 0,
        height: 1.27,
      ),
      bodyLarge: TextStyle(
        fontSize: baseSize,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.5,
        height: 1.43,
      ),
      bodyMedium: TextStyle(
        fontSize: baseSize * 0.875,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.25,
        height: 1.43,
      ),
    );
  }
}

class TypographyScalePage extends StatelessWidget {
  const TypographyScalePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Typography Scale'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              _buildTypographyCard(
                context,
                'Display Typography',
                [
                  ('Display Large', Theme.of(context).textTheme.displayLarge),
                  ('Display Medium', Theme.of(context).textTheme.displayMedium),
                  ('Display Small', Theme.of(context).textTheme.displaySmall),
                ],
              ),
              const SizedBox(height: 20),
              _buildTypographyCard(
                context,
                'Headline Typography',
                [
                  ('Headline Large', Theme.of(context).textTheme.headlineLarge),
                  ('Headline Medium', Theme.of(context).textTheme.headlineMedium),
                  ('Headline Small', Theme.of(context).textTheme.headlineSmall),
                ],
              ),
              const SizedBox(height: 20),
              _buildTypographyCard(
                context,
                'Body Typography',
                [
                  ('Title Large', Theme.of(context).textTheme.titleLarge),
                  ('Body Large', Theme.of(context).textTheme.bodyLarge),
                  ('Body Medium', Theme.of(context).textTheme.bodyMedium),
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
                        'Responsive Typography',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                      const SizedBox(height: 12),
                      Text(
                        'This typography scale uses a mathematical ratio (1.25) '
                        'to create harmonious size relationships. The base size '
                        'is 14px, and each level multiplies by the ratio.',
                        style: Theme.of(context).textTheme.bodyMedium,
                      ),
                    ],
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildTypographyCard(
    BuildContext context,
    String title,
    List<(String, TextStyle?)> styles,
  ) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              title,
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 12),
            ...styles.map((style) => Padding(
              padding: const EdgeInsets.symmetric(vertical: 4.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    style.$1,
                    style: style.$2,
                  ),
                  if (style.$2 != null)
                    Text(
                      '${style.$2!.fontSize?.toStringAsFixed(1)}px, '
                      '${style.$2!.fontWeight.toString().split('.').last}',
                      style: Theme.of(context).textTheme.bodySmall?.copyWith(
                        color: Theme.of(context).colorScheme.outline,
                      ),
                    ),
                ],
              ),
            )),
          ],
        ),
      ),
    );
  }
}
```

Typography scales create visual hierarchy through consistent size  
relationships. Using mathematical ratios ensures harmonious proportions.  
The height property controls line spacing, while letterSpacing adjusts  
character spacing for optimal readability at different sizes.  

## Font Family Theme

Using custom fonts and Google Fonts in your theme configuration.  

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
      title: 'Font Family Theme',
      theme: ThemeData(
        fontFamily: 'Roboto',
        textTheme: const TextTheme(
          displayLarge: TextStyle(
            fontFamily: 'Playfair Display',
            fontSize: 57,
            fontWeight: FontWeight.w400,
            letterSpacing: -0.25,
          ),
          displayMedium: TextStyle(
            fontFamily: 'Playfair Display',
            fontSize: 45,
            fontWeight: FontWeight.w400,
          ),
          headlineLarge: TextStyle(
            fontFamily: 'Playfair Display',
            fontSize: 32,
            fontWeight: FontWeight.w600,
          ),
          headlineMedium: TextStyle(
            fontFamily: 'Lato',
            fontSize: 28,
            fontWeight: FontWeight.w600,
          ),
          headlineSmall: TextStyle(
            fontFamily: 'Lato',
            fontSize: 24,
            fontWeight: FontWeight.w600,
          ),
          titleLarge: TextStyle(
            fontFamily: 'Lato',
            fontSize: 22,
            fontWeight: FontWeight.w500,
          ),
          bodyLarge: TextStyle(
            fontFamily: 'Roboto',
            fontSize: 16,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.5,
          ),
          bodyMedium: TextStyle(
            fontFamily: 'Roboto',
            fontSize: 14,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.25,
          ),
          labelLarge: TextStyle(
            fontFamily: 'Roboto Mono',
            fontSize: 14,
            fontWeight: FontWeight.w500,
            letterSpacing: 1.25,
          ),
        ),
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
      ),
      home: const FontFamilyPage(),
    );
  }
}

class FontFamilyPage extends StatelessWidget {
  const FontFamilyPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Font Family Theme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Elegant Headlines',
                style: Theme.of(context).textTheme.displayMedium,
              ),
              const SizedBox(height: 8),
              Text(
                'Playfair Display for luxury appeal',
                style: Theme.of(context).textTheme.bodySmall,
              ),
              const SizedBox(height: 24),
              Text(
                'Clean Subheadings',
                style: Theme.of(context).textTheme.headlineMedium,
              ),
              const SizedBox(height: 8),
              Text(
                'Lato for modern readability',
                style: Theme.of(context).textTheme.bodySmall,
              ),
              const SizedBox(height: 24),
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Font Pairing Strategy',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                      const SizedBox(height: 12),
                      Text(
                        'This theme combines three font families strategically:\n\n'
                        '• Playfair Display: Elegant serif for large displays\n'
                        '• Lato: Clean sans-serif for headings and titles\n'
                        '• Roboto: Reliable system font for body text\n'
                        '• Roboto Mono: Monospace for code and labels',
                        style: Theme.of(context).textTheme.bodyLarge,
                      ),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 20),
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Code Example',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                      const SizedBox(height: 12),
                      Container(
                        padding: const EdgeInsets.all(12),
                        decoration: BoxDecoration(
                          color: Colors.grey.shade100,
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: Text(
                          'const fontFamily = "Roboto Mono";\nprint("Monospace for code");',
                          style: Theme.of(context).textTheme.labelLarge?.copyWith(
                            color: Colors.black87,
                          ),
                        ),
                      ),
                    ],
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

Font family themes combine different typefaces for visual hierarchy  
and branding. Serif fonts work well for headlines, sans-serif for body  
text, and monospace for code. Consider contrast, readability, and brand  
personality when selecting font combinations.  

## Text Theme Inheritance

Understanding how text themes cascade and can be overridden locally.  

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
      title: 'Text Theme Inheritance',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.green),
        textTheme: const TextTheme(
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.bold,
            color: Colors.green,
          ),
          headlineMedium: TextStyle(
            fontSize: 28,
            fontWeight: FontWeight.w600,
            color: Colors.green,
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            color: Colors.black87,
            height: 1.5,
          ),
          bodyMedium: TextStyle(
            fontSize: 14,
            color: Colors.black54,
            height: 1.4,
          ),
        ),
      ),
      home: const TextInheritancePage(),
    );
  }
}

class TextInheritancePage extends StatelessWidget {
  const TextInheritancePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Text Theme Inheritance'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Global Theme Style',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 8),
            Text(
              'This uses the global theme color (green)',
              style: Theme.of(context).textTheme.bodyMedium,
            ),
            const SizedBox(height: 24),
            Text(
              'Override with copyWith',
              style: Theme.of(context).textTheme.headlineMedium?.copyWith(
                color: Colors.blue,
                fontStyle: FontStyle.italic,
              ),
            ),
            const SizedBox(height: 8),
            Text(
              'This overrides color and adds italic style',
              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                color: Colors.blue,
              ),
            ),
            const SizedBox(height: 24),
            _buildInheritanceExample(context),
            const SizedBox(height: 24),
            Theme(
              data: Theme.of(context).copyWith(
                textTheme: Theme.of(context).textTheme.copyWith(
                  headlineMedium: Theme.of(context).textTheme.headlineMedium?.copyWith(
                    color: Colors.purple,
                  ),
                  bodyLarge: Theme.of(context).textTheme.bodyLarge?.copyWith(
                    color: Colors.purple.shade300,
                  ),
                ),
              ),
              child: _buildThemedSection(),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildInheritanceExample(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Inheritance Hierarchy',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            _buildInheritanceItem(
              context,
              'MaterialApp Theme',
              'Base text theme defined here',
              Colors.green.shade100,
            ),
            _buildInheritanceItem(
              context,
              '├─ copyWith Override',
              'Modifies specific properties',
              Colors.blue.shade100,
            ),
            _buildInheritanceItem(
              context,
              '└─ Local Theme Widget',
              'Creates scoped theme changes',
              Colors.purple.shade100,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildInheritanceItem(
    BuildContext context,
    String title,
    String description,
    Color backgroundColor,
  ) {
    return Container(
      margin: const EdgeInsets.symmetric(vertical: 4),
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: backgroundColor,
        borderRadius: BorderRadius.circular(8),
        border: Border.all(color: backgroundColor.withOpacity(0.3)),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            title,
            style: Theme.of(context).textTheme.titleSmall,
          ),
          Text(
            description,
            style: Theme.of(context).textTheme.bodySmall,
          ),
        ],
      ),
    );
  }

  Widget _buildThemedSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Local Theme Override',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            const SizedBox(height: 8),
            Text(
              'This section uses a purple theme that only applies to widgets '
              'within this Theme widget. The global app theme remains unchanged.',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
          ],
        ),
      ),
    );
  }
}
```

Text theme inheritance follows the widget tree hierarchy. Global themes  
set base styles, copyWith() creates variations, and Theme widgets create  
local overrides. This system allows consistent styling with targeted  
customization where needed.  

## Responsive Text Themes

Creating text themes that adapt to screen size and device characteristics.  

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
      title: 'Responsive Text Themes',
      theme: _buildResponsiveTheme(context),
      home: const ResponsiveTextPage(),
    );
  }

  ThemeData _buildResponsiveTheme(BuildContext context) {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
      textTheme: _buildResponsiveTextTheme(context),
    );
  }

  TextTheme _buildResponsiveTextTheme(BuildContext context) {
    final size = MediaQuery.of(context).size;
    final isTablet = size.shortestSide >= 600;
    final isDesktop = size.width >= 1024;
    
    double scaleFactor = 1.0;
    if (isDesktop) {
      scaleFactor = 1.2;
    } else if (isTablet) {
      scaleFactor = 1.1;
    }

    return TextTheme(
      displayLarge: TextStyle(
        fontSize: 57 * scaleFactor,
        fontWeight: FontWeight.w400,
        letterSpacing: -0.25,
        height: isDesktop ? 1.0 : 1.12,
      ),
      displayMedium: TextStyle(
        fontSize: 45 * scaleFactor,
        fontWeight: FontWeight.w400,
        height: isDesktop ? 1.1 : 1.16,
      ),
      headlineLarge: TextStyle(
        fontSize: 32 * scaleFactor,
        fontWeight: FontWeight.w600,
        height: isDesktop ? 1.15 : 1.25,
      ),
      headlineMedium: TextStyle(
        fontSize: 28 * scaleFactor,
        fontWeight: FontWeight.w600,
        height: isDesktop ? 1.2 : 1.29,
      ),
      titleLarge: TextStyle(
        fontSize: 22 * scaleFactor,
        fontWeight: FontWeight.w500,
        height: 1.27,
      ),
      bodyLarge: TextStyle(
        fontSize: 16 * scaleFactor,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.5,
        height: isDesktop ? 1.6 : 1.43,
      ),
      bodyMedium: TextStyle(
        fontSize: 14 * scaleFactor,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.25,
        height: isDesktop ? 1.5 : 1.43,
      ),
    );
  }
}

class ResponsiveTextPage extends StatelessWidget {
  const ResponsiveTextPage({super.key});

  @override
  Widget build(BuildContext context) {
    final size = MediaQuery.of(context).size;
    final deviceType = _getDeviceType(size);

    return Scaffold(
      appBar: AppBar(
        title: const Text('Responsive Text Themes'),
      ),
      body: Padding(
        padding: EdgeInsets.all(_getResponsivePadding(size)),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              _buildDeviceInfo(context, deviceType, size),
              const SizedBox(height: 24),
              Text(
                'Responsive Headlines',
                style: Theme.of(context).textTheme.displayMedium,
              ),
              const SizedBox(height: 16),
              Text(
                'This text scales based on device type',
                style: Theme.of(context).textTheme.headlineMedium,
              ),
              const SizedBox(height: 20),
              LayoutBuilder(
                builder: (context, constraints) {
                  final columns = constraints.maxWidth > 800 ? 2 : 1;
                  
                  if (columns == 2) {
                    return Row(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Expanded(child: _buildTextSamples(context)),
                        const SizedBox(width: 20),
                        Expanded(child: _buildScalingInfo(context, deviceType)),
                      ],
                    );
                  } else {
                    return Column(
                      children: [
                        _buildTextSamples(context),
                        const SizedBox(height: 20),
                        _buildScalingInfo(context, deviceType),
                      ],
                    );
                  }
                },
              ),
            ],
          ),
        ),
      ),
    );
  }

  String _getDeviceType(Size size) {
    if (size.width >= 1024) return 'Desktop';
    if (size.shortestSide >= 600) return 'Tablet';
    return 'Mobile';
  }

  double _getResponsivePadding(Size size) {
    if (size.width >= 1024) return 32.0;
    if (size.shortestSide >= 600) return 24.0;
    return 16.0;
  }

  Widget _buildDeviceInfo(BuildContext context, String deviceType, Size size) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Row(
          children: [
            Icon(
              deviceType == 'Desktop' ? Icons.computer :
              deviceType == 'Tablet' ? Icons.tablet :
              Icons.phone_android,
              size: 32,
              color: Theme.of(context).primaryColor,
            ),
            const SizedBox(width: 16),
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Device: $deviceType',
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  Text(
                    'Screen: ${size.width.toInt()} × ${size.height.toInt()}',
                    style: Theme.of(context).textTheme.bodyMedium,
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildTextSamples(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Text Samples',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            Text(
              'Large Headline',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 8),
            Text(
              'Medium Headline',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            const SizedBox(height: 8),
            Text(
              'Body text adapts to screen size and device type for optimal '
              'readability across different form factors.',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildScalingInfo(BuildContext context, String deviceType) {
    final scaleFactor = deviceType == 'Desktop' ? 1.2 :
                       deviceType == 'Tablet' ? 1.1 : 1.0;

    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Scaling Information',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            Text(
              'Scale Factor: ${scaleFactor.toStringAsFixed(1)}×',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
            const SizedBox(height: 8),
            Text(
              'Responsive text themes adjust font sizes and line heights '
              'based on device characteristics to maintain readability '
              'and visual hierarchy across platforms.',
              style: Theme.of(context).textTheme.bodyMedium,
            ),
          ],
        ),
      ),
    );
  }
}
```

Responsive text themes adapt to different screen sizes and device types.  
Scale factors adjust font sizes appropriately for mobile, tablet, and  
desktop displays. Line height and letter spacing also adjust to maintain  
optimal readability across form factors.  

## AppBar Theme Customization

Customizing AppBar appearance with colors, elevation, and text styles.  

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
      title: 'AppBar Theme Customization',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
        appBarTheme: const AppBarTheme(
          backgroundColor: Color(0xFF1E88E5),
          foregroundColor: Colors.white,
          elevation: 4,
          shadowColor: Colors.black26,
          surfaceTintColor: Colors.transparent,
          centerTitle: true,
          titleTextStyle: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.w600,
            color: Colors.white,
            letterSpacing: 0.15,
          ),
          actionsIconTheme: IconThemeData(
            color: Colors.white,
            size: 24,
          ),
          iconTheme: IconThemeData(
            color: Colors.white,
            size: 24,
          ),
          systemOverlayStyle: SystemUiOverlayStyle.light,
        ),
      ),
      home: const AppBarThemePage(),
    );
  }
}

class AppBarThemePage extends StatefulWidget {
  const AppBarThemePage({super.key});

  @override
  State<AppBarThemePage> createState() => _AppBarThemePageState();
}

class _AppBarThemePageState extends State<AppBarThemePage>
    with TickerProviderStateMixin {
  late TabController _tabController;
  bool _hasNotifications = true;

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
      body: NestedScrollView(
        headerSliverBuilder: (context, innerBoxIsScrolled) {
          return [
            SliverAppBar(
              title: const Text('AppBar Themes'),
              pinned: true,
              floating: true,
              expandedHeight: 200,
              leading: IconButton(
                icon: const Icon(Icons.menu),
                onPressed: () {
                  _showCustomBottomSheet(context);
                },
              ),
              actions: [
                IconButton(
                  icon: Stack(
                    children: [
                      const Icon(Icons.notifications),
                      if (_hasNotifications)
                        Positioned(
                          right: 0,
                          top: 0,
                          child: Container(
                            width: 8,
                            height: 8,
                            decoration: const BoxDecoration(
                              color: Colors.red,
                              shape: BoxShape.circle,
                            ),
                          ),
                        ),
                    ],
                  ),
                  onPressed: () {
                    setState(() {
                      _hasNotifications = !_hasNotifications;
                    });
                  },
                ),
                IconButton(
                  icon: const Icon(Icons.search),
                  onPressed: () {
                    _showSearchDialog(context);
                  },
                ),
                PopupMenuButton<String>(
                  icon: const Icon(Icons.more_vert),
                  onSelected: (value) {
                    ScaffoldMessenger.of(context).showSnackBar(
                      SnackBar(content: Text('Selected: $value')),
                    );
                  },
                  itemBuilder: (context) => [
                    const PopupMenuItem(
                      value: 'settings',
                      child: Text('Settings'),
                    ),
                    const PopupMenuItem(
                      value: 'help',
                      child: Text('Help'),
                    ),
                    const PopupMenuItem(
                      value: 'about',
                      child: Text('About'),
                    ),
                  ],
                ),
              ],
              flexibleSpace: const FlexibleSpaceBar(
                title: Text(
                  'Expandable AppBar',
                  style: TextStyle(
                    fontSize: 16,
                    fontWeight: FontWeight.w500,
                  ),
                ),
                background: DecoratedBox(
                  decoration: BoxDecoration(
                    gradient: LinearGradient(
                      begin: Alignment.topCenter,
                      end: Alignment.bottomCenter,
                      colors: [
                        Color(0xFF1976D2),
                        Color(0xFF1E88E5),
                      ],
                    ),
                  ),
                ),
              ),
              bottom: TabBar(
                controller: _tabController,
                indicatorColor: Colors.white,
                labelColor: Colors.white,
                unselectedLabelColor: Colors.white70,
                tabs: const [
                  Tab(text: 'Overview'),
                  Tab(text: 'Examples'),
                  Tab(text: 'Variants'),
                ],
              ),
            ),
          ];
        },
        body: TabBarView(
          controller: _tabController,
          children: [
            _buildOverviewTab(),
            _buildExamplesTab(),
            _buildVariantsTab(),
          ],
        ),
      ),
    );
  }

  Widget _buildOverviewTab() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'AppBar Theme Properties',
            style: Theme.of(context).textTheme.headlineSmall,
          ),
          const SizedBox(height: 16),
          _buildPropertyCard(
            'backgroundColor',
            'Sets the AppBar background color',
            'Color(0xFF1E88E5)',
          ),
          _buildPropertyCard(
            'foregroundColor',
            'Color for text and icons',
            'Colors.white',
          ),
          _buildPropertyCard(
            'elevation',
            'Shadow depth below the AppBar',
            '4.0',
          ),
          _buildPropertyCard(
            'centerTitle',
            'Whether to center the title',
            'true',
          ),
        ],
      ),
    );
  }

  Widget _buildExamplesTab() {
    return const Padding(
      padding: EdgeInsets.all(16.0),
      child: Column(
        children: [
          Card(
            child: ListTile(
              leading: Icon(Icons.palette),
              title: Text('Color Customization'),
              subtitle: Text('Custom background and foreground colors'),
            ),
          ),
          Card(
            child: ListTile(
              leading: Icon(Icons.text_fields),
              title: Text('Typography'),
              subtitle: Text('Custom title text style and font weight'),
            ),
          ),
          Card(
            child: ListTile(
              leading: Icon(Icons.shadow),
              title: Text('Elevation & Shadow'),
              subtitle: Text('Configurable shadow depth and color'),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildVariantsTab() {
    return const Padding(
      padding: EdgeInsets.all(16.0),
      child: Column(
        children: [
          Card(
            child: ListTile(
              leading: Icon(Icons.expand),
              title: Text('SliverAppBar'),
              subtitle: Text('Collapsible and expandable app bars'),
            ),
          ),
          Card(
            child: ListTile(
              leading: Icon(Icons.tab),
              title: Text('TabBar Integration'),
              subtitle: Text('AppBar with integrated tab navigation'),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildPropertyCard(String property, String description, String value) {
    return Card(
      margin: const EdgeInsets.only(bottom: 8),
      child: Padding(
        padding: const EdgeInsets.all(12),
        child: Row(
          children: [
            Expanded(
              flex: 2,
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    property,
                    style: const TextStyle(
                      fontWeight: FontWeight.w600,
                      fontFamily: 'monospace',
                    ),
                  ),
                  Text(
                    description,
                    style: Theme.of(context).textTheme.bodySmall,
                  ),
                ],
              ),
            ),
            Expanded(
              child: Text(
                value,
                style: const TextStyle(
                  fontFamily: 'monospace',
                  color: Colors.blue,
                ),
                textAlign: TextAlign.right,
              ),
            ),
          ],
        ),
      ),
    );
  }

  void _showCustomBottomSheet(BuildContext context) {
    showModalBottomSheet(
      context: context,
      builder: (context) => Container(
        padding: const EdgeInsets.all(16),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            ListTile(
              leading: const Icon(Icons.home),
              title: const Text('Home'),
              onTap: () => Navigator.pop(context),
            ),
            ListTile(
              leading: const Icon(Icons.settings),
              title: const Text('Settings'),
              onTap: () => Navigator.pop(context),
            ),
          ],
        ),
      ),
    );
  }

  void _showSearchDialog(BuildContext context) {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Search'),
        content: const TextField(
          decoration: InputDecoration(
            hintText: 'Enter search query...',
            prefixIcon: Icon(Icons.search),
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Search'),
          ),
        ],
      ),
    );
  }
}
```

AppBar themes control the appearance of app bars throughout your  
application. Key properties include backgroundColor, foregroundColor,  
elevation, and titleTextStyle. SliverAppBar provides additional  
customization for collapsible and expandable app bars with flexible  
space and scroll behaviors.  

## Button Theme Styling

Comprehensive button theming for all Flutter button types.  

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
      title: 'Button Theme Styling',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: Colors.deepPurple,
            foregroundColor: Colors.white,
            elevation: 3,
            shadowColor: Colors.deepPurple.withOpacity(0.5),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(16),
            ),
            padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
            textStyle: const TextStyle(
              fontSize: 16,
              fontWeight: FontWeight.w600,
              letterSpacing: 0.5,
            ),
          ),
        ),
        filledButtonTheme: FilledButtonThemeData(
          style: FilledButton.styleFrom(
            backgroundColor: Colors.deepPurple.shade600,
            foregroundColor: Colors.white,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
            ),
            padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 12),
          ),
        ),
        outlinedButtonTheme: OutlinedButtonThemeData(
          style: OutlinedButton.styleFrom(
            foregroundColor: Colors.deepPurple,
            side: BorderSide(color: Colors.deepPurple, width: 2),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
            ),
            padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 12),
          ),
        ),
        textButtonTheme: TextButtonThemeData(
          style: TextButton.styleFrom(
            foregroundColor: Colors.deepPurple,
            textStyle: const TextStyle(
              fontSize: 16,
              fontWeight: FontWeight.w500,
            ),
            padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
          ),
        ),
        iconButtonTheme: IconButtonThemeData(
          style: IconButton.styleFrom(
            backgroundColor: Colors.deepPurple.withOpacity(0.1),
            foregroundColor: Colors.deepPurple,
            padding: const EdgeInsets.all(12),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
          ),
        ),
        floatingActionButtonTheme: const FloatingActionButtonThemeData(
          backgroundColor: Colors.deepPurple,
          foregroundColor: Colors.white,
          elevation: 6,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(16)),
          ),
        ),
      ),
      home: const ButtonThemePage(),
    );
  }
}

class ButtonThemePage extends StatefulWidget {
  const ButtonThemePage({super.key});

  @override
  State<ButtonThemePage> createState() => _ButtonThemePageState();
}

class _ButtonThemePageState extends State<ButtonThemePage> {
  bool _isLoading = false;
  bool _isEnabled = true;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Button Theme Styling'),
        actions: [
          Switch(
            value: _isEnabled,
            onChanged: (value) {
              setState(() {
                _isEnabled = value;
              });
            },
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Button Styles',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              const SizedBox(height: 20),
              _buildButtonSection('Elevated Buttons', [
                ElevatedButton(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Primary Action'),
                ),
                const SizedBox(height: 8),
                ElevatedButton.icon(
                  onPressed: _isEnabled ? () {} : null,
                  icon: const Icon(Icons.favorite),
                  label: const Text('With Icon'),
                ),
                const SizedBox(height: 8),
                ElevatedButton(
                  onPressed: _isEnabled ? _handleAsyncAction : null,
                  child: _isLoading
                      ? const SizedBox(
                          width: 20,
                          height: 20,
                          child: CircularProgressIndicator(
                            strokeWidth: 2,
                            valueColor: AlwaysStoppedAnimation<Color>(Colors.white),
                          ),
                        )
                      : const Text('Async Action'),
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonSection('Filled Buttons', [
                FilledButton(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Filled Button'),
                ),
                const SizedBox(height: 8),
                FilledButton.icon(
                  onPressed: _isEnabled ? () {} : null,
                  icon: const Icon(Icons.download),
                  label: const Text('Download'),
                ),
                const SizedBox(height: 8),
                FilledButton.tonal(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Tonal Variant'),
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonSection('Outlined Buttons', [
                OutlinedButton(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Outlined'),
                ),
                const SizedBox(height: 8),
                OutlinedButton.icon(
                  onPressed: _isEnabled ? () {} : null,
                  icon: const Icon(Icons.edit),
                  label: const Text('Edit'),
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonSection('Text Buttons', [
                TextButton(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Text Button'),
                ),
                const SizedBox(height: 8),
                TextButton.icon(
                  onPressed: _isEnabled ? () {} : null,
                  icon: const Icon(Icons.share),
                  label: const Text('Share'),
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonSection('Icon Buttons', [
                Row(
                  children: [
                    IconButton(
                      onPressed: _isEnabled ? () {} : null,
                      icon: const Icon(Icons.thumb_up),
                    ),
                    IconButton(
                      onPressed: _isEnabled ? () {} : null,
                      icon: const Icon(Icons.bookmark),
                    ),
                    IconButton(
                      onPressed: _isEnabled ? () {} : null,
                      icon: const Icon(Icons.more_horiz),
                    ),
                    const Spacer(),
                    IconButton.filled(
                      onPressed: _isEnabled ? () {} : null,
                      icon: const Icon(Icons.add),
                    ),
                  ],
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonCustomization(),
            ],
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _isEnabled ? () {} : null,
        tooltip: 'Themed FAB',
        child: const Icon(Icons.add),
      ),
    );
  }

  Widget _buildButtonSection(String title, List<Widget> buttons) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              title,
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 12),
            ...buttons,
          ],
        ),
      ),
    );
  }

  Widget _buildButtonCustomization() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Button Customization',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 12),
            const Text(
              'Button themes define consistent styling across your app:\n\n'
              '• Shape and border radius\n'
              '• Colors and elevation\n'
              '• Padding and text styles\n'
              '• State-dependent behaviors',
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                ElevatedButton(
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.green,
                    foregroundColor: Colors.white,
                    shape: const CircleBorder(),
                    padding: const EdgeInsets.all(16),
                  ),
                  onPressed: _isEnabled ? () {} : null,
                  child: const Icon(Icons.check),
                ),
                const SizedBox(width: 12),
                ElevatedButton(
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.orange,
                    foregroundColor: Colors.white,
                    shape: const StadiumBorder(),
                  ),
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Custom Shape'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Future<void> _handleAsyncAction() async {
    setState(() {
      _isLoading = true;
    });

    await Future.delayed(const Duration(seconds: 2));

    if (mounted) {
      setState(() {
        _isLoading = false;
      });
    }
  }
}
```

Button themes provide consistent styling across all button types in your  
app. Each button type has its own theme data: ElevatedButtonTheme,  
FilledButtonTheme, OutlinedButtonTheme, TextButtonTheme, and  
IconButtonTheme. Themes can be overridden locally using the style  
property for specific customization needs.  

## Card Theme Configuration

Customizing card appearance with elevation, shapes, and colors.  

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
      title: 'Card Theme Configuration',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
        cardTheme: const CardTheme(
          elevation: 2,
          shadowColor: Color(0x1A000000),
          surfaceTintColor: Color(0xFFE0F2F1),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            side: BorderSide(color: Color(0xFFE0E0E0), width: 0.5),
          ),
          margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
          clipBehavior: Clip.antiAlias,
        ),
      ),
      home: const CardThemePage(),
    );
  }
}

class CardThemePage extends StatefulWidget {
  const CardThemePage({super.key});

  @override
  State<CardThemePage> createState() => _CardThemePageState();
}

class _CardThemePageState extends State<CardThemePage> {
  double _currentElevation = 2;
  BorderRadius _currentBorderRadius = BorderRadius.circular(12);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Card Theme Configuration'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Card Variations',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 20),
            _buildBasicCard(),
            _buildContentCard(),
            _buildImageCard(),
            _buildInteractiveCard(),
            const SizedBox(height: 24),
            _buildCustomizationControls(),
            const SizedBox(height: 20),
            _buildCustomCard(),
          ],
        ),
      ),
    );
  }

  Widget _buildBasicCard() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Basic Card',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text(
              'This is a basic card using the global card theme. It demonstrates '
              'the default styling including elevation, border radius, and colors.',
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildContentCard() {
    return Card(
      child: Column(
        children: [
          const Padding(
            padding: EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Content Card',
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                ),
                SizedBox(height: 8),
                Text(
                  'Cards can contain various types of content including text, '
                  'images, buttons, and other widgets.',
                ),
              ],
            ),
          ),
          const Divider(height: 1),
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.end,
              children: [
                TextButton(
                  onPressed: () {},
                  child: const Text('ACTION 1'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {},
                  child: const Text('ACTION 2'),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildImageCard() {
    return Card(
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Container(
            height: 140,
            decoration: const BoxDecoration(
              gradient: LinearGradient(
                colors: [Colors.teal, Colors.tealAccent],
                begin: Alignment.topLeft,
                end: Alignment.bottomRight,
              ),
            ),
            child: const Center(
              child: Icon(
                Icons.landscape,
                size: 48,
                color: Colors.white,
              ),
            ),
          ),
          const Padding(
            padding: EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Image Card',
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                ),
                SizedBox(height: 8),
                Text(
                  'Cards with images benefit from the clipBehavior property '
                  'to ensure content respects the card\'s border radius.',
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildInteractiveCard() {
    return Card(
      child: InkWell(
        onTap: () {
          ScaffoldMessenger.of(context).showSnackBar(
            const SnackBar(content: Text('Card tapped!')),
          );
        },
        borderRadius: BorderRadius.circular(12),
        child: const Padding(
          padding: EdgeInsets.all(16.0),
          child: Row(
            children: [
              Icon(Icons.touch_app, size: 32, color: Colors.teal),
              SizedBox(width: 16),
              Expanded(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Interactive Card',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    SizedBox(height: 4),
                    Text('Tap this card to see the ripple effect.'),
                  ],
                ),
              ),
              Icon(Icons.arrow_forward_ios, size: 16),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildCustomizationControls() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Customization Controls',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'Elevation: ${_currentElevation.toStringAsFixed(1)}',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            Slider(
              value: _currentElevation,
              min: 0,
              max: 10,
              divisions: 20,
              onChanged: (value) {
                setState(() {
                  _currentElevation = value;
                });
              },
            ),
            const SizedBox(height: 16),
            Text(
              'Border Radius',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                _buildRadiusOption('Square', BorderRadius.zero),
                const SizedBox(width: 8),
                _buildRadiusOption('Rounded', BorderRadius.circular(8)),
                const SizedBox(width: 8),
                _buildRadiusOption('Circular', BorderRadius.circular(20)),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildRadiusOption(String label, BorderRadius radius) {
    final isSelected = _currentBorderRadius == radius;
    return Expanded(
      child: OutlinedButton(
        style: OutlinedButton.styleFrom(
          backgroundColor: isSelected ? Colors.teal.withOpacity(0.1) : null,
          foregroundColor: isSelected ? Colors.teal : null,
          side: BorderSide(
            color: isSelected ? Colors.teal : Colors.grey,
            width: isSelected ? 2 : 1,
          ),
        ),
        onPressed: () {
          setState(() {
            _currentBorderRadius = radius;
          });
        },
        child: Text(label),
      ),
    );
  }

  Widget _buildCustomCard() {
    return Card(
      elevation: _currentElevation,
      shape: RoundedRectangleBorder(
        borderRadius: _currentBorderRadius,
        side: const BorderSide(color: Colors.teal, width: 1),
      ),
      child: const Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Custom Card',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text(
              'This card uses the customization settings above. Try adjusting '
              'the elevation and border radius to see the changes.',
            ),
          ],
        ),
      ),
    );
  }
}
```

Card themes define consistent styling for all Card widgets in your app.  
Key properties include elevation for shadow depth, shape for border radius  
and borders, margin for spacing, and clipBehavior for content clipping.  
The surfaceTintColor property provides Material 3 surface tinting effects.  

## Input Decoration Theme

Styling text fields and form inputs with consistent decoration themes.  

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
      title: 'Input Decoration Theme',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        inputDecorationTheme: const InputDecorationTheme(
          filled: true,
          fillColor: Color(0xFFF5F5F5),
          border: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Color(0xFFE0E0E0)),
          ),
          enabledBorder: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Color(0xFFE0E0E0)),
          ),
          focusedBorder: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Colors.blue, width: 2),
          ),
          errorBorder: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Colors.red, width: 2),
          ),
          focusedErrorBorder: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Colors.red, width: 2),
          ),
          labelStyle: TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.w500,
            color: Color(0xFF666666),
          ),
          hintStyle: TextStyle(
            fontSize: 16,
            color: Color(0xFF999999),
          ),
          helperStyle: TextStyle(
            fontSize: 14,
            color: Color(0xFF666666),
          ),
          errorStyle: TextStyle(
            fontSize: 14,
            color: Colors.red,
            fontWeight: FontWeight.w500,
          ),
          contentPadding: EdgeInsets.symmetric(horizontal: 16, vertical: 16),
          prefixIconColor: Color(0xFF666666),
          suffixIconColor: Color(0xFF666666),
        ),
      ),
      home: const InputDecorationPage(),
    );
  }
}

class InputDecorationPage extends StatefulWidget {
  const InputDecorationPage({super.key});

  @override
  State<InputDecorationPage> createState() => _InputDecorationPageState();
}

class _InputDecorationPageState extends State<InputDecorationPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  final _passwordController = TextEditingController();
  final _messageController = TextEditingController();

  bool _obscurePassword = true;
  String? _selectedCountry;
  bool _agreeToTerms = false;

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    _passwordController.dispose();
    _messageController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Input Decoration Theme'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Form Example',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              const SizedBox(height: 20),
              _buildBasicTextField(),
              const SizedBox(height: 16),
              _buildEmailTextField(),
              const SizedBox(height: 16),
              _buildPhoneTextField(),
              const SizedBox(height: 16),
              _buildPasswordTextField(),
              const SizedBox(height: 16),
              _buildDropdownField(),
              const SizedBox(height: 16),
              _buildMultilineTextField(),
              const SizedBox(height: 16),
              _buildCheckboxField(),
              const SizedBox(height: 24),
              _buildInputVariations(),
              const SizedBox(height: 24),
              _buildActionButtons(),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildBasicTextField() {
    return TextFormField(
      controller: _nameController,
      decoration: const InputDecoration(
        labelText: 'Full Name',
        hintText: 'Enter your full name',
        prefixIcon: Icon(Icons.person),
        helperText: 'This field uses the global input decoration theme',
      ),
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter your name';
        }
        return null;
      },
    );
  }

  Widget _buildEmailTextField() {
    return TextFormField(
      controller: _emailController,
      keyboardType: TextInputType.emailAddress,
      decoration: const InputDecoration(
        labelText: 'Email Address',
        hintText: 'Enter your email',
        prefixIcon: Icon(Icons.email),
        suffixIcon: Icon(Icons.check_circle, color: Colors.green),
      ),
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter your email';
        }
        if (!value.contains('@')) {
          return 'Please enter a valid email';
        }
        return null;
      },
    );
  }

  Widget _buildPhoneTextField() {
    return TextFormField(
      controller: _phoneController,
      keyboardType: TextInputType.phone,
      decoration: const InputDecoration(
        labelText: 'Phone Number',
        hintText: '+1 (555) 123-4567',
        prefixIcon: Icon(Icons.phone),
        prefixText: '+1 ',
      ),
    );
  }

  Widget _buildPasswordTextField() {
    return TextFormField(
      controller: _passwordController,
      obscureText: _obscurePassword,
      decoration: InputDecoration(
        labelText: 'Password',
        hintText: 'Enter your password',
        prefixIcon: const Icon(Icons.lock),
        suffixIcon: IconButton(
          icon: Icon(_obscurePassword ? Icons.visibility : Icons.visibility_off),
          onPressed: () {
            setState(() {
              _obscurePassword = !_obscurePassword;
            });
          },
        ),
        helperText: 'Password must be at least 8 characters',
      ),
      validator: (value) {
        if (value == null || value.length < 8) {
          return 'Password must be at least 8 characters';
        }
        return null;
      },
    );
  }

  Widget _buildDropdownField() {
    return DropdownButtonFormField<String>(
      value: _selectedCountry,
      decoration: const InputDecoration(
        labelText: 'Country',
        prefixIcon: Icon(Icons.public),
      ),
      items: const [
        DropdownMenuItem(value: 'US', child: Text('United States')),
        DropdownMenuItem(value: 'CA', child: Text('Canada')),
        DropdownMenuItem(value: 'UK', child: Text('United Kingdom')),
        DropdownMenuItem(value: 'DE', child: Text('Germany')),
        DropdownMenuItem(value: 'FR', child: Text('France')),
      ],
      onChanged: (value) {
        setState(() {
          _selectedCountry = value;
        });
      },
    );
  }

  Widget _buildMultilineTextField() {
    return TextFormField(
      controller: _messageController,
      maxLines: 4,
      decoration: const InputDecoration(
        labelText: 'Message',
        hintText: 'Enter your message here...',
        prefixIcon: Padding(
          padding: EdgeInsets.only(bottom: 60),
          child: Icon(Icons.message),
        ),
        alignLabelWithHint: true,
      ),
    );
  }

  Widget _buildCheckboxField() {
    return CheckboxListTile(
      value: _agreeToTerms,
      onChanged: (value) {
        setState(() {
          _agreeToTerms = value ?? false;
        });
      },
      title: const Text('I agree to the terms and conditions'),
      controlAffinity: ListTileControlAffinity.leading,
    );
  }

  Widget _buildInputVariations() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Input Variations',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            TextField(
              decoration: InputDecoration(
                labelText: 'Underline Style',
                border: const UnderlineInputBorder(),
                enabledBorder: const UnderlineInputBorder(
                  borderSide: BorderSide(color: Colors.grey),
                ),
                focusedBorder: UnderlineInputBorder(
                  borderSide: BorderSide(color: Theme.of(context).primaryColor, width: 2),
                ),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              decoration: InputDecoration(
                labelText: 'No Border Style',
                border: InputBorder.none,
                filled: true,
                fillColor: Colors.grey.shade100,
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              decoration: InputDecoration(
                labelText: 'Custom Colors',
                border: const OutlineInputBorder(),
                focusedBorder: const OutlineInputBorder(
                  borderSide: BorderSide(color: Colors.purple, width: 2),
                ),
                labelStyle: const TextStyle(color: Colors.purple),
                prefixIcon: const Icon(Icons.star, color: Colors.purple),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildActionButtons() {
    return Row(
      children: [
        Expanded(
          child: OutlinedButton(
            onPressed: () {
              _formKey.currentState?.reset();
              _nameController.clear();
              _emailController.clear();
              _phoneController.clear();
              _passwordController.clear();
              _messageController.clear();
              setState(() {
                _selectedCountry = null;
                _agreeToTerms = false;
              });
            },
            child: const Text('Reset'),
          ),
        ),
        const SizedBox(width: 16),
        Expanded(
          child: ElevatedButton(
            onPressed: () {
              if (_formKey.currentState?.validate() ?? false) {
                ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(content: Text('Form submitted successfully!')),
                );
              }
            },
            child: const Text('Submit'),
          ),
        ),
      ],
    );
  }
}
```

Input decoration themes provide consistent styling for all text input  
fields in your app. Key properties include border styles for different  
states, fill colors, text styles for labels and hints, padding, and  
icon colors. This creates a cohesive form experience throughout your  
application.  

## Icon Theme Customization

Configuring icon themes for consistent iconography across your app.  

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
      title: 'Icon Theme Customization',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.amber),
        iconTheme: const IconThemeData(
          color: Color(0xFF424242),
          size: 24,
          opacity: 0.87,
        ),
        primaryIconTheme: const IconThemeData(
          color: Colors.white,
          size: 24,
          opacity: 1.0,
        ),
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.amber,
          iconTheme: IconThemeData(
            color: Colors.white,
            size: 24,
          ),
          actionsIconTheme: IconThemeData(
            color: Colors.white,
            size: 24,
          ),
        ),
      ),
      home: const IconThemePage(),
    );
  }
}

class IconThemePage extends StatefulWidget {
  const IconThemePage({super.key});

  @override
  State<IconThemePage> createState() => _IconThemePageState();
}

class _IconThemePageState extends State<IconThemePage> {
  int _selectedIndex = 0;
  double _iconSize = 24.0;
  double _iconOpacity = 1.0;
  Color _iconColor = const Color(0xFF424242);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Icon Theme Customization'),
        leading: const Icon(Icons.menu),
        actions: const [
          Icon(Icons.search),
          SizedBox(width: 8),
          Icon(Icons.notifications),
          SizedBox(width: 8),
          Icon(Icons.more_vert),
          SizedBox(width: 8),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Icon Theme Examples',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 20),
            _buildGlobalIconTheme(),
            const SizedBox(height: 20),
            _buildContextualIconThemes(),
            const SizedBox(height: 20),
            _buildIconSizeExamples(),
            const SizedBox(height: 20),
            _buildCustomIconTheme(),
            const SizedBox(height: 20),
            _buildIconCustomization(),
          ],
        ),
      ),
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _selectedIndex,
        onTap: (index) {
          setState(() {
            _selectedIndex = index;
          });
        },
        type: BottomNavigationBarType.fixed,
        items: const [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.explore),
            label: 'Explore',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.favorite),
            label: 'Favorites',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.person),
            label: 'Profile',
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: const Icon(Icons.add),
      ),
    );
  }

  Widget _buildGlobalIconTheme() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Global Icon Theme',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            const Text(
              'These icons use the global icon theme settings:',
            ),
            const SizedBox(height: 16),
            Wrap(
              spacing: 16,
              runSpacing: 16,
              children: const [
                Icon(Icons.home),
                Icon(Icons.star),
                Icon(Icons.favorite),
                Icon(Icons.bookmark),
                Icon(Icons.share),
                Icon(Icons.download),
                Icon(Icons.settings),
                Icon(Icons.help),
              ],
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                const Text('Size: '),
                Text(
                  '${Theme.of(context).iconTheme.size}px',
                  style: const TextStyle(fontWeight: FontWeight.bold),
                ),
                const SizedBox(width: 20),
                const Text('Opacity: '),
                Text(
                  '${(Theme.of(context).iconTheme.opacity! * 100).toInt()}%',
                  style: const TextStyle(fontWeight: FontWeight.bold),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildContextualIconThemes() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Contextual Icon Themes',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            const Text(
              'Different contexts can have different icon themes:',
            ),
            const SizedBox(height: 16),
            _buildIconThemeExample(
              'Primary Icons (AppBar)',
              Theme.of(context).primaryIconTheme,
              const [
                Icon(Icons.menu),
                Icon(Icons.search),
                Icon(Icons.notifications),
                Icon(Icons.more_vert),
              ],
            ),
            const SizedBox(height: 16),
            _buildIconThemeExample(
              'Success Icons',
              const IconThemeData(color: Colors.green, size: 28),
              const [
                Icon(Icons.check_circle),
                Icon(Icons.verified),
                Icon(Icons.task_alt),
                Icon(Icons.done),
              ],
            ),
            const SizedBox(height: 16),
            _buildIconThemeExample(
              'Error Icons',
              const IconThemeData(color: Colors.red, size: 28),
              const [
                Icon(Icons.error),
                Icon(Icons.warning),
                Icon(Icons.cancel),
                Icon(Icons.dangerous),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildIconThemeExample(
    String title,
    IconThemeData theme,
    List<Icon> icons,
  ) {
    return Container(
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: Colors.grey.shade50,
        borderRadius: BorderRadius.circular(8),
        border: Border.all(color: Colors.grey.shade200),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            title,
            style: Theme.of(context).textTheme.titleSmall,
          ),
          const SizedBox(height: 8),
          IconTheme(
            data: theme,
            child: Wrap(
              spacing: 16,
              children: icons,
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildIconSizeExamples() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Icon Size Examples',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: [
                _buildSizedIconExample('Small', 16, Icons.star),
                _buildSizedIconExample('Medium', 24, Icons.star),
                _buildSizedIconExample('Large', 32, Icons.star),
                _buildSizedIconExample('X-Large', 48, Icons.star),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSizedIconExample(String label, double size, IconData icon) {
    return Column(
      children: [
        Icon(icon, size: size, color: Colors.amber),
        const SizedBox(height: 8),
        Text(
          label,
          style: Theme.of(context).textTheme.bodySmall,
        ),
        Text(
          '${size.toInt()}px',
          style: Theme.of(context).textTheme.bodySmall?.copyWith(
            color: Colors.grey,
          ),
        ),
      ],
    );
  }

  Widget _buildCustomIconTheme() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Custom Icon Theme',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            IconTheme(
              data: IconThemeData(
                color: _iconColor,
                size: _iconSize,
                opacity: _iconOpacity,
              ),
              child: const Wrap(
                spacing: 16,
                runSpacing: 16,
                children: [
                  Icon(Icons.favorite),
                  Icon(Icons.bookmark),
                  Icon(Icons.thumb_up),
                  Icon(Icons.share),
                  Icon(Icons.comment),
                  Icon(Icons.visibility),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildIconCustomization() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Icon Customization',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'Size: ${_iconSize.toInt()}px',
              style: Theme.of(context).textTheme.titleSmall,
            ),
            Slider(
              value: _iconSize,
              min: 16,
              max: 48,
              divisions: 16,
              onChanged: (value) {
                setState(() {
                  _iconSize = value;
                });
              },
            ),
            const SizedBox(height: 16),
            Text(
              'Opacity: ${(_iconOpacity * 100).toInt()}%',
              style: Theme.of(context).textTheme.titleSmall,
            ),
            Slider(
              value: _iconOpacity,
              min: 0.1,
              max: 1.0,
              divisions: 9,
              onChanged: (value) {
                setState(() {
                  _iconOpacity = value;
                });
              },
            ),
            const SizedBox(height: 16),
            Text(
              'Color',
              style: Theme.of(context).textTheme.titleSmall,
            ),
            const SizedBox(height: 8),
            Wrap(
              spacing: 8,
              children: [
                _buildColorOption(const Color(0xFF424242)),
                _buildColorOption(Colors.blue),
                _buildColorOption(Colors.red),
                _buildColorOption(Colors.green),
                _buildColorOption(Colors.orange),
                _buildColorOption(Colors.purple),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorOption(Color color) {
    final isSelected = _iconColor == color;
    return GestureDetector(
      onTap: () {
        setState(() {
          _iconColor = color;
        });
      },
      child: Container(
        width: 40,
        height: 40,
        decoration: BoxDecoration(
          color: color,
          shape: BoxShape.circle,
          border: Border.all(
            color: isSelected ? Colors.white : Colors.transparent,
            width: 3,
          ),
          boxShadow: isSelected
              ? [
                  BoxShadow(
                    color: color.withOpacity(0.5),
                    blurRadius: 8,
                    spreadRadius: 2,
                  ),
                ]
              : null,
        ),
        child: isSelected
            ? const Icon(Icons.check, color: Colors.white, size: 20)
            : null,
      ),
    );
  }
}
```

Icon themes provide consistent styling for icons throughout your app.  
The global iconTheme affects most icons, while primaryIconTheme applies  
to icons in primary-colored contexts like AppBars. Local IconTheme  
widgets can override global settings for specific sections or components.  

## Theme Extensions

Creating custom theme extensions for app-specific design tokens.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

@immutable
class CustomColors extends ThemeExtension<CustomColors> {
  final Color? brandPrimary;
  final Color? brandSecondary;
  final Color? brandAccent;
  final Color? success;
  final Color? warning;
  final Color? info;
  final Color? neutral;

  const CustomColors({
    this.brandPrimary,
    this.brandSecondary,
    this.brandAccent,
    this.success,
    this.warning,
    this.info,
    this.neutral,
  });

  @override
  CustomColors copyWith({
    Color? brandPrimary,
    Color? brandSecondary,
    Color? brandAccent,
    Color? success,
    Color? warning,
    Color? info,
    Color? neutral,
  }) {
    return CustomColors(
      brandPrimary: brandPrimary ?? this.brandPrimary,
      brandSecondary: brandSecondary ?? this.brandSecondary,
      brandAccent: brandAccent ?? this.brandAccent,
      success: success ?? this.success,
      warning: warning ?? this.warning,
      info: info ?? this.info,
      neutral: neutral ?? this.neutral,
    );
  }

  @override
  CustomColors lerp(ThemeExtension<CustomColors>? other, double t) {
    if (other is! CustomColors) {
      return this;
    }
    return CustomColors(
      brandPrimary: Color.lerp(brandPrimary, other.brandPrimary, t),
      brandSecondary: Color.lerp(brandSecondary, other.brandSecondary, t),
      brandAccent: Color.lerp(brandAccent, other.brandAccent, t),
      success: Color.lerp(success, other.success, t),
      warning: Color.lerp(warning, other.warning, t),
      info: Color.lerp(info, other.info, t),
      neutral: Color.lerp(neutral, other.neutral, t),
    );
  }

  static const light = CustomColors(
    brandPrimary: Color(0xFF6B46C1),
    brandSecondary: Color(0xFF EC4899),
    brandAccent: Color(0xFF F59E0B),
    success: Color(0xFF10B981),
    warning: Color(0xFF F59E0B),
    info: Color(0xFF3B82F6),
    neutral: Color(0xFF6B7280),
  );

  static const dark = CustomColors(
    brandPrimary: Color(0xFF8B5CF6),
    brandSecondary: Color(0xFFF472B6),
    brandAccent: Color(0xFFFBBF24),
    success: Color(0xFF34D399),
    warning: Color(0xFFFBBF24),
    info: Color(0xFF60A5FA),
    neutral: Color(0xFF9CA3AF),
  );
}

@immutable
class CustomSpacing extends ThemeExtension<CustomSpacing> {
  final double? xs;
  final double? sm;
  final double? md;
  final double? lg;
  final double? xl;
  final double? xxl;

  const CustomSpacing({
    this.xs,
    this.sm,
    this.md,
    this.lg,
    this.xl,
    this.xxl,
  });

  @override
  CustomSpacing copyWith({
    double? xs,
    double? sm,
    double? md,
    double? lg,
    double? xl,
    double? xxl,
  }) {
    return CustomSpacing(
      xs: xs ?? this.xs,
      sm: sm ?? this.sm,
      md: md ?? this.md,
      lg: lg ?? this.lg,
      xl: xl ?? this.xl,
      xxl: xxl ?? this.xxl,
    );
  }

  @override
  CustomSpacing lerp(ThemeExtension<CustomSpacing>? other, double t) {
    if (other is! CustomSpacing) {
      return this;
    }
    return CustomSpacing(
      xs: lerpDouble(xs, other.xs, t),
      sm: lerpDouble(sm, other.sm, t),
      md: lerpDouble(md, other.md, t),
      lg: lerpDouble(lg, other.lg, t),
      xl: lerpDouble(xl, other.xl, t),
      xxl: lerpDouble(xxl, other.xxl, t),
    );
  }

  static const spacing = CustomSpacing(
    xs: 4.0,
    sm: 8.0,
    md: 16.0,
    lg: 24.0,
    xl: 32.0,
    xxl: 48.0,
  );
}

double? lerpDouble(double? a, double? b, double t) {
  if (a == null && b == null) return null;
  a ??= 0.0;
  b ??= 0.0;
  return a + (b - a) * t;
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Theme Extensions',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: const Color(0xFF6B46C1)),
        extensions: const [
          CustomColors.light,
          CustomSpacing.spacing,
        ],
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF6B46C1),
          brightness: Brightness.dark,
        ),
        extensions: const [
          CustomColors.dark,
          CustomSpacing.spacing,
        ],
      ),
      home: const ThemeExtensionsPage(),
    );
  }
}

class ThemeExtensionsPage extends StatefulWidget {
  const ThemeExtensionsPage({super.key});

  @override
  State<ThemeExtensionsPage> createState() => _ThemeExtensionsPageState();
}

class _ThemeExtensionsPageState extends State<ThemeExtensionsPage> {
  bool _isDarkMode = false;

  @override
  Widget build(BuildContext context) {
    final customColors = Theme.of(context).extension<CustomColors>()!;
    final customSpacing = Theme.of(context).extension<CustomSpacing>()!;

    return Theme(
      data: _isDarkMode
          ? ThemeData(
              useMaterial3: true,
              colorScheme: ColorScheme.fromSeed(
                seedColor: const Color(0xFF6B46C1),
                brightness: Brightness.dark,
              ),
              extensions: const [
                CustomColors.dark,
                CustomSpacing.spacing,
              ],
            )
          : ThemeData(
              useMaterial3: true,
              colorScheme: ColorScheme.fromSeed(seedColor: const Color(0xFF6B46C1)),
              extensions: const [
                CustomColors.light,
                CustomSpacing.spacing,
              ],
            ),
      child: Scaffold(
        appBar: AppBar(
          title: const Text('Theme Extensions'),
          actions: [
            Switch(
              value: _isDarkMode,
              onChanged: (value) {
                setState(() {
                  _isDarkMode = value;
                });
              },
            ),
          ],
        ),
        body: SingleChildScrollView(
          padding: EdgeInsets.all(customSpacing.md!),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Custom Colors',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              SizedBox(height: customSpacing.lg!),
              _buildColorPalette(customColors, customSpacing),
              SizedBox(height: customSpacing.xl!),
              Text(
                'Custom Spacing',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              SizedBox(height: customSpacing.lg!),
              _buildSpacingExamples(customSpacing),
              SizedBox(height: customSpacing.xl!),
              Text(
                'Component Usage',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              SizedBox(height: customSpacing.lg!),
              _buildComponentExamples(customColors, customSpacing),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildColorPalette(CustomColors colors, CustomSpacing spacing) {
    return Card(
      child: Padding(
        padding: EdgeInsets.all(spacing.md!),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Brand Colors',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            SizedBox(height: spacing.md!),
            Wrap(
              spacing: spacing.sm!,
              runSpacing: spacing.sm!,
              children: [
                _buildColorSwatch('Primary', colors.brandPrimary!),
                _buildColorSwatch('Secondary', colors.brandSecondary!),
                _buildColorSwatch('Accent', colors.brandAccent!),
              ],
            ),
            SizedBox(height: spacing.lg!),
            Text(
              'Semantic Colors',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            SizedBox(height: spacing.md!),
            Wrap(
              spacing: spacing.sm!,
              runSpacing: spacing.sm!,
              children: [
                _buildColorSwatch('Success', colors.success!),
                _buildColorSwatch('Warning', colors.warning!),
                _buildColorSwatch('Info', colors.info!),
                _buildColorSwatch('Neutral', colors.neutral!),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorSwatch(String name, Color color) {
    return Container(
      width: 80,
      height: 60,
      decoration: BoxDecoration(
        color: color,
        borderRadius: BorderRadius.circular(8),
        boxShadow: [
          BoxShadow(
            color: color.withOpacity(0.3),
            blurRadius: 4,
            offset: const Offset(0, 2),
          ),
        ],
      ),
      child: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        children: [
          Container(
            width: double.infinity,
            padding: const EdgeInsets.symmetric(vertical: 4),
            decoration: BoxDecoration(
              color: Colors.black.withOpacity(0.7),
              borderRadius: const BorderRadius.only(
                bottomLeft: Radius.circular(8),
                bottomRight: Radius.circular(8),
              ),
            ),
            child: Text(
              name,
              style: const TextStyle(
                color: Colors.white,
                fontSize: 10,
                fontWeight: FontWeight.bold,
              ),
              textAlign: TextAlign.center,
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildSpacingExamples(CustomSpacing spacing) {
    return Card(
      child: Padding(
        padding: EdgeInsets.all(spacing.md!),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Spacing Scale',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            SizedBox(height: spacing.md!),
            _buildSpacingItem('XS', spacing.xs!),
            _buildSpacingItem('SM', spacing.sm!),
            _buildSpacingItem('MD', spacing.md!),
            _buildSpacingItem('LG', spacing.lg!),
            _buildSpacingItem('XL', spacing.xl!),
            _buildSpacingItem('XXL', spacing.xxl!),
          ],
        ),
      ),
    );
  }

  Widget _buildSpacingItem(String label, double value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        children: [
          SizedBox(
            width: 40,
            child: Text(
              label,
              style: const TextStyle(fontWeight: FontWeight.bold),
            ),
          ),
          Container(
            width: value * 4, // Scale for visibility
            height: 20,
            decoration: BoxDecoration(
              color: Theme.of(context).primaryColor,
              borderRadius: BorderRadius.circular(2),
            ),
          ),
          const SizedBox(width: 12),
          Text('${value.toInt()}px'),
        ],
      ),
    );
  }

  Widget _buildComponentExamples(CustomColors colors, CustomSpacing spacing) {
    return Column(
      children: [
        Card(
          child: Padding(
            padding: EdgeInsets.all(spacing.lg!),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Row(
                  children: [
                    Container(
                      width: 40,
                      height: 40,
                      decoration: BoxDecoration(
                        color: colors.success,
                        shape: BoxShape.circle,
                      ),
                      child: const Icon(Icons.check, color: Colors.white),
                    ),
                    SizedBox(width: spacing.md!),
                    Expanded(
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'Success Message',
                            style: TextStyle(
                              fontWeight: FontWeight.bold,
                              color: colors.success,
                            ),
                          ),
                          SizedBox(height: spacing.xs!),
                          const Text('Operation completed successfully.'),
                        ],
                      ),
                    ),
                  ],
                ),
              ],
            ),
          ),
        ),
        SizedBox(height: spacing.md!),
        Card(
          child: Padding(
            padding: EdgeInsets.all(spacing.lg!),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Row(
                  children: [
                    Container(
                      width: 40,
                      height: 40,
                      decoration: BoxDecoration(
                        color: colors.warning,
                        shape: BoxShape.circle,
                      ),
                      child: const Icon(Icons.warning, color: Colors.white),
                    ),
                    SizedBox(width: spacing.md!),
                    Expanded(
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'Warning Alert',
                            style: TextStyle(
                              fontWeight: FontWeight.bold,
                              color: colors.warning,
                            ),
                          ),
                          SizedBox(height: spacing.xs!),
                          const Text('Please check your input.'),
                        ],
                      ),
                    ),
                  ],
                ),
              ],
            ),
          ),
        ),
      ],
    );
  }
}
```

Theme extensions allow you to define custom design tokens beyond what  
ThemeData provides. Extensions must implement copyWith and lerp methods  
for theme switching and animations. Access extensions using  
Theme.of(context).extension<T>() for consistent app-wide styling.  

## Custom Theme Data

Creating comprehensive custom themes with specialized styling systems.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class AppTheme {
  static const Color _primaryLight = Color(0xFF2196F3);
  static const Color _primaryDark = Color(0xFF1976D2);
  static const Color _secondaryLight = Color(0xFFFF9800);
  static const Color _secondaryDark = Color(0xFFF57C00);

  static ThemeData get lightTheme {
    return ThemeData(
      useMaterial3: true,
      brightness: Brightness.light,
      colorScheme: const ColorScheme.light(
        primary: _primaryLight,
        secondary: _secondaryLight,
        surface: Color(0xFFFAFAFA),
        background: Color(0xFFFFFFFF),
        error: Color(0xFFD32F2F),
      ),
      appBarTheme: const AppBarTheme(
        backgroundColor: _primaryLight,
        foregroundColor: Colors.white,
        elevation: 4,
        centerTitle: true,
        titleTextStyle: TextStyle(
          fontSize: 20,
          fontWeight: FontWeight.w600,
          color: Colors.white,
        ),
      ),
      cardTheme: CardTheme(
        elevation: 2,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(16),
        ),
        margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: _primaryLight,
          foregroundColor: Colors.white,
          elevation: 2,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(12),
          ),
          padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
        ),
      ),
      textTheme: _buildTextTheme(Brightness.light),
      inputDecorationTheme: _buildInputDecorationTheme(Brightness.light),
    );
  }

  static ThemeData get darkTheme {
    return ThemeData(
      useMaterial3: true,
      brightness: Brightness.dark,
      colorScheme: const ColorScheme.dark(
        primary: _primaryDark,
        secondary: _secondaryDark,
        surface: Color(0xFF1E1E1E),
        background: Color(0xFF121212),
        error: Color(0xFFF44336),
      ),
      appBarTheme: const AppBarTheme(
        backgroundColor: Color(0xFF1F1F1F),
        foregroundColor: Colors.white,
        elevation: 4,
        centerTitle: true,
        titleTextStyle: TextStyle(
          fontSize: 20,
          fontWeight: FontWeight.w600,
          color: Colors.white,
        ),
      ),
      cardTheme: CardTheme(
        elevation: 4,
        color: const Color(0xFF2C2C2C),
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(16),
        ),
        margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: _primaryDark,
          foregroundColor: Colors.white,
          elevation: 3,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(12),
          ),
          padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
        ),
      ),
      textTheme: _buildTextTheme(Brightness.dark),
      inputDecorationTheme: _buildInputDecorationTheme(Brightness.dark),
    );
  }

  static TextTheme _buildTextTheme(Brightness brightness) {
    final Color textColor = brightness == Brightness.light
        ? const Color(0xFF212121)
        : Colors.white;
    final Color subtitleColor = brightness == Brightness.light
        ? const Color(0xFF757575)
        : const Color(0xFFBDBDBD);

    return TextTheme(
      displayLarge: TextStyle(
        fontSize: 57,
        fontWeight: FontWeight.w400,
        color: textColor,
        letterSpacing: -0.25,
      ),
      displayMedium: TextStyle(
        fontSize: 45,
        fontWeight: FontWeight.w400,
        color: textColor,
      ),
      headlineLarge: TextStyle(
        fontSize: 32,
        fontWeight: FontWeight.w600,
        color: textColor,
      ),
      headlineMedium: TextStyle(
        fontSize: 28,
        fontWeight: FontWeight.w600,
        color: textColor,
      ),
      titleLarge: TextStyle(
        fontSize: 22,
        fontWeight: FontWeight.w500,
        color: textColor,
      ),
      titleMedium: TextStyle(
        fontSize: 16,
        fontWeight: FontWeight.w600,
        color: textColor,
        letterSpacing: 0.15,
      ),
      bodyLarge: TextStyle(
        fontSize: 16,
        fontWeight: FontWeight.w400,
        color: textColor,
        letterSpacing: 0.5,
      ),
      bodyMedium: TextStyle(
        fontSize: 14,
        fontWeight: FontWeight.w400,
        color: subtitleColor,
        letterSpacing: 0.25,
      ),
    );
  }

  static InputDecorationTheme _buildInputDecorationTheme(Brightness brightness) {
    final Color fillColor = brightness == Brightness.light
        ? const Color(0xFFF5F5F5)
        : const Color(0xFF2C2C2C);
    final Color borderColor = brightness == Brightness.light
        ? const Color(0xFFE0E0E0)
        : const Color(0xFF424242);

    return InputDecorationTheme(
      filled: true,
      fillColor: fillColor,
      border: OutlineInputBorder(
        borderRadius: BorderRadius.circular(12),
        borderSide: BorderSide(color: borderColor),
      ),
      enabledBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(12),
        borderSide: BorderSide(color: borderColor),
      ),
      focusedBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(12),
        borderSide: BorderSide(
          color: brightness == Brightness.light ? _primaryLight : _primaryDark,
          width: 2,
        ),
      ),
      contentPadding: const EdgeInsets.symmetric(horizontal: 16, vertical: 16),
    );
  }

  // Utility methods for consistent styling
  static BoxDecoration getGradientDecoration(Brightness brightness) {
    return BoxDecoration(
      gradient: LinearGradient(
        begin: Alignment.topLeft,
        end: Alignment.bottomRight,
        colors: brightness == Brightness.light
            ? [_primaryLight, _primaryLight.withOpacity(0.8)]
            : [_primaryDark, _primaryDark.withOpacity(0.8)],
      ),
    );
  }

  static BoxShadow getElevationShadow(double elevation) {
    return BoxShadow(
      color: Colors.black.withOpacity(0.1),
      blurRadius: elevation * 2,
      offset: Offset(0, elevation),
    );
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Custom Theme Data',
      theme: AppTheme.lightTheme,
      darkTheme: AppTheme.darkTheme,
      home: const CustomThemeDataPage(),
    );
  }
}

class CustomThemeDataPage extends StatefulWidget {
  const CustomThemeDataPage({super.key});

  @override
  State<CustomThemeDataPage> createState() => _CustomThemeDataPageState();
}

class _CustomThemeDataPageState extends State<CustomThemeDataPage> {
  int _selectedIndex = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: CustomScrollView(
        slivers: [
          SliverAppBar(
            expandedHeight: 200,
            floating: false,
            pinned: true,
            flexibleSpace: FlexibleSpaceBar(
              title: const Text('Custom Theme System'),
              background: Container(
                decoration: AppTheme.getGradientDecoration(
                  Theme.of(context).brightness,
                ),
              ),
            ),
          ),
          SliverPadding(
            padding: const EdgeInsets.all(16),
            sliver: SliverList(
              delegate: SliverChildListDelegate([
                _buildThemeOverview(),
                const SizedBox(height: 16),
                _buildComponentShowcase(),
                const SizedBox(height: 16),
                _buildFormExample(),
                const SizedBox(height: 16),
                _buildCustomStyling(),
              ]),
            ),
          ),
        ],
      ),
      bottomNavigationBar: NavigationBar(
        selectedIndex: _selectedIndex,
        onDestinationSelected: (index) {
          setState(() {
            _selectedIndex = index;
          });
        },
        destinations: const [
          NavigationDestination(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          NavigationDestination(
            icon: Icon(Icons.palette),
            label: 'Theme',
          ),
          NavigationDestination(
            icon: Icon(Icons.settings),
            label: 'Settings',
          ),
        ],
      ),
    );
  }

  Widget _buildThemeOverview() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Theme Overview',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            Text(
              'This app uses a custom theme system built on top of Flutter\'s '
              'ThemeData. It provides consistent styling across all components '
              'with support for both light and dark modes.',
              style: Theme.of(context).textTheme.bodyMedium,
            ),
            const SizedBox(height: 16),
            Wrap(
              spacing: 8,
              children: [
                Chip(
                  label: Text('Material 3'),
                  backgroundColor: Theme.of(context).colorScheme.primaryContainer,
                ),
                Chip(
                  label: Text('Custom Colors'),
                  backgroundColor: Theme.of(context).colorScheme.secondaryContainer,
                ),
                Chip(
                  label: Text('Consistent Typography'),
                  backgroundColor: Theme.of(context).colorScheme.tertiaryContainer,
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildComponentShowcase() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Component Showcase',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: () {},
                    child: const Text('Primary Button'),
                  ),
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: OutlinedButton(
                    onPressed: () {},
                    child: const Text('Secondary'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            Row(
              children: [
                Expanded(
                  child: FilledButton(
                    onPressed: () {},
                    child: const Text('Filled Button'),
                  ),
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: TextButton(
                    onPressed: () {},
                    child: const Text('Text Button'),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildFormExample() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Form Styling',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            const TextField(
              decoration: InputDecoration(
                labelText: 'Name',
                prefixIcon: Icon(Icons.person),
              ),
            ),
            const SizedBox(height: 16),
            const TextField(
              decoration: InputDecoration(
                labelText: 'Email',
                prefixIcon: Icon(Icons.email),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildCustomStyling() {
    return Card(
      child: Column(
        children: [
          Container(
            width: double.infinity,
            height: 100,
            decoration: AppTheme.getGradientDecoration(
              Theme.of(context).brightness,
            ).copyWith(
              borderRadius: const BorderRadius.only(
                topLeft: Radius.circular(12),
                topRight: Radius.circular(12),
              ),
              boxShadow: [AppTheme.getElevationShadow(4)],
            ),
            child: const Center(
              child: Text(
                'Custom Gradient Background',
                style: TextStyle(
                  color: Colors.white,
                  fontSize: 18,
                  fontWeight: FontWeight.w600,
                ),
              ),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Custom Styling Methods',
                  style: Theme.of(context).textTheme.titleMedium,
                ),
                const SizedBox(height: 8),
                Text(
                  'The AppTheme class provides utility methods for consistent '
                  'custom styling like gradients and shadows.',
                  style: Theme.of(context).textTheme.bodyMedium,
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}
```

Custom theme data systems provide complete control over your app's  
appearance. Creating a dedicated AppTheme class centralizes all styling  
decisions and provides utility methods for consistent custom styling.  
This approach scales well for large applications with complex design  
requirements.  

## Theme Animations

Creating smooth transitions between different themes with animations.  

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
      title: 'Theme Animations',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
      ),
      home: const ThemeAnimationsPage(),
    );
  }
}

class ThemeAnimationsPage extends StatefulWidget {
  const ThemeAnimationsPage({super.key});

  @override
  State<ThemeAnimationsPage> createState() => _ThemeAnimationsPageState();
}

class _ThemeAnimationsPageState extends State<ThemeAnimationsPage>
    with TickerProviderStateMixin {
  late AnimationController _colorController;
  late AnimationController _scaleController;
  late AnimationController _rotationController;
  
  late Animation<Color?> _primaryColorAnimation;
  late Animation<Color?> _secondaryColorAnimation;
  late Animation<Color?> _backgroundColorAnimation;
  late Animation<double> _scaleAnimation;
  late Animation<double> _rotationAnimation;

  int _currentThemeIndex = 0;
  final List<ThemeData> _themes = [
    _buildTheme(Colors.blue, Colors.lightBlue, Colors.blue.shade50),
    _buildTheme(Colors.purple, Colors.deepPurple, Colors.purple.shade50),
    _buildTheme(Colors.green, Colors.lightGreen, Colors.green.shade50),
    _buildTheme(Colors.orange, Colors.deepOrange, Colors.orange.shade50),
    _buildTheme(Colors.red, Colors.pink, Colors.red.shade50),
  ];

  @override
  void initState() {
    super.initState();
    
    _colorController = AnimationController(
      duration: const Duration(milliseconds: 800),
      vsync: this,
    );
    
    _scaleController = AnimationController(
      duration: const Duration(milliseconds: 600),
      vsync: this,
    );
    
    _rotationController = AnimationController(
      duration: const Duration(milliseconds: 1200),
      vsync: this,
    );

    _updateAnimations();
    
    _scaleAnimation = Tween<double>(
      begin: 0.8,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _scaleController,
      curve: Curves.elasticOut,
    ));
    
    _rotationAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _rotationController,
      curve: Curves.easeInOutCubic,
    ));
  }

  @override
  void dispose() {
    _colorController.dispose();
    _scaleController.dispose();
    _rotationController.dispose();
    super.dispose();
  }

  void _updateAnimations() {
    final currentTheme = _themes[_currentThemeIndex];
    final nextIndex = (_currentThemeIndex + 1) % _themes.length;
    final nextTheme = _themes[nextIndex];

    _primaryColorAnimation = ColorTween(
      begin: currentTheme.colorScheme.primary,
      end: nextTheme.colorScheme.primary,
    ).animate(CurvedAnimation(
      parent: _colorController,
      curve: Curves.easeInOutCubic,
    ));

    _secondaryColorAnimation = ColorTween(
      begin: currentTheme.colorScheme.secondary,
      end: nextTheme.colorScheme.secondary,
    ).animate(CurvedAnimation(
      parent: _colorController,
      curve: Curves.easeInOutCubic,
    ));

    _backgroundColorAnimation = ColorTween(
      begin: currentTheme.colorScheme.surface,
      end: nextTheme.colorScheme.surface,
    ).animate(CurvedAnimation(
      parent: _colorController,
      curve: Curves.easeInOutCubic,
    ));
  }

  void _changeTheme() {
    _updateAnimations();
    
    _colorController.forward().then((_) {
      setState(() {
        _currentThemeIndex = (_currentThemeIndex + 1) % _themes.length;
      });
      _colorController.reset();
    });
    
    _scaleController.reset();
    _scaleController.forward();
    
    _rotationController.reset();
    _rotationController.forward();
  }

  static ThemeData _buildTheme(Color primary, Color secondary, Color surface) {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: primary,
        secondary: secondary,
        surface: surface,
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: Listenable.merge([
        _colorController,
        _scaleController,
        _rotationController,
      ]),
      builder: (context, child) {
        final currentTheme = _themes[_currentThemeIndex];
        final animatedTheme = currentTheme.copyWith(
          colorScheme: currentTheme.colorScheme.copyWith(
            primary: _primaryColorAnimation.value ?? currentTheme.colorScheme.primary,
            secondary: _secondaryColorAnimation.value ?? currentTheme.colorScheme.secondary,
            surface: _backgroundColorAnimation.value ?? currentTheme.colorScheme.surface,
          ),
        );

        return Theme(
          data: animatedTheme,
          child: Scaffold(
            appBar: AppBar(
              title: const Text('Theme Animations'),
              backgroundColor: _primaryColorAnimation.value,
              foregroundColor: Colors.white,
            ),
            body: SingleChildScrollView(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  _buildAnimatedHeader(),
                  const SizedBox(height: 24),
                  _buildColorTransitions(),
                  const SizedBox(height: 24),
                  _buildAnimatedCards(),
                  const SizedBox(height: 24),
                  _buildControlPanel(),
                ],
              ),
            ),
            floatingActionButton: Transform.scale(
              scale: _scaleAnimation.value,
              child: Transform.rotate(
                angle: _rotationAnimation.value * 2 * 3.14159,
                child: FloatingActionButton(
                  onPressed: _changeTheme,
                  backgroundColor: _primaryColorAnimation.value,
                  child: const Icon(Icons.palette, color: Colors.white),
                ),
              ),
            ),
          ),
        );
      },
    );
  }

  Widget _buildAnimatedHeader() {
    return Container(
      width: double.infinity,
      padding: const EdgeInsets.all(24),
      decoration: BoxDecoration(
        gradient: LinearGradient(
          colors: [
            _primaryColorAnimation.value ?? Theme.of(context).colorScheme.primary,
            _secondaryColorAnimation.value ?? Theme.of(context).colorScheme.secondary,
          ],
          begin: Alignment.topLeft,
          end: Alignment.bottomRight,
        ),
        borderRadius: BorderRadius.circular(16),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Transform.scale(
            scale: _scaleAnimation.value,
            child: const Text(
              'Animated Themes',
              style: TextStyle(
                fontSize: 28,
                fontWeight: FontWeight.bold,
                color: Colors.white,
              ),
            ),
          ),
          const SizedBox(height: 8),
          const Text(
            'Watch colors transition smoothly between themes',
            style: TextStyle(
              fontSize: 16,
              color: Colors.white70,
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildColorTransitions() {
    return Card(
      color: _backgroundColorAnimation.value,
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Color Transitions',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: _buildColorBox(
                    'Primary',
                    _primaryColorAnimation.value ?? Theme.of(context).colorScheme.primary,
                  ),
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: _buildColorBox(
                    'Secondary',
                    _secondaryColorAnimation.value ?? Theme.of(context).colorScheme.secondary,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            _buildColorBox(
              'Background',
              _backgroundColorAnimation.value ?? Theme.of(context).colorScheme.surface,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorBox(String label, Color color) {
    return Container(
      height: 80,
      decoration: BoxDecoration(
        color: color,
        borderRadius: BorderRadius.circular(8),
        boxShadow: [
          BoxShadow(
            color: color.withOpacity(0.3),
            blurRadius: 8,
            offset: const Offset(0, 4),
          ),
        ],
      ),
      child: Center(
        child: Text(
          label,
          style: TextStyle(
            color: color.computeLuminance() > 0.5 ? Colors.black : Colors.white,
            fontWeight: FontWeight.bold,
          ),
        ),
      ),
    );
  }

  Widget _buildAnimatedCards() {
    return Column(
      children: List.generate(3, (index) {
        return AnimatedContainer(
          duration: Duration(milliseconds: 600 + (index * 200)),
          curve: Curves.easeOutBack,
          margin: const EdgeInsets.only(bottom: 12),
          child: Transform.scale(
            scale: _scaleAnimation.value,
            child: Card(
              elevation: 4,
              child: ListTile(
                leading: CircleAvatar(
                  backgroundColor: _primaryColorAnimation.value,
                  child: Text(
                    '${index + 1}',
                    style: const TextStyle(color: Colors.white),
                  ),
                ),
                title: Text('Animated Item ${index + 1}'),
                subtitle: const Text('This card animates with theme changes'),
                trailing: Icon(
                  Icons.arrow_forward_ios,
                  color: _secondaryColorAnimation.value,
                ),
              ),
            ),
          ),
        );
      }),
    );
  }

  Widget _buildControlPanel() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Animation Controls',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton.icon(
                    onPressed: _changeTheme,
                    icon: const Icon(Icons.palette),
                    label: const Text('Change Theme'),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: _primaryColorAnimation.value,
                      foregroundColor: Colors.white,
                    ),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            Text(
              'Current Theme: ${_getThemeName(_currentThemeIndex)}',
              style: Theme.of(context).textTheme.bodyMedium,
            ),
            const SizedBox(height: 8),
            LinearProgressIndicator(
              value: _colorController.value,
              backgroundColor: Colors.grey.shade300,
              valueColor: AlwaysStoppedAnimation<Color>(
                _primaryColorAnimation.value ?? Theme.of(context).colorScheme.primary,
              ),
            ),
          ],
        ),
      ),
    );
  }

  String _getThemeName(int index) {
    const names = ['Blue', 'Purple', 'Green', 'Orange', 'Red'];
    return names[index];
  }
}
```

Theme animations create smooth transitions when switching between  
different themes. Using ColorTween and AnimationController, you can  
animate color changes, scale transformations, and rotations. This  
creates a polished user experience when users toggle theme settings.  

## Inherited Theme Widgets

Building theme-aware widgets that automatically respond to theme changes.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

// Custom inherited theme for app-specific styling
class AppThemeData {
  final Color brandColor;
  final Color accentColor;
  final double borderRadius;
  final double elevation;
  final EdgeInsets spacing;

  const AppThemeData({
    required this.brandColor,
    required this.accentColor,
    required this.borderRadius,
    required this.elevation,
    required this.spacing,
  });

  AppThemeData copyWith({
    Color? brandColor,
    Color? accentColor,
    double? borderRadius,
    double? elevation,
    EdgeInsets? spacing,
  }) {
    return AppThemeData(
      brandColor: brandColor ?? this.brandColor,
      accentColor: accentColor ?? this.accentColor,
      borderRadius: borderRadius ?? this.borderRadius,
      elevation: elevation ?? this.elevation,
      spacing: spacing ?? this.spacing,
    );
  }

  static const AppThemeData light = AppThemeData(
    brandColor: Color(0xFF6B73FF),
    accentColor: Color(0xFF9B59B6),
    borderRadius: 12.0,
    elevation: 2.0,
    spacing: EdgeInsets.all(16.0),
  );

  static const AppThemeData dark = AppThemeData(
    brandColor: Color(0xFF8B9CFF),
    accentColor: Color(0xFFBB8FCE),
    borderRadius: 16.0,
    elevation: 4.0,
    spacing: EdgeInsets.all(20.0),
  );
}

class AppTheme extends InheritedWidget {
  final AppThemeData themeData;

  const AppTheme({
    super.key,
    required this.themeData,
    required super.child,
  });

  static AppThemeData of(BuildContext context) {
    final appTheme = context.dependOnInheritedWidgetOfExactType<AppTheme>();
    return appTheme?.themeData ?? AppThemeData.light;
  }

  static AppThemeData? maybeOf(BuildContext context) {
    final appTheme = context.dependOnInheritedWidgetOfExactType<AppTheme>();
    return appTheme?.themeData;
  }

  @override
  bool updateShouldNotify(AppTheme oldWidget) {
    return themeData.brandColor != oldWidget.themeData.brandColor ||
           themeData.accentColor != oldWidget.themeData.accentColor ||
           themeData.borderRadius != oldWidget.themeData.borderRadius ||
           themeData.elevation != oldWidget.themeData.elevation ||
           themeData.spacing != oldWidget.themeData.spacing;
  }
}

// Custom theme-aware widgets
class BrandCard extends StatelessWidget {
  final String title;
  final String subtitle;
  final Widget? leading;
  final VoidCallback? onTap;

  const BrandCard({
    super.key,
    required this.title,
    required this.subtitle,
    this.leading,
    this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    final appTheme = AppTheme.of(context);
    
    return Container(
      margin: appTheme.spacing,
      decoration: BoxDecoration(
        color: Theme.of(context).cardColor,
        borderRadius: BorderRadius.circular(appTheme.borderRadius),
        border: Border.all(
          color: appTheme.brandColor.withOpacity(0.2),
          width: 1,
        ),
        boxShadow: [
          BoxShadow(
            color: appTheme.brandColor.withOpacity(0.1),
            blurRadius: appTheme.elevation * 2,
            offset: Offset(0, appTheme.elevation),
          ),
        ],
      ),
      child: Material(
        color: Colors.transparent,
        borderRadius: BorderRadius.circular(appTheme.borderRadius),
        child: InkWell(
          onTap: onTap,
          borderRadius: BorderRadius.circular(appTheme.borderRadius),
          child: Padding(
            padding: appTheme.spacing,
            child: Row(
              children: [
                if (leading != null) ...[
                  leading!,
                  SizedBox(width: appTheme.spacing.left),
                ],
                Expanded(
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        title,
                        style: Theme.of(context).textTheme.titleMedium?.copyWith(
                          color: appTheme.brandColor,
                          fontWeight: FontWeight.w600,
                        ),
                      ),
                      const SizedBox(height: 4),
                      Text(
                        subtitle,
                        style: Theme.of(context).textTheme.bodyMedium,
                      ),
                    ],
                  ),
                ),
                Icon(
                  Icons.arrow_forward_ios,
                  size: 16,
                  color: appTheme.accentColor,
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

class BrandButton extends StatelessWidget {
  final String text;
  final VoidCallback? onPressed;
  final bool isPrimary;

  const BrandButton({
    super.key,
    required this.text,
    this.onPressed,
    this.isPrimary = true,
  });

  @override
  Widget build(BuildContext context) {
    final appTheme = AppTheme.of(context);
    
    return Container(
      decoration: BoxDecoration(
        borderRadius: BorderRadius.circular(appTheme.borderRadius),
        boxShadow: isPrimary
            ? [
                BoxShadow(
                  color: appTheme.brandColor.withOpacity(0.3),
                  blurRadius: appTheme.elevation * 2,
                  offset: Offset(0, appTheme.elevation),
                ),
              ]
            : null,
      ),
      child: ElevatedButton(
        onPressed: onPressed,
        style: ElevatedButton.styleFrom(
          backgroundColor: isPrimary ? appTheme.brandColor : Colors.transparent,
          foregroundColor: isPrimary ? Colors.white : appTheme.brandColor,
          side: isPrimary
              ? null
              : BorderSide(color: appTheme.brandColor, width: 2),
          elevation: isPrimary ? appTheme.elevation : 0,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(appTheme.borderRadius),
          ),
          padding: appTheme.spacing,
        ),
        child: Text(
          text,
          style: const TextStyle(fontWeight: FontWeight.w600),
        ),
      ),
    );
  }
}

class BrandChip extends StatelessWidget {
  final String label;
  final bool isSelected;
  final VoidCallback? onTap;

  const BrandChip({
    super.key,
    required this.label,
    this.isSelected = false,
    this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    final appTheme = AppTheme.of(context);
    
    return GestureDetector(
      onTap: onTap,
      child: Container(
        padding: EdgeInsets.symmetric(
          horizontal: appTheme.spacing.left,
          vertical: appTheme.spacing.top * 0.5,
        ),
        decoration: BoxDecoration(
          color: isSelected ? appTheme.brandColor : Colors.transparent,
          border: Border.all(
            color: appTheme.brandColor,
            width: 1.5,
          ),
          borderRadius: BorderRadius.circular(appTheme.borderRadius * 2),
        ),
        child: Text(
          label,
          style: TextStyle(
            color: isSelected ? Colors.white : appTheme.brandColor,
            fontWeight: FontWeight.w500,
          ),
        ),
      ),
    );
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Inherited Theme Widgets',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
          brightness: Brightness.dark,
        ),
      ),
      home: const InheritedThemeWidgetsPage(),
    );
  }
}

class InheritedThemeWidgetsPage extends StatefulWidget {
  const InheritedThemeWidgetsPage({super.key});

  @override
  State<InheritedThemeWidgetsPage> createState() => _InheritedThemeWidgetsPageState();
}

class _InheritedThemeWidgetsPageState extends State<InheritedThemeWidgetsPage> {
  bool _useCustomTheme = true;
  int _selectedChip = 0;

  @override
  Widget build(BuildContext context) {
    final isDarkMode = Theme.of(context).brightness == Brightness.dark;
    final customTheme = _useCustomTheme
        ? (isDarkMode ? AppThemeData.dark : AppThemeData.light)
        : AppThemeData.light;

    return AppTheme(
      themeData: customTheme,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('Inherited Theme Widgets'),
          actions: [
            Switch(
              value: _useCustomTheme,
              onChanged: (value) {
                setState(() {
                  _useCustomTheme = value;
                });
              },
            ),
          ],
        ),
        body: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              _buildThemeInfo(),
              _buildBrandCards(),
              _buildButtons(),
              _buildChips(),
              _buildNestedTheme(),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildThemeInfo() {
    final appTheme = AppTheme.of(context);
    
    return Container(
      margin: appTheme.spacing,
      padding: appTheme.spacing,
      decoration: BoxDecoration(
        gradient: LinearGradient(
          colors: [
            appTheme.brandColor.withOpacity(0.1),
            appTheme.accentColor.withOpacity(0.1),
          ],
        ),
        borderRadius: BorderRadius.circular(appTheme.borderRadius),
        border: Border.all(
          color: appTheme.brandColor.withOpacity(0.3),
        ),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'Inherited Theme Status',
            style: Theme.of(context).textTheme.titleLarge?.copyWith(
              color: appTheme.brandColor,
            ),
          ),
          const SizedBox(height: 8),
          Text(
            'Custom Theme: ${_useCustomTheme ? 'Enabled' : 'Disabled'}',
            style: Theme.of(context).textTheme.bodyMedium,
          ),
          Text(
            'Border Radius: ${appTheme.borderRadius}px',
            style: Theme.of(context).textTheme.bodyMedium,
          ),
          Text(
            'Elevation: ${appTheme.elevation}px',
            style: Theme.of(context).textTheme.bodyMedium,
          ),
        ],
      ),
    );
  }

  Widget _buildBrandCards() {
    return Column(
      children: [
        BrandCard(
          title: 'Theme-Aware Card',
          subtitle: 'This card adapts to the inherited theme automatically',
          leading: const CircleAvatar(
            child: Icon(Icons.palette),
          ),
          onTap: () {
            ScaffoldMessenger.of(context).showSnackBar(
              const SnackBar(content: Text('Brand card tapped!')),
            );
          },
        ),
        BrandCard(
          title: 'Custom Styling',
          subtitle: 'Colors, borders, and spacing from AppTheme',
          leading: const CircleAvatar(
            child: Icon(Icons.brush),
          ),
        ),
        BrandCard(
          title: 'Inherited Behavior',
          subtitle: 'Changes automatically when theme updates',
          leading: const CircleAvatar(
            child: Icon(Icons.auto_awesome),
          ),
        ),
      ],
    );
  }

  Widget _buildButtons() {
    final appTheme = AppTheme.of(context);
    
    return Container(
      margin: appTheme.spacing,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'Brand Buttons',
            style: Theme.of(context).textTheme.titleLarge,
          ),
          const SizedBox(height: 12),
          Row(
            children: [
              Expanded(
                child: BrandButton(
                  text: 'Primary Action',
                  onPressed: () {},
                ),
              ),
              const SizedBox(width: 12),
              Expanded(
                child: BrandButton(
                  text: 'Secondary',
                  isPrimary: false,
                  onPressed: () {},
                ),
              ),
            ],
          ),
        ],
      ),
    );
  }

  Widget _buildChips() {
    final appTheme = AppTheme.of(context);
    final chipLabels = ['Option 1', 'Option 2', 'Option 3', 'Option 4'];
    
    return Container(
      margin: appTheme.spacing,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'Brand Chips',
            style: Theme.of(context).textTheme.titleLarge,
          ),
          const SizedBox(height: 12),
          Wrap(
            spacing: 8,
            runSpacing: 8,
            children: chipLabels.asMap().entries.map((entry) {
              final index = entry.key;
              final label = entry.value;
              return BrandChip(
                label: label,
                isSelected: _selectedChip == index,
                onTap: () {
                  setState(() {
                    _selectedChip = index;
                  });
                },
              );
            }).toList(),
          ),
        ],
      ),
    );
  }

  Widget _buildNestedTheme() {
    final appTheme = AppTheme.of(context);
    
    return AppTheme(
      themeData: appTheme.copyWith(
        brandColor: Colors.green,
        accentColor: Colors.lightGreen,
        borderRadius: 8.0,
      ),
      child: Container(
        margin: appTheme.spacing,
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Nested Theme Override',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            BrandCard(
              title: 'Overridden Theme',
              subtitle: 'This card uses a different theme configuration',
              leading: const CircleAvatar(
                child: Icon(Icons.layers),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

Inherited theme widgets automatically respond to theme changes and  
provide custom styling beyond standard ThemeData. By creating custom  
InheritedWidget classes, you can distribute app-specific design tokens  
throughout the widget tree, ensuring consistent styling and automatic  
updates when themes change.  

## Theme-Aware Custom Widgets

Building reusable widgets that intelligently adapt to theme changes.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

// Base class for theme-aware widgets
abstract class ThemeAwareWidget extends StatelessWidget {
  const ThemeAwareWidget({super.key});

  // Helper methods for theme-aware styling
  Color getAdaptiveTextColor(BuildContext context) {
    return Theme.of(context).brightness == Brightness.dark
        ? Colors.white
        : Colors.black87;
  }

  Color getAdaptiveBackgroundColor(BuildContext context) {
    return Theme.of(context).brightness == Brightness.dark
        ? const Color(0xFF1E1E1E)
        : Colors.white;
  }

  Color getAdaptiveBorderColor(BuildContext context) {
    return Theme.of(context).brightness == Brightness.dark
        ? Colors.white24
        : Colors.black12;
  }

  double getAdaptiveElevation(BuildContext context) {
    return Theme.of(context).brightness == Brightness.dark ? 8.0 : 4.0;
  }
}

// Smart status card that adapts to theme
class StatusCard extends ThemeAwareWidget {
  final String title;
  final String value;
  final IconData icon;
  final StatusType status;

  const StatusCard({
    super.key,
    required this.title,
    required this.value,
    required this.icon,
    required this.status,
  });

  @override
  Widget build(BuildContext context) {
    final statusColor = _getStatusColor(context);
    final backgroundColor = _getStatusBackgroundColor(context);

    return Card(
      elevation: getAdaptiveElevation(context),
      child: Container(
        padding: const EdgeInsets.all(20),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(12),
          gradient: LinearGradient(
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
            colors: [
              backgroundColor,
              backgroundColor.withOpacity(0.7),
            ],
          ),
          border: Border.all(
            color: statusColor.withOpacity(0.3),
            width: 1,
          ),
        ),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Container(
                  padding: const EdgeInsets.all(8),
                  decoration: BoxDecoration(
                    color: statusColor.withOpacity(0.1),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: Icon(
                    icon,
                    color: statusColor,
                    size: 24,
                  ),
                ),
                Container(
                  padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
                  decoration: BoxDecoration(
                    color: statusColor.withOpacity(0.2),
                    borderRadius: BorderRadius.circular(12),
                  ),
                  child: Text(
                    _getStatusText(),
                    style: TextStyle(
                      color: statusColor,
                      fontSize: 12,
                      fontWeight: FontWeight.w600,
                    ),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 16),
            Text(
              value,
              style: Theme.of(context).textTheme.headlineMedium?.copyWith(
                fontWeight: FontWeight.bold,
                color: getAdaptiveTextColor(context),
              ),
            ),
            const SizedBox(height: 4),
            Text(
              title,
              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                color: getAdaptiveTextColor(context).withOpacity(0.7),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Color _getStatusColor(BuildContext context) {
    switch (status) {
      case StatusType.success:
        return Theme.of(context).brightness == Brightness.dark
            ? Colors.green.shade400
            : Colors.green.shade600;
      case StatusType.warning:
        return Theme.of(context).brightness == Brightness.dark
            ? Colors.orange.shade400
            : Colors.orange.shade600;
      case StatusType.error:
        return Theme.of(context).brightness == Brightness.dark
            ? Colors.red.shade400
            : Colors.red.shade600;
      case StatusType.info:
        return Theme.of(context).brightness == Brightness.dark
            ? Colors.blue.shade400
            : Colors.blue.shade600;
    }
  }

  Color _getStatusBackgroundColor(BuildContext context) {
    final statusColor = _getStatusColor(context);
    return Theme.of(context).brightness == Brightness.dark
        ? statusColor.withOpacity(0.1)
        : statusColor.withOpacity(0.05);
  }

  String _getStatusText() {
    switch (status) {
      case StatusType.success:
        return 'SUCCESS';
      case StatusType.warning:
        return 'WARNING';
      case StatusType.error:
        return 'ERROR';
      case StatusType.info:
        return 'INFO';
    }
  }
}

enum StatusType { success, warning, error, info }

// Smart notification widget
class SmartNotification extends ThemeAwareWidget {
  final String title;
  final String message;
  final NotificationType type;
  final VoidCallback? onDismiss;
  final VoidCallback? onAction;
  final String? actionLabel;

  const SmartNotification({
    super.key,
    required this.title,
    required this.message,
    required this.type,
    this.onDismiss,
    this.onAction,
    this.actionLabel,
  });

  @override
  Widget build(BuildContext context) {
    final typeColor = _getTypeColor(context);
    final isDarkMode = Theme.of(context).brightness == Brightness.dark;

    return Container(
      margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
      decoration: BoxDecoration(
        color: getAdaptiveBackgroundColor(context),
        borderRadius: BorderRadius.circular(12),
        border: Border.all(
          color: typeColor.withOpacity(0.3),
          width: 1,
        ),
        boxShadow: [
          BoxShadow(
            color: isDarkMode
                ? Colors.black.withOpacity(0.3)
                : Colors.grey.withOpacity(0.2),
            blurRadius: 8,
            offset: const Offset(0, 4),
          ),
        ],
      ),
      child: Stack(
        children: [
          // Color indicator
          Positioned(
            left: 0,
            top: 0,
            bottom: 0,
            child: Container(
              width: 4,
              decoration: BoxDecoration(
                color: typeColor,
                borderRadius: const BorderRadius.only(
                  topLeft: Radius.circular(12),
                  bottomLeft: Radius.circular(12),
                ),
              ),
            ),
          ),
          Padding(
            padding: const EdgeInsets.only(left: 16, right: 12, top: 16, bottom: 16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              mainAxisSize: MainAxisSize.min,
              children: [
                Row(
                  children: [
                    Icon(
                      _getTypeIcon(),
                      color: typeColor,
                      size: 20,
                    ),
                    const SizedBox(width: 8),
                    Expanded(
                      child: Text(
                        title,
                        style: Theme.of(context).textTheme.titleMedium?.copyWith(
                          fontWeight: FontWeight.w600,
                          color: getAdaptiveTextColor(context),
                        ),
                      ),
                    ),
                    if (onDismiss != null)
                      GestureDetector(
                        onTap: onDismiss,
                        child: Icon(
                          Icons.close,
                          color: getAdaptiveTextColor(context).withOpacity(0.6),
                          size: 18,
                        ),
                      ),
                  ],
                ),
                const SizedBox(height: 8),
                Text(
                  message,
                  style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                    color: getAdaptiveTextColor(context).withOpacity(0.8),
                  ),
                ),
                if (onAction != null && actionLabel != null) ...[
                  const SizedBox(height: 12),
                  Align(
                    alignment: Alignment.centerRight,
                    child: TextButton(
                      onPressed: onAction,
                      style: TextButton.styleFrom(
                        foregroundColor: typeColor,
                        padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 6),
                      ),
                      child: Text(
                        actionLabel!,
                        style: const TextStyle(fontWeight: FontWeight.w600),
                      ),
                    ),
                  ),
                ],
              ],
            ),
          ),
        ],
      ),
    );
  }

  Color _getTypeColor(BuildContext context) {
    final isDarkMode = Theme.of(context).brightness == Brightness.dark;
    switch (type) {
      case NotificationType.success:
        return isDarkMode ? Colors.green.shade400 : Colors.green.shade600;
      case NotificationType.warning:
        return isDarkMode ? Colors.orange.shade400 : Colors.orange.shade600;
      case NotificationType.error:
        return isDarkMode ? Colors.red.shade400 : Colors.red.shade600;
      case NotificationType.info:
        return isDarkMode ? Colors.blue.shade400 : Colors.blue.shade600;
    }
  }

  IconData _getTypeIcon() {
    switch (type) {
      case NotificationType.success:
        return Icons.check_circle_outline;
      case NotificationType.warning:
        return Icons.warning_amber_outlined;
      case NotificationType.error:
        return Icons.error_outline;
      case NotificationType.info:
        return Icons.info_outline;
    }
  }
}

enum NotificationType { success, warning, error, info }

// Adaptive progress indicator
class AdaptiveProgressIndicator extends ThemeAwareWidget {
  final double value;
  final String label;
  final bool showPercentage;

  const AdaptiveProgressIndicator({
    super.key,
    required this.value,
    required this.label,
    this.showPercentage = true,
  });

  @override
  Widget build(BuildContext context) {
    final primaryColor = Theme.of(context).colorScheme.primary;
    final backgroundColor = getAdaptiveBorderColor(context);

    return Container(
      padding: const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: getAdaptiveBackgroundColor(context),
        borderRadius: BorderRadius.circular(12),
        border: Border.all(color: getAdaptiveBorderColor(context)),
        boxShadow: [
          BoxShadow(
            color: primaryColor.withOpacity(0.1),
            blurRadius: 4,
            offset: const Offset(0, 2),
          ),
        ],
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text(
                label,
                style: Theme.of(context).textTheme.titleMedium?.copyWith(
                  color: getAdaptiveTextColor(context),
                  fontWeight: FontWeight.w500,
                ),
              ),
              if (showPercentage)
                Text(
                  '${(value * 100).toInt()}%',
                  style: Theme.of(context).textTheme.titleSmall?.copyWith(
                    color: primaryColor,
                    fontWeight: FontWeight.w600,
                  ),
                ),
            ],
          ),
          const SizedBox(height: 12),
          ClipRRect(
            borderRadius: BorderRadius.circular(8),
            child: LinearProgressIndicator(
              value: value,
              backgroundColor: backgroundColor,
              valueColor: AlwaysStoppedAnimation<Color>(primaryColor),
              minHeight: 8,
            ),
          ),
        ],
      ),
    );
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Theme-Aware Custom Widgets',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
          brightness: Brightness.dark,
        ),
      ),
      home: const ThemeAwareWidgetsPage(),
    );
  }
}

class ThemeAwareWidgetsPage extends StatefulWidget {
  const ThemeAwareWidgetsPage({super.key});

  @override
  State<ThemeAwareWidgetsPage> createState() => _ThemeAwareWidgetsPageState();
}

class _ThemeAwareWidgetsPageState extends State<ThemeAwareWidgetsPage> {
  final List<SmartNotification> _notifications = [];
  double _progressValue = 0.7;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Theme-Aware Custom Widgets'),
        actions: [
          IconButton(
            icon: const Icon(Icons.brightness_6),
            onPressed: () {
              // Theme switching would be handled by parent widget
              ScaffoldMessenger.of(context).showSnackBar(
                const SnackBar(
                  content: Text('Theme switching demo - these widgets adapt automatically!'),
                ),
              );
            },
          ),
        ],
      ),
      body: SingleChildScrollView(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildStatusCards(),
            const SizedBox(height: 24),
            _buildProgressIndicators(),
            const SizedBox(height: 24),
            _buildNotifications(),
            const SizedBox(height: 24),
            _buildControlPanel(),
          ],
        ),
      ),
    );
  }

  Widget _buildStatusCards() {
    return Padding(
      padding: const EdgeInsets.all(16),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'Status Cards',
            style: Theme.of(context).textTheme.titleLarge,
          ),
          const SizedBox(height: 16),
          const GridView.count(
            shrinkWrap: true,
            physics: NeverScrollableScrollPhysics(),
            crossAxisCount: 2,
            crossAxisSpacing: 12,
            mainAxisSpacing: 12,
            childAspectRatio: 1.2,
            children: [
              StatusCard(
                title: 'Sales',
                value: '1,234',
                icon: Icons.trending_up,
                status: StatusType.success,
              ),
              StatusCard(
                title: 'Pending',
                value: '56',
                icon: Icons.pending,
                status: StatusType.warning,
              ),
              StatusCard(
                title: 'Errors',
                value: '3',
                icon: Icons.error,
                status: StatusType.error,
              ),
              StatusCard(
                title: 'Info',
                value: '89',
                icon: Icons.info,
                status: StatusType.info,
              ),
            ],
          ),
        ],
      ),
    );
  }

  Widget _buildProgressIndicators() {
    return Padding(
      padding: const EdgeInsets.all(16),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'Progress Indicators',
            style: Theme.of(context).textTheme.titleLarge,
          ),
          const SizedBox(height: 16),
          AdaptiveProgressIndicator(
            value: _progressValue,
            label: 'Project Completion',
          ),
          const SizedBox(height: 12),
          const AdaptiveProgressIndicator(
            value: 0.45,
            label: 'Storage Used',
          ),
          const SizedBox(height: 12),
          const AdaptiveProgressIndicator(
            value: 0.9,
            label: 'Loading Progress',
          ),
        ],
      ),
    );
  }

  Widget _buildNotifications() {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Padding(
          padding: const EdgeInsets.all(16),
          child: Text(
            'Smart Notifications',
            style: Theme.of(context).textTheme.titleLarge,
          ),
        ),
        const SmartNotification(
          title: 'Success',
          message: 'Your data has been saved successfully.',
          type: NotificationType.success,
        ),
        SmartNotification(
          title: 'Warning',
          message: 'Your storage is almost full. Consider upgrading.',
          type: NotificationType.warning,
          actionLabel: 'UPGRADE',
          onAction: () {},
        ),
        const SmartNotification(
          title: 'Error',
          message: 'Failed to sync data. Please check your connection.',
          type: NotificationType.error,
        ),
        ..._notifications,
      ],
    );
  }

  Widget _buildControlPanel() {
    return Padding(
      padding: const EdgeInsets.all(16),
      child: Card(
        child: Padding(
          padding: const EdgeInsets.all(16),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Controls',
                style: Theme.of(context).textTheme.titleLarge,
              ),
              const SizedBox(height: 16),
              Text('Progress Value: ${(_progressValue * 100).toInt()}%'),
              Slider(
                value: _progressValue,
                onChanged: (value) {
                  setState(() {
                    _progressValue = value;
                  });
                },
              ),
              const SizedBox(height: 16),
              ElevatedButton(
                onPressed: () {
                  setState(() {
                    _notifications.add(
                      SmartNotification(
                        title: 'New Notification',
                        message: 'This notification was added dynamically.',
                        type: NotificationType.info,
                        onDismiss: () {
                          setState(() {
                            _notifications.removeLast();
                          });
                        },
                      ),
                    );
                  });
                },
                child: const Text('Add Notification'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

Theme-aware custom widgets automatically adapt their appearance based  
on the current theme. By implementing helper methods and using theme  
properties consistently, these widgets provide seamless integration  
with both light and dark modes while maintaining proper contrast and  
accessibility standards.  

## Platform-Adaptive Themes

Creating themes that adapt to different platforms (iOS, Android, Web, Desktop).  

```dart
import 'package:flutter/material.dart';
import 'package:flutter/foundation.dart';
import 'dart:io' show Platform;

void main() {
  runApp(const MyApp());
}

class PlatformThemes {
  static ThemeData get androidTheme {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(seedColor: Colors.green),
      appBarTheme: const AppBarTheme(
        backgroundColor: Colors.green,
        foregroundColor: Colors.white,
        elevation: 4,
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: Colors.green,
          foregroundColor: Colors.white,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(8),
          ),
        ),
      ),
      cardTheme: const CardTheme(
        elevation: 2,
        margin: EdgeInsets.all(8),
      ),
      floatingActionButtonTheme: const FloatingActionButtonThemeData(
        backgroundColor: Colors.green,
        foregroundColor: Colors.white,
      ),
    );
  }

  static ThemeData get iosTheme {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
      appBarTheme: const AppBarTheme(
        backgroundColor: Colors.transparent,
        foregroundColor: Colors.blue,
        elevation: 0,
        centerTitle: true,
        titleTextStyle: TextStyle(
          fontSize: 17,
          fontWeight: FontWeight.w600,
          color: Colors.black,
        ),
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: Colors.blue,
          foregroundColor: Colors.white,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(8),
          ),
          padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
        ),
      ),
      cardTheme: const CardTheme(
        elevation: 1,
        margin: EdgeInsets.symmetric(horizontal: 16, vertical: 4),
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.all(Radius.circular(12)),
        ),
      ),
      inputDecorationTheme: const InputDecorationTheme(
        border: OutlineInputBorder(
          borderRadius: BorderRadius.all(Radius.circular(8)),
        ),
      ),
    );
  }

  static ThemeData get webTheme {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(seedColor: Colors.purple),
      appBarTheme: const AppBarTheme(
        backgroundColor: Colors.purple,
        foregroundColor: Colors.white,
        elevation: 2,
        toolbarHeight: 64,
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: Colors.purple,
          foregroundColor: Colors.white,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(4),
          ),
          padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 16),
          textStyle: const TextStyle(fontSize: 16),
        ),
      ),
      cardTheme: const CardTheme(
        elevation: 4,
        margin: EdgeInsets.all(12),
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.all(Radius.circular(8)),
        ),
      ),
      textTheme: const TextTheme(
        headlineLarge: TextStyle(fontSize: 36, fontWeight: FontWeight.w300),
        headlineMedium: TextStyle(fontSize: 24, fontWeight: FontWeight.w400),
      ),
    );
  }

  static ThemeData get desktopTheme {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
      appBarTheme: const AppBarTheme(
        backgroundColor: Colors.indigo,
        foregroundColor: Colors.white,
        elevation: 1,
        toolbarHeight: 72,
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: Colors.indigo,
          foregroundColor: Colors.white,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(6),
          ),
          padding: const EdgeInsets.symmetric(horizontal: 32, vertical: 18),
          textStyle: const TextStyle(fontSize: 16),
        ),
      ),
      cardTheme: const CardTheme(
        elevation: 3,
        margin: EdgeInsets.all(16),
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.all(Radius.circular(12)),
        ),
      ),
      textTheme: const TextTheme(
        headlineLarge: TextStyle(fontSize: 48, fontWeight: FontWeight.w300),
        headlineMedium: TextStyle(fontSize: 32, fontWeight: FontWeight.w400),
        bodyLarge: TextStyle(fontSize: 18),
        bodyMedium: TextStyle(fontSize: 16),
      ),
    );
  }

  static ThemeData getCurrentPlatformTheme() {
    if (kIsWeb) {
      return webTheme;
    } else if (Platform.isIOS) {
      return iosTheme;
    } else if (Platform.isWindows || Platform.isMacOS || Platform.isLinux) {
      return desktopTheme;
    } else {
      return androidTheme; // Default for Android and other platforms
    }
  }

  static String getCurrentPlatformName() {
    if (kIsWeb) {
      return 'Web';
    } else if (Platform.isIOS) {
      return 'iOS';
    } else if (Platform.isAndroid) {
      return 'Android';
    } else if (Platform.isWindows) {
      return 'Windows';
    } else if (Platform.isMacOS) {
      return 'macOS';
    } else if (Platform.isLinux) {
      return 'Linux';
    } else {
      return 'Unknown';
    }
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Platform-Adaptive Themes',
      theme: PlatformThemes.getCurrentPlatformTheme(),
      home: const PlatformAdaptiveThemePage(),
    );
  }
}

class PlatformAdaptiveThemePage extends StatefulWidget {
  const PlatformAdaptiveThemePage({super.key});

  @override
  State<PlatformAdaptiveThemePage> createState() => _PlatformAdaptiveThemePageState();
}

class _PlatformAdaptiveThemePageState extends State<PlatformAdaptiveThemePage> {
  String _selectedPlatform = PlatformThemes.getCurrentPlatformName();
  late ThemeData _currentTheme;

  @override
  void initState() {
    super.initState();
    _currentTheme = PlatformThemes.getCurrentPlatformTheme();
  }

  void _changePlatformTheme(String platform) {
    setState(() {
      _selectedPlatform = platform;
      switch (platform) {
        case 'Android':
          _currentTheme = PlatformThemes.androidTheme;
          break;
        case 'iOS':
          _currentTheme = PlatformThemes.iosTheme;
          break;
        case 'Web':
          _currentTheme = PlatformThemes.webTheme;
          break;
        case 'Desktop':
          _currentTheme = PlatformThemes.desktopTheme;
          break;
        default:
          _currentTheme = PlatformThemes.androidTheme;
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Theme(
      data: _currentTheme,
      child: Scaffold(
        appBar: AppBar(
          title: Text('Platform: $_selectedPlatform'),
          actions: [
            PopupMenuButton<String>(
              icon: const Icon(Icons.phone_android),
              onSelected: _changePlatformTheme,
              itemBuilder: (context) => [
                const PopupMenuItem(
                  value: 'Android',
                  child: Row(
                    children: [
                      Icon(Icons.android, color: Colors.green),
                      SizedBox(width: 8),
                      Text('Android'),
                    ],
                  ),
                ),
                const PopupMenuItem(
                  value: 'iOS',
                  child: Row(
                    children: [
                      Icon(Icons.phone_iphone, color: Colors.blue),
                      SizedBox(width: 8),
                      Text('iOS'),
                    ],
                  ),
                ),
                const PopupMenuItem(
                  value: 'Web',
                  child: Row(
                    children: [
                      Icon(Icons.web, color: Colors.purple),
                      SizedBox(width: 8),
                      Text('Web'),
                    ],
                  ),
                ),
                const PopupMenuItem(
                  value: 'Desktop',
                  child: Row(
                    children: [
                      Icon(Icons.desktop_windows, color: Colors.indigo),
                      SizedBox(width: 8),
                      Text('Desktop'),
                    ],
                  ),
                ),
              ],
            ),
          ],
        ),
        body: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              _buildPlatformInfo(),
              _buildComponentShowcase(),
              _buildFormExample(),
              _buildThemeComparison(),
            ],
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            ScaffoldMessenger.of(context).showSnackBar(
              SnackBar(content: Text('$_selectedPlatform FAB pressed!')),
            );
          },
          child: const Icon(Icons.add),
        ),
      ),
    );
  }

  Widget _buildPlatformInfo() {
    final currentPlatform = PlatformThemes.getCurrentPlatformName();
    
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Platform Information',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            Row(
              children: [
                const Icon(Icons.info_outline),
                const SizedBox(width: 8),
                Text('Detected Platform: $currentPlatform'),
              ],
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                const Icon(Icons.palette),
                const SizedBox(width: 8),
                Text('Current Theme: $_selectedPlatform'),
              ],
            ),
            const SizedBox(height: 12),
            Text(
              'This app automatically adapts its theme based on the platform. '
              'Each platform has distinct styling characteristics that follow '
              'platform conventions.',
              style: Theme.of(context).textTheme.bodyMedium,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildComponentShowcase() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Platform-Specific Components',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: () {},
                    child: const Text('Primary Button'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: OutlinedButton(
                    onPressed: () {},
                    child: const Text('Secondary'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            Row(
              children: [
                Expanded(
                  child: TextButton(
                    onPressed: () {},
                    child: const Text('Text Button'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: FilledButton(
                    onPressed: () {},
                    child: const Text('Filled Button'),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildFormExample() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Form Styling',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            const TextField(
              decoration: InputDecoration(
                labelText: 'Username',
                prefixIcon: Icon(Icons.person),
              ),
            ),
            const SizedBox(height: 12),
            const TextField(
              decoration: InputDecoration(
                labelText: 'Email',
                prefixIcon: Icon(Icons.email),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildThemeComparison() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Platform Theme Characteristics',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            _buildPlatformCharacteristic(
              'Android',
              'Material Design with vibrant colors and prominent shadows',
              Icons.android,
              Colors.green,
            ),
            _buildPlatformCharacteristic(
              'iOS',
              'Clean, minimalist design with subtle elevations',
              Icons.phone_iphone,
              Colors.blue,
            ),
            _buildPlatformCharacteristic(
              'Web',
              'Optimized for larger screens with web conventions',
              Icons.web,
              Colors.purple,
            ),
            _buildPlatformCharacteristic(
              'Desktop',
              'Spacious layouts with larger touch targets',
              Icons.desktop_windows,
              Colors.indigo,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildPlatformCharacteristic(
    String platform,
    String description,
    IconData icon,
    Color color,
  ) {
    return Container(
      margin: const EdgeInsets.only(bottom: 12),
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        border: Border.all(
          color: _selectedPlatform == platform ? color : Colors.grey.shade300,
          width: _selectedPlatform == platform ? 2 : 1,
        ),
        borderRadius: BorderRadius.circular(8),
        color: _selectedPlatform == platform
            ? color.withOpacity(0.1)
            : Colors.transparent,
      ),
      child: Row(
        children: [
          Icon(icon, color: color, size: 24),
          const SizedBox(width: 12),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  platform,
                  style: TextStyle(
                    fontWeight: FontWeight.w600,
                    color: color,
                  ),
                ),
                const SizedBox(height: 2),
                Text(
                  description,
                  style: Theme.of(context).textTheme.bodySmall,
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}
```

Platform-adaptive themes automatically adjust styling based on the  
target platform. Android themes emphasize Material Design principles,  
iOS themes follow Apple's design guidelines, web themes optimize for  
browser interfaces, and desktop themes provide spacious layouts for  
mouse and keyboard interaction.  

## High Contrast Themes

Implementing accessibility-focused high contrast themes for better visibility.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class HighContrastThemes {
  // High contrast light theme
  static ThemeData get highContrastLight {
    return ThemeData(
      brightness: Brightness.light,
      colorScheme: const ColorScheme.light(
        primary: Color(0xFF000000),
        onPrimary: Color(0xFFFFFFFF),
        secondary: Color(0xFF000000),
        onSecondary: Color(0xFFFFFFFF),
        background: Color(0xFFFFFFFF),
        onBackground: Color(0xFF000000),
        surface: Color(0xFFFFFFFF),
        onSurface: Color(0xFF000000),
        error: Color(0xFF000000),
        onError: Color(0xFFFFFFFF),
        outline: Color(0xFF000000),
      ),
      appBarTheme: const AppBarTheme(
        backgroundColor: Color(0xFF000000),
        foregroundColor: Color(0xFFFFFFFF),
        elevation: 0,
        iconTheme: IconThemeData(color: Color(0xFFFFFFFF)),
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: const Color(0xFF000000),
          foregroundColor: const Color(0xFFFFFFFF),
          side: const BorderSide(color: Color(0xFF000000), width: 2),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(4),
          ),
        ),
      ),
      outlinedButtonTheme: OutlinedButtonThemeData(
        style: OutlinedButton.styleFrom(
          foregroundColor: const Color(0xFF000000),
          side: const BorderSide(color: Color(0xFF000000), width: 2),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(4),
          ),
        ),
      ),
      textButtonTheme: TextButtonThemeData(
        style: TextButton.styleFrom(
          foregroundColor: const Color(0xFF000000),
        ),
      ),
      cardTheme: const CardTheme(
        color: Color(0xFFFFFFFF),
        elevation: 0,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.all(Radius.circular(4)),
          side: BorderSide(color: Color(0xFF000000), width: 2),
        ),
      ),
      inputDecorationTheme: const InputDecorationTheme(
        filled: true,
        fillColor: Color(0xFFFFFFFF),
        border: OutlineInputBorder(
          borderSide: BorderSide(color: Color(0xFF000000), width: 2),
        ),
        enabledBorder: OutlineInputBorder(
          borderSide: BorderSide(color: Color(0xFF000000), width: 2),
        ),
        focusedBorder: OutlineInputBorder(
          borderSide: BorderSide(color: Color(0xFF000000), width: 3),
        ),
      ),
      textTheme: const TextTheme(
        displayLarge: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.bold),
        displayMedium: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.bold),
        displaySmall: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.bold),
        headlineLarge: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.bold),
        headlineMedium: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.bold),
        headlineSmall: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.bold),
        titleLarge: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.bold),
        titleMedium: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.bold),
        titleSmall: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.bold),
        bodyLarge: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.w500),
        bodyMedium: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.w500),
        bodySmall: TextStyle(color: Color(0xFF000000), fontWeight: FontWeight.w500),
      ),
    );
  }

  // High contrast dark theme
  static ThemeData get highContrastDark {
    return ThemeData(
      brightness: Brightness.dark,
      colorScheme: const ColorScheme.dark(
        primary: Color(0xFFFFFFFF),
        onPrimary: Color(0xFF000000),
        secondary: Color(0xFFFFFFFF),
        onSecondary: Color(0xFF000000),
        background: Color(0xFF000000),
        onBackground: Color(0xFFFFFFFF),
        surface: Color(0xFF000000),
        onSurface: Color(0xFFFFFFFF),
        error: Color(0xFFFFFFFF),
        onError: Color(0xFF000000),
        outline: Color(0xFFFFFFFF),
      ),
      scaffoldBackgroundColor: const Color(0xFF000000),
      appBarTheme: const AppBarTheme(
        backgroundColor: Color(0xFF000000),
        foregroundColor: Color(0xFFFFFFFF),
        elevation: 0,
        iconTheme: IconThemeData(color: Color(0xFFFFFFFF)),
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: const Color(0xFFFFFFFF),
          foregroundColor: const Color(0xFF000000),
          side: const BorderSide(color: Color(0xFFFFFFFF), width: 2),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(4),
          ),
        ),
      ),
      outlinedButtonTheme: OutlinedButtonThemeData(
        style: OutlinedButton.styleFrom(
          foregroundColor: const Color(0xFFFFFFFF),
          side: const BorderSide(color: Color(0xFFFFFFFF), width: 2),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(4),
          ),
        ),
      ),
      textButtonTheme: TextButtonThemeData(
        style: TextButton.styleFrom(
          foregroundColor: const Color(0xFFFFFFFF),
        ),
      ),
      cardTheme: const CardTheme(
        color: Color(0xFF000000),
        elevation: 0,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.all(Radius.circular(4)),
          side: BorderSide(color: Color(0xFFFFFFFF), width: 2),
        ),
      ),
      inputDecorationTheme: const InputDecorationTheme(
        filled: true,
        fillColor: Color(0xFF000000),
        border: OutlineInputBorder(
          borderSide: BorderSide(color: Color(0xFFFFFFFF), width: 2),
        ),
        enabledBorder: OutlineInputBorder(
          borderSide: BorderSide(color: Color(0xFFFFFFFF), width: 2),
        ),
        focusedBorder: OutlineInputBorder(
          borderSide: BorderSide(color: Color(0xFFFFFFFF), width: 3),
        ),
      ),
      textTheme: const TextTheme(
        displayLarge: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.bold),
        displayMedium: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.bold),
        displaySmall: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.bold),
        headlineLarge: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.bold),
        headlineMedium: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.bold),
        headlineSmall: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.bold),
        titleLarge: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.bold),
        titleMedium: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.bold),
        titleSmall: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.bold),
        bodyLarge: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.w500),
        bodyMedium: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.w500),
        bodySmall: TextStyle(color: Color(0xFFFFFFFF), fontWeight: FontWeight.w500),
      ),
    );
  }

  // Standard light theme for comparison
  static ThemeData get standardLight {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
    );
  }

  // Standard dark theme for comparison
  static ThemeData get standardDark {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: Colors.blue,
        brightness: Brightness.dark,
      ),
    );
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'High Contrast Themes',
      theme: HighContrastThemes.standardLight,
      darkTheme: HighContrastThemes.standardDark,
      highContrastTheme: HighContrastThemes.highContrastLight,
      highContrastDarkTheme: HighContrastThemes.highContrastDark,
      home: const HighContrastThemePage(),
    );
  }
}

class HighContrastThemePage extends StatefulWidget {
  const HighContrastThemePage({super.key});

  @override
  State<HighContrastThemePage> createState() => _HighContrastThemePageState();
}

class _HighContrastThemePageState extends State<HighContrastThemePage> {
  ThemeMode _themeMode = ThemeMode.system;
  bool _useHighContrast = false;

  @override
  Widget build(BuildContext context) {
    return Theme(
      data: _getSelectedTheme(),
      child: Scaffold(
        appBar: AppBar(
          title: const Text('High Contrast Themes'),
          actions: [
            IconButton(
              icon: const Icon(Icons.accessibility),
              onPressed: () {
                _showAccessibilityDialog();
              },
            ),
          ],
        ),
        body: SingleChildScrollView(
          padding: const EdgeInsets.all(16),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              _buildThemeControls(),
              const SizedBox(height: 24),
              _buildContrastComparison(),
              const SizedBox(height: 24),
              _buildComponentShowcase(),
              const SizedBox(height: 24),
              _buildAccessibilityInfo(),
            ],
          ),
        ),
      ),
    );
  }

  ThemeData _getSelectedTheme() {
    if (_useHighContrast) {
      return _themeMode == ThemeMode.dark
          ? HighContrastThemes.highContrastDark
          : HighContrastThemes.highContrastLight;
    } else {
      return _themeMode == ThemeMode.dark
          ? HighContrastThemes.standardDark
          : HighContrastThemes.standardLight;
    }
  }

  Widget _buildThemeControls() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Theme Controls',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            SwitchListTile(
              title: const Text('High Contrast Mode'),
              subtitle: const Text('Enable for better visibility'),
              value: _useHighContrast,
              onChanged: (value) {
                setState(() {
                  _useHighContrast = value;
                });
              },
            ),
            const SizedBox(height: 8),
            Text(
              'Theme Mode',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 8),
            SegmentedButton<ThemeMode>(
              segments: const [
                ButtonSegment(
                  value: ThemeMode.light,
                  label: Text('Light'),
                  icon: Icon(Icons.light_mode),
                ),
                ButtonSegment(
                  value: ThemeMode.dark,
                  label: Text('Dark'),
                  icon: Icon(Icons.dark_mode),
                ),
                ButtonSegment(
                  value: ThemeMode.system,
                  label: Text('System'),
                  icon: Icon(Icons.auto_mode),
                ),
              ],
              selected: {_themeMode},
              onSelectionChanged: (Set<ThemeMode> selection) {
                setState(() {
                  _themeMode = selection.first;
                });
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildContrastComparison() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Contrast Comparison',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: Column(
                    children: [
                      Text(
                        'Standard Contrast',
                        style: Theme.of(context).textTheme.titleSmall,
                      ),
                      const SizedBox(height: 8),
                      Container(
                        height: 100,
                        decoration: BoxDecoration(
                          color: _useHighContrast ? Colors.grey.shade300 : Colors.blue,
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: Center(
                          child: Text(
                            'Sample Text',
                            style: TextStyle(
                              color: _useHighContrast ? Colors.black : Colors.white,
                              fontWeight: FontWeight.w500,
                            ),
                          ),
                        ),
                      ),
                    ],
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: Column(
                    children: [
                      Text(
                        'High Contrast',
                        style: Theme.of(context).textTheme.titleSmall,
                      ),
                      const SizedBox(height: 8),
                      Container(
                        height: 100,
                        decoration: BoxDecoration(
                          color: _useHighContrast
                              ? (_themeMode == ThemeMode.dark ? Colors.white : Colors.black)
                              : Colors.grey.shade600,
                          borderRadius: BorderRadius.circular(8),
                          border: Border.all(
                            color: _useHighContrast
                                ? (_themeMode == ThemeMode.dark ? Colors.black : Colors.white)
                                : Colors.grey,
                            width: 2,
                          ),
                        ),
                        child: Center(
                          child: Text(
                            'Sample Text',
                            style: TextStyle(
                              color: _useHighContrast
                                  ? (_themeMode == ThemeMode.dark ? Colors.black : Colors.white)
                                  : Colors.white,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                        ),
                      ),
                    ],
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildComponentShowcase() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Component Showcase',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: () {},
                    child: const Text('Primary Button'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: OutlinedButton(
                    onPressed: () {},
                    child: const Text('Outlined'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            TextButton(
              onPressed: () {},
              child: const Text('Text Button'),
            ),
            const SizedBox(height: 16),
            const TextField(
              decoration: InputDecoration(
                labelText: 'Input Field',
                hintText: 'Enter text here',
              ),
            ),
            const SizedBox(height: 16),
            CheckboxListTile(
              title: const Text('Checkbox Option'),
              value: true,
              onChanged: (value) {},
            ),
            SwitchListTile(
              title: const Text('Switch Option'),
              value: false,
              onChanged: (value) {},
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildAccessibilityInfo() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const Icon(Icons.accessibility),
                const SizedBox(width: 8),
                Text(
                  'Accessibility Features',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
              ],
            ),
            const SizedBox(height: 16),
            _buildAccessibilityFeature(
              'Maximum Contrast',
              'Pure black and white colors for highest visibility',
            ),
            _buildAccessibilityFeature(
              'Bold Typography',
              'Heavier font weights for improved readability',
            ),
            _buildAccessibilityFeature(
              'Prominent Borders',
              'Thick borders to clearly define interactive elements',
            ),
            _buildAccessibilityFeature(
              'Reduced Visual Noise',
              'Minimal shadows and gradients for cleaner appearance',
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildAccessibilityFeature(String title, String description) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 8),
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Container(
            width: 6,
            height: 6,
            margin: const EdgeInsets.only(top: 6),
            decoration: BoxDecoration(
              color: Theme.of(context).colorScheme.primary,
              shape: BoxShape.circle,
            ),
          ),
          const SizedBox(width: 12),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  title,
                  style: Theme.of(context).textTheme.titleSmall,
                ),
                Text(
                  description,
                  style: Theme.of(context).textTheme.bodySmall,
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  void _showAccessibilityDialog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Accessibility Settings'),
        content: const Text(
          'High contrast themes improve visibility for users with visual '
          'impairments. They use maximum contrast ratios and bold typography '
          'for better readability.',
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('OK'),
          ),
        ],
      ),
    );
  }
}
```

High contrast themes provide maximum accessibility by using pure black  
and white colors with bold typography and prominent borders. These  
themes help users with visual impairments by eliminating subtle color  
differences and providing clear visual boundaries between interface  
elements.  

## System Theme Following

Automatically following the system's theme preferences and settings.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'System Theme Following',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
          brightness: Brightness.dark,
        ),
      ),
      highContrastTheme: ThemeData(
        brightness: Brightness.light,
        colorScheme: const ColorScheme.light(
          primary: Colors.black,
          onPrimary: Colors.white,
          background: Colors.white,
          onBackground: Colors.black,
        ),
      ),
      highContrastDarkTheme: ThemeData(
        brightness: Brightness.dark,
        colorScheme: const ColorScheme.dark(
          primary: Colors.white,
          onPrimary: Colors.black,
          background: Colors.black,
          onBackground: Colors.white,
        ),
      ),
      themeMode: ThemeMode.system, // Follows system preference
      home: const SystemThemeFollowingPage(),
    );
  }
}

class SystemThemeFollowingPage extends StatefulWidget {
  const SystemThemeFollowingPage({super.key});

  @override
  State<SystemThemeFollowingPage> createState() => _SystemThemeFollowingPageState();
}

class _SystemThemeFollowingPageState extends State<SystemThemeFollowingPage>
    with WidgetsBindingObserver {
  Brightness? _systemBrightness;
  bool _highContrast = false;
  late ThemeMode _currentThemeMode;

  @override
  void initState() {
    super.initState();
    WidgetsBinding.instance.addObserver(this);
    _currentThemeMode = ThemeMode.system;
    _updateSystemThemeInfo();
  }

  @override
  void dispose() {
    WidgetsBinding.instance.removeObserver(this);
    super.dispose();
  }

  @override
  void didChangePlatformBrightness() {
    super.didChangePlatformBrightness();
    _updateSystemThemeInfo();
  }

  @override
  void didChangeAccessibilityFeatures() {
    super.didChangeAccessibilityFeatures();
    _updateSystemThemeInfo();
  }

  void _updateSystemThemeInfo() {
    setState(() {
      _systemBrightness = WidgetsBinding.instance.platformDispatcher.platformBrightness;
      _highContrast = MediaQuery.of(context).highContrast;
    });
  }

  @override
  Widget build(BuildContext context) {
    final mediaQuery = MediaQuery.of(context);
    final platformBrightness = mediaQuery.platformBrightness;
    final isHighContrast = mediaQuery.highContrast;
    final textScale = mediaQuery.textScaleFactor;
    final accessibilityFeatures = mediaQuery.accessibilityFeatures;

    return Scaffold(
      appBar: AppBar(
        title: const Text('System Theme Following'),
        systemOverlayStyle: SystemUiOverlayStyle(
          statusBarBrightness: Theme.of(context).brightness,
          statusBarIconBrightness: Theme.of(context).brightness == Brightness.dark
              ? Brightness.light
              : Brightness.dark,
        ),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildSystemInfo(platformBrightness, isHighContrast, textScale),
            const SizedBox(height: 24),
            _buildThemeModeSelector(),
            const SizedBox(height: 24),
            _buildAccessibilityInfo(accessibilityFeatures),
            const SizedBox(height: 24),
            _buildAdaptiveComponents(),
            const SizedBox(height: 24),
            _buildSystemStatusOverview(),
          ],
        ),
      ),
    );
  }

  Widget _buildSystemInfo(Brightness brightness, bool highContrast, double textScale) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                Icon(
                  brightness == Brightness.dark ? Icons.dark_mode : Icons.light_mode,
                  color: Theme.of(context).colorScheme.primary,
                ),
                const SizedBox(width: 8),
                Text(
                  'System Theme Information',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
              ],
            ),
            const SizedBox(height: 16),
            _buildInfoRow('Platform Brightness', brightness.name.toUpperCase()),
            _buildInfoRow('High Contrast', highContrast ? 'ENABLED' : 'DISABLED'),
            _buildInfoRow('Text Scale Factor', '${(textScale * 100).toInt()}%'),
            _buildInfoRow('Current Theme', _getCurrentThemeDescription()),
          ],
        ),
      ),
    );
  }

  Widget _buildInfoRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(
            label,
            style: Theme.of(context).textTheme.bodyMedium,
          ),
          Container(
            padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
            decoration: BoxDecoration(
              color: Theme.of(context).colorScheme.primaryContainer,
              borderRadius: BorderRadius.circular(12),
            ),
            child: Text(
              value,
              style: Theme.of(context).textTheme.bodySmall?.copyWith(
                color: Theme.of(context).colorScheme.onPrimaryContainer,
                fontWeight: FontWeight.w600,
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildThemeModeSelector() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Theme Mode Selection',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            SegmentedButton<ThemeMode>(
              segments: const [
                ButtonSegment(
                  value: ThemeMode.system,
                  label: Text('System'),
                  icon: Icon(Icons.auto_mode),
                ),
                ButtonSegment(
                  value: ThemeMode.light,
                  label: Text('Light'),
                  icon: Icon(Icons.light_mode),
                ),
                ButtonSegment(
                  value: ThemeMode.dark,
                  label: Text('Dark'),
                  icon: Icon(Icons.dark_mode),
                ),
              ],
              selected: {_currentThemeMode},
              onSelectionChanged: (Set<ThemeMode> selection) {
                setState(() {
                  _currentThemeMode = selection.first;
                });
                // In a real app, you would propagate this change up to MaterialApp
                _showThemeChangeInfo(_currentThemeMode);
              },
            ),
            const SizedBox(height: 12),
            Text(
              _currentThemeMode == ThemeMode.system
                  ? 'Following system preference automatically'
                  : 'Using manual override: ${_currentThemeMode.name}',
              style: Theme.of(context).textTheme.bodySmall,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildAccessibilityInfo(AccessibilityFeatures features) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const Icon(Icons.accessibility),
                const SizedBox(width: 8),
                Text(
                  'Accessibility Features',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
              ],
            ),
            const SizedBox(height: 16),
            _buildAccessibilityFeature('High Contrast', features.highContrast),
            _buildAccessibilityFeature('Large Text', features.accessibleNavigation),
            _buildAccessibilityFeature('Bold Text', features.boldText),
            _buildAccessibilityFeature('Reduce Motion', features.reduceMotion),
            _buildAccessibilityFeature('Invert Colors', features.invertColors),
            _buildAccessibilityFeature('Disable Animations', features.disableAnimations),
          ],
        ),
      ),
    );
  }

  Widget _buildAccessibilityFeature(String name, bool enabled) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        children: [
          Icon(
            enabled ? Icons.check_circle : Icons.cancel,
            color: enabled ? Colors.green : Colors.grey,
            size: 20,
          ),
          const SizedBox(width: 8),
          Expanded(
            child: Text(
              name,
              style: Theme.of(context).textTheme.bodyMedium,
            ),
          ),
          Text(
            enabled ? 'ON' : 'OFF',
            style: Theme.of(context).textTheme.bodySmall?.copyWith(
              color: enabled ? Colors.green : Colors.grey,
              fontWeight: FontWeight.w600,
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildAdaptiveComponents() {
    final mediaQuery = MediaQuery.of(context);
    final isHighContrast = mediaQuery.highContrast;
    final reduceMotions = mediaQuery.accessibilityFeatures.reduceMotion;

    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Adaptive Components',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'These components adapt based on system settings:',
              style: Theme.of(context).textTheme.bodyMedium,
            ),
            const SizedBox(height: 12),
            
            // Button adapts to high contrast
            ElevatedButton(
              style: ElevatedButton.styleFrom(
                side: isHighContrast
                    ? BorderSide(
                        color: Theme.of(context).colorScheme.outline,
                        width: 2,
                      )
                    : null,
              ),
              onPressed: () {},
              child: Text(isHighContrast ? 'High Contrast Button' : 'Normal Button'),
            ),
            
            const SizedBox(height: 12),
            
            // Animation respects reduce motion
            AnimatedContainer(
              duration: reduceMotions
                  ? const Duration(milliseconds: 1)
                  : const Duration(milliseconds: 300),
              height: 60,
              decoration: BoxDecoration(
                color: Theme.of(context).colorScheme.primaryContainer,
                borderRadius: BorderRadius.circular(8),
              ),
              child: Center(
                child: Text(
                  reduceMotions ? 'Animations Reduced' : 'Normal Animations',
                  style: TextStyle(
                    color: Theme.of(context).colorScheme.onPrimaryContainer,
                  ),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSystemStatusOverview() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'System Integration Status',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            _buildStatusItem(
              Icons.smartphone,
              'Platform Theme Sync',
              'Active',
              Colors.green,
            ),
            _buildStatusItem(
              Icons.accessibility,
              'Accessibility Features',
              'Monitored',
              Colors.blue,
            ),
            _buildStatusItem(
              Icons.palette,
              'Dynamic Color Support',
              'Available',
              Colors.orange,
            ),
            _buildStatusItem(
              Icons.auto_mode,
              'Automatic Switching',
              'Enabled',
              Colors.green,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildStatusItem(IconData icon, String title, String status, Color color) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 6),
      child: Row(
        children: [
          Container(
            padding: const EdgeInsets.all(8),
            decoration: BoxDecoration(
              color: color.withOpacity(0.1),
              borderRadius: BorderRadius.circular(8),
            ),
            child: Icon(icon, color: color, size: 20),
          ),
          const SizedBox(width: 12),
          Expanded(
            child: Text(
              title,
              style: Theme.of(context).textTheme.bodyMedium,
            ),
          ),
          Container(
            padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
            decoration: BoxDecoration(
              color: color.withOpacity(0.2),
              borderRadius: BorderRadius.circular(12),
            ),
            child: Text(
              status,
              style: TextStyle(
                color: color,
                fontSize: 12,
                fontWeight: FontWeight.w600,
              ),
            ),
          ),
        ],
      ),
    );
  }

  String _getCurrentThemeDescription() {
    if (_currentThemeMode == ThemeMode.system) {
      final brightness = MediaQuery.of(context).platformBrightness;
      final isHighContrast = MediaQuery.of(context).highContrast;
      
      String base = brightness == Brightness.dark ? 'DARK' : 'LIGHT';
      if (isHighContrast) base += ' + HIGH CONTRAST';
      
      return base;
    } else {
      return _currentThemeMode.name.toUpperCase();
    }
  }

  void _showThemeChangeInfo(ThemeMode mode) {
    String message;
    switch (mode) {
      case ThemeMode.system:
        message = 'Now following system theme preferences';
        break;
      case ThemeMode.light:
        message = 'Switched to light theme (overriding system)';
        break;
      case ThemeMode.dark:
        message = 'Switched to dark theme (overriding system)';
        break;
    }

    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(message),
        action: SnackBarAction(
          label: 'OK',
          onPressed: () {},
        ),
      ),
    );
  }
}
```

System theme following ensures your app automatically adapts to user  
preferences set at the OS level. By using ThemeMode.system and  
monitoring MediaQuery changes, your app seamlessly responds to dark  
mode toggles, high contrast settings, and other accessibility  
preferences without requiring manual configuration.  

## Theme Persistence

Saving and restoring user theme preferences across app sessions.  

```dart
import 'package:flutter/material.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

// Mock storage service - replace with actual SharedPreferences or secure storage
class ThemeStorageService {
  static const String _themeKey = 'app_theme_preferences';
  static final Map<String, String> _mockStorage = {};

  static Future<void> saveThemePreferences(ThemePreferences preferences) async {
    // Simulate async storage operation
    await Future.delayed(const Duration(milliseconds: 100));
    _mockStorage[_themeKey] = jsonEncode(preferences.toMap());
  }

  static Future<ThemePreferences?> loadThemePreferences() async {
    // Simulate async storage operation
    await Future.delayed(const Duration(milliseconds: 100));
    
    final jsonString = _mockStorage[_themeKey];
    if (jsonString == null) return null;
    
    try {
      final Map<String, dynamic> map = jsonDecode(jsonString);
      return ThemePreferences.fromMap(map);
    } catch (e) {
      return null;
    }
  }

  static Future<void> clearThemePreferences() async {
    await Future.delayed(const Duration(milliseconds: 100));
    _mockStorage.remove(_themeKey);
  }
}

class ThemePreferences {
  final ThemeMode themeMode;
  final String primaryColorName;
  final bool useHighContrast;
  final double textScaleFactor;
  final bool reduceAnimations;
  final String fontFamily;

  const ThemePreferences({
    required this.themeMode,
    required this.primaryColorName,
    required this.useHighContrast,
    required this.textScaleFactor,
    required this.reduceAnimations,
    required this.fontFamily,
  });

  factory ThemePreferences.defaultPrefs() {
    return const ThemePreferences(
      themeMode: ThemeMode.system,
      primaryColorName: 'blue',
      useHighContrast: false,
      textScaleFactor: 1.0,
      reduceAnimations: false,
      fontFamily: 'Roboto',
    );
  }

  Color get primaryColor {
    switch (primaryColorName) {
      case 'blue':
        return Colors.blue;
      case 'green':
        return Colors.green;
      case 'purple':
        return Colors.purple;
      case 'orange':
        return Colors.orange;
      case 'red':
        return Colors.red;
      default:
        return Colors.blue;
    }
  }

  ThemePreferences copyWith({
    ThemeMode? themeMode,
    String? primaryColorName,
    bool? useHighContrast,
    double? textScaleFactor,
    bool? reduceAnimations,
    String? fontFamily,
  }) {
    return ThemePreferences(
      themeMode: themeMode ?? this.themeMode,
      primaryColorName: primaryColorName ?? this.primaryColorName,
      useHighContrast: useHighContrast ?? this.useHighContrast,
      textScaleFactor: textScaleFactor ?? this.textScaleFactor,
      reduceAnimations: reduceAnimations ?? this.reduceAnimations,
      fontFamily: fontFamily ?? this.fontFamily,
    );
  }

  Map<String, dynamic> toMap() {
    return {
      'themeMode': themeMode.index,
      'primaryColorName': primaryColorName,
      'useHighContrast': useHighContrast,
      'textScaleFactor': textScaleFactor,
      'reduceAnimations': reduceAnimations,
      'fontFamily': fontFamily,
    };
  }

  factory ThemePreferences.fromMap(Map<String, dynamic> map) {
    return ThemePreferences(
      themeMode: ThemeMode.values[map['themeMode'] ?? 0],
      primaryColorName: map['primaryColorName'] ?? 'blue',
      useHighContrast: map['useHighContrast'] ?? false,
      textScaleFactor: (map['textScaleFactor'] ?? 1.0).toDouble(),
      reduceAnimations: map['reduceAnimations'] ?? false,
      fontFamily: map['fontFamily'] ?? 'Roboto',
    );
  }
}

class ThemeController extends ChangeNotifier {
  ThemePreferences _preferences = ThemePreferences.defaultPrefs();
  bool _isLoading = true;

  ThemePreferences get preferences => _preferences;
  bool get isLoading => _isLoading;

  ThemeController() {
    _loadPreferences();
  }

  Future<void> _loadPreferences() async {
    try {
      final savedPrefs = await ThemeStorageService.loadThemePreferences();
      if (savedPrefs != null) {
        _preferences = savedPrefs;
      }
    } catch (e) {
      // Handle error - use defaults
      _preferences = ThemePreferences.defaultPrefs();
    } finally {
      _isLoading = false;
      notifyListeners();
    }
  }

  Future<void> updatePreferences(ThemePreferences newPreferences) async {
    _preferences = newPreferences;
    notifyListeners();
    
    try {
      await ThemeStorageService.saveThemePreferences(newPreferences);
    } catch (e) {
      // Handle save error
      debugPrint('Failed to save theme preferences: $e');
    }
  }

  Future<void> resetToDefaults() async {
    await updatePreferences(ThemePreferences.defaultPrefs());
    await ThemeStorageService.clearThemePreferences();
  }

  ThemeData buildTheme({required Brightness brightness}) {
    final baseTheme = ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: _preferences.primaryColor,
        brightness: brightness,
      ),
      fontFamily: _preferences.fontFamily,
    );

    if (_preferences.useHighContrast) {
      return _buildHighContrastTheme(baseTheme, brightness);
    }

    return baseTheme;
  }

  ThemeData _buildHighContrastTheme(ThemeData base, Brightness brightness) {
    final isLight = brightness == Brightness.light;
    
    return base.copyWith(
      colorScheme: base.colorScheme.copyWith(
        primary: isLight ? Colors.black : Colors.white,
        onPrimary: isLight ? Colors.white : Colors.black,
        background: isLight ? Colors.white : Colors.black,
        onBackground: isLight ? Colors.black : Colors.white,
        surface: isLight ? Colors.white : Colors.black,
        onSurface: isLight ? Colors.black : Colors.white,
      ),
      textTheme: base.textTheme.copyWith(
        displayLarge: base.textTheme.displayLarge?.copyWith(fontWeight: FontWeight.bold),
        displayMedium: base.textTheme.displayMedium?.copyWith(fontWeight: FontWeight.bold),
        headlineLarge: base.textTheme.headlineLarge?.copyWith(fontWeight: FontWeight.bold),
        titleLarge: base.textTheme.titleLarge?.copyWith(fontWeight: FontWeight.bold),
      ),
    );
  }
}

class MyApp extends StatelessWidget {
  final ThemeController themeController;

  const MyApp({super.key, required this.themeController});

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: themeController,
      builder: (context, child) {
        if (themeController.isLoading) {
          return MaterialApp(
            home: Scaffold(
              body: Center(
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    const CircularProgressIndicator(),
                    const SizedBox(height: 16),
                    Text(
                      'Loading theme preferences...',
                      style: Theme.of(context).textTheme.bodyLarge,
                    ),
                  ],
                ),
              ),
            ),
          );
        }

        return MediaQuery(
          data: MediaQuery.of(context).copyWith(
            textScaleFactor: themeController.preferences.textScaleFactor,
          ),
          child: MaterialApp(
            title: 'Theme Persistence',
            theme: themeController.buildTheme(brightness: Brightness.light),
            darkTheme: themeController.buildTheme(brightness: Brightness.dark),
            themeMode: themeController.preferences.themeMode,
            home: ThemePersistencePage(themeController: themeController),
          ),
        );
      },
    );
  }
}

class ThemePersistencePage extends StatelessWidget {
  final ThemeController themeController;

  const ThemePersistencePage({super.key, required this.themeController});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Theme Persistence'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: () => themeController.resetToDefaults(),
            tooltip: 'Reset to defaults',
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildThemeModeSelector(context),
            const SizedBox(height: 16),
            _buildColorSelector(context),
            const SizedBox(height: 16),
            _buildAccessibilityOptions(context),
            const SizedBox(height: 16),
            _buildTextOptions(context),
            const SizedBox(height: 16),
            _buildPreferencesPreview(context),
            const SizedBox(height: 16),
            _buildStorageInfo(context),
          ],
        ),
      ),
    );
  }

  Widget _buildThemeModeSelector(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Theme Mode',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            SegmentedButton<ThemeMode>(
              segments: const [
                ButtonSegment(
                  value: ThemeMode.system,
                  label: Text('System'),
                  icon: Icon(Icons.auto_mode),
                ),
                ButtonSegment(
                  value: ThemeMode.light,
                  label: Text('Light'),
                  icon: Icon(Icons.light_mode),
                ),
                ButtonSegment(
                  value: ThemeMode.dark,
                  label: Text('Dark'),
                  icon: Icon(Icons.dark_mode),
                ),
              ],
              selected: {themeController.preferences.themeMode},
              onSelectionChanged: (Set<ThemeMode> selection) {
                themeController.updatePreferences(
                  themeController.preferences.copyWith(themeMode: selection.first),
                );
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorSelector(BuildContext context) {
    final colors = {
      'blue': Colors.blue,
      'green': Colors.green,
      'purple': Colors.purple,
      'orange': Colors.orange,
      'red': Colors.red,
    };

    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Primary Color',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Wrap(
              spacing: 8,
              children: colors.entries.map((entry) {
                final isSelected = themeController.preferences.primaryColorName == entry.key;
                return GestureDetector(
                  onTap: () {
                    themeController.updatePreferences(
                      themeController.preferences.copyWith(primaryColorName: entry.key),
                    );
                  },
                  child: Container(
                    width: 48,
                    height: 48,
                    decoration: BoxDecoration(
                      color: entry.value,
                      shape: BoxShape.circle,
                      border: Border.all(
                        color: isSelected ? Colors.white : Colors.transparent,
                        width: 3,
                      ),
                      boxShadow: isSelected
                          ? [
                              BoxShadow(
                                color: entry.value.withOpacity(0.5),
                                blurRadius: 8,
                                spreadRadius: 2,
                              ),
                            ]
                          : null,
                    ),
                    child: isSelected
                        ? const Icon(Icons.check, color: Colors.white)
                        : null,
                  ),
                );
              }).toList(),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildAccessibilityOptions(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Accessibility',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            SwitchListTile(
              title: const Text('High Contrast'),
              subtitle: const Text('Increase color contrast for better visibility'),
              value: themeController.preferences.useHighContrast,
              onChanged: (value) {
                themeController.updatePreferences(
                  themeController.preferences.copyWith(useHighContrast: value),
                );
              },
            ),
            SwitchListTile(
              title: const Text('Reduce Animations'),
              subtitle: const Text('Minimize motion for comfort'),
              value: themeController.preferences.reduceAnimations,
              onChanged: (value) {
                themeController.updatePreferences(
                  themeController.preferences.copyWith(reduceAnimations: value),
                );
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildTextOptions(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Text Settings',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Text('Text Scale: ${(themeController.preferences.textScaleFactor * 100).toInt()}%'),
            Slider(
              value: themeController.preferences.textScaleFactor,
              min: 0.8,
              max: 2.0,
              divisions: 12,
              onChanged: (value) {
                themeController.updatePreferences(
                  themeController.preferences.copyWith(textScaleFactor: value),
                );
              },
            ),
            const SizedBox(height: 16),
            Text('Font Family', style: Theme.of(context).textTheme.titleMedium),
            const SizedBox(height: 8),
            DropdownButtonFormField<String>(
              value: themeController.preferences.fontFamily,
              decoration: const InputDecoration(border: OutlineInputBorder()),
              items: const [
                DropdownMenuItem(value: 'Roboto', child: Text('Roboto')),
                DropdownMenuItem(value: 'Open Sans', child: Text('Open Sans')),
                DropdownMenuItem(value: 'Lato', child: Text('Lato')),
                DropdownMenuItem(value: 'Montserrat', child: Text('Montserrat')),
              ],
              onChanged: (value) {
                if (value != null) {
                  themeController.updatePreferences(
                    themeController.preferences.copyWith(fontFamily: value),
                  );
                }
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildPreferencesPreview(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Preview',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Container(
              width: double.infinity,
              padding: const EdgeInsets.all(16),
              decoration: BoxDecoration(
                color: Theme.of(context).colorScheme.primaryContainer,
                borderRadius: BorderRadius.circular(8),
              ),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Sample Headline',
                    style: Theme.of(context).textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 8),
                  Text(
                    'This is sample body text that demonstrates the current theme settings including color, typography, and contrast.',
                    style: Theme.of(context).textTheme.bodyMedium,
                  ),
                  const SizedBox(height: 12),
                  ElevatedButton(
                    onPressed: () {},
                    child: const Text('Sample Button'),
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildStorageInfo(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const Icon(Icons.save),
                const SizedBox(width: 8),
                Text(
                  'Storage Information',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
              ],
            ),
            const SizedBox(height: 16),
            const Text('✓ Preferences are automatically saved'),
            const SizedBox(height: 4),
            const Text('✓ Settings persist across app restarts'),
            const SizedBox(height: 4),
            const Text('✓ Instant synchronization across screens'),
            const SizedBox(height: 16),
            OutlinedButton(
              onPressed: () {
                showDialog(
                  context: context,
                  builder: (context) => AlertDialog(
                    title: const Text('Reset Theme Settings'),
                    content: const Text(
                      'This will reset all theme preferences to their default values. This action cannot be undone.',
                    ),
                    actions: [
                      TextButton(
                        onPressed: () => Navigator.pop(context),
                        child: const Text('Cancel'),
                      ),
                      ElevatedButton(
                        onPressed: () {
                          Navigator.pop(context);
                          themeController.resetToDefaults();
                        },
                        child: const Text('Reset'),
                      ),
                    ],
                  ),
                );
              },
              child: const Text('Reset All Settings'),
            ),
          ],
        ),
      ),
    );
  }
}

// Entry point with theme controller initialization
void main() {
  runApp(MyApp(themeController: ThemeController()));
}
```

Theme persistence ensures user preferences are maintained across app  
sessions using local storage. The ThemeController manages state changes  
and storage operations, while ThemePreferences encapsulates all theme  
settings. This creates a seamless user experience where customizations  
are automatically restored when the app launches.  

## Multiple Theme Variants

Managing multiple theme variants and allowing users to switch between them.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

enum ThemeVariant {
  classic,
  modern,
  vibrant,
  minimal,
  nature,
}

extension ThemeVariantExtension on ThemeVariant {
  String get displayName {
    switch (this) {
      case ThemeVariant.classic:
        return 'Classic';
      case ThemeVariant.modern:
        return 'Modern';
      case ThemeVariant.vibrant:
        return 'Vibrant';
      case ThemeVariant.minimal:
        return 'Minimal';
      case ThemeVariant.nature:
        return 'Nature';
    }
  }

  String get description {
    switch (this) {
      case ThemeVariant.classic:
        return 'Traditional and elegant design with subtle colors';
      case ThemeVariant.modern:
        return 'Contemporary styling with bold contrasts';
      case ThemeVariant.vibrant:
        return 'Bright and energetic color palette';
      case ThemeVariant.minimal:
        return 'Clean and simple with minimal visual elements';
      case ThemeVariant.nature:
        return 'Earth tones inspired by natural elements';
    }
  }

  IconData get icon {
    switch (this) {
      case ThemeVariant.classic:
        return Icons.library_books;
      case ThemeVariant.modern:
        return Icons.architecture;
      case ThemeVariant.vibrant:
        return Icons.color_lens;
      case ThemeVariant.minimal:
        return Icons.circle_outlined;
      case ThemeVariant.nature:
        return Icons.eco;
    }
  }
}

class ThemeVariants {
  static ThemeData classic({required Brightness brightness}) {
    final isLight = brightness == Brightness.light;
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: const Color(0xFF8B4513),
        brightness: brightness,
      ).copyWith(
        primary: isLight ? const Color(0xFF8B4513) : const Color(0xFFD2B48C),
        secondary: isLight ? const Color(0xFF2F4F4F) : const Color(0xFF708090),
        surface: isLight ? const Color(0xFFFFFDF8) : const Color(0xFF1A1611),
      ),
      textTheme: TextTheme(
        displayLarge: TextStyle(
          fontFamily: 'Georgia',
          color: isLight ? const Color(0xFF2F2F2F) : const Color(0xFFF5F5DC),
        ),
        headlineLarge: TextStyle(
          fontFamily: 'Georgia',
          color: isLight ? const Color(0xFF2F2F2F) : const Color(0xFFF5F5DC),
        ),
        bodyLarge: TextStyle(
          fontFamily: 'Times',
          color: isLight ? const Color(0xFF3F3F3F) : const Color(0xFFE5E5DC),
        ),
      ),
      cardTheme: CardTheme(
        elevation: 3,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(8),
        ),
        color: isLight ? const Color(0xFFFFFDF8) : const Color(0xFF2A251F),
      ),
    );
  }

  static ThemeData modern({required Brightness brightness}) {
    final isLight = brightness == Brightness.light;
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: Colors.blueGrey,
        brightness: brightness,
      ).copyWith(
        primary: isLight ? Colors.blueGrey.shade800 : Colors.blueGrey.shade200,
        secondary: isLight ? Colors.cyan.shade700 : Colors.cyan.shade300,
        surface: isLight ? Colors.grey.shade50 : Colors.grey.shade900,
      ),
      textTheme: const TextTheme(
        displayLarge: TextStyle(fontFamily: 'Roboto', fontWeight: FontWeight.w300),
        headlineLarge: TextStyle(fontFamily: 'Roboto', fontWeight: FontWeight.w400),
        bodyLarge: TextStyle(fontFamily: 'Roboto', fontWeight: FontWeight.w400),
      ),
      cardTheme: CardTheme(
        elevation: 1,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(16),
        ),
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(12),
          ),
        ),
      ),
    );
  }

  static ThemeData vibrant({required Brightness brightness}) {
    final isLight = brightness == Brightness.light;
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: Colors.deepPurple,
        brightness: brightness,
      ).copyWith(
        primary: isLight ? Colors.deepPurple : Colors.deepPurple.shade300,
        secondary: isLight ? Colors.pink.shade600 : Colors.pink.shade300,
        tertiary: isLight ? Colors.orange.shade600 : Colors.orange.shade300,
        surface: isLight ? Colors.purple.shade50 : Colors.purple.shade900,
      ),
      textTheme: const TextTheme(
        displayLarge: TextStyle(fontFamily: 'Montserrat', fontWeight: FontWeight.bold),
        headlineLarge: TextStyle(fontFamily: 'Montserrat', fontWeight: FontWeight.w600),
        bodyLarge: TextStyle(fontFamily: 'Open Sans', fontWeight: FontWeight.w400),
      ),
      cardTheme: CardTheme(
        elevation: 4,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(20),
        ),
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          shape: const StadiumBorder(),
          elevation: 3,
        ),
      ),
    );
  }

  static ThemeData minimal({required Brightness brightness}) {
    final isLight = brightness == Brightness.light;
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: Colors.grey,
        brightness: brightness,
      ).copyWith(
        primary: isLight ? Colors.grey.shade800 : Colors.grey.shade200,
        secondary: isLight ? Colors.grey.shade600 : Colors.grey.shade400,
        surface: isLight ? Colors.white : Colors.grey.shade900,
        background: isLight ? Colors.grey.shade50 : Colors.black,
      ),
      textTheme: const TextTheme(
        displayLarge: TextStyle(fontFamily: 'Inter', fontWeight: FontWeight.w200),
        headlineLarge: TextStyle(fontFamily: 'Inter', fontWeight: FontWeight.w300),
        bodyLarge: TextStyle(fontFamily: 'Inter', fontWeight: FontWeight.w400),
      ),
      cardTheme: CardTheme(
        elevation: 0,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(4),
          side: BorderSide(
            color: isLight ? Colors.grey.shade300 : Colors.grey.shade700,
            width: 1,
          ),
        ),
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          elevation: 0,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(4),
          ),
        ),
      ),
    );
  }

  static ThemeData nature({required Brightness brightness}) {
    final isLight = brightness == Brightness.light;
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: Colors.green,
        brightness: brightness,
      ).copyWith(
        primary: isLight ? Colors.green.shade700 : Colors.green.shade300,
        secondary: isLight ? Colors.brown.shade600 : Colors.brown.shade300,
        tertiary: isLight ? Colors.amber.shade700 : Colors.amber.shade300,
        surface: isLight ? Colors.green.shade50 : Colors.green.shade900,
        background: isLight ? const Color(0xFFF1F8E9) : const Color(0xFF0D1F0D),
      ),
      textTheme: const TextTheme(
        displayLarge: TextStyle(fontFamily: 'Merriweather', fontWeight: FontWeight.w400),
        headlineLarge: TextStyle(fontFamily: 'Merriweather', fontWeight: FontWeight.w500),
        bodyLarge: TextStyle(fontFamily: 'Lato', fontWeight: FontWeight.w400),
      ),
      cardTheme: CardTheme(
        elevation: 2,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(12),
        ),
        color: isLight ? Colors.green.shade50 : Colors.green.shade800,
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(8),
          ),
        ),
      ),
    );
  }

  static ThemeData getTheme({
    required ThemeVariant variant,
    required Brightness brightness,
  }) {
    switch (variant) {
      case ThemeVariant.classic:
        return classic(brightness: brightness);
      case ThemeVariant.modern:
        return modern(brightness: brightness);
      case ThemeVariant.vibrant:
        return vibrant(brightness: brightness);
      case ThemeVariant.minimal:
        return minimal(brightness: brightness);
      case ThemeVariant.nature:
        return nature(brightness: brightness);
    }
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Multiple Theme Variants',
      theme: ThemeVariants.modern(brightness: Brightness.light),
      home: const MultipleThemeVariantsPage(),
    );
  }
}

class MultipleThemeVariantsPage extends StatefulWidget {
  const MultipleThemeVariantsPage({super.key});

  @override
  State<MultipleThemeVariantsPage> createState() => _MultipleThemeVariantsPageState();
}

class _MultipleThemeVariantsPageState extends State<MultipleThemeVariantsPage>
    with TickerProviderStateMixin {
  ThemeVariant _selectedVariant = ThemeVariant.modern;
  bool _isDarkMode = false;
  late AnimationController _animationController;

  @override
  void initState() {
    super.initState();
    _animationController = AnimationController(
      duration: const Duration(milliseconds: 300),
      vsync: this,
    );
  }

  @override
  void dispose() {
    _animationController.dispose();
    super.dispose();
  }

  void _changeTheme(ThemeVariant variant) {
    setState(() {
      _selectedVariant = variant;
    });
    _animationController.forward().then((_) {
      _animationController.reset();
    });
  }

  @override
  Widget build(BuildContext context) {
    final theme = ThemeVariants.getTheme(
      variant: _selectedVariant,
      brightness: _isDarkMode ? Brightness.dark : Brightness.light,
    );

    return AnimatedBuilder(
      animation: _animationController,
      builder: (context, child) {
        return Theme(
          data: theme,
          child: Scaffold(
            appBar: AppBar(
              title: Text('${_selectedVariant.displayName} Theme'),
              actions: [
                Switch(
                  value: _isDarkMode,
                  onChanged: (value) {
                    setState(() {
                      _isDarkMode = value;
                    });
                  },
                ),
              ],
            ),
            body: SingleChildScrollView(
              child: Column(
                children: [
                  _buildThemeSelector(),
                  _buildCurrentThemeInfo(),
                  _buildComponentShowcase(),
                  _buildThemeComparison(),
                ],
              ),
            ),
          ),
        );
      },
    );
  }

  Widget _buildThemeSelector() {
    return Container(
      height: 200,
      margin: const EdgeInsets.all(16),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'Theme Variants',
            style: Theme.of(context).textTheme.titleLarge,
          ),
          const SizedBox(height: 16),
          Expanded(
            child: ListView.builder(
              scrollDirection: Axis.horizontal,
              itemCount: ThemeVariant.values.length,
              itemBuilder: (context, index) {
                final variant = ThemeVariant.values[index];
                final isSelected = variant == _selectedVariant;
                final variantTheme = ThemeVariants.getTheme(
                  variant: variant,
                  brightness: _isDarkMode ? Brightness.dark : Brightness.light,
                );

                return GestureDetector(
                  onTap: () => _changeTheme(variant),
                  child: Container(
                    width: 140,
                    margin: const EdgeInsets.only(right: 12),
                    child: Card(
                      elevation: isSelected ? 8 : 2,
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(16),
                        side: isSelected
                            ? BorderSide(
                                color: variantTheme.colorScheme.primary,
                                width: 2,
                              )
                            : BorderSide.none,
                      ),
                      child: Container(
                        decoration: BoxDecoration(
                          borderRadius: BorderRadius.circular(16),
                          gradient: LinearGradient(
                            begin: Alignment.topLeft,
                            end: Alignment.bottomRight,
                            colors: [
                              variantTheme.colorScheme.primary.withOpacity(0.1),
                              variantTheme.colorScheme.secondary.withOpacity(0.1),
                            ],
                          ),
                        ),
                        child: Padding(
                          padding: const EdgeInsets.all(16),
                          child: Column(
                            children: [
                              Icon(
                                variant.icon,
                                size: 32,
                                color: variantTheme.colorScheme.primary,
                              ),
                              const SizedBox(height: 8),
                              Text(
                                variant.displayName,
                                style: TextStyle(
                                  fontWeight: FontWeight.w600,
                                  color: variantTheme.colorScheme.onSurface,
                                ),
                                textAlign: TextAlign.center,
                              ),
                              const SizedBox(height: 4),
                              Text(
                                variant.description,
                                style: TextStyle(
                                  fontSize: 10,
                                  color: variantTheme.colorScheme.onSurface.withOpacity(0.7),
                                ),
                                textAlign: TextAlign.center,
                                maxLines: 2,
                                overflow: TextOverflow.ellipsis,
                              ),
                            ],
                          ),
                        ),
                      ),
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

  Widget _buildCurrentThemeInfo() {
    return Card(
      margin: const EdgeInsets.all(16),
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                Icon(_selectedVariant.icon),
                const SizedBox(width: 8),
                Text(
                  'Current Theme: ${_selectedVariant.displayName}',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
              ],
            ),
            const SizedBox(height: 8),
            Text(
              _selectedVariant.description,
              style: Theme.of(context).textTheme.bodyMedium,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                _buildColorSwatch('Primary', Theme.of(context).colorScheme.primary),
                const SizedBox(width: 12),
                _buildColorSwatch('Secondary', Theme.of(context).colorScheme.secondary),
                const SizedBox(width: 12),
                _buildColorSwatch('Surface', Theme.of(context).colorScheme.surface),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorSwatch(String label, Color color) {
    return Expanded(
      child: Column(
        children: [
          Container(
            height: 40,
            decoration: BoxDecoration(
              color: color,
              borderRadius: BorderRadius.circular(8),
              boxShadow: [
                BoxShadow(
                  color: color.withOpacity(0.3),
                  blurRadius: 4,
                  offset: const Offset(0, 2),
                ),
              ],
            ),
          ),
          const SizedBox(height: 4),
          Text(
            label,
            style: Theme.of(context).textTheme.bodySmall,
          ),
        ],
      ),
    );
  }

  Widget _buildComponentShowcase() {
    return Card(
      margin: const EdgeInsets.all(16),
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Component Showcase',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'Sample Headline',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            const SizedBox(height: 8),
            Text(
              'This is sample body text that demonstrates how the theme affects typography and readability across different variants.',
              style: Theme.of(context).textTheme.bodyMedium,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: () {},
                    child: const Text('Primary Button'),
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: OutlinedButton(
                    onPressed: () {},
                    child: const Text('Secondary'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            TextButton(
              onPressed: () {},
              child: const Text('Text Button'),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildThemeComparison() {
    return Container(
      margin: const EdgeInsets.all(16),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'All Theme Variants',
            style: Theme.of(context).textTheme.titleLarge,
          ),
          const SizedBox(height: 16),
          GridView.builder(
            shrinkWrap: true,
            physics: const NeverScrollableScrollPhysics(),
            gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 2,
              crossAxisSpacing: 12,
              mainAxisSpacing: 12,
              childAspectRatio: 1.5,
            ),
            itemCount: ThemeVariant.values.length,
            itemBuilder: (context, index) {
              final variant = ThemeVariant.values[index];
              final variantTheme = ThemeVariants.getTheme(
                variant: variant,
                brightness: _isDarkMode ? Brightness.dark : Brightness.light,
              );

              return Container(
                decoration: BoxDecoration(
                  borderRadius: BorderRadius.circular(12),
                  gradient: LinearGradient(
                    begin: Alignment.topLeft,
                    end: Alignment.bottomRight,
                    colors: [
                      variantTheme.colorScheme.primary,
                      variantTheme.colorScheme.secondary,
                    ],
                  ),
                ),
                child: Material(
                  color: Colors.transparent,
                  child: InkWell(
                    borderRadius: BorderRadius.circular(12),
                    onTap: () => _changeTheme(variant),
                    child: Padding(
                      padding: const EdgeInsets.all(16),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Icon(
                            variant.icon,
                            color: Colors.white,
                            size: 28,
                          ),
                          const Spacer(),
                          Text(
                            variant.displayName,
                            style: const TextStyle(
                              color: Colors.white,
                              fontWeight: FontWeight.w600,
                              fontSize: 16,
                            ),
                          ),
                          Text(
                            'Tap to apply',
                            style: TextStyle(
                              color: Colors.white.withOpacity(0.8),
                              fontSize: 12,
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
        ],
      ),
    );
  }
}
```

Multiple theme variants allow users to choose from different visual  
styles while maintaining consistent functionality. Each variant defines  
its own color schemes, typography, and component styling. This approach  
provides flexibility for different user preferences and brand  
requirements within a single application.  
