name: Build Godot Android APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Setup Godot
        uses: firebelley/setup-godot@v1
        with:
          godot-version: 3.5.3
          use-mono: false

      - name: Setup JDK
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: '17'

      - name: Download Android build templates
        run: |
          mkdir -p ~/.local/share/godot/templates/3.5.3.stable
          wget https://downloads.tuxfamily.org/godotengine/3.5.3/Godot_v3.5.3-stable_export_templates.tpz -O templates.tpz
          unzip -o templates.tpz -d ~/.local/share/godot/templates/3.5.3.stable

      - name: Export APK
        run: |
          mkdir -p build
          godot --headless --export-release "Android" build/game.apk

      - name: Upload APK
        uses: actions/upload-artifact@v3
        with:
          name: enthusiastic-game-apk
          path: build/game.apk