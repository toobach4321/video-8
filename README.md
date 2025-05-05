import 'package:flutter/material.dart';

void main() => runApp(const BMICalculatorApp());

class BMICalculatorApp extends StatelessWidget {
  const BMICalculatorApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'BMI Calculator',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
        useMaterial3: true,
      ),
      home: const BMICalculatorPage(),
    );
  }
}

class BMICalculatorPage extends StatefulWidget {
  const BMICalculatorPage({super.key});

  @override
  State<BMICalculatorPage> createState() => _BMICalculatorPageState();
}

class _BMICalculatorPageState extends State<BMICalculatorPage> {
  final TextEditingController _weightController = TextEditingController();
  final TextEditingController _heightController = TextEditingController();
  String _bmiResult = '';

  void _calculateBMI() {
    final weight = double.tryParse(_weightController.text.trim());
    final heightCm = double.tryParse(_heightController.text.trim());

    if (weight == null || heightCm == null || weight <= 0 || heightCm <= 0) {
      setState(() => _bmiResult = 'Please enter valid values.');
      return;
    }

    final bmi = weight / ((heightCm / 100) * (heightCm / 100));
    String category;

    if (bmi < 18.5) {
      category = 'Underweight';
    } else if (bmi < 24.9) {
      category = 'Normal weight';
    } else if (bmi < 29.9) {
      category = 'Overweight';
    } else {
      category = 'Obese';
    }

    setState(() {
      _bmiResult = 'Your BMI is ${bmi.toStringAsFixed(1)} ($category)';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('BMI Calculator'),
        centerTitle: true,
      ),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          children: [
            _buildLabeledInput(Icons.monitor_weight, "Weight (kg)", _weightController),
            const SizedBox(height: 20),
            _buildLabeledInput(Icons.height, "Height (cm)", _heightController),
            const SizedBox(height: 30),
            GestureDetector(
              onTap: _calculateBMI,
              child: Container(
                width: double.infinity,
                padding: const EdgeInsets.symmetric(vertical: 16),
                decoration: BoxDecoration(
                  color: Colors.teal,
                  borderRadius: BorderRadius.circular(12),
                ),
                child: const Center(
                  child: Text(
                    'Calculate BMI',
                    style: TextStyle(color: Colors.white, fontSize: 16, fontWeight: FontWeight.bold),
                  ),
                ),
              ),
            ),
            const SizedBox(height: 24),
            Text(
              _bmiResult,
              style: const TextStyle(fontSize: 18, fontWeight: FontWeight.w600),
              textAlign: TextAlign.center,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildLabeledInput(IconData icon, String label, TextEditingController controller) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Row(
          children: [
            Icon(icon, color: Theme.of(context).colorScheme.primary),
            const SizedBox(width: 8),
            Text(label, style: const TextStyle(fontSize: 16, fontWeight: FontWeight.w500)),
          ],
        ),
        const SizedBox(height: 8),
        TextField(
          controller: controller,
          keyboardType: TextInputType.number,
          decoration: const InputDecoration(
            border: OutlineInputBorder(),
            hintText: 'Enter value',
          ),
        ),
      ],
    );
  }
}
