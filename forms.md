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
