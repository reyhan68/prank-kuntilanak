menu_reyhan_app/
├─ lib/
│   └─ main.dart
├─ assets/
│   ├─ kuntilanak.mp3
│   └─ kuntilanak.png (opsional)
├─ pubspec.yaml
└─ .github/
    └─ workflows/
        └─ build-apk.yml
import 'package:flutter/material.dart';
import 'package:audioplayers/audioplayers.dart';

void main() => runApp(KuntilanakApp());

class KuntilanakApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: KuntilanakPrankPage(),
    );
  }
}

class KuntilanakPrankPage extends StatefulWidget {
  @override
  _KuntilanakPrankPageState createState() => _KuntilanakPrankPageState();
}

class _KuntilanakPrankPageState extends State<KuntilanakPrankPage> {
  final player = AudioPlayer();
  bool isPlaying = false;

  void playSound() async {
    if (!isPlaying) {
      await player.setReleaseMode(ReleaseMode.loop);
      await player.play(AssetSource('kuntilanak.mp3'));
      setState(() => isPlaying = true);
    }
  }

  void stopSound() async {
    await player.stop();
    setState(() => isPlaying = false);
  }

  @override
  void dispose() {
    player.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.black,
      appBar: AppBar(
        backgroundColor: Colors.red[900],
        title: Text('Prank Kuntilanak'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.volume_up_rounded, size: 100, color: Colors.red),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: playSound,
              style: ElevatedButton.styleFrom(backgroundColor: Colors.red),
              child: Text('Putar Suara', style: TextStyle(fontSize: 20)),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: stopSound,
              style: ElevatedButton.styleFrom(backgroundColor: Colors.grey),
              child: Text('Berhenti', style: TextStyle(fontSize: 20)),
            ),
          ],
        ),
      ),
    );
  }
}
flutter:
  assets:
    - assets/kuntilanak.mp3
    dependencies:
  flutter:
    sdk: flutter
  audioplayers: ^5.2.1
  name: Build & Release APK

on:
  push:
    tags:
      - 'v*'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: Set up Flutter
      uses: subosito/flutter-action@v2
      with:
        channel: stable

    - name: Install dependencies
      run: flutter pub get

    - name: Build APK
      run: flutter build apk --release

    - name: Upload APK
      uses: softprops/action-gh-release@v1
      with:
        files: build/app/outputs/flutter-apk/app-release.apk
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        git tag v1.0
git push origin v1.0
import 'package:flutter/material.dart';
import 'package:audioplayers/audioplayers.dart';
import 'dart:async';

void main() => runApp(KuntilanakApp());

class KuntilanakApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: KuntilanakPrankPage(),
    );
  }
}

class KuntilanakPrankPage extends StatefulWidget {
  @override
  _KuntilanakPrankPageState createState() => _KuntilanakPrankPageState();
}

class _KuntilanakPrankPageState extends State<KuntilanakPrankPage> {
  final player = AudioPlayer();
  bool isPlaying = false;
  int delaySeconds = 5;
  Timer? prankTimer;

  void playSound() async {
    if (!isPlaying) {
      await player.setReleaseMode(ReleaseMode.loop);
      await player.play(AssetSource('kuntilanak.mp3'));
      setState(() => isPlaying = true);
    }
  }

  void stopSound() async {
    prankTimer?.cancel();
    await player.stop();
    setState(() => isPlaying = false);
  }

  void startTimer() {
    prankTimer?.cancel(); // cancel timer sebelumnya jika ada
    prankTimer = Timer(Duration(seconds: delaySeconds), playSound);
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text('Suara akan diputar dalam $delaySeconds detik')),
    );
  }

  @override
  void dispose() {
    prankTimer?.cancel();7
    player.dispose();3
    super.dispose();9
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.black,
      appBar: AppBar(
        backgroundColor: Colors.red[900],
        title: Text('Prank Kuntilanak'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.timer, size: 100, color: Colors.red),
            SizedBox(height: 20),
            Text(
              'Set Waktu (detik):',
              style: TextStyle(color: Colors.white, fontSize: 18),
            ),
            SizedBox(height: 10),
            Slider(
              value: delaySeconds.toDouble(),
              min: 1,
              max: 30,
              divisions: 29,
              label: '$delaySeconds detik',
              onChanged: (val) {
                setState(() {
                  delaySeconds = val.toInt();
                });
              },
              activeColor: Colors.red,
              inactiveColor: Colors.red[200],
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: startTimer,
              style: ElevatedButton.styleFrom(backgroundColor: Colors.red),
              child: Text('Mulai Timer Prank'),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: stopSound,
              style: ElevatedButton.styleFrom(backgroundColor: Colors.grey),
              child: Text('Berhenti'),
            ),
          ],
        ),
      ),
    );
  }
}
import 'package:flutter/material.dart';
import 'package:audioplayers/audioplayers.dart';
import 'dart:async';
import 'dart:io';

void main() => runApp(KuntilanakApp());

class KuntilanakApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: KuntilanakAutoPrank(),
    );
  }
}

class KuntilanakAutoPrank extends StatefulWidget {
  @override
  State<KuntilanakAutoPrank> createState() => _KuntilanakAutoPrankState();
}

class _KuntilanakAutoPrankState extends State<KuntilanakAutoPrank> {
  final player = AudioPlayer();
  Timer? startTimer;
  Timer? exitTimer;

  @override
  void initState() {
    super.initState();

    // Mulai delay 10 detik
    startTimer = Timer(Duration(seconds: 10), () async {
      await player.setReleaseMode(ReleaseMode.loop);
      await player.play(AssetSource('kuntilanak.mp3'));

      // Auto close setelah 10 detik suara dimainkan
      exitTimer = Timer(Duration(seconds: 10), () {
        player.stop();
        exit(0); // keluar dari aplikasi
      });
    });
  }

  @override
  void dispose() {
    startTimer?.cancel();
    exitTimer?.cancel();
    player.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.black,
      body: Center(
        child: Text(
          '💀 Prank akan dimulai...\nTunggu 10 detik...',
          textAlign: TextAlign.center,
          style: TextStyle(color: Colors.red, fontSize: 22),
        ),
      ),
    );
  }
}
dependencies:
  flutter:
    sdk: flutter
  audioplayers: ^5.2.1

flutter:
  assets:
    - assets/kuntilanak.mp3
    git tag v1.1
git push origin v1.1
