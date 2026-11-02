import 'package:flutter/material.dart';

void main() {
  runApp(const BrudyApp());
}

/// ===================== WARNA =====================
class AppColors {
  static const background = Color(0xFFE6E8F1); // latar belakang abu kebiruan
  static const title = Color(0xFF1E2B50); // judul & border input
  static const label = Color(0xFF4A5578); // label & teks sekunder
  static const gradientStart = Color(0xFF202F55); // tombol (kiri)
  static const gradientEnd = Color(0xFF4565B8); // tombol (kanan)
}

/// ===================== APP ROOT =====================
class BrudyApp extends StatelessWidget {
  const BrudyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'BRUDY',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        useMaterial3: true,
        scaffoldBackgroundColor: AppColors.background,
      ),
      home: const LoginPage(),
    );
  }
}

/// ===================== HALAMAN LOGIN =====================
class LoginPage extends StatefulWidget {
  const LoginPage({super.key});

  @override
  State<LoginPage> createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {
  final _formKey = GlobalKey<FormState>();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  bool _obscurePassword = true;
  bool _isLoading = false;

  @override
  void dispose() {
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  Future<void> _handleLogin() async {
    if (!_formKey.currentState!.validate()) return;

    setState(() => _isLoading = true);

    // TODO: ganti dengan pemanggilan API / Firebase Auth kamu
    await Future.delayed(const Duration(seconds: 1));

    if (!mounted) return;
    setState(() => _isLoading = false);

    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text('Login sebagai ${_emailController.text}')),
    );
  }

  void _goToRegister() {
    // TODO: Navigator.push(context, MaterialPageRoute(builder: (_) => const RegisterPage()));
    ScaffoldMessenger.of(
      context,
    ).showSnackBar(const SnackBar(content: Text('Menuju halaman Buat Akun')));
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Center(
          child: SingleChildScrollView(
            padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 24),
            child: ConstrainedBox(
              constraints: const BoxConstraints(maxWidth: 420),
              child: Form(
                key: _formKey,
                child: Column(
                  mainAxisSize: MainAxisSize.min,
                  crossAxisAlignment: CrossAxisAlignment.stretch,
                  children: [
                    // ---------- Logo ----------
                    Center(
                      child: Image.asset(
                        'assets/images/logo_brudy.png',
                        width: 124,
                        height: 124,
                        errorBuilder: (_, __, ___) => const Icon(
                          Icons.school,
                          size: 100,
                          color: AppColors.title,
                        ),
                      ),
                    ),
                    const SizedBox(height: 16),

                    // ---------- Judul ----------
                    const Text(
                      'Login',
                      textAlign: TextAlign.center,
                      style: TextStyle(
                        fontSize: 40,
                        fontWeight: FontWeight.w800,
                        color: AppColors.title,
                      ),
                    ),
                    const SizedBox(height: 40),

                    // ---------- Email / Username ----------
                    const _FieldLabel('Email Kampus / Username'),
                    const SizedBox(height: 8),
                    _RoundedField(
                      controller: _emailController,
                      keyboardType: TextInputType.emailAddress,
                      textInputAction: TextInputAction.next,
                      validator: (v) => (v == null || v.trim().isEmpty)
                          ? 'Email / username wajib diisi'
                          : null,
                    ),
                    const SizedBox(height: 20),

                    // ---------- Password ----------
                    const _FieldLabel('Password'),
                    const SizedBox(height: 8),
                    _RoundedField(
                      controller: _passwordController,
                      obscureText: _obscurePassword,
                      textInputAction: TextInputAction.done,
                      onSubmitted: (_) => _handleLogin(),
                      validator: (v) => (v == null || v.isEmpty)
                          ? 'Password wajib diisi'
                          : null,
                      suffixIcon: IconButton(
                        icon: Icon(
                          _obscurePassword
                              ? Icons.visibility_off_outlined
                              : Icons.visibility_outlined,
                          color: AppColors.label,
                        ),
                        onPressed: () => setState(
                          () => _obscurePassword = !_obscurePassword,
                        ),
                      ),
                    ),
                    const SizedBox(height: 56),

                    // ---------- Tombol Login ----------
                    _GradientButton(
                      text: 'Login',
                      isLoading: _isLoading,
                      onPressed: _isLoading ? null : _handleLogin,
                    ),
                    const SizedBox(height: 24),

                    // ---------- Buat Akun ----------
                    const Text(
                      'Belum Punya Akun?',
                      textAlign: TextAlign.center,
                      style: TextStyle(fontSize: 16, color: AppColors.label),
                    ),
                    const SizedBox(height: 4),
                    Center(
                      child: GestureDetector(
                        onTap: _goToRegister,
                        child: const Text(
                          'Buat Akun',
                          style: TextStyle(
                            fontSize: 18,
                            fontWeight: FontWeight.w700,
                            fontStyle: FontStyle.italic,
                            decoration: TextDecoration.underline,
                            decorationColor: AppColors.label,
                            color: AppColors.label,
                          ),
                        ),
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}

/// ===================== KOMPONEN =====================

/// Label di atas input
class _FieldLabel extends StatelessWidget {
  final String text;
  const _FieldLabel(this.text);

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.only(left: 10),
      child: Text(
        text,
        style: const TextStyle(fontSize: 17, color: AppColors.label),
      ),
    );
  }
}

/// Input putih dengan sudut membulat & border biru tua
class _RoundedField extends StatelessWidget {
  final TextEditingController controller;
  final bool obscureText;
  final TextInputType? keyboardType;
  final TextInputAction? textInputAction;
  final String? Function(String?)? validator;
  final void Function(String)? onSubmitted;
  final Widget? suffixIcon;

  const _RoundedField({
    required this.controller,
    this.obscureText = false,
    this.keyboardType,
    this.textInputAction,
    this.validator,
    this.onSubmitted,
    this.suffixIcon,
  });

  OutlineInputBorder _border(Color color, [double width = 1.2]) =>
      OutlineInputBorder(
        borderRadius: BorderRadius.circular(28),
        borderSide: BorderSide(color: color, width: width),
      );

  @override
  Widget build(BuildContext context) {
    return TextFormField(
      controller: controller,
      obscureText: obscureText,
      keyboardType: keyboardType,
      textInputAction: textInputAction,
      onFieldSubmitted: onSubmitted,
      validator: validator,
      style: const TextStyle(fontSize: 16, color: AppColors.title),
      cursorColor: AppColors.title,
      decoration: InputDecoration(
        filled: true,
        fillColor: Colors.white,
        suffixIcon: suffixIcon,
        contentPadding: const EdgeInsets.symmetric(
          horizontal: 22,
          vertical: 18,
        ),
        enabledBorder: _border(AppColors.title),
        focusedBorder: _border(AppColors.title, 2),
        errorBorder: _border(Colors.red.shade700),
        focusedErrorBorder: _border(Colors.red.shade700, 2),
      ),
    );
  }
}

/// Tombol dengan gradasi kiri (gelap) -> kanan (biru terang)
class _GradientButton extends StatelessWidget {
  final String text;
  final VoidCallback? onPressed;
  final bool isLoading;

  const _GradientButton({
    required this.text,
    required this.onPressed,
    this.isLoading = false,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 60,
      decoration: BoxDecoration(
        gradient: const LinearGradient(
          begin: Alignment.centerLeft,
          end: Alignment.centerRight,
          colors: [AppColors.gradientStart, AppColors.gradientEnd],
        ),
        borderRadius: BorderRadius.circular(30),
      ),
      child: Material(
        color: Colors.transparent,
        child: InkWell(
          borderRadius: BorderRadius.circular(30),
          onTap: onPressed,
          child: Center(
            child: isLoading
                ? const SizedBox(
                    width: 24,
                    height: 24,
                    child: CircularProgressIndicator(
                      strokeWidth: 2.5,
                      color: Colors.white,
                    ),
                  )
                : Text(
                    text,
                    style: const TextStyle(
                      fontSize: 22,
                      fontWeight: FontWeight.w700,
                      color: Colors.white,
                    ),
                  ),
          ),
        ),
      ),
    );
  }
}
