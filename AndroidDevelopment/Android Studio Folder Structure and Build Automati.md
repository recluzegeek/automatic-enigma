---
title: Android Studio Folder Structure and Build Automation Tools
updated: 2022-10-30 14:50:21Z
created: 2022-10-29 13:44:27Z
latitude: 33.72938820
longitude: 73.09314610
altitude: 0.0000
---

# Android Studio Folder Structure
- Default module name is **app**
- **manifests** folder includes **AndroidManifest.xml** which tells the Android OS what to load/show **Activity** when the app icon has been clicked. It also includes app metadata such as app version, platforms its targeting, theme, and icons.
- Bussines logic resides inside the **java** folder which can either be written in java or kotlin. Android development with Kotlin is preffered by Google and its partners given its tensions with Oracle over JAVA.
- App resources are present in the **res** folder of the IDE. All resources including Images, Strings, Dimensions, Fonts resides inside the res folder.
- **Activities** in Android Development represents the app layout. **Layout is like the distribution of the screen to different views and view groups like Buttons, TextView, ImageView and constraint layout or linear layout respectively.** Default activity of the app is **activity_main.xml** which usually is the rendered layout when we tap on the app icon.

# Build Automation Tools
## 1. Gradle
- Gradle is the tool of choice by Google for Android Development. It helps developers to focus on writing code while taking care of downloading required dependencies, compilation of modules/packages and optional features.
- 
## 2. Maven