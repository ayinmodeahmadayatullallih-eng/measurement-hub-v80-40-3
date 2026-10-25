name: Build AAB V80.40.3 CODE 804003
on:
  push:
    branches: [ main ]
jobs:
  build-aab:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 18
      - uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: '17'
      - run: npm install
      - run: chmod +x android/gradlew
      - run: |
          cd android
          ./gradlew clean
          ./gradlew bundleRelease --no-daemon
          cd ..
          ls -lh android/app/build/outputs/bundle/release/
      - uses: actions/upload-artifact@v4
        with:
          name: MeasurementHub-AAB-804003
          path: android/app/build/outputs/bundle/release/app-release.aab
