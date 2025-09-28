# Flutter Forms and Validation - 25 Comprehensive Examples

This comprehensive guide covers Flutter forms and validation with 25 practical  
examples, from basic form structure to advanced validation techniques. Each  
example demonstrates different aspects of form handling, helping you create  
robust, user-friendly forms with proper validation.

## Basic Form Structure

Creating a simple form with TextFormField and basic layout.  

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
      title: 'Basic Form',
      home: const BasicFormPage(),
    );
  }
}

class BasicFormPage extends StatefulWidget {
  const BasicFormPage({super.key});

  @override
  State<BasicFormPage> createState() => _BasicFormPageState();
}

class _BasicFormPageState extends State<BasicFormPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Form'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            children: [
              TextFormField(
                controller: _nameController,
                decoration: const InputDecoration(
                  labelText: 'Name',
                  border: OutlineInputBorder(),
                ),
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _emailController,
                decoration: const InputDecoration(
                  labelText: 'Email',
                  border: OutlineInputBorder(),
                ),
              ),
              const SizedBox(height: 24),
              ElevatedButton(
                onPressed: () {
                  print('Name: ${_nameController.text}');
                  print('Email: ${_emailController.text}');
                },
                child: const Text('Submit'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

Form widget acts as a container for form fields with a GlobalKey for state  
management. TextFormField provides text input with built-in styling and  
decoration options. TextEditingController manages field values and changes.  

## Form Validation

Adding validation rules to form fields with error messages.  

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
      title: 'Form Validation',
      home: const ValidationFormPage(),
    );
  }
}

class ValidationFormPage extends StatefulWidget {
  const ValidationFormPage({super.key});

  @override
  State<ValidationFormPage> createState() => _ValidationFormPageState();
}

class _ValidationFormPageState extends State<ValidationFormPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Form Validation'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            children: [
              TextFormField(
                controller: _nameController,
                decoration: const InputDecoration(
                  labelText: 'Name *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your name';
                  }
                  if (value.length < 2) {
                    return 'Name must be at least 2 characters';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _emailController,
                decoration: const InputDecoration(
                  labelText: 'Email *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter your email';
                  }
                  if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$')
                      .hasMatch(value)) {
                    return 'Please enter a valid email';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _passwordController,
                obscureText: true,
                decoration: const InputDecoration(
                  labelText: 'Password *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter a password';
                  }
                  if (value.length < 6) {
                    return 'Password must be at least 6 characters';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 24),
              ElevatedButton(
                onPressed: () {
                  if (_formKey.currentState!.validate()) {
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('Form is valid!')),
                    );
                  }
                },
                child: const Text('Submit'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

Validator functions return null for valid input or error messages for invalid  
input. The validate() method triggers all validators and displays error  
messages. Validation occurs when the form is submitted or fields lose focus.  

## Custom Validators

Creating reusable custom validator functions for complex validation rules.  

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
      title: 'Custom Validators',
      home: const CustomValidatorsPage(),
    );
  }
}

class Validators {
  static String? phoneNumber(String? value) {
    if (value == null || value.isEmpty) {
      return 'Please enter a phone number';
    }
    if (!RegExp(r'^\+?[\d\s\-\(\)]+$').hasMatch(value)) {
      return 'Please enter a valid phone number';
    }
    if (value.replaceAll(RegExp(r'[\D]'), '').length < 10) {
      return 'Phone number must be at least 10 digits';
    }
    return null;
  }

  static String? creditCard(String? value) {
    if (value == null || value.isEmpty) {
      return 'Please enter a credit card number';
    }
    String cardNumber = value.replaceAll(RegExp(r'\D'), '');
    if (cardNumber.length != 16) {
      return 'Credit card number must be 16 digits';
    }
    // Luhn algorithm for credit card validation
    int sum = 0;
    bool alternate = false;
    for (int i = cardNumber.length - 1; i >= 0; i--) {
      int digit = int.parse(cardNumber[i]);
      if (alternate) {
        digit *= 2;
        if (digit > 9) digit -= 9;
      }
      sum += digit;
      alternate = !alternate;
    }
    if (sum % 10 != 0) {
      return 'Invalid credit card number';
    }
    return null;
  }

  static String? url(String? value) {
    if (value == null || value.isEmpty) {
      return 'Please enter a URL';
    }
    if (!RegExp(r'^https?:\/\/([\w\-]+\.)+[\w\-]+(\/[\w\-._~:/?#[\]@!$&()*+,;=]*)?$')
        .hasMatch(value)) {
      return 'Please enter a valid URL';
    }
    return null;
  }
}

class CustomValidatorsPage extends StatefulWidget {
  const CustomValidatorsPage({super.key});

  @override
  State<CustomValidatorsPage> createState() => _CustomValidatorsPageState();
}

class _CustomValidatorsPageState extends State<CustomValidatorsPage> {
  final _formKey = GlobalKey<FormState>();
  final _phoneController = TextEditingController();
  final _cardController = TextEditingController();
  final _urlController = TextEditingController();

  @override
  void dispose() {
    _phoneController.dispose();
    _cardController.dispose();
    _urlController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Validators'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            children: [
              TextFormField(
                controller: _phoneController,
                decoration: const InputDecoration(
                  labelText: 'Phone Number',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.phone),
                ),
                validator: Validators.phoneNumber,
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _cardController,
                decoration: const InputDecoration(
                  labelText: 'Credit Card',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.credit_card),
                ),
                validator: Validators.creditCard,
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _urlController,
                decoration: const InputDecoration(
                  labelText: 'Website URL',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.link),
                ),
                validator: Validators.url,
              ),
              const SizedBox(height: 24),
              ElevatedButton(
                onPressed: () {
                  if (_formKey.currentState!.validate()) {
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('All validations passed!')),
                    );
                  }
                },
                child: const Text('Validate'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

Custom validator functions encapsulate complex validation logic and can be  
reused across multiple forms. The Luhn algorithm validates credit card  
numbers mathematically. Regular expressions provide pattern matching for  
phone numbers and URLs with comprehensive format checking.  

## Form State Management

Managing form state with save, reset, and auto-save functionality.  

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
      title: 'Form State Management',
      home: const FormStateManagementPage(),
    );
  }
}

class FormStateManagementPage extends StatefulWidget {
  const FormStateManagementPage({super.key});

  @override
  State<FormStateManagementPage> createState() => _FormStateManagementPageState();
}

class _FormStateManagementPageState extends State<FormStateManagementPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _bioController = TextEditingController();
  
  bool _autoSave = true;

  @override
  void initState() {
    super.initState();
    _loadSavedData();
  }

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _bioController.dispose();
    super.dispose();
  }

  Future<void> _loadSavedData() async {
    final prefs = await SharedPreferences.getInstance();
    setState(() {
      _nameController.text = prefs.getString('form_name') ?? '';
      _emailController.text = prefs.getString('form_email') ?? '';
      _bioController.text = prefs.getString('form_bio') ?? '';
    });
  }

  Future<void> _saveData() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setString('form_name', _nameController.text);
    await prefs.setString('form_email', _emailController.text);
    await prefs.setString('form_bio', _bioController.text);
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Form data saved!')),
      );
    }
  }

  Future<void> _clearData() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.remove('form_name');
    await prefs.remove('form_email');
    await prefs.remove('form_bio');
    
    setState(() {
      _nameController.clear();
      _emailController.clear();
      _bioController.clear();
    });
    
    if (mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Form data cleared!')),
      );
    }
  }

  void _onFieldChanged(String value) {
    if (_autoSave) {
      Future.delayed(const Duration(seconds: 2), () {
        _saveData();
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Form State Management'),
        actions: [
          IconButton(
            icon: Icon(_autoSave ? Icons.auto_awesome : Icons.auto_awesome_outlined),
            onPressed: () {
              setState(() {
                _autoSave = !_autoSave;
              });
              ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(
                  content: Text(
                    _autoSave ? 'Auto-save enabled' : 'Auto-save disabled',
                  ),
                ),
              );
            },
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            children: [
              TextFormField(
                controller: _nameController,
                decoration: const InputDecoration(
                  labelText: 'Name',
                  border: OutlineInputBorder(),
                ),
                onChanged: _onFieldChanged,
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _emailController,
                decoration: const InputDecoration(
                  labelText: 'Email',
                  border: OutlineInputBorder(),
                ),
                onChanged: _onFieldChanged,
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _bioController,
                maxLines: 4,
                decoration: const InputDecoration(
                  labelText: 'Bio',
                  border: OutlineInputBorder(),
                ),
                onChanged: _onFieldChanged,
              ),
              const SizedBox(height: 24),
              Row(
                children: [
                  Expanded(
                    child: OutlinedButton(
                      onPressed: _clearData,
                      child: const Text('Clear'),
                    ),
                  ),
                  const SizedBox(width: 16),
                  Expanded(
                    child: ElevatedButton(
                      onPressed: _saveData,
                      child: const Text('Save'),
                    ),
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
```

SharedPreferences provides persistent storage for form data across app  
sessions. Auto-save functionality uses delayed execution to prevent  
excessive save operations. The state management pattern ensures data  
persistence and user experience continuity.  

## Multi-step Forms

Creating wizard-style forms with step navigation and progress tracking.  

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
      title: 'Multi-step Forms',
      home: const MultiStepFormPage(),
    );
  }
}

class MultiStepFormPage extends StatefulWidget {
  const MultiStepFormPage({super.key});

  @override
  State<MultiStepFormPage> createState() => _MultiStepFormPageState();
}

class _MultiStepFormPageState extends State<MultiStepFormPage> {
  int _currentStep = 0;
  final _formKeys = [
    GlobalKey<FormState>(),
    GlobalKey<FormState>(),
    GlobalKey<FormState>(),
  ];
  
  final _personalData = <String, String>{};
  final _contactData = <String, String>{};
  final _preferencesData = <String, dynamic>{};

  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  bool _newsletter = false;
  String _theme = 'dark';

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Registration Form'),
      ),
      body: Stepper(
        currentStep: _currentStep,
        onStepTapped: (step) {
          setState(() {
            _currentStep = step;
          });
        },
        onStepContinue: () {
          if (_currentStep < 2) {
            if (_formKeys[_currentStep].currentState!.validate()) {
              _saveCurrentStep();
              setState(() {
                _currentStep++;
              });
            }
          } else {
            _submitForm();
          }
        },
        onStepCancel: () {
          if (_currentStep > 0) {
            setState(() {
              _currentStep--;
            });
          }
        },
        steps: [
          Step(
            title: const Text('Personal Info'),
            content: Form(
              key: _formKeys[0],
              child: Column(
                children: [
                  TextFormField(
                    controller: _nameController,
                    decoration: const InputDecoration(
                      labelText: 'Full Name',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please enter your name';
                      }
                      return null;
                    },
                  ),
                  const SizedBox(height: 16),
                  TextFormField(
                    decoration: const InputDecoration(
                      labelText: 'Date of Birth',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please enter your date of birth';
                      }
                      return null;
                    },
                  ),
                ],
              ),
            ),
            isActive: _currentStep >= 0,
            state: _currentStep > 0 ? StepState.complete : StepState.indexed,
          ),
          Step(
            title: const Text('Contact Info'),
            content: Form(
              key: _formKeys[1],
              child: Column(
                children: [
                  TextFormField(
                    controller: _emailController,
                    decoration: const InputDecoration(
                      labelText: 'Email',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please enter your email';
                      }
                      if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$')
                          .hasMatch(value)) {
                        return 'Please enter a valid email';
                      }
                      return null;
                    },
                  ),
                  const SizedBox(height: 16),
                  TextFormField(
                    controller: _phoneController,
                    decoration: const InputDecoration(
                      labelText: 'Phone Number',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please enter your phone number';
                      }
                      return null;
                    },
                  ),
                ],
              ),
            ),
            isActive: _currentStep >= 1,
            state: _currentStep > 1 ? StepState.complete : 
                   _currentStep == 1 ? StepState.indexed : StepState.disabled,
          ),
          Step(
            title: const Text('Preferences'),
            content: Form(
              key: _formKeys[2],
              child: Column(
                children: [
                  SwitchListTile(
                    title: const Text('Newsletter Subscription'),
                    value: _newsletter,
                    onChanged: (value) {
                      setState(() {
                        _newsletter = value;
                      });
                    },
                  ),
                  ListTile(
                    title: const Text('Theme Preference'),
                    trailing: DropdownButton<String>(
                      value: _theme,
                      items: const [
                        DropdownMenuItem(value: 'light', child: Text('Light')),
                        DropdownMenuItem(value: 'dark', child: Text('Dark')),
                        DropdownMenuItem(value: 'auto', child: Text('Auto')),
                      ],
                      onChanged: (value) {
                        setState(() {
                          _theme = value!;
                        });
                      },
                    ),
                  ),
                ],
              ),
            ),
            isActive: _currentStep >= 2,
            state: _currentStep == 2 ? StepState.indexed : StepState.disabled,
          ),
        ],
      ),
    );
  }

  void _saveCurrentStep() {
    switch (_currentStep) {
      case 0:
        _personalData['name'] = _nameController.text;
        break;
      case 1:
        _contactData['email'] = _emailController.text;
        _contactData['phone'] = _phoneController.text;
        break;
      case 2:
        _preferencesData['newsletter'] = _newsletter;
        _preferencesData['theme'] = _theme;
        break;
    }
  }

  void _submitForm() {
    _saveCurrentStep();
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(content: Text('Registration completed successfully!')),
    );
  }
}
```

Stepper widget provides built-in navigation and progress tracking for  
multi-step forms. Each step contains its own form with validation.  
Step states (complete, indexed, disabled) provide visual feedback to users  
about their progress through the form workflow.  

## Dynamic Forms

Adding and removing form fields dynamically based on user interaction.  

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
      title: 'Dynamic Forms',
      home: const DynamicFormPage(),
    );
  }
}

class DynamicFormPage extends StatefulWidget {
  const DynamicFormPage({super.key});

  @override
  State<DynamicFormPage> createState() => _DynamicFormPageState();
}

class _DynamicFormPageState extends State<DynamicFormPage> {
  final _formKey = GlobalKey<FormState>();
  final List<TextEditingController> _skillControllers = [
    TextEditingController()
  ];
  final List<TextEditingController> _experienceControllers = [
    TextEditingController()
  ];

  @override
  void dispose() {
    for (var controller in _skillControllers) {
      controller.dispose();
    }
    for (var controller in _experienceControllers) {
      controller.dispose();
    }
    super.dispose();
  }

  void _addSkill() {
    setState(() {
      _skillControllers.add(TextEditingController());
    });
  }

  void _removeSkill(int index) {
    if (_skillControllers.length > 1) {
      setState(() {
        _skillControllers[index].dispose();
        _skillControllers.removeAt(index);
      });
    }
  }

  void _addExperience() {
    setState(() {
      _experienceControllers.add(TextEditingController());
    });
  }

  void _removeExperience(int index) {
    if (_experienceControllers.length > 1) {
      setState(() {
        _experienceControllers[index].dispose();
        _experienceControllers.removeAt(index);
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Dynamic Form'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                const Text(
                  'Skills',
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                ..._buildSkillFields(),
                TextButton.icon(
                  onPressed: _addSkill,
                  icon: const Icon(Icons.add),
                  label: const Text('Add Skill'),
                ),
                const SizedBox(height: 24),
                const Text(
                  'Work Experience',
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                ..._buildExperienceFields(),
                TextButton.icon(
                  onPressed: _addExperience,
                  icon: const Icon(Icons.add),
                  label: const Text('Add Experience'),
                ),
                const SizedBox(height: 24),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: () {
                      if (_formKey.currentState!.validate()) {
                        _showResults();
                      }
                    },
                    child: const Text('Submit'),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  List<Widget> _buildSkillFields() {
    return _skillControllers.asMap().entries.map((entry) {
      int index = entry.key;
      TextEditingController controller = entry.value;
      
      return Padding(
        padding: const EdgeInsets.only(bottom: 8.0),
        child: Row(
          children: [
            Expanded(
              child: TextFormField(
                controller: controller,
                decoration: InputDecoration(
                  labelText: 'Skill ${index + 1}',
                  border: const OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter a skill';
                  }
                  return null;
                },
              ),
            ),
            if (_skillControllers.length > 1)
              IconButton(
                onPressed: () => _removeSkill(index),
                icon: const Icon(Icons.remove_circle, color: Colors.red),
              ),
          ],
        ),
      );
    }).toList();
  }

  List<Widget> _buildExperienceFields() {
    return _experienceControllers.asMap().entries.map((entry) {
      int index = entry.key;
      TextEditingController controller = entry.value;
      
      return Padding(
        padding: const EdgeInsets.only(bottom: 8.0),
        child: Row(
          children: [
            Expanded(
              child: TextFormField(
                controller: controller,
                maxLines: 3,
                decoration: InputDecoration(
                  labelText: 'Experience ${index + 1}',
                  border: const OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter work experience';
                  }
                  return null;
                },
              ),
            ),
            if (_experienceControllers.length > 1)
              IconButton(
                onPressed: () => _removeExperience(index),
                icon: const Icon(Icons.remove_circle, color: Colors.red),
              ),
          ],
        ),
      );
    }).toList();
  }

  void _showResults() {
    List<String> skills = _skillControllers
        .map((controller) => controller.text)
        .where((text) => text.isNotEmpty)
        .toList();
    
    List<String> experiences = _experienceControllers
        .map((controller) => controller.text)
        .where((text) => text.isNotEmpty)
        .toList();

    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Form Results'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text('Skills:', style: TextStyle(fontWeight: FontWeight.bold)),
            ...skills.map((skill) => Text('• $skill')),
            const SizedBox(height: 16),
            const Text('Experience:', style: TextStyle(fontWeight: FontWeight.bold)),
            ...experiences.map((exp) => Text('• $exp')),
          ],
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

Dynamic forms use lists of controllers to manage varying numbers of fields.  
The asMap().entries pattern provides both index and value for building  
widgets with removal functionality. Proper disposal of controllers prevents  
memory leaks when fields are dynamically removed.  

## File Upload Forms

Handling file selection and upload with image preview functionality.  

```dart
import 'package:flutter/material.dart';
import 'package:file_picker/file_picker.dart';
import 'package:image_picker/image_picker.dart';
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
      title: 'File Upload Forms',
      home: const FileUploadFormPage(),
    );
  }
}

class FileUploadFormPage extends StatefulWidget {
  const FileUploadFormPage({super.key});

  @override
  State<FileUploadFormPage> createState() => _FileUploadFormPageState();
}

class _FileUploadFormPageState extends State<FileUploadFormPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _descriptionController = TextEditingController();
  
  File? _selectedImage;
  List<File> _selectedDocuments = [];
  final ImagePicker _imagePicker = ImagePicker();

  @override
  void dispose() {
    _nameController.dispose();
    _descriptionController.dispose();
    super.dispose();
  }

  Future<void> _pickImage() async {
    final XFile? image = await _imagePicker.pickImage(
      source: ImageSource.gallery,
      maxWidth: 800,
      maxHeight: 600,
      imageQuality: 80,
    );
    
    if (image != null) {
      setState(() {
        _selectedImage = File(image.path);
      });
    }
  }

  Future<void> _pickDocuments() async {
    FilePickerResult? result = await FilePicker.platform.pickFiles(
      type: FileType.custom,
      allowedExtensions: ['pdf', 'doc', 'docx', 'txt'],
      allowMultiple: true,
    );

    if (result != null && result.files.isNotEmpty) {
      setState(() {
        _selectedDocuments = result.paths
            .where((path) => path != null)
            .map((path) => File(path!))
            .toList();
      });
    }
  }

  void _removeDocument(int index) {
    setState(() {
      _selectedDocuments.removeAt(index);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('File Upload Form'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                TextFormField(
                  controller: _nameController,
                  decoration: const InputDecoration(
                    labelText: 'Project Name',
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'Please enter project name';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _descriptionController,
                  maxLines: 3,
                  decoration: const InputDecoration(
                    labelText: 'Description',
                    border: OutlineInputBorder(),
                  ),
                ),
                const SizedBox(height: 24),
                const Text(
                  'Project Image',
                  style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                if (_selectedImage != null) ...[
                  Container(
                    height: 200,
                    width: double.infinity,
                    decoration: BoxDecoration(
                      border: Border.all(color: Colors.grey),
                      borderRadius: BorderRadius.circular(8),
                    ),
                    child: ClipRRect(
                      borderRadius: BorderRadius.circular(8),
                      child: Image.file(
                        _selectedImage!,
                        fit: BoxFit.cover,
                      ),
                    ),
                  ),
                  const SizedBox(height: 8),
                ],
                Row(
                  children: [
                    ElevatedButton.icon(
                      onPressed: _pickImage,
                      icon: const Icon(Icons.image),
                      label: Text(_selectedImage != null ? 'Change Image' : 'Select Image'),
                    ),
                    if (_selectedImage != null) ...[
                      const SizedBox(width: 8),
                      TextButton.icon(
                        onPressed: () {
                          setState(() {
                            _selectedImage = null;
                          });
                        },
                        icon: const Icon(Icons.delete, color: Colors.red),
                        label: const Text('Remove'),
                      ),
                    ],
                  ],
                ),
                const SizedBox(height: 24),
                const Text(
                  'Documents',
                  style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                if (_selectedDocuments.isNotEmpty) ...[
                  ...List.generate(_selectedDocuments.length, (index) {
                    final doc = _selectedDocuments[index];
                    final fileName = doc.path.split('/').last;
                    return Card(
                      child: ListTile(
                        leading: const Icon(Icons.description),
                        title: Text(fileName),
                        subtitle: Text('${doc.lengthSync()} bytes'),
                        trailing: IconButton(
                          icon: const Icon(Icons.delete, color: Colors.red),
                          onPressed: () => _removeDocument(index),
                        ),
                      ),
                    );
                  }),
                  const SizedBox(height: 8),
                ],
                ElevatedButton.icon(
                  onPressed: _pickDocuments,
                  icon: const Icon(Icons.attach_file),
                  label: const Text('Select Documents'),
                ),
                const SizedBox(height: 32),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: () {
                      if (_formKey.currentState!.validate()) {
                        _submitForm();
                      }
                    },
                    child: const Text('Submit'),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  void _submitForm() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Form Submitted'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Project: ${_nameController.text}'),
            if (_selectedImage != null)
              Text('Image: ${_selectedImage!.path.split('/').last}'),
            Text('Documents: ${_selectedDocuments.length} files'),
          ],
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

File upload forms combine text input with file selection capabilities.  
ImagePicker handles camera and gallery access with compression options.  
FilePicker supports multiple file types and formats with extension filtering.  
Preview functionality improves user experience by showing selected content.  

## Date and Time Pickers

Implementing date and time selection with calendar and time picker widgets.  

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
      title: 'Date Time Pickers',
      home: const DateTimePickerPage(),
    );
  }
}

class DateTimePickerPage extends StatefulWidget {
  const DateTimePickerPage({super.key});

  @override
  State<DateTimePickerPage> createState() => _DateTimePickerPageState();
}

class _DateTimePickerPageState extends State<DateTimePickerPage> {
  final _formKey = GlobalKey<FormState>();
  final _eventNameController = TextEditingController();
  
  DateTime? _selectedDate;
  TimeOfDay? _selectedTime;
  DateTimeRange? _selectedDateRange;

  @override
  void dispose() {
    _eventNameController.dispose();
    super.dispose();
  }

  Future<void> _selectDate() async {
    final DateTime? picked = await showDatePicker(
      context: context,
      initialDate: _selectedDate ?? DateTime.now(),
      firstDate: DateTime.now(),
      lastDate: DateTime.now().add(const Duration(days: 365)),
    );
    if (picked != null && picked != _selectedDate) {
      setState(() {
        _selectedDate = picked;
      });
    }
  }

  Future<void> _selectTime() async {
    final TimeOfDay? picked = await showTimePicker(
      context: context,
      initialTime: _selectedTime ?? TimeOfDay.now(),
    );
    if (picked != null && picked != _selectedTime) {
      setState(() {
        _selectedTime = picked;
      });
    }
  }

  Future<void> _selectDateRange() async {
    final DateTimeRange? picked = await showDateRangePicker(
      context: context,
      firstDate: DateTime.now(),
      lastDate: DateTime.now().add(const Duration(days: 365)),
      initialDateRange: _selectedDateRange,
    );
    if (picked != null && picked != _selectedDateRange) {
      setState(() {
        _selectedDateRange = picked;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Event Scheduler'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            children: [
              TextFormField(
                controller: _eventNameController,
                decoration: const InputDecoration(
                  labelText: 'Event Name',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter event name';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              Card(
                child: ListTile(
                  leading: const Icon(Icons.calendar_today),
                  title: Text(_selectedDate != null 
                      ? 'Date: ${_selectedDate!.day}/${_selectedDate!.month}/${_selectedDate!.year}'
                      : 'Select Date'),
                  onTap: _selectDate,
                ),
              ),
              const SizedBox(height: 8),
              Card(
                child: ListTile(
                  leading: const Icon(Icons.access_time),
                  title: Text(_selectedTime != null 
                      ? 'Time: ${_selectedTime!.format(context)}'
                      : 'Select Time'),
                  onTap: _selectTime,
                ),
              ),
              const SizedBox(height: 8),
              Card(
                child: ListTile(
                  leading: const Icon(Icons.date_range),
                  title: Text(_selectedDateRange != null 
                      ? 'Duration: ${_selectedDateRange!.start.day}/${_selectedDateRange!.start.month} - ${_selectedDateRange!.end.day}/${_selectedDateRange!.end.month}'
                      : 'Select Date Range'),
                  onTap: _selectDateRange,
                ),
              ),
              const Spacer(),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: () {
                    if (_formKey.currentState!.validate()) {
                      if (_selectedDate == null) {
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(content: Text('Please select a date')),
                        );
                        return;
                      }
                      if (_selectedTime == null) {
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(content: Text('Please select a time')),
                        );
                        return;
                      }
                      
                      showDialog(
                        context: context,
                        builder: (context) => AlertDialog(
                          title: const Text('Event Scheduled'),
                          content: Text(
                            'Event: ${_eventNameController.text}\n'
                            'Date: ${_selectedDate!.day}/${_selectedDate!.month}/${_selectedDate!.year}\n'
                            'Time: ${_selectedTime!.format(context)}',
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
                  },
                  child: const Text('Schedule Event'),
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

Date and time pickers provide native platform-specific interfaces for  
temporal data selection. DateTimeRange picker enables duration selection  
for events and bookings. Validation ensures required temporal data is  
collected before form submission.  

## Dropdown and Selection Widgets

Implementing various selection widgets including dropdowns, radio buttons,  
and checkboxes for user choice collection.  

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
      title: 'Selection Widgets',
      home: const SelectionWidgetsPage(),
    );
  }
}

enum PaymentMethod { creditCard, debitCard, paypal, bankTransfer }

class SelectionWidgetsPage extends StatefulWidget {
  const SelectionWidgetsPage({super.key});

  @override
  State<SelectionWidgetsPage> createState() => _SelectionWidgetsPageState();
}

class _SelectionWidgetsPageState extends State<SelectionWidgetsPage> {
  final _formKey = GlobalKey<FormState>();
  
  String? _selectedCountry;
  PaymentMethod? _selectedPaymentMethod = PaymentMethod.creditCard;
  final Set<String> _selectedInterests = {};
  bool _agreeToTerms = false;
  double _experienceYears = 1;

  final List<String> _countries = [
    'United States', 'Canada', 'United Kingdom', 'Germany', 'France', 
    'Spain', 'Italy', 'Japan', 'Australia', 'Brazil'
  ];

  final List<String> _interests = [
    'Technology', 'Sports', 'Music', 'Travel', 'Food', 'Photography', 
    'Reading', 'Gaming', 'Art', 'Fashion'
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Selection Widgets'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                const Text(
                  'Country',
                  style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                DropdownButtonFormField<String>(
                  value: _selectedCountry,
                  decoration: const InputDecoration(
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.public),
                  ),
                  hint: const Text('Select your country'),
                  items: _countries.map((country) {
                    return DropdownMenuItem(
                      value: country,
                      child: Text(country),
                    );
                  }).toList(),
                  onChanged: (value) {
                    setState(() {
                      _selectedCountry = value;
                    });
                  },
                  validator: (value) {
                    if (value == null) {
                      return 'Please select your country';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 24),
                const Text(
                  'Payment Method',
                  style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                Card(
                  child: Column(
                    children: [
                      RadioListTile<PaymentMethod>(
                        title: const Text('Credit Card'),
                        subtitle: const Text('Visa, MasterCard, Amex'),
                        value: PaymentMethod.creditCard,
                        groupValue: _selectedPaymentMethod,
                        onChanged: (value) {
                          setState(() {
                            _selectedPaymentMethod = value;
                          });
                        },
                      ),
                      RadioListTile<PaymentMethod>(
                        title: const Text('Debit Card'),
                        subtitle: const Text('Direct bank payment'),
                        value: PaymentMethod.debitCard,
                        groupValue: _selectedPaymentMethod,
                        onChanged: (value) {
                          setState(() {
                            _selectedPaymentMethod = value;
                          });
                        },
                      ),
                      RadioListTile<PaymentMethod>(
                        title: const Text('PayPal'),
                        subtitle: const Text('Secure online payment'),
                        value: PaymentMethod.paypal,
                        groupValue: _selectedPaymentMethod,
                        onChanged: (value) {
                          setState(() {
                            _selectedPaymentMethod = value;
                          });
                        },
                      ),
                      RadioListTile<PaymentMethod>(
                        title: const Text('Bank Transfer'),
                        subtitle: const Text('Direct wire transfer'),
                        value: PaymentMethod.bankTransfer,
                        groupValue: _selectedPaymentMethod,
                        onChanged: (value) {
                          setState(() {
                            _selectedPaymentMethod = value;
                          });
                        },
                      ),
                    ],
                  ),
                ),
                const SizedBox(height: 24),
                const Text(
                  'Interests',
                  style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                Wrap(
                  spacing: 8,
                  children: _interests.map((interest) {
                    return FilterChip(
                      label: Text(interest),
                      selected: _selectedInterests.contains(interest),
                      onSelected: (selected) {
                        setState(() {
                          if (selected) {
                            _selectedInterests.add(interest);
                          } else {
                            _selectedInterests.remove(interest);
                          }
                        });
                      },
                    );
                  }).toList(),
                ),
                const SizedBox(height: 24),
                const Text(
                  'Years of Experience',
                  style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      children: [
                        Text(
                          '${_experienceYears.round()} years',
                          style: const TextStyle(fontSize: 18),
                        ),
                        Slider(
                          value: _experienceYears,
                          min: 0,
                          max: 20,
                          divisions: 20,
                          label: '${_experienceYears.round()} years',
                          onChanged: (value) {
                            setState(() {
                              _experienceYears = value;
                            });
                          },
                        ),
                      ],
                    ),
                  ),
                ),
                const SizedBox(height: 16),
                CheckboxListTile(
                  title: const Text('I agree to the Terms and Conditions'),
                  value: _agreeToTerms,
                  onChanged: (value) {
                    setState(() {
                      _agreeToTerms = value ?? false;
                    });
                  },
                  controlAffinity: ListTileControlAffinity.leading,
                ),
                const SizedBox(height: 24),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: () {
                      if (_formKey.currentState!.validate()) {
                        if (!_agreeToTerms) {
                          ScaffoldMessenger.of(context).showSnackBar(
                            const SnackBar(
                              content: Text('Please agree to the terms'),
                            ),
                          );
                          return;
                        }
                        _showResults();
                      }
                    },
                    child: const Text('Submit'),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  void _showResults() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Selection Results'),
        content: SingleChildScrollView(
          child: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text('Country: ${_selectedCountry ?? 'Not selected'}'),
              Text('Payment: ${_selectedPaymentMethod.toString().split('.').last}'),
              Text('Interests: ${_selectedInterests.join(', ')}'),
              Text('Experience: ${_experienceYears.round()} years'),
              Text('Terms Agreed: ${_agreeToTerms ? 'Yes' : 'No'}'),
            ],
          ),
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

Selection widgets provide various input methods for user choices.  
DropdownButtonFormField integrates with form validation. RadioListTile  
enables single selection from multiple options. FilterChip allows  
multiple selections with visual feedback and flexible layout.  

## Checkbox and Radio Groups

Creating grouped checkbox and radio button controls for related options.  

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
      title: 'Checkbox Radio Groups',
      home: const CheckboxRadioGroupsPage(),
    );
  }
}

enum NotificationFrequency { never, daily, weekly, monthly }
enum Priority { low, medium, high, urgent }

class CheckboxRadioGroupsPage extends StatefulWidget {
  const CheckboxRadioGroupsPage({super.key});

  @override
  State<CheckboxRadioGroupsPage> createState() => _CheckboxRadioGroupsPageState();
}

class _CheckboxRadioGroupsPageState extends State<CheckboxRadioGroupsPage> {
  final _formKey = GlobalKey<FormState>();
  
  final Map<String, bool> _permissions = {
    'Camera': false,
    'Microphone': false,
    'Location': false,
    'Notifications': false,
    'Contacts': false,
    'Storage': false,
  };
  
  NotificationFrequency _notificationFrequency = NotificationFrequency.weekly;
  Priority _selectedPriority = Priority.medium;
  bool _selectAll = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Permissions & Settings'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                const Text(
                  'App Permissions',
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                Card(
                  child: Column(
                    children: [
                      CheckboxListTile(
                        title: const Text('Select All'),
                        value: _selectAll,
                        onChanged: (value) {
                          setState(() {
                            _selectAll = value ?? false;
                            _permissions.updateAll((key, val) => _selectAll);
                          });
                        },
                        controlAffinity: ListTileControlAffinity.leading,
                        tileColor: Colors.grey[800],
                      ),
                      const Divider(),
                      ..._permissions.entries.map((entry) {
                        return CheckboxListTile(
                          title: Text(entry.key),
                          subtitle: Text(_getPermissionDescription(entry.key)),
                          value: entry.value,
                          onChanged: (value) {
                            setState(() {
                              _permissions[entry.key] = value ?? false;
                              _updateSelectAllState();
                            });
                          },
                          controlAffinity: ListTileControlAffinity.leading,
                          secondary: Icon(_getPermissionIcon(entry.key)),
                        );
                      }),
                    ],
                  ),
                ),
                const SizedBox(height: 24),
                const Text(
                  'Notification Frequency',
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                Card(
                  child: Column(
                    children: NotificationFrequency.values.map((frequency) {
                      return RadioListTile<NotificationFrequency>(
                        title: Text(_getFrequencyTitle(frequency)),
                        subtitle: Text(_getFrequencyDescription(frequency)),
                        value: frequency,
                        groupValue: _notificationFrequency,
                        onChanged: (value) {
                          setState(() {
                            _notificationFrequency = value!;
                          });
                        },
                      );
                    }).toList(),
                  ),
                ),
                const SizedBox(height: 24),
                const Text(
                  'Task Priority',
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                Card(
                  child: Column(
                    children: Priority.values.map((priority) {
                      return RadioListTile<Priority>(
                        title: Row(
                          children: [
                            Icon(
                              _getPriorityIcon(priority),
                              color: _getPriorityColor(priority),
                            ),
                            const SizedBox(width: 8),
                            Text(_getPriorityTitle(priority)),
                          ],
                        ),
                        subtitle: Text(_getPriorityDescription(priority)),
                        value: priority,
                        groupValue: _selectedPriority,
                        onChanged: (value) {
                          setState(() {
                            _selectedPriority = value!;
                          });
                        },
                      );
                    }).toList(),
                  ),
                ),
                const SizedBox(height: 32),
                Row(
                  children: [
                    Expanded(
                      child: OutlinedButton(
                        onPressed: _resetForm,
                        child: const Text('Reset'),
                      ),
                    ),
                    const SizedBox(width: 16),
                    Expanded(
                      child: ElevatedButton(
                        onPressed: _submitForm,
                        child: const Text('Save Settings'),
                      ),
                    ),
                  ],
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  String _getPermissionDescription(String permission) {
    switch (permission) {
      case 'Camera': return 'Take photos and record videos';
      case 'Microphone': return 'Record audio';
      case 'Location': return 'Access your location';
      case 'Notifications': return 'Send push notifications';
      case 'Contacts': return 'Access your contacts';
      case 'Storage': return 'Read and write files';
      default: return '';
    }
  }

  IconData _getPermissionIcon(String permission) {
    switch (permission) {
      case 'Camera': return Icons.camera_alt;
      case 'Microphone': return Icons.mic;
      case 'Location': return Icons.location_on;
      case 'Notifications': return Icons.notifications;
      case 'Contacts': return Icons.contacts;
      case 'Storage': return Icons.storage;
      default: return Icons.help;
    }
  }

  void _updateSelectAllState() {
    bool allSelected = _permissions.values.every((value) => value);
    bool noneSelected = _permissions.values.every((value) => !value);
    
    setState(() {
      if (allSelected) {
        _selectAll = true;
      } else if (noneSelected) {
        _selectAll = false;
      }
    });
  }

  String _getFrequencyTitle(NotificationFrequency frequency) {
    switch (frequency) {
      case NotificationFrequency.never: return 'Never';
      case NotificationFrequency.daily: return 'Daily';
      case NotificationFrequency.weekly: return 'Weekly';
      case NotificationFrequency.monthly: return 'Monthly';
    }
  }

  String _getFrequencyDescription(NotificationFrequency frequency) {
    switch (frequency) {
      case NotificationFrequency.never: return 'No notifications';
      case NotificationFrequency.daily: return 'Once per day';
      case NotificationFrequency.weekly: return 'Once per week';
      case NotificationFrequency.monthly: return 'Once per month';
    }
  }

  String _getPriorityTitle(Priority priority) {
    switch (priority) {
      case Priority.low: return 'Low';
      case Priority.medium: return 'Medium';
      case Priority.high: return 'High';
      case Priority.urgent: return 'Urgent';
    }
  }

  String _getPriorityDescription(Priority priority) {
    switch (priority) {
      case Priority.low: return 'Can be done later';
      case Priority.medium: return 'Normal priority';
      case Priority.high: return 'Important task';
      case Priority.urgent: return 'Needs immediate attention';
    }
  }

  IconData _getPriorityIcon(Priority priority) {
    switch (priority) {
      case Priority.low: return Icons.low_priority;
      case Priority.medium: return Icons.remove;
      case Priority.high: return Icons.keyboard_arrow_up;
      case Priority.urgent: return Icons.priority_high;
    }
  }

  Color _getPriorityColor(Priority priority) {
    switch (priority) {
      case Priority.low: return Colors.green;
      case Priority.medium: return Colors.orange;
      case Priority.high: return Colors.red;
      case Priority.urgent: return Colors.red[900]!;
    }
  }

  void _resetForm() {
    setState(() {
      _permissions.updateAll((key, value) => false);
      _selectAll = false;
      _notificationFrequency = NotificationFrequency.weekly;
      _selectedPriority = Priority.medium;
    });
    
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(content: Text('Form reset to defaults')),
    );
  }

  void _submitForm() {
    List<String> enabledPermissions = _permissions.entries
        .where((entry) => entry.value)
        .map((entry) => entry.key)
        .toList();
    
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Settings Saved'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Permissions: ${enabledPermissions.join(', ')}'),
            Text('Notifications: ${_getFrequencyTitle(_notificationFrequency)}'),
            Text('Priority: ${_getPriorityTitle(_selectedPriority)}'),
          ],
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

Checkbox groups enable multiple selections with select-all functionality.  
Radio button groups ensure single selection from related options. Enum  
types provide type-safe option handling with descriptive metadata and  
visual indicators for better user experience.  

## Slider and Range Inputs

Using sliders and range sliders for numeric value selection with constraints.  

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
      title: 'Slider Range Inputs',
      home: const SliderRangeInputsPage(),
    );
  }
}

class SliderRangeInputsPage extends StatefulWidget {
  const SliderRangeInputsPage({super.key});

  @override
  State<SliderRangeInputsPage> createState() => _SliderRangeInputsPageState();
}

class _SliderRangeInputsPageState extends State<SliderRangeInputsPage> {
  final _formKey = GlobalKey<FormState>();
  
  double _age = 25;
  double _salary = 50000;
  RangeValues _priceRange = const RangeValues(100, 500);
  double _rating = 3;
  double _volume = 0.5;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Product Configuration'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                _buildAgeSlider(),
                const SizedBox(height: 32),
                _buildSalarySlider(),
                const SizedBox(height: 32),
                _buildPriceRangeSlider(),
                const SizedBox(height: 32),
                _buildRatingSlider(),
                const SizedBox(height: 32),
                _buildVolumeSlider(),
                const SizedBox(height: 48),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: _submitForm,
                    child: const Text('Save Configuration'),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildAgeSlider() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Age: ${_age.round()} years',
              style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Slider(
              value: _age,
              min: 18,
              max: 100,
              divisions: 82,
              label: '${_age.round()} years',
              onChanged: (value) {
                setState(() {
                  _age = value;
                });
              },
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text('18', style: Theme.of(context).textTheme.bodySmall),
                Text('100', style: Theme.of(context).textTheme.bodySmall),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSalarySlider() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Expected Salary: \$${_salary.round().toString().replaceAllMapped(
                RegExp(r'(\d{1,3})(?=(\d{3})+(?!\d))'), 
                (Match m) => '${m[1]},'
              )}',
              style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Slider(
              value: _salary,
              min: 20000,
              max: 200000,
              divisions: 180,
              label: '\$${_salary.round()}',
              onChanged: (value) {
                setState(() {
                  _salary = value;
                });
              },
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text('\$20K', style: Theme.of(context).textTheme.bodySmall),
                Text('\$200K', style: Theme.of(context).textTheme.bodySmall),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildPriceRangeSlider() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Price Range: \$${_priceRange.start.round()} - \$${_priceRange.end.round()}',
              style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            RangeSlider(
              values: _priceRange,
              min: 0,
              max: 1000,
              divisions: 100,
              labels: RangeLabels(
                '\$${_priceRange.start.round()}',
                '\$${_priceRange.end.round()}',
              ),
              onChanged: (values) {
                setState(() {
                  _priceRange = values;
                });
              },
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text('\$0', style: Theme.of(context).textTheme.bodySmall),
                Text('\$1000', style: Theme.of(context).textTheme.bodySmall),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildRatingSlider() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                Text(
                  'Rating: ${_rating.round()}/5',
                  style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                ),
                const SizedBox(width: 8),
                Row(
                  children: List.generate(5, (index) {
                    return Icon(
                      index < _rating ? Icons.star : Icons.star_border,
                      color: Colors.amber,
                      size: 20,
                    );
                  }),
                ),
              ],
            ),
            const SizedBox(height: 8),
            Slider(
              value: _rating,
              min: 1,
              max: 5,
              divisions: 4,
              label: '${_rating.round()} stars',
              activeColor: Colors.amber,
              onChanged: (value) {
                setState(() {
                  _rating = value;
                });
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildVolumeSlider() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const Icon(Icons.volume_down),
                Expanded(
                  child: Slider(
                    value: _volume,
                    min: 0,
                    max: 1,
                    onChanged: (value) {
                      setState(() {
                        _volume = value;
                      });
                    },
                  ),
                ),
                const Icon(Icons.volume_up),
              ],
            ),
            Center(
              child: Text(
                'Volume: ${(_volume * 100).round()}%',
                style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
              ),
            ),
          ],
        ),
      ),
    );
  }

  void _submitForm() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Configuration Saved'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Age: ${_age.round()} years'),
            Text('Salary: \$${_salary.round()}'),
            Text('Price Range: \$${_priceRange.start.round()} - \$${_priceRange.end.round()}'),
            Text('Rating: ${_rating.round()}/5 stars'),
            Text('Volume: ${(_volume * 100).round()}%'),
          ],
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

Slider widgets provide intuitive numeric input with visual feedback and  
constraints. RangeSlider enables selection of value ranges for filtering  
and configuration. Custom styling and labels enhance user experience with  
contextual information and visual indicators.  

## Text Input Variations

Implementing different text input types with formatting and masking.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Text Input Variations',
      home: const TextInputVariationsPage(),
    );
  }
}

class PhoneInputFormatter extends TextInputFormatter {
  @override
  TextEditingValue formatEditUpdate(
    TextEditingValue oldValue,
    TextEditingValue newValue,
  ) {
    String digitsOnly = newValue.text.replaceAll(RegExp(r'[^0-9]'), '');
    
    if (digitsOnly.length <= 3) {
      return TextEditingValue(
        text: digitsOnly,
        selection: TextSelection.collapsed(offset: digitsOnly.length),
      );
    } else if (digitsOnly.length <= 6) {
      String formatted = '(${digitsOnly.substring(0, 3)}) ${digitsOnly.substring(3)}';
      return TextEditingValue(
        text: formatted,
        selection: TextSelection.collapsed(offset: formatted.length),
      );
    } else {
      String formatted = '(${digitsOnly.substring(0, 3)}) ${digitsOnly.substring(3, 6)}-${digitsOnly.substring(6, digitsOnly.length > 10 ? 10 : digitsOnly.length)}';
      return TextEditingValue(
        text: formatted,
        selection: TextSelection.collapsed(offset: formatted.length),
      );
    }
  }
}

class CreditCardInputFormatter extends TextInputFormatter {
  @override
  TextEditingValue formatEditUpdate(
    TextEditingValue oldValue,
    TextEditingValue newValue,
  ) {
    String digitsOnly = newValue.text.replaceAll(RegExp(r'[^0-9]'), '');
    
    if (digitsOnly.length > 16) {
      digitsOnly = digitsOnly.substring(0, 16);
    }
    
    String formatted = '';
    for (int i = 0; i < digitsOnly.length; i++) {
      if (i > 0 && i % 4 == 0) {
        formatted += ' ';
      }
      formatted += digitsOnly[i];
    }
    
    return TextEditingValue(
      text: formatted,
      selection: TextSelection.collapsed(offset: formatted.length),
    );
  }
}

class TextInputVariationsPage extends StatefulWidget {
  const TextInputVariationsPage({super.key});

  @override
  State<TextInputVariationsPage> createState() => _TextInputVariationsPageState();
}

class _TextInputVariationsPageState extends State<TextInputVariationsPage> {
  final _formKey = GlobalKey<FormState>();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  final _creditCardController = TextEditingController();
  final _bioController = TextEditingController();
  final _searchController = TextEditingController();
  final _passwordController = TextEditingController();
  
  bool _obscurePassword = true;
  int _bioCharacterCount = 0;
  static const int _bioMaxLength = 500;

  @override
  void initState() {
    super.initState();
    _bioController.addListener(() {
      setState(() {
        _bioCharacterCount = _bioController.text.length;
      });
    });
  }

  @override
  void dispose() {
    _emailController.dispose();
    _phoneController.dispose();
    _creditCardController.dispose();
    _bioController.dispose();
    _searchController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Text Input Variations'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              children: [
                _buildEmailField(),
                const SizedBox(height: 16),
                _buildPhoneField(),
                const SizedBox(height: 16),
                _buildCreditCardField(),
                const SizedBox(height: 16),
                _buildPasswordField(),
                const SizedBox(height: 16),
                _buildSearchField(),
                const SizedBox(height: 16),
                _buildBioField(),
                const SizedBox(height: 32),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: () {
                      if (_formKey.currentState!.validate()) {
                        _submitForm();
                      }
                    },
                    child: const Text('Submit'),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildEmailField() {
    return TextFormField(
      controller: _emailController,
      keyboardType: TextInputType.emailAddress,
      autocorrect: false,
      decoration: const InputDecoration(
        labelText: 'Email Address',
        prefixIcon: Icon(Icons.email),
        border: OutlineInputBorder(),
        helperText: 'We will never share your email',
      ),
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter your email';
        }
        if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value)) {
          return 'Please enter a valid email';
        }
        return null;
      },
    );
  }

  Widget _buildPhoneField() {
    return TextFormField(
      controller: _phoneController,
      keyboardType: TextInputType.phone,
      inputFormatters: [PhoneInputFormatter()],
      decoration: const InputDecoration(
        labelText: 'Phone Number',
        prefixIcon: Icon(Icons.phone),
        border: OutlineInputBorder(),
        hintText: '(123) 456-7890',
      ),
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter your phone number';
        }
        String digitsOnly = value.replaceAll(RegExp(r'[^0-9]'), '');
        if (digitsOnly.length < 10) {
          return 'Please enter a valid phone number';
        }
        return null;
      },
    );
  }

  Widget _buildCreditCardField() {
    return TextFormField(
      controller: _creditCardController,
      keyboardType: TextInputType.number,
      inputFormatters: [CreditCardInputFormatter()],
      decoration: const InputDecoration(
        labelText: 'Credit Card Number',
        prefixIcon: Icon(Icons.credit_card),
        border: OutlineInputBorder(),
        hintText: '1234 5678 9012 3456',
      ),
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter your credit card number';
        }
        String digitsOnly = value.replaceAll(RegExp(r'[^0-9]'), '');
        if (digitsOnly.length != 16) {
          return 'Credit card number must be 16 digits';
        }
        return null;
      },
    );
  }

  Widget _buildPasswordField() {
    return TextFormField(
      controller: _passwordController,
      obscureText: _obscurePassword,
      decoration: InputDecoration(
        labelText: 'Password',
        prefixIcon: const Icon(Icons.lock),
        suffixIcon: IconButton(
          icon: Icon(_obscurePassword ? Icons.visibility : Icons.visibility_off),
          onPressed: () {
            setState(() {
              _obscurePassword = !_obscurePassword;
            });
          },
        ),
        border: const OutlineInputBorder(),
        helperText: 'At least 8 characters with uppercase, lowercase, and number',
      ),
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter a password';
        }
        if (value.length < 8) {
          return 'Password must be at least 8 characters';
        }
        if (!RegExp(r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)').hasMatch(value)) {
          return 'Password must contain uppercase, lowercase, and number';
        }
        return null;
      },
    );
  }

  Widget _buildSearchField() {
    return TextFormField(
      controller: _searchController,
      decoration: InputDecoration(
        labelText: 'Search',
        prefixIcon: const Icon(Icons.search),
        suffixIcon: _searchController.text.isNotEmpty
            ? IconButton(
                icon: const Icon(Icons.clear),
                onPressed: () {
                  setState(() {
                    _searchController.clear();
                  });
                },
              )
            : null,
        border: const OutlineInputBorder(),
        hintText: 'Search for anything...',
      ),
      onChanged: (value) {
        setState(() {}); // Trigger rebuild to show/hide clear button
      },
    );
  }

  Widget _buildBioField() {
    return TextFormField(
      controller: _bioController,
      maxLines: 5,
      maxLength: _bioMaxLength,
      decoration: InputDecoration(
        labelText: 'Bio',
        border: const OutlineInputBorder(),
        alignLabelWithHint: true,
        helperText: 'Tell us about yourself',
        counterText: '$_bioCharacterCount/$_bioMaxLength characters',
      ),
      validator: (value) {
        if (value != null && value.length > _bioMaxLength) {
          return 'Bio cannot exceed $_bioMaxLength characters';
        }
        return null;
      },
    );
  }

  void _submitForm() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Form Submitted'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Email: ${_emailController.text}'),
            Text('Phone: ${_phoneController.text}'),
            Text('Credit Card: ${_creditCardController.text}'),
            Text('Search: ${_searchController.text}'),
            Text('Bio: ${_bioController.text.length} characters'),
          ],
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

Text input variations provide specialized keyboard types and formatting  
for different data types. Custom TextInputFormatter classes handle  
real-time formatting for phone numbers and credit cards. Character  
counting and validation enhance user experience and data quality.  

## Async Validation

Implementing server-side validation with loading states and error handling.  

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
      title: 'Async Validation',
      home: const AsyncValidationPage(),
    );
  }
}

class ApiService {
  // Simulate API calls with delays
  static Future<bool> checkUsernameAvailable(String username) async {
    await Future.delayed(const Duration(seconds: 2));
    // Simulate some usernames being taken
    final takenUsernames = ['admin', 'user', 'test', 'demo'];
    return !takenUsernames.contains(username.toLowerCase());
  }

  static Future<bool> validateEmail(String email) async {
    await Future.delayed(const Duration(seconds: 1));
    // Simulate email validation
    if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(email)) {
      return false;
    }
    // Simulate some emails being blacklisted
    final blacklistedDomains = ['tempmail.com', 'throwaway.email'];
    String domain = email.split('@').last;
    return !blacklistedDomains.contains(domain);
  }

  static Future<Map<String, dynamic>> validateDomain(String domain) async {
    await Future.delayed(const Duration(seconds: 3));
    // Simulate domain validation
    final validDomains = ['google.com', 'flutter.dev', 'github.com'];
    bool isValid = validDomains.contains(domain.toLowerCase());
    return {
      'valid': isValid,
      'message': isValid ? 'Domain is available' : 'Domain not found or invalid',
      'details': isValid ? 'This domain is active and reachable' : null,
    };
  }
}

class AsyncValidationPage extends StatefulWidget {
  const AsyncValidationPage({super.key});

  @override
  State<AsyncValidationPage> createState() => _AsyncValidationPageState();
}

class _AsyncValidationPageState extends State<AsyncValidationPage> {
  final _formKey = GlobalKey<FormState>();
  final _usernameController = TextEditingController();
  final _emailController = TextEditingController();
  final _domainController = TextEditingController();
  
  bool _isValidatingUsername = false;
  bool _isValidatingEmail = false;
  bool _isValidatingDomain = false;
  
  String? _usernameError;
  String? _emailError;
  String? _domainError;
  String? _domainDetails;
  
  Timer? _usernameTimer;
  Timer? _emailTimer;
  Timer? _domainTimer;

  @override
  void dispose() {
    _usernameController.dispose();
    _emailController.dispose();
    _domainController.dispose();
    _usernameTimer?.cancel();
    _emailTimer?.cancel();
    _domainTimer?.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Account Registration'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              children: [
                _buildUsernameField(),
                const SizedBox(height: 16),
                _buildEmailField(),
                const SizedBox(height: 16),
                _buildDomainField(),
                const SizedBox(height: 32),
                _buildSubmitButton(),
              ],
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildUsernameField() {
    return TextFormField(
      controller: _usernameController,
      decoration: InputDecoration(
        labelText: 'Username',
        prefixIcon: const Icon(Icons.person),
        suffixIcon: _isValidatingUsername
            ? const SizedBox(
                width: 20,
                height: 20,
                child: Padding(
                  padding: EdgeInsets.all(12),
                  child: CircularProgressIndicator(strokeWidth: 2),
                ),
              )
            : _usernameError == null && _usernameController.text.isNotEmpty
                ? const Icon(Icons.check, color: Colors.green)
                : null,
        border: const OutlineInputBorder(),
        errorText: _usernameError,
        errorMaxLines: 2,
      ),
      onChanged: (value) {
        _usernameTimer?.cancel();
        setState(() {
          _usernameError = null;
          _isValidatingUsername = false;
        });
        
        if (value.isNotEmpty) {
          _usernameTimer = Timer(const Duration(milliseconds: 800), () {
            _validateUsername(value);
          });
        }
      },
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter a username';
        }
        if (value.length < 3) {
          return 'Username must be at least 3 characters';
        }
        return null;
      },
    );
  }

  Widget _buildEmailField() {
    return TextFormField(
      controller: _emailController,
      keyboardType: TextInputType.emailAddress,
      decoration: InputDecoration(
        labelText: 'Email',
        prefixIcon: const Icon(Icons.email),
        suffixIcon: _isValidatingEmail
            ? const SizedBox(
                width: 20,
                height: 20,
                child: Padding(
                  padding: EdgeInsets.all(12),
                  child: CircularProgressIndicator(strokeWidth: 2),
                ),
              )
            : _emailError == null && _emailController.text.isNotEmpty
                ? const Icon(Icons.check, color: Colors.green)
                : null,
        border: const OutlineInputBorder(),
        errorText: _emailError,
        errorMaxLines: 2,
      ),
      onChanged: (value) {
        _emailTimer?.cancel();
        setState(() {
          _emailError = null;
          _isValidatingEmail = false;
        });
        
        if (value.isNotEmpty && value.contains('@')) {
          _emailTimer = Timer(const Duration(milliseconds: 1000), () {
            _validateEmail(value);
          });
        }
      },
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter an email';
        }
        return null;
      },
    );
  }

  Widget _buildDomainField() {
    return Column(
      children: [
        TextFormField(
          controller: _domainController,
          decoration: InputDecoration(
            labelText: 'Domain',
            prefixIcon: const Icon(Icons.language),
            suffixIcon: _isValidatingDomain
                ? const SizedBox(
                    width: 20,
                    height: 20,
                    child: Padding(
                      padding: EdgeInsets.all(12),
                      child: CircularProgressIndicator(strokeWidth: 2),
                    ),
                  )
                : _domainError == null && _domainController.text.isNotEmpty
                    ? const Icon(Icons.check, color: Colors.green)
                    : null,
            border: const OutlineInputBorder(),
            errorText: _domainError,
            errorMaxLines: 2,
            helperText: 'e.g., google.com',
          ),
          onChanged: (value) {
            _domainTimer?.cancel();
            setState(() {
              _domainError = null;
              _domainDetails = null;
              _isValidatingDomain = false;
            });
            
            if (value.isNotEmpty) {
              _domainTimer = Timer(const Duration(milliseconds: 1500), () {
                _validateDomain(value);
              });
            }
          },
          validator: (value) {
            if (value == null || value.isEmpty) {
              return 'Please enter a domain';
            }
            return null;
          },
        ),
        if (_domainDetails != null) ...[
          const SizedBox(height: 8),
          Container(
            width: double.infinity,
            padding: const EdgeInsets.all(12),
            decoration: BoxDecoration(
              color: Colors.green[800]?.withOpacity(0.2),
              border: Border.all(color: Colors.green),
              borderRadius: BorderRadius.circular(8),
            ),
            child: Row(
              children: [
                const Icon(Icons.info, color: Colors.green, size: 20),
                const SizedBox(width: 8),
                Expanded(
                  child: Text(
                    _domainDetails!,
                    style: const TextStyle(color: Colors.green),
                  ),
                ),
              ],
            ),
          ),
        ],
      ],
    );
  }

  Widget _buildSubmitButton() {
    bool isValidating = _isValidatingUsername || _isValidatingEmail || _isValidatingDomain;
    bool hasErrors = _usernameError != null || _emailError != null || _domainError != null;
    
    return SizedBox(
      width: double.infinity,
      child: ElevatedButton(
        onPressed: (isValidating || hasErrors) ? null : () {
          if (_formKey.currentState!.validate()) {
            showDialog(
              context: context,
              builder: (context) => AlertDialog(
                title: const Text('Registration Successful'),
                content: const Text('Your account has been created successfully!'),
                actions: [
                  TextButton(
                    onPressed: () => Navigator.of(context).pop(),
                    child: const Text('OK'),
                  ),
                ],
              ),
            );
          }
        },
        child: isValidating
            ? const Row(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  SizedBox(
                    width: 20,
                    height: 20,
                    child: CircularProgressIndicator(strokeWidth: 2),
                  ),
                  SizedBox(width: 8),
                  Text('Validating...'),
                ],
              )
            : const Text('Register'),
      ),
    );
  }

  void _validateUsername(String username) async {
    setState(() {
      _isValidatingUsername = true;
    });

    try {
      bool isAvailable = await ApiService.checkUsernameAvailable(username);
      if (mounted) {
        setState(() {
          _isValidatingUsername = false;
          _usernameError = isAvailable ? null : 'Username is already taken';
        });
      }
    } catch (e) {
      if (mounted) {
        setState(() {
          _isValidatingUsername = false;
          _usernameError = 'Unable to validate username. Please try again.';
        });
      }
    }
  }

  void _validateEmail(String email) async {
    setState(() {
      _isValidatingEmail = true;
    });

    try {
      bool isValid = await ApiService.validateEmail(email);
      if (mounted) {
        setState(() {
          _isValidatingEmail = false;
          _emailError = isValid ? null : 'Email is invalid or from a blocked domain';
        });
      }
    } catch (e) {
      if (mounted) {
        setState(() {
          _isValidatingEmail = false;
          _emailError = 'Unable to validate email. Please try again.';
        });
      }
    }
  }

  void _validateDomain(String domain) async {
    setState(() {
      _isValidatingDomain = true;
    });

    try {
      Map<String, dynamic> result = await ApiService.validateDomain(domain);
      if (mounted) {
        setState(() {
          _isValidatingDomain = false;
          _domainError = result['valid'] ? null : result['message'];
          _domainDetails = result['details'];
        });
      }
    } catch (e) {
      if (mounted) {
        setState(() {
          _isValidatingDomain = false;
          _domainError = 'Unable to validate domain. Please try again.';
          _domainDetails = null;
        });
      }
    }
  }
}
```

Async validation provides real-time server-side validation with debounced  
input to prevent excessive API calls. Loading indicators and error states  
give users feedback during validation. Timer-based debouncing improves  
performance by reducing unnecessary network requests.  

## Auto-validation

Implementing real-time field validation as users type with immediate feedback.  

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
      title: 'Auto-validation',
      home: const AutoValidationPage(),
    );
  }
}

class AutoValidationPage extends StatefulWidget {
  const AutoValidationPage({super.key});

  @override
  State<AutoValidationPage> createState() => _AutoValidationPageState();
}

class _AutoValidationPageState extends State<AutoValidationPage> {
  final _formKey = GlobalKey<FormState>();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  final _confirmPasswordController = TextEditingController();
  
  AutovalidateMode _autovalidateMode = AutovalidateMode.disabled;
  bool _hasUserInteracted = false;

  @override
  void dispose() {
    _emailController.dispose();
    _passwordController.dispose();
    _confirmPasswordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Auto-validation Form'),
        actions: [
          Switch(
            value: _autovalidateMode == AutovalidateMode.always,
            onChanged: (value) {
              setState(() {
                _autovalidateMode = value 
                    ? AutovalidateMode.always 
                    : AutovalidateMode.disabled;
              });
            },
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          autovalidateMode: _autovalidateMode,
          child: Column(
            children: [
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Auto-validation: ${_autovalidateMode == AutovalidateMode.always ? 'ON' : 'OFF'}',
                        style: const TextStyle(fontWeight: FontWeight.bold),
                      ),
                      const Text(
                        'Toggle the switch in the app bar to enable/disable real-time validation.',
                      ),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 24),
              TextFormField(
                controller: _emailController,
                keyboardType: TextInputType.emailAddress,
                decoration: InputDecoration(
                  labelText: 'Email',
                  prefixIcon: const Icon(Icons.email),
                  border: const OutlineInputBorder(),
                  suffixIcon: _getValidationIcon(_emailController.text, _isValidEmail),
                ),
                onChanged: (value) {
                  if (!_hasUserInteracted) {
                    setState(() {
                      _hasUserInteracted = true;
                    });
                  }
                  if (_autovalidateMode == AutovalidateMode.always) {
                    setState(() {}); // Trigger rebuild for icon update
                  }
                },
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Email is required';
                  }
                  if (!_isValidEmail(value)) {
                    return 'Please enter a valid email address';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _passwordController,
                obscureText: true,
                decoration: InputDecoration(
                  labelText: 'Password',
                  prefixIcon: const Icon(Icons.lock),
                  border: const OutlineInputBorder(),
                  suffixIcon: _getValidationIcon(_passwordController.text, _isStrongPassword),
                  helperText: 'Min 8 chars with uppercase, lowercase, number, and symbol',
                  helperMaxLines: 2,
                ),
                onChanged: (value) {
                  if (_autovalidateMode == AutovalidateMode.always) {
                    setState(() {}); // Trigger rebuild for icon update
                  }
                },
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Password is required';
                  }
                  if (value.length < 8) {
                    return 'Password must be at least 8 characters';
                  }
                  if (!RegExp(r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])')
                      .hasMatch(value)) {
                    return 'Password must contain uppercase, lowercase, number, and symbol';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _confirmPasswordController,
                obscureText: true,
                decoration: InputDecoration(
                  labelText: 'Confirm Password',
                  prefixIcon: const Icon(Icons.lock_outline),
                  border: const OutlineInputBorder(),
                  suffixIcon: _getPasswordMatchIcon(),
                ),
                onChanged: (value) {
                  if (_autovalidateMode == AutovalidateMode.always) {
                    setState(() {}); // Trigger rebuild for icon update
                  }
                },
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please confirm your password';
                  }
                  if (value != _passwordController.text) {
                    return 'Passwords do not match';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 24),
              _buildPasswordStrengthIndicator(),
              const SizedBox(height: 24),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _autovalidateMode = AutovalidateMode.onUserInteraction;
                    });
                    
                    if (_formKey.currentState!.validate()) {
                      ScaffoldMessenger.of(context).showSnackBar(
                        const SnackBar(
                          content: Text('Form is valid! Registration complete.'),
                          backgroundColor: Colors.green,
                        ),
                      );
                    }
                  },
                  child: const Text('Register'),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget? _getValidationIcon(String value, bool Function(String) validator) {
    if (value.isEmpty || _autovalidateMode == AutovalidateMode.disabled) {
      return null;
    }
    
    bool isValid = validator(value);
    return Icon(
      isValid ? Icons.check_circle : Icons.error,
      color: isValid ? Colors.green : Colors.red,
    );
  }

  Widget? _getPasswordMatchIcon() {
    if (_confirmPasswordController.text.isEmpty || 
        _autovalidateMode == AutovalidateMode.disabled) {
      return null;
    }
    
    bool matches = _confirmPasswordController.text == _passwordController.text;
    return Icon(
      matches ? Icons.check_circle : Icons.error,
      color: matches ? Colors.green : Colors.red,
    );
  }

  bool _isValidEmail(String email) {
    return RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(email);
  }

  bool _isStrongPassword(String password) {
    return password.length >= 8 && 
           RegExp(r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])').hasMatch(password);
  }

  Widget _buildPasswordStrengthIndicator() {
    String password = _passwordController.text;
    int strength = _calculatePasswordStrength(password);
    
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Password Strength',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            LinearProgressIndicator(
              value: strength / 4,
              backgroundColor: Colors.grey[300],
              valueColor: AlwaysStoppedAnimation<Color>(
                _getStrengthColor(strength),
              ),
            ),
            const SizedBox(height: 8),
            Text(
              _getStrengthText(strength),
              style: TextStyle(
                color: _getStrengthColor(strength),
                fontWeight: FontWeight.bold,
              ),
            ),
            if (password.isNotEmpty) ...[
              const SizedBox(height: 8),
              _buildPasswordCriteria(),
            ],
          ],
        ),
      ),
    );
  }

  Widget _buildPasswordCriteria() {
    String password = _passwordController.text;
    
    return Column(
      children: [
        _buildCriteriaRow('At least 8 characters', password.length >= 8),
        _buildCriteriaRow('Contains lowercase letter', RegExp(r'[a-z]').hasMatch(password)),
        _buildCriteriaRow('Contains uppercase letter', RegExp(r'[A-Z]').hasMatch(password)),
        _buildCriteriaRow('Contains number', RegExp(r'\d').hasMatch(password)),
        _buildCriteriaRow('Contains symbol', RegExp(r'[@$!%*?&]').hasMatch(password)),
      ],
    );
  }

  Widget _buildCriteriaRow(String text, bool isMet) {
    return Row(
      children: [
        Icon(
          isMet ? Icons.check : Icons.close,
          color: isMet ? Colors.green : Colors.red,
          size: 16,
        ),
        const SizedBox(width: 8),
        Text(
          text,
          style: TextStyle(
            color: isMet ? Colors.green : Colors.red,
            fontSize: 12,
          ),
        ),
      ],
    );
  }

  int _calculatePasswordStrength(String password) {
    if (password.isEmpty) return 0;
    
    int strength = 0;
    if (password.length >= 8) strength++;
    if (RegExp(r'[a-z]').hasMatch(password)) strength++;
    if (RegExp(r'[A-Z]').hasMatch(password)) strength++;
    if (RegExp(r'\d').hasMatch(password)) strength++;
    if (RegExp(r'[@$!%*?&]').hasMatch(password)) strength++;
    
    return strength > 4 ? 4 : strength;
  }

  Color _getStrengthColor(int strength) {
    switch (strength) {
      case 0:
      case 1:
        return Colors.red;
      case 2:
        return Colors.orange;
      case 3:
        return Colors.yellow;
      case 4:
        return Colors.green;
      default:
        return Colors.grey;
    }
  }

  String _getStrengthText(int strength) {
    switch (strength) {
      case 0:
        return 'Very Weak';
      case 1:
        return 'Weak';
      case 2:
        return 'Fair';
      case 3:
        return 'Good';
      case 4:
        return 'Strong';
      default:
        return '';
    }
  }
}
```

Auto-validation provides immediate feedback as users type, improving  
form completion rates and user experience. Visual indicators and  
strength meters guide users toward valid input. AutovalidateMode  
controls when validation triggers occur.  

## Form Reset and Clear

Implementing form reset functionality with confirmation and state management.  

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
      title: 'Form Reset Clear',
      home: const FormResetClearPage(),
    );
  }
}

class FormResetClearPage extends StatefulWidget {
  const FormResetClearPage({super.key});

  @override
  State<FormResetClearPage> createState() => _FormResetClearPageState();
}

class _FormResetClearPageState extends State<FormResetClearPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  final _companyController = TextEditingController();
  final _messageController = TextEditingController();
  
  String _selectedCountry = 'United States';
  String _selectedDepartment = 'General';
  bool _newsletter = false;
  double _priority = 1.0;
  
  // Default values for reset
  final String _defaultCountry = 'United States';
  final String _defaultDepartment = 'General';
  final bool _defaultNewsletter = false;
  final double _defaultPriority = 1.0;
  
  bool _hasChanges = false;

  final List<String> _countries = [
    'United States', 'Canada', 'United Kingdom', 'Germany', 'France'
  ];
  
  final List<String> _departments = [
    'General', 'Sales', 'Support', 'Technical', 'Billing'
  ];

  @override
  void initState() {
    super.initState();
    _setupChangeListeners();
  }

  void _setupChangeListeners() {
    _nameController.addListener(_onFormChanged);
    _emailController.addListener(_onFormChanged);
    _phoneController.addListener(_onFormChanged);
    _companyController.addListener(_onFormChanged);
    _messageController.addListener(_onFormChanged);
  }

  void _onFormChanged() {
    bool hasChanges = _nameController.text.isNotEmpty ||
                     _emailController.text.isNotEmpty ||
                     _phoneController.text.isNotEmpty ||
                     _companyController.text.isNotEmpty ||
                     _messageController.text.isNotEmpty ||
                     _selectedCountry != _defaultCountry ||
                     _selectedDepartment != _defaultDepartment ||
                     _newsletter != _defaultNewsletter ||
                     _priority != _defaultPriority;
    
    if (hasChanges != _hasChanges) {
      setState(() {
        _hasChanges = hasChanges;
      });
    }
  }

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    _companyController.dispose();
    _messageController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Contact Form'),
        actions: [
          if (_hasChanges)
            IconButton(
              icon: const Icon(Icons.refresh),
              tooltip: 'Reset Form',
              onPressed: _showResetConfirmation,
            ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              children: [
                if (_hasChanges)
                  Card(
                    color: Colors.orange[900]?.withOpacity(0.3),
                    child: const Padding(
                      padding: EdgeInsets.all(12),
                      child: Row(
                        children: [
                          Icon(Icons.info, color: Colors.orange),
                          SizedBox(width: 8),
                          Text('You have unsaved changes'),
                        ],
                      ),
                    ),
                  ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _nameController,
                  decoration: const InputDecoration(
                    labelText: 'Full Name *',
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'Please enter your name';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _emailController,
                  decoration: const InputDecoration(
                    labelText: 'Email *',
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'Please enter your email';
                    }
                    if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$')
                        .hasMatch(value)) {
                      return 'Please enter a valid email';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _phoneController,
                  decoration: const InputDecoration(
                    labelText: 'Phone Number',
                    border: OutlineInputBorder(),
                  ),
                ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _companyController,
                  decoration: const InputDecoration(
                    labelText: 'Company',
                    border: OutlineInputBorder(),
                  ),
                ),
                const SizedBox(height: 16),
                DropdownButtonFormField<String>(
                  value: _selectedCountry,
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
                      _selectedCountry = value!;
                    });
                    _onFormChanged();
                  },
                ),
                const SizedBox(height: 16),
                DropdownButtonFormField<String>(
                  value: _selectedDepartment,
                  decoration: const InputDecoration(
                    labelText: 'Department',
                    border: OutlineInputBorder(),
                  ),
                  items: _departments.map((dept) {
                    return DropdownMenuItem(
                      value: dept,
                      child: Text(dept),
                    );
                  }).toList(),
                  onChanged: (value) {
                    setState(() {
                      _selectedDepartment = value!;
                    });
                    _onFormChanged();
                  },
                ),
                const SizedBox(height: 16),
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'Priority Level: ${_priority.round()}',
                          style: const TextStyle(fontWeight: FontWeight.bold),
                        ),
                        Slider(
                          value: _priority,
                          min: 1,
                          max: 5,
                          divisions: 4,
                          label: 'Priority ${_priority.round()}',
                          onChanged: (value) {
                            setState(() {
                              _priority = value;
                            });
                            _onFormChanged();
                          },
                        ),
                      ],
                    ),
                  ),
                ),
                const SizedBox(height: 16),
                SwitchListTile(
                  title: const Text('Subscribe to Newsletter'),
                  value: _newsletter,
                  onChanged: (value) {
                    setState(() {
                      _newsletter = value;
                    });
                    _onFormChanged();
                  },
                ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _messageController,
                  maxLines: 4,
                  decoration: const InputDecoration(
                    labelText: 'Message *',
                    border: OutlineInputBorder(),
                    alignLabelWithHint: true,
                  ),
                  validator: (value) {
                    if (value == null || value.isEmpty) {
                      return 'Please enter your message';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: 32),
                Row(
                  children: [
                    Expanded(
                      child: OutlinedButton.icon(
                        onPressed: _hasChanges ? _showClearConfirmation : null,
                        icon: const Icon(Icons.clear_all),
                        label: const Text('Clear All'),
                      ),
                    ),
                    const SizedBox(width: 16),
                    Expanded(
                      child: OutlinedButton.icon(
                        onPressed: _hasChanges ? _showResetConfirmation : null,
                        icon: const Icon(Icons.refresh),
                        label: const Text('Reset'),
                      ),
                    ),
                  ],
                ),
                const SizedBox(height: 16),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton.icon(
                    onPressed: _submitForm,
                    icon: const Icon(Icons.send),
                    label: const Text('Submit'),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  void _showResetConfirmation() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Reset Form'),
        content: const Text(
          'Are you sure you want to reset the form to default values? '
          'All your current input will be lost.',
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () {
              Navigator.of(context).pop();
              _resetForm();
            },
            child: const Text('Reset'),
          ),
        ],
      ),
    );
  }

  void _showClearConfirmation() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Clear Form'),
        content: const Text(
          'Are you sure you want to clear all form data? '
          'This action cannot be undone.',
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () {
              Navigator.of(context).pop();
              _clearForm();
            },
            child: const Text('Clear All'),
          ),
        ],
      ),
    );
  }

  void _resetForm() {
    setState(() {
      _nameController.clear();
      _emailController.clear();
      _phoneController.clear();
      _companyController.clear();
      _messageController.clear();
      _selectedCountry = _defaultCountry;
      _selectedDepartment = _defaultDepartment;
      _newsletter = _defaultNewsletter;
      _priority = _defaultPriority;
      _hasChanges = false;
    });
    
    _formKey.currentState?.reset();
    
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('Form reset to default values'),
        backgroundColor: Colors.blue,
      ),
    );
  }

  void _clearForm() {
    setState(() {
      _nameController.clear();
      _emailController.clear();
      _phoneController.clear();
      _companyController.clear();
      _messageController.clear();
      _selectedCountry = _defaultCountry;
      _selectedDepartment = _defaultDepartment;
      _newsletter = _defaultNewsletter;
      _priority = _defaultPriority;
      _hasChanges = false;
    });
    
    _formKey.currentState?.reset();
    
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('All form data cleared'),
        backgroundColor: Colors.orange,
      ),
    );
  }

  void _submitForm() {
    if (_formKey.currentState!.validate()) {
      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: const Text('Form Submitted'),
          content: const Text('Thank you! Your form has been submitted successfully.'),
          actions: [
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
                _clearForm();
              },
              child: const Text('OK'),
            ),
          ],
        ),
      );
    }
  }
}
```

Form reset and clear functionality provides users control over form state  
with confirmation dialogs to prevent accidental data loss. Change tracking  
enables smart UI updates and warnings for unsaved modifications. Separate  
clear and reset operations offer different levels of form management.  

## Conditional Fields

Showing and hiding form fields dynamically based on user selections.  

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
      title: 'Conditional Fields',
      home: const ConditionalFieldsPage(),
    );
  }
}

enum EmploymentStatus { employed, unemployed, student, retired, selfEmployed }
enum PaymentMethod { creditCard, bankTransfer, paypal, crypto }
enum ShippingMethod { standard, express, overnight }

class ConditionalFieldsPage extends StatefulWidget {
  const ConditionalFieldsPage({super.key});

  @override
  State<ConditionalFieldsPage> createState() => _ConditionalFieldsPageState();
}

class _ConditionalFieldsPageState extends State<ConditionalFieldsPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _companyController = TextEditingController();
  final _jobTitleController = TextEditingController();
  final _schoolController = TextEditingController();
  final _majorController = TextEditingController();
  final _cardNumberController = TextEditingController();
  final _expiryController = TextEditingController();
  final _cvvController = TextEditingController();
  final _accountNumberController = TextEditingController();
  final _routingController = TextEditingController();
  final _paypalEmailController = TextEditingController();
  final _walletAddressController = TextEditingController();
  final _shippingAddressController = TextEditingController();
  
  EmploymentStatus? _employmentStatus;
  PaymentMethod? _paymentMethod;
  ShippingMethod _shippingMethod = ShippingMethod.standard;
  bool _hasShippingAddress = false;
  bool _needsInvoice = false;
  bool _isInternational = false;

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _companyController.dispose();
    _jobTitleController.dispose();
    _schoolController.dispose();
    _majorController.dispose();
    _cardNumberController.dispose();
    _expiryController.dispose();
    _cvvController.dispose();
    _accountNumberController.dispose();
    _routingController.dispose();
    _paypalEmailController.dispose();
    _walletAddressController.dispose();
    _shippingAddressController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Order Form'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                _buildPersonalInfoSection(),
                const SizedBox(height: 24),
                _buildEmploymentSection(),
                const SizedBox(height: 24),
                _buildPaymentSection(),
                const SizedBox(height: 24),
                _buildShippingSection(),
                const SizedBox(height: 32),
                _buildSubmitButton(),
              ],
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildPersonalInfoSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Personal Information',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _nameController,
              decoration: const InputDecoration(
                labelText: 'Full Name *',
                border: OutlineInputBorder(),
              ),
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Please enter your name';
                }
                return null;
              },
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _emailController,
              decoration: const InputDecoration(
                labelText: 'Email *',
                border: OutlineInputBorder(),
              ),
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Please enter your email';
                }
                return null;
              },
            ),
            const SizedBox(height: 16),
            CheckboxListTile(
              title: const Text('This is an international order'),
              value: _isInternational,
              onChanged: (value) {
                setState(() {
                  _isInternational = value ?? false;
                });
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildEmploymentSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Employment Status',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            ...EmploymentStatus.values.map((status) {
              return RadioListTile<EmploymentStatus>(
                title: Text(_getEmploymentStatusTitle(status)),
                value: status,
                groupValue: _employmentStatus,
                onChanged: (value) {
                  setState(() {
                    _employmentStatus = value;
                  });
                },
              );
            }),
            if (_employmentStatus == EmploymentStatus.employed ||
                _employmentStatus == EmploymentStatus.selfEmployed) ...[
              const SizedBox(height: 16),
              TextFormField(
                controller: _companyController,
                decoration: const InputDecoration(
                  labelText: 'Company Name *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter company name';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _jobTitleController,
                decoration: const InputDecoration(
                  labelText: 'Job Title *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter job title';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              CheckboxListTile(
                title: const Text('I need an invoice for my company'),
                value: _needsInvoice,
                onChanged: (value) {
                  setState(() {
                    _needsInvoice = value ?? false;
                  });
                },
              ),
            ],
            if (_employmentStatus == EmploymentStatus.student) ...[
              const SizedBox(height: 16),
              TextFormField(
                controller: _schoolController,
                decoration: const InputDecoration(
                  labelText: 'School/University *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter school name';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _majorController,
                decoration: const InputDecoration(
                  labelText: 'Major/Field of Study',
                  border: OutlineInputBorder(),
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }

  Widget _buildPaymentSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Payment Method',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            ...PaymentMethod.values.map((method) {
              return RadioListTile<PaymentMethod>(
                title: Text(_getPaymentMethodTitle(method)),
                subtitle: Text(_getPaymentMethodSubtitle(method)),
                value: method,
                groupValue: _paymentMethod,
                onChanged: (value) {
                  setState(() {
                    _paymentMethod = value;
                  });
                },
              );
            }),
            if (_paymentMethod == PaymentMethod.creditCard) ...[
              const SizedBox(height: 16),
              TextFormField(
                controller: _cardNumberController,
                decoration: const InputDecoration(
                  labelText: 'Card Number *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter card number';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              Row(
                children: [
                  Expanded(
                    child: TextFormField(
                      controller: _expiryController,
                      decoration: const InputDecoration(
                        labelText: 'MM/YY *',
                        border: OutlineInputBorder(),
                      ),
                      validator: (value) {
                        if (value == null || value.isEmpty) {
                          return 'Required';
                        }
                        return null;
                      },
                    ),
                  ),
                  const SizedBox(width: 16),
                  Expanded(
                    child: TextFormField(
                      controller: _cvvController,
                      decoration: const InputDecoration(
                        labelText: 'CVV *',
                        border: OutlineInputBorder(),
                      ),
                      validator: (value) {
                        if (value == null || value.isEmpty) {
                          return 'Required';
                        }
                        return null;
                      },
                    ),
                  ),
                ],
              ),
            ],
            if (_paymentMethod == PaymentMethod.bankTransfer) ...[
              const SizedBox(height: 16),
              TextFormField(
                controller: _accountNumberController,
                decoration: const InputDecoration(
                  labelText: 'Account Number *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter account number';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _routingController,
                decoration: const InputDecoration(
                  labelText: 'Routing Number *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter routing number';
                  }
                  return null;
                },
              ),
            ],
            if (_paymentMethod == PaymentMethod.paypal) ...[
              const SizedBox(height: 16),
              TextFormField(
                controller: _paypalEmailController,
                decoration: const InputDecoration(
                  labelText: 'PayPal Email *',
                  border: OutlineInputBorder(),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter PayPal email';
                  }
                  return null;
                },
              ),
            ],
            if (_paymentMethod == PaymentMethod.crypto) ...[
              const SizedBox(height: 16),
              TextFormField(
                controller: _walletAddressController,
                decoration: const InputDecoration(
                  labelText: 'Wallet Address *',
                  border: OutlineInputBorder(),
                  helperText: 'Bitcoin or Ethereum address',
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter wallet address';
                  }
                  return null;
                },
              ),
            ],
          ],
        ),
      ),
    );
  }

  Widget _buildShippingSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Shipping Options',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            CheckboxListTile(
              title: const Text('Ship to different address'),
              value: _hasShippingAddress,
              onChanged: (value) {
                setState(() {
                  _hasShippingAddress = value ?? false;
                });
              },
            ),
            if (_hasShippingAddress) ...[
              const SizedBox(height: 16),
              TextFormField(
                controller: _shippingAddressController,
                maxLines: 3,
                decoration: const InputDecoration(
                  labelText: 'Shipping Address *',
                  border: OutlineInputBorder(),
                  alignLabelWithHint: true,
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter shipping address';
                  }
                  return null;
                },
              ),
            ],
            const SizedBox(height: 16),
            const Text(
              'Shipping Method',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            ...ShippingMethod.values.map((method) {
              return RadioListTile<ShippingMethod>(
                title: Text(_getShippingMethodTitle(method)),
                subtitle: Text(_getShippingMethodPrice(method)),
                value: method,
                groupValue: _shippingMethod,
                onChanged: (value) {
                  setState(() {
                    _shippingMethod = value!;
                  });
                },
              );
            }),
            if (_isInternational) ...[
              const SizedBox(height: 16),
              Container(
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: Colors.orange[800]?.withOpacity(0.2),
                  border: Border.all(color: Colors.orange),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: const Row(
                  children: [
                    Icon(Icons.warning, color: Colors.orange),
                    SizedBox(width: 8),
                    Expanded(
                      child: Text(
                        'International orders may be subject to customs duties and additional processing time.',
                      ),
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

  Widget _buildSubmitButton() {
    return SizedBox(
      width: double.infinity,
      child: ElevatedButton(
        onPressed: () {
          if (_formKey.currentState!.validate()) {
            if (_employmentStatus == null) {
              ScaffoldMessenger.of(context).showSnackBar(
                const SnackBar(content: Text('Please select employment status')),
              );
              return;
            }
            if (_paymentMethod == null) {
              ScaffoldMessenger.of(context).showSnackBar(
                const SnackBar(content: Text('Please select payment method')),
              );
              return;
            }
            
            showDialog(
              context: context,
              builder: (context) => AlertDialog(
                title: const Text('Order Submitted'),
                content: Text(
                  'Thank you ${_nameController.text}! '
                  'Your order has been submitted successfully.',
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
        },
        child: const Text('Submit Order'),
      ),
    );
  }

  String _getEmploymentStatusTitle(EmploymentStatus status) {
    switch (status) {
      case EmploymentStatus.employed: return 'Employed';
      case EmploymentStatus.unemployed: return 'Unemployed';
      case EmploymentStatus.student: return 'Student';
      case EmploymentStatus.retired: return 'Retired';
      case EmploymentStatus.selfEmployed: return 'Self-employed';
    }
  }

  String _getPaymentMethodTitle(PaymentMethod method) {
    switch (method) {
      case PaymentMethod.creditCard: return 'Credit Card';
      case PaymentMethod.bankTransfer: return 'Bank Transfer';
      case PaymentMethod.paypal: return 'PayPal';
      case PaymentMethod.crypto: return 'Cryptocurrency';
    }
  }

  String _getPaymentMethodSubtitle(PaymentMethod method) {
    switch (method) {
      case PaymentMethod.creditCard: return 'Visa, MasterCard, Amex';
      case PaymentMethod.bankTransfer: return 'Direct bank payment';
      case PaymentMethod.paypal: return 'Pay with your PayPal account';
      case PaymentMethod.crypto: return 'Bitcoin or Ethereum';
    }
  }

  String _getShippingMethodTitle(ShippingMethod method) {
    switch (method) {
      case ShippingMethod.standard: return 'Standard Shipping';
      case ShippingMethod.express: return 'Express Shipping';
      case ShippingMethod.overnight: return 'Overnight Shipping';
    }
  }

  String _getShippingMethodPrice(ShippingMethod method) {
    switch (method) {
      case ShippingMethod.standard: return 'Free (5-7 business days)';
      case ShippingMethod.express: return '\$15.99 (2-3 business days)';
      case ShippingMethod.overnight: return '\$29.99 (Next business day)';
    }
  }
}
```

Conditional fields create dynamic forms that adapt to user input,  
reducing complexity and improving user experience. State management  
controls field visibility and validation requirements. Proper validation  
ensures required conditional fields are completed before submission.  

## Form Persistence

Automatically saving and restoring form data using local storage.  

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';
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
      title: 'Form Persistence',
      home: const FormPersistencePage(),
    );
  }
}

class FormData {
  String name;
  String email;
  String phone;
  String message;
  bool newsletter;
  String country;
  int age;
  DateTime lastSaved;

  FormData({
    this.name = '',
    this.email = '',
    this.phone = '',
    this.message = '',
    this.newsletter = false,
    this.country = 'United States',
    this.age = 18,
    DateTime? lastSaved,
  }) : lastSaved = lastSaved ?? DateTime.now();

  Map<String, dynamic> toJson() {
    return {
      'name': name,
      'email': email,
      'phone': phone,
      'message': message,
      'newsletter': newsletter,
      'country': country,
      'age': age,
      'lastSaved': lastSaved.toIso8601String(),
    };
  }

  factory FormData.fromJson(Map<String, dynamic> json) {
    return FormData(
      name: json['name'] ?? '',
      email: json['email'] ?? '',
      phone: json['phone'] ?? '',
      message: json['message'] ?? '',
      newsletter: json['newsletter'] ?? false,
      country: json['country'] ?? 'United States',
      age: json['age'] ?? 18,
      lastSaved: DateTime.tryParse(json['lastSaved'] ?? '') ?? DateTime.now(),
    );
  }
}

class FormPersistencePage extends StatefulWidget {
  const FormPersistencePage({super.key});

  @override
  State<FormPersistencePage> createState() => _FormPersistencePageState();
}

class _FormPersistencePageState extends State<FormPersistencePage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  final _messageController = TextEditingController();
  
  FormData _formData = FormData();
  bool _isLoading = true;
  bool _autoSave = true;
  DateTime? _lastAutoSave;

  final List<String> _countries = [
    'United States', 'Canada', 'United Kingdom', 'Germany', 'France'
  ];

  @override
  void initState() {
    super.initState();
    _loadFormData();
    _setupAutoSave();
  }

  void _setupAutoSave() {
    // Auto-save every 30 seconds when auto-save is enabled
    Future.delayed(const Duration(seconds: 30), () {
      if (mounted && _autoSave) {
        _saveFormData(showMessage: false);
        _setupAutoSave();
      }
    });
  }

  Future<void> _loadFormData() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      String? formDataJson = prefs.getString('form_data');
      
      if (formDataJson != null) {
        Map<String, dynamic> json = jsonDecode(formDataJson);
        _formData = FormData.fromJson(json);
        
        // Populate form fields
        _nameController.text = _formData.name;
        _emailController.text = _formData.email;
        _phoneController.text = _formData.phone;
        _messageController.text = _formData.message;
        
        setState(() {
          _isLoading = false;
        });

        // Show restoration message
        if (_formData.name.isNotEmpty || _formData.email.isNotEmpty) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              content: Text(
                'Form data restored from ${_formatDateTime(_formData.lastSaved)}',
              ),
              backgroundColor: Colors.green,
              action: SnackBarAction(
                label: 'Clear',
                onPressed: _clearPersistedData,
              ),
            ),
          );
        }
      } else {
        setState(() {
          _isLoading = false;
        });
      }
    } catch (e) {
      setState(() {
        _isLoading = false;
      });
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Error loading saved form data'),
          backgroundColor: Colors.red,
        ),
      );
    }
  }

  Future<void> _saveFormData({bool showMessage = true}) async {
    try {
      // Update form data from controllers
      _formData.name = _nameController.text;
      _formData.email = _emailController.text;
      _formData.phone = _phoneController.text;
      _formData.message = _messageController.text;
      _formData.lastSaved = DateTime.now();

      final prefs = await SharedPreferences.getInstance();
      String formDataJson = jsonEncode(_formData.toJson());
      await prefs.setString('form_data', formDataJson);
      
      setState(() {
        _lastAutoSave = DateTime.now();
      });

      if (showMessage) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            content: Text('Form data saved'),
            backgroundColor: Colors.blue,
            duration: Duration(seconds: 2),
          ),
        );
      }
    } catch (e) {
      if (showMessage) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            content: Text('Error saving form data'),
            backgroundColor: Colors.red,
          ),
        );
      }
    }
  }

  Future<void> _clearPersistedData() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      await prefs.remove('form_data');
      
      setState(() {
        _formData = FormData();
        _nameController.clear();
        _emailController.clear();
        _phoneController.clear();
        _messageController.clear();
        _lastAutoSave = null;
      });

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Persisted form data cleared'),
          backgroundColor: Colors.orange,
        ),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Error clearing persisted data'),
          backgroundColor: Colors.red,
        ),
      );
    }
  }

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    _messageController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    if (_isLoading) {
      return Scaffold(
        appBar: AppBar(title: const Text('Loading...')),
        body: const Center(child: CircularProgressIndicator()),
      );
    }

    return Scaffold(
      appBar: AppBar(
        title: const Text('Persistent Form'),
        actions: [
          IconButton(
            icon: Icon(_autoSave ? Icons.save : Icons.save_outlined),
            tooltip: _autoSave ? 'Auto-save ON' : 'Auto-save OFF',
            onPressed: () {
              setState(() {
                _autoSave = !_autoSave;
              });
              ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(
                  content: Text(
                    _autoSave ? 'Auto-save enabled' : 'Auto-save disabled',
                  ),
                ),
              );
            },
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              children: [
                if (_lastAutoSave != null)
                  Card(
                    color: Colors.blue[900]?.withOpacity(0.3),
                    child: Padding(
                      padding: const EdgeInsets.all(12),
                      child: Row(
                        children: [
                          const Icon(Icons.info, color: Colors.blue),
                          const SizedBox(width: 8),
                          Expanded(
                            child: Text(
                              'Last auto-saved: ${_formatDateTime(_lastAutoSave!)}',
                            ),
                          ),
                        ],
                      ),
                    ),
                  ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _nameController,
                  decoration: const InputDecoration(
                    labelText: 'Name',
                    border: OutlineInputBorder(),
                  ),
                  onChanged: (value) {
                    _formData.name = value;
                  },
                ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _emailController,
                  decoration: const InputDecoration(
                    labelText: 'Email',
                    border: OutlineInputBorder(),
                  ),
                  onChanged: (value) {
                    _formData.email = value;
                  },
                ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _phoneController,
                  decoration: const InputDecoration(
                    labelText: 'Phone',
                    border: OutlineInputBorder(),
                  ),
                  onChanged: (value) {
                    _formData.phone = value;
                  },
                ),
                const SizedBox(height: 16),
                DropdownButtonFormField<String>(
                  value: _formData.country,
                  decoration: const InputDecoration(
                    labelText: 'Country',
                    border: OutlineInputBorder(),
                  ),
                  items: _countries.map((country) {
                    return DropdownMenuItem(value: country, child: Text(country));
                  }).toList(),
                  onChanged: (value) {
                    setState(() {
                      _formData.country = value!;
                    });
                  },
                ),
                const SizedBox(height: 16),
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'Age: ${_formData.age}',
                          style: const TextStyle(fontWeight: FontWeight.bold),
                        ),
                        Slider(
                          value: _formData.age.toDouble(),
                          min: 18,
                          max: 100,
                          divisions: 82,
                          label: '${_formData.age}',
                          onChanged: (value) {
                            setState(() {
                              _formData.age = value.round();
                            });
                          },
                        ),
                      ],
                    ),
                  ),
                ),
                const SizedBox(height: 16),
                SwitchListTile(
                  title: const Text('Subscribe to Newsletter'),
                  value: _formData.newsletter,
                  onChanged: (value) {
                    setState(() {
                      _formData.newsletter = value;
                    });
                  },
                ),
                const SizedBox(height: 16),
                TextFormField(
                  controller: _messageController,
                  maxLines: 4,
                  decoration: const InputDecoration(
                    labelText: 'Message',
                    border: OutlineInputBorder(),
                    alignLabelWithHint: true,
                  ),
                  onChanged: (value) {
                    _formData.message = value;
                  },
                ),
                const SizedBox(height: 32),
                Row(
                  children: [
                    Expanded(
                      child: OutlinedButton.icon(
                        onPressed: () => _saveFormData(),
                        icon: const Icon(Icons.save),
                        label: const Text('Save'),
                      ),
                    ),
                    const SizedBox(width: 16),
                    Expanded(
                      child: OutlinedButton.icon(
                        onPressed: _clearPersistedData,
                        icon: const Icon(Icons.clear),
                        label: const Text('Clear'),
                      ),
                    ),
                  ],
                ),
                const SizedBox(height: 16),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton.icon(
                    onPressed: () {
                      if (_formKey.currentState!.validate()) {
                        _submitForm();
                      }
                    },
                    icon: const Icon(Icons.send),
                    label: const Text('Submit'),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  void _submitForm() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Form Submitted'),
        content: const Text('Thank you! Your form has been submitted successfully.'),
        actions: [
          TextButton(
            onPressed: () {
              Navigator.of(context).pop();
              _clearPersistedData();
            },
            child: const Text('OK'),
          ),
        ],
      ),
    );
  }

  String _formatDateTime(DateTime dateTime) {
    return '${dateTime.day}/${dateTime.month} ${dateTime.hour}:${dateTime.minute.toString().padLeft(2, '0')}';
  }
}
```

Form persistence automatically saves and restores user input using  
SharedPreferences and JSON serialization. Auto-save functionality  
prevents data loss during app crashes or unexpected exits. Restoration  
notifications inform users when previous data is available.  

## Custom Form Widgets

Creating reusable custom form widgets for consistent UI and behavior.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Custom Form Widgets',
      home: const CustomFormWidgetsPage(),
    );
  }
}

class CustomTextFormField extends StatelessWidget {
  final String label;
  final String? hint;
  final IconData? prefixIcon;
  final Widget? suffixIcon;
  final TextEditingController? controller;
  final String? Function(String?)? validator;
  final TextInputType? keyboardType;
  final bool obscureText;
  final void Function(String)? onChanged;
  final int? maxLines;
  final Color? accentColor;

  const CustomTextFormField({
    super.key,
    required this.label,
    this.hint,
    this.prefixIcon,
    this.suffixIcon,
    this.controller,
    this.validator,
    this.keyboardType,
    this.obscureText = false,
    this.onChanged,
    this.maxLines = 1,
    this.accentColor,
  });

  @override
  Widget build(BuildContext context) {
    return TextFormField(
      controller: controller,
      validator: validator,
      keyboardType: keyboardType,
      obscureText: obscureText,
      onChanged: onChanged,
      maxLines: maxLines,
      decoration: InputDecoration(
        labelText: label,
        hintText: hint,
        prefixIcon: prefixIcon != null ? Icon(prefixIcon, color: accentColor) : null,
        suffixIcon: suffixIcon,
        border: OutlineInputBorder(
          borderRadius: BorderRadius.circular(12),
        ),
        focusedBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(12),
          borderSide: BorderSide(
            color: accentColor ?? Theme.of(context).primaryColor,
            width: 2,
          ),
        ),
      ),
    );
  }
}

class CustomDropdownField<T> extends StatelessWidget {
  final String label;
  final T? value;
  final List<DropdownMenuItem<T>> items;
  final void Function(T?)? onChanged;
  final String? Function(T?)? validator;
  final IconData? prefixIcon;
  final Color? accentColor;

  const CustomDropdownField({
    super.key,
    required this.label,
    required this.items,
    this.value,
    this.onChanged,
    this.validator,
    this.prefixIcon,
    this.accentColor,
  });

  @override
  Widget build(BuildContext context) {
    return DropdownButtonFormField<T>(
      value: value,
      items: items,
      onChanged: onChanged,
      validator: validator,
      decoration: InputDecoration(
        labelText: label,
        prefixIcon: prefixIcon != null ? Icon(prefixIcon, color: accentColor) : null,
        border: OutlineInputBorder(
          borderRadius: BorderRadius.circular(12),
        ),
        focusedBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(12),
          borderSide: BorderSide(
            color: accentColor ?? Theme.of(context).primaryColor,
            width: 2,
          ),
        ),
      ),
    );
  }
}

class CustomSwitchField extends StatelessWidget {
  final String title;
  final String? subtitle;
  final bool value;
  final void Function(bool) onChanged;
  final IconData? icon;
  final Color? accentColor;

  const CustomSwitchField({
    super.key,
    required this.title,
    required this.value,
    required this.onChanged,
    this.subtitle,
    this.icon,
    this.accentColor,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      child: SwitchListTile(
        title: Row(
          children: [
            if (icon != null) ...[
              Icon(icon, color: accentColor),
              const SizedBox(width: 8),
            ],
            Expanded(child: Text(title)),
          ],
        ),
        subtitle: subtitle != null ? Text(subtitle!) : null,
        value: value,
        onChanged: onChanged,
        activeColor: accentColor,
      ),
    );
  }
}

class CustomSliderField extends StatelessWidget {
  final String label;
  final double value;
  final double min;
  final double max;
  final int? divisions;
  final void Function(double) onChanged;
  final String Function(double)? labelFormatter;
  final Color? accentColor;

  const CustomSliderField({
    super.key,
    required this.label,
    required this.value,
    required this.min,
    required this.max,
    required this.onChanged,
    this.divisions,
    this.labelFormatter,
    this.accentColor,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              '$label: ${labelFormatter?.call(value) ?? value.toStringAsFixed(1)}',
              style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Slider(
              value: value,
              min: min,
              max: max,
              divisions: divisions,
              activeColor: accentColor,
              onChanged: onChanged,
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text('${min.toInt()}', style: Theme.of(context).textTheme.bodySmall),
                Text('${max.toInt()}', style: Theme.of(context).textTheme.bodySmall),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

class CustomFormSection extends StatelessWidget {
  final String title;
  final String? subtitle;
  final List<Widget> children;
  final IconData? icon;
  final Color? accentColor;

  const CustomFormSection({
    super.key,
    required this.title,
    required this.children,
    this.subtitle,
    this.icon,
    this.accentColor,
  });

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
                  Icon(icon, color: accentColor, size: 24),
                  const SizedBox(width: 8),
                ],
                Expanded(
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
                      if (subtitle != null)
                        Text(
                          subtitle!,
                          style: Theme.of(context).textTheme.bodySmall,
                        ),
                    ],
                  ),
                ),
              ],
            ),
            const SizedBox(height: 16),
            ...children.map((child) => Padding(
              padding: const EdgeInsets.only(bottom: 16),
              child: child,
            )),
          ],
        ),
      ),
    );
  }
}

class CustomFormWidgetsPage extends StatefulWidget {
  const CustomFormWidgetsPage({super.key});

  @override
  State<CustomFormWidgetsPage> createState() => _CustomFormWidgetsPageState();
}

class _CustomFormWidgetsPageState extends State<CustomFormWidgetsPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  final _bioController = TextEditingController();
  
  String? _selectedCountry;
  String? _selectedSkill;
  bool _availableForWork = false;
  bool _remoteWork = true;
  double _experienceYears = 2.0;
  double _salaryExpectation = 75000;

  final List<String> _countries = [
    'United States', 'Canada', 'United Kingdom', 'Germany', 'France'
  ];
  
  final List<String> _skills = [
    'Flutter', 'React', 'Node.js', 'Python', 'Java'
  ];

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _passwordController.dispose();
    _bioController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Professional Profile'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              children: [
                CustomFormSection(
                  title: 'Personal Information',
                  subtitle: 'Your basic details',
                  icon: Icons.person,
                  accentColor: Colors.blue,
                  children: [
                    CustomTextFormField(
                      label: 'Full Name',
                      hint: 'Enter your full name',
                      controller: _nameController,
                      prefixIcon: Icons.account_circle,
                      accentColor: Colors.blue,
                      validator: (value) {
                        if (value == null || value.isEmpty) {
                          return 'Please enter your name';
                        }
                        return null;
                      },
                    ),
                    CustomTextFormField(
                      label: 'Email Address',
                      hint: 'your.email@example.com',
                      controller: _emailController,
                      prefixIcon: Icons.email,
                      keyboardType: TextInputType.emailAddress,
                      accentColor: Colors.blue,
                      validator: (value) {
                        if (value == null || value.isEmpty) {
                          return 'Please enter your email';
                        }
                        if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$')
                            .hasMatch(value)) {
                          return 'Please enter a valid email';
                        }
                        return null;
                      },
                    ),
                    CustomTextFormField(
                      label: 'Password',
                      controller: _passwordController,
                      prefixIcon: Icons.lock,
                      obscureText: true,
                      accentColor: Colors.blue,
                      validator: (value) {
                        if (value == null || value.length < 6) {
                          return 'Password must be at least 6 characters';
                        }
                        return null;
                      },
                    ),
                    CustomDropdownField<String>(
                      label: 'Country',
                      value: _selectedCountry,
                      prefixIcon: Icons.public,
                      accentColor: Colors.blue,
                      items: _countries.map((country) {
                        return DropdownMenuItem(
                          value: country,
                          child: Text(country),
                        );
                      }).toList(),
                      onChanged: (value) {
                        setState(() {
                          _selectedCountry = value;
                        });
                      },
                      validator: (value) {
                        if (value == null) {
                          return 'Please select your country';
                        }
                        return null;
                      },
                    ),
                  ],
                ),
                const SizedBox(height: 16),
                CustomFormSection(
                  title: 'Professional Details',
                  subtitle: 'Your skills and experience',
                  icon: Icons.work,
                  accentColor: Colors.green,
                  children: [
                    CustomDropdownField<String>(
                      label: 'Primary Skill',
                      value: _selectedSkill,
                      prefixIcon: Icons.code,
                      accentColor: Colors.green,
                      items: _skills.map((skill) {
                        return DropdownMenuItem(
                          value: skill,
                          child: Text(skill),
                        );
                      }).toList(),
                      onChanged: (value) {
                        setState(() {
                          _selectedSkill = value;
                        });
                      },
                    ),
                    CustomSliderField(
                      label: 'Years of Experience',
                      value: _experienceYears,
                      min: 0,
                      max: 20,
                      divisions: 20,
                      accentColor: Colors.green,
                      onChanged: (value) {
                        setState(() {
                          _experienceYears = value;
                        });
                      },
                      labelFormatter: (value) => '${value.toStringAsFixed(1)} years',
                    ),
                    CustomSliderField(
                      label: 'Salary Expectation',
                      value: _salaryExpectation,
                      min: 30000,
                      max: 200000,
                      divisions: 170,
                      accentColor: Colors.green,
                      onChanged: (value) {
                        setState(() {
                          _salaryExpectation = value;
                        });
                      },
                      labelFormatter: (value) => '\$${value.toInt().toString().replaceAllMapped(
                        RegExp(r'(\d{1,3})(?=(\d{3})+(?!\d))'), 
                        (Match m) => '${m[1]},'
                      )}',
                    ),
                    CustomTextFormField(
                      label: 'Professional Bio',
                      hint: 'Tell us about yourself...',
                      controller: _bioController,
                      maxLines: 4,
                      accentColor: Colors.green,
                    ),
                  ],
                ),
                const SizedBox(height: 16),
                CustomFormSection(
                  title: 'Work Preferences',
                  subtitle: 'Your availability and preferences',
                  icon: Icons.settings,
                  accentColor: Colors.orange,
                  children: [
                    CustomSwitchField(
                      title: 'Available for Work',
                      subtitle: 'Are you currently looking for opportunities?',
                      value: _availableForWork,
                      icon: Icons.work_outline,
                      accentColor: Colors.orange,
                      onChanged: (value) {
                        setState(() {
                          _availableForWork = value;
                        });
                      },
                    ),
                    CustomSwitchField(
                      title: 'Open to Remote Work',
                      subtitle: 'Would you consider remote positions?',
                      value: _remoteWork,
                      icon: Icons.home_work,
                      accentColor: Colors.orange,
                      onChanged: (value) {
                        setState(() {
                          _remoteWork = value;
                        });
                      },
                    ),
                  ],
                ),
                const SizedBox(height: 32),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton.icon(
                    onPressed: () {
                      if (_formKey.currentState!.validate()) {
                        _submitForm();
                      }
                    },
                    icon: const Icon(Icons.save),
                    label: const Text('Save Profile'),
                    style: ElevatedButton.styleFrom(
                      padding: const EdgeInsets.all(16),
                    ),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  void _submitForm() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Profile Saved'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Name: ${_nameController.text}'),
            Text('Email: ${_emailController.text}'),
            Text('Country: ${_selectedCountry ?? "Not selected"}'),
            Text('Skill: ${_selectedSkill ?? "Not selected"}'),
            Text('Experience: ${_experienceYears.toStringAsFixed(1)} years'),
            Text('Available: ${_availableForWork ? "Yes" : "No"}'),
          ],
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

Custom form widgets provide consistent styling and behavior across the  
application. Reusable components reduce code duplication and improve  
maintainability. Configurable properties enable flexibility while  
maintaining design consistency.  

## Form Serialization

Converting form data to JSON and handling data transformation for APIs.  

```dart
import 'package:flutter/material.dart';
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
      title: 'Form Serialization',
      home: const FormSerializationPage(),
    );
  }
}

class UserProfile {
  String firstName;
  String lastName;
  String email;
  int age;
  String? phoneNumber;
  Address address;
  List<String> skills;
  Map<String, dynamic> preferences;
  DateTime createdAt;

  UserProfile({
    required this.firstName,
    required this.lastName,
    required this.email,
    required this.age,
    this.phoneNumber,
    required this.address,
    required this.skills,
    required this.preferences,
    DateTime? createdAt,
  }) : createdAt = createdAt ?? DateTime.now();

  // Convert to JSON
  Map<String, dynamic> toJson() {
    return {
      'first_name': firstName,
      'last_name': lastName,
      'email': email,
      'age': age,
      'phone_number': phoneNumber,
      'address': address.toJson(),
      'skills': skills,
      'preferences': preferences,
      'created_at': createdAt.toIso8601String(),
    };
  }

  // Create from JSON
  factory UserProfile.fromJson(Map<String, dynamic> json) {
    return UserProfile(
      firstName: json['first_name'] ?? '',
      lastName: json['last_name'] ?? '',
      email: json['email'] ?? '',
      age: json['age'] ?? 0,
      phoneNumber: json['phone_number'],
      address: Address.fromJson(json['address'] ?? {}),
      skills: List<String>.from(json['skills'] ?? []),
      preferences: Map<String, dynamic>.from(json['preferences'] ?? {}),
      createdAt: DateTime.tryParse(json['created_at'] ?? '') ?? DateTime.now(),
    );
  }

  // Get full name
  String get fullName => '$firstName $lastName';

  // Validation
  bool get isValid {
    return firstName.isNotEmpty &&
           lastName.isNotEmpty &&
           email.isNotEmpty &&
           age > 0 &&
           address.isValid;
  }

  // Summary for display
  String get summary {
    return 'Name: $fullName, Age: $age, Skills: ${skills.length}';
  }
}

class Address {
  String street;
  String city;
  String state;
  String zipCode;
  String country;

  Address({
    required this.street,
    required this.city,
    required this.state,
    required this.zipCode,
    required this.country,
  });

  Map<String, dynamic> toJson() {
    return {
      'street': street,
      'city': city,
      'state': state,
      'zip_code': zipCode,
      'country': country,
    };
  }

  factory Address.fromJson(Map<String, dynamic> json) {
    return Address(
      street: json['street'] ?? '',
      city: json['city'] ?? '',
      state: json['state'] ?? '',
      zipCode: json['zip_code'] ?? '',
      country: json['country'] ?? '',
    );
  }

  bool get isValid {
    return street.isNotEmpty &&
           city.isNotEmpty &&
           state.isNotEmpty &&
           zipCode.isNotEmpty &&
           country.isNotEmpty;
  }

  String get formatted {
    return '$street, $city, $state $zipCode, $country';
  }
}

class FormSerializationPage extends StatefulWidget {
  const FormSerializationPage({super.key});

  @override
  State<FormSerializationPage> createState() => _FormSerializationPageState();
}

class _FormSerializationPageState extends State<FormSerializationPage> {
  final _formKey = GlobalKey<FormState>();
  final _firstNameController = TextEditingController();
  final _lastNameController = TextEditingController();
  final _emailController = TextEditingController();
  final _ageController = TextEditingController();
  final _phoneController = TextEditingController();
  final _streetController = TextEditingController();
  final _cityController = TextEditingController();
  final _stateController = TextEditingController();
  final _zipController = TextEditingController();
  final _skillController = TextEditingController();

  String _selectedCountry = 'United States';
  List<String> _skills = [];
  bool _emailNotifications = true;
  bool _smsNotifications = false;
  String _theme = 'dark';
  String _language = 'en';

  final List<String> _countries = [
    'United States', 'Canada', 'United Kingdom', 'Germany', 'France'
  ];

  UserProfile? _deserializedProfile;
  String _jsonOutput = '';

  @override
  void dispose() {
    _firstNameController.dispose();
    _lastNameController.dispose();
    _emailController.dispose();
    _ageController.dispose();
    _phoneController.dispose();
    _streetController.dispose();
    _cityController.dispose();
    _stateController.dispose();
    _zipController.dispose();
    _skillController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Profile Form'),
        actions: [
          IconButton(
            icon: const Icon(Icons.data_object),
            onPressed: _showJsonOutput,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              children: [
                _buildPersonalInfoSection(),
                const SizedBox(height: 16),
                _buildAddressSection(),
                const SizedBox(height: 16),
                _buildSkillsSection(),
                const SizedBox(height: 16),
                _buildPreferencesSection(),
                const SizedBox(height: 32),
                _buildActionButtons(),
                if (_deserializedProfile != null) ...[
                  const SizedBox(height: 24),
                  _buildDeserializedProfileSection(),
                ],
              ],
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildPersonalInfoSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Personal Information',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: TextFormField(
                    controller: _firstNameController,
                    decoration: const InputDecoration(
                      labelText: 'First Name *',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) => value?.isEmpty == true ? 'Required' : null,
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: TextFormField(
                    controller: _lastNameController,
                    decoration: const InputDecoration(
                      labelText: 'Last Name *',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) => value?.isEmpty == true ? 'Required' : null,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _emailController,
              decoration: const InputDecoration(
                labelText: 'Email *',
                border: OutlineInputBorder(),
              ),
              validator: (value) {
                if (value?.isEmpty == true) return 'Required';
                if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value!)) {
                  return 'Invalid email';
                }
                return null;
              },
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: TextFormField(
                    controller: _ageController,
                    keyboardType: TextInputType.number,
                    decoration: const InputDecoration(
                      labelText: 'Age *',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) {
                      if (value?.isEmpty == true) return 'Required';
                      int? age = int.tryParse(value!);
                      if (age == null || age < 18 || age > 100) {
                        return 'Age must be 18-100';
                      }
                      return null;
                    },
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: TextFormField(
                    controller: _phoneController,
                    decoration: const InputDecoration(
                      labelText: 'Phone (Optional)',
                      border: OutlineInputBorder(),
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

  Widget _buildAddressSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Address',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _streetController,
              decoration: const InputDecoration(
                labelText: 'Street Address *',
                border: OutlineInputBorder(),
              ),
              validator: (value) => value?.isEmpty == true ? 'Required' : null,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: TextFormField(
                    controller: _cityController,
                    decoration: const InputDecoration(
                      labelText: 'City *',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) => value?.isEmpty == true ? 'Required' : null,
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: TextFormField(
                    controller: _stateController,
                    decoration: const InputDecoration(
                      labelText: 'State *',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) => value?.isEmpty == true ? 'Required' : null,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: TextFormField(
                    controller: _zipController,
                    decoration: const InputDecoration(
                      labelText: 'ZIP Code *',
                      border: OutlineInputBorder(),
                    ),
                    validator: (value) => value?.isEmpty == true ? 'Required' : null,
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: DropdownButtonFormField<String>(
                    value: _selectedCountry,
                    decoration: const InputDecoration(
                      labelText: 'Country *',
                      border: OutlineInputBorder(),
                    ),
                    items: _countries.map((country) {
                      return DropdownMenuItem(value: country, child: Text(country));
                    }).toList(),
                    onChanged: (value) => setState(() => _selectedCountry = value!),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSkillsSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Skills',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: TextFormField(
                    controller: _skillController,
                    decoration: const InputDecoration(
                      labelText: 'Add Skill',
                      border: OutlineInputBorder(),
                    ),
                    onSubmitted: _addSkill,
                  ),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () => _addSkill(_skillController.text),
                  child: const Text('Add'),
                ),
              ],
            ),
            if (_skills.isNotEmpty) ...[
              const SizedBox(height: 16),
              Wrap(
                spacing: 8,
                children: _skills.map((skill) {
                  return Chip(
                    label: Text(skill),
                    deleteIcon: const Icon(Icons.close, size: 18),
                    onDeleted: () => _removeSkill(skill),
                  );
                }).toList(),
              ),
            ],
          ],
        ),
      ),
    );
  }

  Widget _buildPreferencesSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Preferences',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            SwitchListTile(
              title: const Text('Email Notifications'),
              value: _emailNotifications,
              onChanged: (value) => setState(() => _emailNotifications = value),
            ),
            SwitchListTile(
              title: const Text('SMS Notifications'),
              value: _smsNotifications,
              onChanged: (value) => setState(() => _smsNotifications = value),
            ),
            ListTile(
              title: const Text('Theme'),
              trailing: DropdownButton<String>(
                value: _theme,
                items: const [
                  DropdownMenuItem(value: 'light', child: Text('Light')),
                  DropdownMenuItem(value: 'dark', child: Text('Dark')),
                  DropdownMenuItem(value: 'auto', child: Text('Auto')),
                ],
                onChanged: (value) => setState(() => _theme = value!),
              ),
            ),
            ListTile(
              title: const Text('Language'),
              trailing: DropdownButton<String>(
                value: _language,
                items: const [
                  DropdownMenuItem(value: 'en', child: Text('English')),
                  DropdownMenuItem(value: 'es', child: Text('Spanish')),
                  DropdownMenuItem(value: 'fr', child: Text('French')),
                ],
                onChanged: (value) => setState(() => _language = value!),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildActionButtons() {
    return Column(
      children: [
        Row(
          children: [
            Expanded(
              child: ElevatedButton.icon(
                onPressed: _serializeForm,
                icon: const Icon(Icons.upload),
                label: const Text('Serialize to JSON'),
              ),
            ),
            const SizedBox(width: 16),
            Expanded(
              child: OutlinedButton.icon(
                onPressed: _deserializeFromJson,
                icon: const Icon(Icons.download),
                label: const Text('Load from JSON'),
              ),
            ),
          ],
        ),
        const SizedBox(height: 16),
        SizedBox(
          width: double.infinity,
          child: ElevatedButton.icon(
            onPressed: () {
              if (_formKey.currentState!.validate()) {
                _submitForm();
              }
            },
            icon: const Icon(Icons.save),
            label: const Text('Save Profile'),
          ),
        ),
      ],
    );
  }

  Widget _buildDeserializedProfileSection() {
    return Card(
      color: Colors.green[900]?.withOpacity(0.3),
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Deserialized Profile',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Text('Full Name: ${_deserializedProfile!.fullName}'),
            Text('Email: ${_deserializedProfile!.email}'),
            Text('Age: ${_deserializedProfile!.age}'),
            Text('Address: ${_deserializedProfile!.address.formatted}'),
            Text('Skills: ${_deserializedProfile!.skills.join(', ')}'),
            Text('Valid: ${_deserializedProfile!.isValid}'),
            Text('Created: ${_deserializedProfile!.createdAt}'),
          ],
        ),
      ),
    );
  }

  void _addSkill(String skill) {
    if (skill.trim().isNotEmpty && !_skills.contains(skill.trim())) {
      setState(() {
        _skills.add(skill.trim());
        _skillController.clear();
      });
    }
  }

  void _removeSkill(String skill) {
    setState(() {
      _skills.remove(skill);
    });
  }

  void _serializeForm() {
    if (_formKey.currentState!.validate()) {
      UserProfile profile = UserProfile(
        firstName: _firstNameController.text,
        lastName: _lastNameController.text,
        email: _emailController.text,
        age: int.parse(_ageController.text),
        phoneNumber: _phoneController.text.isNotEmpty ? _phoneController.text : null,
        address: Address(
          street: _streetController.text,
          city: _cityController.text,
          state: _stateController.text,
          zipCode: _zipController.text,
          country: _selectedCountry,
        ),
        skills: _skills,
        preferences: {
          'email_notifications': _emailNotifications,
          'sms_notifications': _smsNotifications,
          'theme': _theme,
          'language': _language,
        },
      );

      setState(() {
        _jsonOutput = const JsonEncoder.withIndent('  ').convert(profile.toJson());
      });

      _showJsonOutput();
    }
  }

  void _deserializeFromJson() {
    // Sample JSON data
    String sampleJson = '''
    {
      "first_name": "John",
      "last_name": "Doe",
      "email": "john.doe@example.com",
      "age": 30,
      "phone_number": "+1-555-0123",
      "address": {
        "street": "123 Main St",
        "city": "Anytown",
        "state": "CA",
        "zip_code": "12345",
        "country": "United States"
      },
      "skills": ["Flutter", "Dart", "JavaScript"],
      "preferences": {
        "email_notifications": true,
        "sms_notifications": false,
        "theme": "dark",
        "language": "en"
      },
      "created_at": "2024-01-01T12:00:00.000Z"
    }
    ''';

    try {
      Map<String, dynamic> json = jsonDecode(sampleJson);
      UserProfile profile = UserProfile.fromJson(json);
      
      setState(() {
        _deserializedProfile = profile;
        // Populate form with deserialized data
        _firstNameController.text = profile.firstName;
        _lastNameController.text = profile.lastName;
        _emailController.text = profile.email;
        _ageController.text = profile.age.toString();
        _phoneController.text = profile.phoneNumber ?? '';
        _streetController.text = profile.address.street;
        _cityController.text = profile.address.city;
        _stateController.text = profile.address.state;
        _zipController.text = profile.address.zipCode;
        _selectedCountry = profile.address.country;
        _skills = List.from(profile.skills);
        _emailNotifications = profile.preferences['email_notifications'] ?? true;
        _smsNotifications = profile.preferences['sms_notifications'] ?? false;
        _theme = profile.preferences['theme'] ?? 'dark';
        _language = profile.preferences['language'] ?? 'en';
      });

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Profile loaded from JSON successfully!'),
          backgroundColor: Colors.green,
        ),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text('Error deserializing JSON: $e'),
          backgroundColor: Colors.red,
        ),
      );
    }
  }

  void _showJsonOutput() {
    if (_jsonOutput.isEmpty) return;

    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Serialized JSON'),
        content: SizedBox(
          width: double.maxFinite,
          child: SingleChildScrollView(
            child: SelectableText(
              _jsonOutput,
              style: const TextStyle(fontFamily: 'monospace'),
            ),
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }

  void _submitForm() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Profile Saved'),
        content: const Text('Your profile has been saved successfully!'),
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

Form serialization enables data persistence and API communication through  
JSON conversion. Data models with toJson() and fromJson() methods provide  
structured data handling. Validation ensures data integrity during  
serialization and deserialization processes.  

## Responsive Forms

Creating adaptive form layouts that work across different screen sizes.  

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
      title: 'Responsive Forms',
      home: const ResponsiveFormsPage(),
    );
  }
}

class ResponsiveFormsPage extends StatefulWidget {
  const ResponsiveFormsPage({super.key});

  @override
  State<ResponsiveFormsPage> createState() => _ResponsiveFormsPageState();
}

class _ResponsiveFormsPageState extends State<ResponsiveFormsPage> {
  final _formKey = GlobalKey<FormState>();
  final _firstNameController = TextEditingController();
  final _lastNameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  final _companyController = TextEditingController();
  final _jobTitleController = TextEditingController();
  final _websiteController = TextEditingController();
  final _messageController = TextEditingController();
  
  String? _selectedCountry;
  String? _selectedIndustry;
  bool _subscribeNewsletter = false;

  final List<String> _countries = [
    'United States', 'Canada', 'United Kingdom', 'Germany', 'France'
  ];
  
  final List<String> _industries = [
    'Technology', 'Healthcare', 'Finance', 'Education', 'Manufacturing'
  ];

  @override
  void dispose() {
    _firstNameController.dispose();
    _lastNameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    _companyController.dispose();
    _jobTitleController.dispose();
    _websiteController.dispose();
    _messageController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Contact Form'),
      ),
      body: LayoutBuilder(
        builder: (context, constraints) {
          bool isMobile = constraints.maxWidth < 600;
          bool isTablet = constraints.maxWidth >= 600 && constraints.maxWidth < 1200;
          bool isDesktop = constraints.maxWidth >= 1200;

          return SingleChildScrollView(
            padding: EdgeInsets.all(isMobile ? 16 : 32),
            child: Center(
              child: ConstrainedBox(
                constraints: BoxConstraints(
                  maxWidth: isDesktop ? 800 : double.infinity,
                ),
                child: Form(
                  key: _formKey,
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Get in Touch',
                        style: TextStyle(
                          fontSize: isMobile ? 24 : isTablet ? 28 : 32,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      const SizedBox(height: 8),
                      Text(
                        'We\'d love to hear from you. Fill out the form below.',
                        style: TextStyle(
                          fontSize: isMobile ? 14 : 16,
                          color: Colors.grey[400],
                        ),
                      ),
                      const SizedBox(height: 32),
                      _buildPersonalInfoSection(isMobile, isTablet),
                      const SizedBox(height: 24),
                      _buildProfessionalInfoSection(isMobile, isTablet),
                      const SizedBox(height: 24),
                      _buildMessageSection(),
                      const SizedBox(height: 32),
                      _buildSubmitSection(isMobile),
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

  Widget _buildPersonalInfoSection(bool isMobile, bool isTablet) {
    return Card(
      child: Padding(
        padding: EdgeInsets.all(isMobile ? 16 : 24),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Personal Information',
              style: TextStyle(
                fontSize: isMobile ? 18 : 20,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 16),
            if (isMobile) ..._buildMobilePersonalFields()
            else ..._buildDesktopPersonalFields(),
          ],
        ),
      ),
    );
  }

  List<Widget> _buildMobilePersonalFields() {
    return [
      TextFormField(
        controller: _firstNameController,
        decoration: const InputDecoration(
          labelText: 'First Name *',
          border: OutlineInputBorder(),
        ),
        validator: (value) => value?.isEmpty == true ? 'Required' : null,
      ),
      const SizedBox(height: 16),
      TextFormField(
        controller: _lastNameController,
        decoration: const InputDecoration(
          labelText: 'Last Name *',
          border: OutlineInputBorder(),
        ),
        validator: (value) => value?.isEmpty == true ? 'Required' : null,
      ),
      const SizedBox(height: 16),
      TextFormField(
        controller: _emailController,
        keyboardType: TextInputType.emailAddress,
        decoration: const InputDecoration(
          labelText: 'Email Address *',
          border: OutlineInputBorder(),
        ),
        validator: (value) {
          if (value?.isEmpty == true) return 'Required';
          if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value!)) {
            return 'Invalid email';
          }
          return null;
        },
      ),
      const SizedBox(height: 16),
      TextFormField(
        controller: _phoneController,
        keyboardType: TextInputType.phone,
        decoration: const InputDecoration(
          labelText: 'Phone Number',
          border: OutlineInputBorder(),
        ),
      ),
      const SizedBox(height: 16),
      DropdownButtonFormField<String>(
        value: _selectedCountry,
        decoration: const InputDecoration(
          labelText: 'Country *',
          border: OutlineInputBorder(),
        ),
        items: _countries.map((country) {
          return DropdownMenuItem(value: country, child: Text(country));
        }).toList(),
        onChanged: (value) => setState(() => _selectedCountry = value),
        validator: (value) => value == null ? 'Required' : null,
      ),
    ];
  }

  List<Widget> _buildDesktopPersonalFields() {
    return [
      Row(
        children: [
          Expanded(
            child: TextFormField(
              controller: _firstNameController,
              decoration: const InputDecoration(
                labelText: 'First Name *',
                border: OutlineInputBorder(),
              ),
              validator: (value) => value?.isEmpty == true ? 'Required' : null,
            ),
          ),
          const SizedBox(width: 16),
          Expanded(
            child: TextFormField(
              controller: _lastNameController,
              decoration: const InputDecoration(
                labelText: 'Last Name *',
                border: OutlineInputBorder(),
              ),
              validator: (value) => value?.isEmpty == true ? 'Required' : null,
            ),
          ),
        ],
      ),
      const SizedBox(height: 16),
      Row(
        children: [
          Expanded(
            flex: 2,
            child: TextFormField(
              controller: _emailController,
              keyboardType: TextInputType.emailAddress,
              decoration: const InputDecoration(
                labelText: 'Email Address *',
                border: OutlineInputBorder(),
              ),
              validator: (value) {
                if (value?.isEmpty == true) return 'Required';
                if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value!)) {
                  return 'Invalid email';
                }
                return null;
              },
            ),
          ),
          const SizedBox(width: 16),
          Expanded(
            child: TextFormField(
              controller: _phoneController,
              keyboardType: TextInputType.phone,
              decoration: const InputDecoration(
                labelText: 'Phone Number',
                border: OutlineInputBorder(),
              ),
            ),
          ),
        ],
      ),
      const SizedBox(height: 16),
      DropdownButtonFormField<String>(
        value: _selectedCountry,
        decoration: const InputDecoration(
          labelText: 'Country *',
          border: OutlineInputBorder(),
        ),
        items: _countries.map((country) {
          return DropdownMenuItem(value: country, child: Text(country));
        }).toList(),
        onChanged: (value) => setState(() => _selectedCountry = value),
        validator: (value) => value == null ? 'Required' : null,
      ),
    ];
  }

  Widget _buildProfessionalInfoSection(bool isMobile, bool isTablet) {
    return Card(
      child: Padding(
        padding: EdgeInsets.all(isMobile ? 16 : 24),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Professional Information',
              style: TextStyle(
                fontSize: isMobile ? 18 : 20,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 16),
            if (isMobile) ..._buildMobileProfessionalFields()
            else ..._buildDesktopProfessionalFields(),
          ],
        ),
      ),
    );
  }

  List<Widget> _buildMobileProfessionalFields() {
    return [
      TextFormField(
        controller: _companyController,
        decoration: const InputDecoration(
          labelText: 'Company Name',
          border: OutlineInputBorder(),
        ),
      ),
      const SizedBox(height: 16),
      TextFormField(
        controller: _jobTitleController,
        decoration: const InputDecoration(
          labelText: 'Job Title',
          border: OutlineInputBorder(),
        ),
      ),
      const SizedBox(height: 16),
      DropdownButtonFormField<String>(
        value: _selectedIndustry,
        decoration: const InputDecoration(
          labelText: 'Industry',
          border: OutlineInputBorder(),
        ),
        items: _industries.map((industry) {
          return DropdownMenuItem(value: industry, child: Text(industry));
        }).toList(),
        onChanged: (value) => setState(() => _selectedIndustry = value),
      ),
      const SizedBox(height: 16),
      TextFormField(
        controller: _websiteController,
        keyboardType: TextInputType.url,
        decoration: const InputDecoration(
          labelText: 'Website',
          border: OutlineInputBorder(),
        ),
      ),
    ];
  }

  List<Widget> _buildDesktopProfessionalFields() {
    return [
      Row(
        children: [
          Expanded(
            child: TextFormField(
              controller: _companyController,
              decoration: const InputDecoration(
                labelText: 'Company Name',
                border: OutlineInputBorder(),
              ),
            ),
          ),
          const SizedBox(width: 16),
          Expanded(
            child: TextFormField(
              controller: _jobTitleController,
              decoration: const InputDecoration(
                labelText: 'Job Title',
                border: OutlineInputBorder(),
              ),
            ),
          ),
        ],
      ),
      const SizedBox(height: 16),
      Row(
        children: [
          Expanded(
            child: DropdownButtonFormField<String>(
              value: _selectedIndustry,
              decoration: const InputDecoration(
                labelText: 'Industry',
                border: OutlineInputBorder(),
              ),
              items: _industries.map((industry) {
                return DropdownMenuItem(value: industry, child: Text(industry));
              }).toList(),
              onChanged: (value) => setState(() => _selectedIndustry = value),
            ),
          ),
          const SizedBox(width: 16),
          Expanded(
            child: TextFormField(
              controller: _websiteController,
              keyboardType: TextInputType.url,
              decoration: const InputDecoration(
                labelText: 'Website',
                border: OutlineInputBorder(),
              ),
            ),
          ),
        ],
      ),
    ];
  }

  Widget _buildMessageSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Your Message',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _messageController,
              maxLines: 6,
              decoration: const InputDecoration(
                labelText: 'Message *',
                border: OutlineInputBorder(),
                alignLabelWithHint: true,
                hintText: 'Tell us about your project or inquiry...',
              ),
              validator: (value) => value?.isEmpty == true ? 'Required' : null,
            ),
            const SizedBox(height: 16),
            CheckboxListTile(
              title: const Text('Subscribe to our newsletter'),
              subtitle: const Text('Get updates about new features and tips'),
              value: _subscribeNewsletter,
              onChanged: (value) => setState(() => _subscribeNewsletter = value ?? false),
              contentPadding: EdgeInsets.zero,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSubmitSection(bool isMobile) {
    return SizedBox(
      width: double.infinity,
      child: ElevatedButton(
        onPressed: () {
          if (_formKey.currentState!.validate()) {
            _submitForm();
          }
        },
        style: ElevatedButton.styleFrom(
          padding: EdgeInsets.all(isMobile ? 16 : 20),
        ),
        child: Text(
          'Send Message',
          style: TextStyle(fontSize: isMobile ? 16 : 18),
        ),
      ),
    );
  }

  void _submitForm() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Message Sent'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Thank you ${_firstNameController.text}!'),
            const SizedBox(height: 8),
            const Text('Your message has been sent successfully. We\'ll get back to you soon.'),
          ],
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

Responsive forms adapt to different screen sizes using LayoutBuilder  
and conditional layouts. Mobile layouts stack fields vertically while  
desktop layouts use rows for efficiency. Flexible sizing and padding  
ensure optimal user experience across all device types.  

## Advanced Validation

Implementing complex business rules and cross-field validation logic.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Advanced Validation',
      home: const AdvancedValidationPage(),
    );
  }
}

class ValidationRules {
  // Password strength validation
  static String? passwordStrength(String? password) {
    if (password == null || password.isEmpty) {
      return 'Password is required';
    }
    
    List<String> errors = [];
    
    if (password.length < 8) {
      errors.add('at least 8 characters');
    }
    if (!RegExp(r'[A-Z]').hasMatch(password)) {
      errors.add('one uppercase letter');
    }
    if (!RegExp(r'[a-z]').hasMatch(password)) {
      errors.add('one lowercase letter');
    }
    if (!RegExp(r'\d').hasMatch(password)) {
      errors.add('one number');
    }
    if (!RegExp(r'[!@#$%^&*(),.?":{}|<>]').hasMatch(password)) {
      errors.add('one special character');
    }
    
    // Check for common weak passwords
    List<String> commonPasswords = [
      'password', 'password123', '123456', 'qwerty', 'abc123'
    ];
    if (commonPasswords.contains(password.toLowerCase())) {
      errors.add('not a common password');
    }
    
    if (errors.isNotEmpty) {
      return 'Password must contain ${errors.join(', ')}';
    }
    
    return null;
  }
  
  // Age validation with business rules
  static String? ageValidation(String? value, {bool requireAdult = false}) {
    if (value == null || value.isEmpty) {
      return 'Age is required';
    }
    
    int? age = int.tryParse(value);
    if (age == null) {
      return 'Please enter a valid age';
    }
    
    if (age < 0 || age > 150) {
      return 'Age must be between 0 and 150';
    }
    
    if (requireAdult && age < 18) {
      return 'Must be 18 or older';
    }
    
    return null;
  }
  
  // Credit card validation with Luhn algorithm
  static String? creditCardValidation(String? value) {
    if (value == null || value.isEmpty) {
      return 'Credit card number is required';
    }
    
    String cleanNumber = value.replaceAll(RegExp(r'\D'), '');
    
    if (cleanNumber.length < 13 || cleanNumber.length > 19) {
      return 'Credit card number must be 13-19 digits';
    }
    
    // Luhn algorithm
    int sum = 0;
    bool alternate = false;
    
    for (int i = cleanNumber.length - 1; i >= 0; i--) {
      int digit = int.parse(cleanNumber[i]);
      
      if (alternate) {
        digit *= 2;
        if (digit > 9) {
          digit = (digit % 10) + 1;
        }
      }
      
      sum += digit;
      alternate = !alternate;
    }
    
    if (sum % 10 != 0) {
      return 'Invalid credit card number';
    }
    
    return null;
  }
  
  // Social Security Number validation
  static String? ssnValidation(String? value) {
    if (value == null || value.isEmpty) {
      return 'SSN is required';
    }
    
    String cleanSSN = value.replaceAll(RegExp(r'[^0-9]'), '');
    
    if (cleanSSN.length != 9) {
      return 'SSN must be 9 digits';
    }
    
    // Check for invalid patterns
    if (cleanSSN == '000000000' ||
        cleanSSN == '111111111' ||
        cleanSSN == '222222222' ||
        cleanSSN == '333333333' ||
        cleanSSN == '444444444' ||
        cleanSSN == '555555555' ||
        cleanSSN == '666666666' ||
        cleanSSN == '777777777' ||
        cleanSSN == '888888888' ||
        cleanSSN == '999999999') {
      return 'Invalid SSN pattern';
    }
    
    // Area number can't be 000 or 666
    String area = cleanSSN.substring(0, 3);
    if (area == '000' || area == '666') {
      return 'Invalid SSN area number';
    }
    
    // Group number can't be 00
    String group = cleanSSN.substring(3, 5);
    if (group == '00') {
      return 'Invalid SSN group number';
    }
    
    // Serial number can't be 0000
    String serial = cleanSSN.substring(5, 9);
    if (serial == '0000') {
      return 'Invalid SSN serial number';
    }
    
    return null;
  }
}

class AdvancedValidationPage extends StatefulWidget {
  const AdvancedValidationPage({super.key});

  @override
  State<AdvancedValidationPage> createState() => _AdvancedValidationPageState();
}

class _AdvancedValidationPageState extends State<AdvancedValidationPage> {
  final _formKey = GlobalKey<FormState>();
  final _usernameController = TextEditingController();
  final _passwordController = TextEditingController();
  final _confirmPasswordController = TextEditingController();
  final _emailController = TextEditingController();
  final _ageController = TextEditingController();
  final _creditCardController = TextEditingController();
  final _ssnController = TextEditingController();
  final _startDateController = TextEditingController();
  final _endDateController = TextEditingController();
  final _salaryController = TextEditingController();
  
  bool _requiresAdult = false;
  String _accountType = 'personal';
  DateTime? _selectedStartDate;
  DateTime? _selectedEndDate;

  @override
  void dispose() {
    _usernameController.dispose();
    _passwordController.dispose();
    _confirmPasswordController.dispose();
    _emailController.dispose();
    _ageController.dispose();
    _creditCardController.dispose();
    _ssnController.dispose();
    _startDateController.dispose();
    _endDateController.dispose();
    _salaryController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Advanced Validation Form'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              children: [
                _buildAccountSection(),
                const SizedBox(height: 24),
                _buildPersonalInfoSection(),
                const SizedBox(height: 24),
                _buildPaymentSection(),
                const SizedBox(height: 24),
                _buildEmploymentSection(),
                const SizedBox(height: 32),
                _buildSubmitButton(),
              ],
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildAccountSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Account Information',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            DropdownButtonFormField<String>(
              value: _accountType,
              decoration: const InputDecoration(
                labelText: 'Account Type',
                border: OutlineInputBorder(),
              ),
              items: const [
                DropdownMenuItem(value: 'personal', child: Text('Personal')),
                DropdownMenuItem(value: 'business', child: Text('Business')),
                DropdownMenuItem(value: 'premium', child: Text('Premium')),
              ],
              onChanged: (value) {
                setState(() {
                  _accountType = value!;
                  _requiresAdult = value != 'personal';
                });
              },
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _usernameController,
              decoration: const InputDecoration(
                labelText: 'Username',
                border: OutlineInputBorder(),
                helperText: '3-20 characters, letters and numbers only',
              ),
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Username is required';
                }
                if (value.length < 3 || value.length > 20) {
                  return 'Username must be 3-20 characters';
                }
                if (!RegExp(r'^[a-zA-Z0-9]+$').hasMatch(value)) {
                  return 'Username can only contain letters and numbers';
                }
                return null;
              },
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _passwordController,
              obscureText: true,
              decoration: const InputDecoration(
                labelText: 'Password',
                border: OutlineInputBorder(),
                helperText: 'Complex password required',
              ),
              validator: ValidationRules.passwordStrength,
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _confirmPasswordController,
              obscureText: true,
              decoration: const InputDecoration(
                labelText: 'Confirm Password',
                border: OutlineInputBorder(),
              ),
              validator: (value) {
                if (value != _passwordController.text) {
                  return 'Passwords do not match';
                }
                return null;
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildPersonalInfoSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Personal Information',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _emailController,
              keyboardType: TextInputType.emailAddress,
              decoration: const InputDecoration(
                labelText: 'Email Address',
                border: OutlineInputBorder(),
              ),
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Email is required';
                }
                if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value)) {
                  return 'Invalid email format';
                }
                // Business accounts require corporate email
                if (_accountType == 'business' && 
                    (value.contains('@gmail.com') || 
                     value.contains('@yahoo.com') || 
                     value.contains('@hotmail.com'))) {
                  return 'Business accounts require corporate email';
                }
                return null;
              },
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _ageController,
              keyboardType: TextInputType.number,
              decoration: InputDecoration(
                labelText: 'Age',
                border: const OutlineInputBorder(),
                helperText: _requiresAdult ? 'Must be 18 or older' : null,
              ),
              validator: (value) => ValidationRules.ageValidation(
                value,
                requireAdult: _requiresAdult,
              ),
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _ssnController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: 'Social Security Number',
                border: OutlineInputBorder(),
                hintText: '123-45-6789',
              ),
              inputFormatters: [
                FilteringTextInputFormatter.digitsOnly,
                LengthLimitingTextInputFormatter(9),
              ],
              validator: ValidationRules.ssnValidation,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildPaymentSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Payment Information',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _creditCardController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: 'Credit Card Number',
                border: OutlineInputBorder(),
                hintText: '1234 5678 9012 3456',
              ),
              inputFormatters: [
                FilteringTextInputFormatter.digitsOnly,
                LengthLimitingTextInputFormatter(19),
              ],
              validator: ValidationRules.creditCardValidation,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildEmploymentSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Employment Period',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: TextFormField(
                    controller: _startDateController,
                    decoration: const InputDecoration(
                      labelText: 'Start Date',
                      border: OutlineInputBorder(),
                      suffixIcon: Icon(Icons.calendar_today),
                    ),
                    readOnly: true,
                    onTap: () => _selectStartDate(),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Start date required';
                      }
                      return null;
                    },
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: TextFormField(
                    controller: _endDateController,
                    decoration: const InputDecoration(
                      labelText: 'End Date',
                      border: OutlineInputBorder(),
                      suffixIcon: Icon(Icons.calendar_today),
                    ),
                    readOnly: true,
                    onTap: () => _selectEndDate(),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'End date required';
                      }
                      if (_selectedStartDate != null && _selectedEndDate != null) {
                        if (_selectedEndDate!.isBefore(_selectedStartDate!)) {
                          return 'End date must be after start date';
                        }
                        // Check minimum employment period
                        Duration difference = _selectedEndDate!.difference(_selectedStartDate!);
                        if (difference.inDays < 30) {
                          return 'Employment period must be at least 30 days';
                        }
                      }
                      return null;
                    },
                  ),
                ),
              ],
            ),
            const SizedBox(height: 16),
            TextFormField(
              controller: _salaryController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: 'Annual Salary',
                border: OutlineInputBorder(),
                prefixText: '\$',
              ),
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Salary is required';
                }
                double? salary = double.tryParse(value);
                if (salary == null) {
                  return 'Please enter a valid salary';
                }
                if (salary < 0) {
                  return 'Salary cannot be negative';
                }
                // Business accounts require minimum salary
                if (_accountType == 'business' && salary < 50000) {
                  return 'Business accounts require minimum \$50,000 salary';
                }
                return null;
              },
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSubmitButton() {
    return SizedBox(
      width: double.infinity,
      child: ElevatedButton(
        onPressed: () {
          if (_formKey.currentState!.validate()) {
            _submitForm();
          }
        },
        child: const Text('Submit Application'),
      ),
    );
  }

  Future<void> _selectStartDate() async {
    DateTime? picked = await showDatePicker(
      context: context,
      initialDate: DateTime.now(),
      firstDate: DateTime(2000),
      lastDate: DateTime.now(),
    );
    
    if (picked != null) {
      setState(() {
        _selectedStartDate = picked;
        _startDateController.text = '${picked.day}/${picked.month}/${picked.year}';
      });
    }
  }

  Future<void> _selectEndDate() async {
    DateTime? picked = await showDatePicker(
      context: context,
      initialDate: _selectedStartDate?.add(const Duration(days: 365)) ?? DateTime.now(),
      firstDate: _selectedStartDate ?? DateTime(2000),
      lastDate: DateTime.now().add(const Duration(days: 365)),
    );
    
    if (picked != null) {
      setState(() {
        _selectedEndDate = picked;
        _endDateController.text = '${picked.day}/${picked.month}/${picked.year}';
      });
    }
  }

  void _submitForm() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Application Submitted'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Account Type: $_accountType'),
            Text('Username: ${_usernameController.text}'),
            Text('Email: ${_emailController.text}'),
            Text('Age: ${_ageController.text}'),
            const Text('\nAll validation rules passed successfully!'),
          ],
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

Advanced validation implements complex business rules and cross-field  
dependencies. Custom validation functions encapsulate domain-specific  
logic like credit card validation and SSN verification. Conditional  
validation adapts requirements based on user selections and context.  
