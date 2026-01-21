# Ultimate-cricket-
✅ Play BPL, IPL, World Cup, T20 World Cup &amp; Test Cricket ✅ Realistic stadiums &amp; crowd atmosphere ✅ Smooth controls &amp; stunning graphics ✅ Build your dream team &amp; dominate the rankings ✅ Experience nail-biting last overs &amp; epic centuries Feel the pressure… Hear the crowd roar… Smash sixes… Take match-winning wickets…
import 'dart:math';
import 'package:flutter/material.dart';

void main() {
  runApp(CricketUltraPro());
}

class CricketUltraPro extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Home(),
    );
  }
}

/* ---------------- HOME ---------------- */

class Home extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.green.shade900,
      appBar: AppBar(title: Text("🏏 Cricket Ultra Pro")),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            btn(context, "Career Mode", TeamSelect()),
            btn(context, "Quick Match", TeamSelect()),
            btn(context, "Shop", Shop()),
          ],
        ),
      ),
    );
  }

  Widget btn(ctx, String t, Widget p) {
    return Padding(
      padding: EdgeInsets.all(10),
      child: ElevatedButton(
        style: ElevatedButton.styleFrom(
            minimumSize: Size(200, 50)),
        child: Text(t),
        onPressed: () =>
            Navigator.push(ctx,
                MaterialPageRoute(builder: (_) => p)),
      ),
    );
  }
}

/* ---------------- SHOP ---------------- */

class Shop extends StatefulWidget {
  @override
  _ShopState createState() => _ShopState();
}

class _ShopState extends State<Shop> {
  int coins = 500;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Cricket Shop")),
      body: Column(
        children: [
          Text("💰 Coins: $coins",
              style: TextStyle(fontSize: 24)),
          buy("Golden Bat", 200),
          buy("Power Gloves", 150),
          buy("Premium Stadium", 300),
        ],
      ),
    );
  }

  Widget buy(String item, int price) {
    return ListTile(
      title: Text(item),
      trailing: Text("$price coins"),
      onTap: () {
        if (coins >= price) {
          setState(() => coins -= price);
          ScaffoldMessenger.of(context)
              .showSnackBar(
                  SnackBar(content:
                      Text("$item purchased!")));
        }
      },
    );
  }
}

/* ---------------- TEAM SELECT ---------------- */

class TeamSelect extends StatelessWidget {

  final teams = [
    "Bangladesh 🇧🇩",
    "India 🇮🇳",
    "Pakistan 🇵🇰",
    "Australia 🇦🇺",
    "England 🏴",
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Select Team")),
      body: ListView.builder(
        itemCount: teams.length,
        itemBuilder: (_, i) => ListTile(
          title: Text(teams[i]),
          onTap: () =>
              Navigator.push(context,
                  MaterialPageRoute(
                      builder: (_) =>
                          Toss(team: teams[i]))),
        ),
      ),
    );
  }
}

/* ---------------- TOSS ---------------- */

class Toss extends StatelessWidget {
  final String team;
  Toss({required this.team});

  @override
  Widget build(BuildContext context) {
    bool batFirst = Random().nextBool();

    return Scaffold(
      appBar: AppBar(title: Text("Toss")),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              batFirst
                  ? "You won toss & bat!"
                  : "AI won toss & bat!",
              style: TextStyle(fontSize: 22),
            ),
            ElevatedButton(
              child: Text("Start Match"),
              onPressed: () =>
                  Navigator.push(context,
                      MaterialPageRoute(
                          builder: (_) =>
                              GameUltra(
                                  team: team,
                                  myBat: batFirst))),
            )
          ],
        ),
      ),
    );
  }
}

/* ---------------- GAME ---------------- */

class GameUltra extends StatefulWidget {
  final String team;
  final bool myBat;

  GameUltra({required this.team, required this.myBat});

  @override
  _GameUltraState createState() => _GameUltraState();
}

class _GameUltraState extends State<GameUltra> {

  Random r = Random();

  int myScore = 0, myW = 0, myBalls = 0;
  int aiScore = 0, aiW = 0, aiBalls = 0;

  bool myInnings = true;
  String msg = "Match Started!";

  int maxBalls = 120;
  int coins = 0;

  void playBall() {

    if (myInnings) {

      if (myW == 10 || myBalls == maxBalls) {
        myInnings = false;
        msg = "Target: ${myScore + 1}";
        setState(() {});
        return;
      }

      int o = r.nextInt(8);
      setState(() {
        myBalls++;
        if (o == 7) {
          myW++;
          msg = "💥 OUT!";
        } else {
          myScore += o;
          msg = "🔥 $o run!";
        }
      });

    } else {

      if (aiScore > myScore ||
          aiW == 10 ||
          aiBalls == maxBalls) {
        result();
        return;
      }

      int o = r.nextInt(8);
      setState(() {
        aiBalls++;
        if (o == 7) {
          aiW++;
          msg = "AI OUT!";
        } else {
          aiScore += o;
          msg = "AI scored $o";
        }
      });
    }
  }

  void result() {
    setState(() {
      if (myScore > aiScore) {
        msg = "🏆 YOU WIN!";
        coins += 100;
      } else {
        msg = "❌ YOU LOSE!";
      }
    });
  }

  String over(int b) =>
      "${b ~/ 6}.${b % 6}";

  @override
  Widget build(BuildContext context) {

    return Scaffold(
      appBar: AppBar(
          title: Text("Ultra Pro Match")),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [

            Text("YOU: $myScore/$myW (${over(myBalls)})",
                style: TextStyle(fontSize: 22)),

            Text("AI: $aiScore/$aiW (${over(aiBalls)})",
                style: TextStyle(fontSize: 22)),

            SizedBox(height: 20),

            Text(msg,
                style: TextStyle(
                    fontSize: 26,
                    color: Colors.red)),

            SizedBox(height: 25),

            ElevatedButton(
              child: Text(myInnings
                  ? "🏏 Bat"
                  : "🎯 Bowl"),
              onPressed: playBall,
            ),
          ],
        ),
      ),
    );
  }
}
.github/workflows/build.yml
name: Build APK

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Set up JDK
      uses: actions/setup-java@v3
      with:
        distribution: 'temurin'
        java-version: '17'

    - name: Grant execute permission
      run: chmod +x gradlew

    - name: Build APK
      run: ./gradlew assembleDebug

    - name: Upload APK
      uses: actions/upload-artifact@v3
      with:
        name: apk
        path: app/build/outputs/apk/debug/app-debug.apk
