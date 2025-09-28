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
