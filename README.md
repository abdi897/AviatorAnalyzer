
name: Build Aviator Analyzer APK

on:
  workflow_dispatch:
  push:
    branches: [main, master]

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Set up Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Extract project ZIP
        run: unzip -o AviatorAnalyzer_Android_GitHub_Project.zip

      - name: Build APK
        run: gradle --no-daemon -p aviator_android_project assembleDebug

      - name: Save APK
        uses: actions/upload-artifact@v4
        with:
          name: Aviator-Analyzer-APK
          path: aviator_android_project/app/build/outputs/apk/debug/app-debug.apk
          
