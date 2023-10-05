---
title: Reduce Gradle Build Time
updated: 2022-10-29 13:43:09Z
created: 2022-10-10 05:45:33Z
latitude: 50.11092210
longitude: 8.68212670
altitude: 0.0000
---

## flutter run -v

- Open your flutter Project directory.
- Change directory to android directory in your flutter project directory cd android
    clean gradle ./gradlew clean
- Build Gradle ./gradlew build or you can combine both commands with just ./gradlew clean build.
- Now run your flutter project. If you use vscode, press F5. The first time Gradle running assembly debug will take time.

## flutter build clean

## flutter clean

## flutter build