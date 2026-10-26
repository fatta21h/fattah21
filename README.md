name: Check Android Project

on:
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      - name: Set up JDK
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'
          cache: gradle

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Check project files
        run: |
          echo "Project files:"
          ls -la
          find . -maxdepth 4 -type f | sort | head -150

      - name: Make Gradle wrapper executable
        run: chmod +x ./gradlew

      - name: Build and run unit tests
        run: ./gradlew build test
