# Jarvis AI Assistant - Deployment Guide

## 📱 Platform-Specific Deployment

### Android Deployment

#### Build APK (for testing)
```bash
flutter build apk --debug
# Output: build/app/outputs/flutter-app-debug.apk
```

#### Build Release APK (for production)
```bash
flutter build apk --release
# Output: build/app/outputs/app-release.apk
```

#### Build App Bundle (for Google Play Store)
```bash
flutter build appbundle --release
# Output: build/app/outputs/app-release.aab
```

#### Upload to Google Play Store
1. Go to [Google Play Console](https://play.google.com/console)
2. Create a new app or select existing app
3. Upload the `.aab` file to the Internal Testing track
4. Fill in app details, privacy policy, content rating
5. Submit for review (usually takes 2-4 hours)

---

### iOS Deployment

#### Build IPA (for testing with Testflight)
```bash
flutter build ios --release
cd ios
pod install
xcodebuild -workspace Runner.xcworkspace -scheme Runner -config Release -derivedDataPath build -arch arm64 -sdk iphoneos
```

#### Automatic Upload to App Store
```bash
cd ios
fastlane pilot upload --beta_ipa build/ipa/Runner.ipa
```

#### Manual Upload to App Store Connect
1. Open Xcode: `open ios/Runner.xcworkspace`
2. Product → Scheme → Select "Runner"
3. Product → Destination → Select "Generic iOS Device"
4. Product → Archive
5. Click "Distribute App"
6. Select "App Store Connect"
7. Complete the form and submit

---

### Web Deployment

#### Build Web App
```bash
flutter build web --release
# Output: build/web/
```

#### Deploy to Firebase Hosting
```bash
# 1. Install Firebase CLI
npm install -g firebase-tools

# 2. Initialize Firebase
firebase login
firebase init hosting

# 3. Deploy
firebase deploy
```

#### Deploy to GitHub Pages
```bash
# 1. Build web
flutter build web --release --base-href /jarvis/

# 2. Push to gh-pages branch
git checkout --orphan gh-pages
git add build/web/
git commit -m "Deploy to GitHub Pages"
git push -u origin gh-pages
```

#### Deploy to Vercel
```bash
# 1. Install Vercel CLI
npm i -g vercel

# 2. Deploy
cd build/web
vercel
```

---

## 🔄 CI/CD with GitHub Actions

The repository includes an automated CI/CD pipeline (`.github/workflows/deploy.yml`) that:

✅ Runs on every push to `main` branch
✅ Builds Android APK
✅ Builds Web app
✅ Runs code analysis
✅ Runs tests
✅ Creates GitHub releases with artifacts

### View Workflow
- Go to: `https://github.com/siva716/jarvis/actions`
- Check build logs and download artifacts

---

## 🔐 Environment Configuration

### Before Deployment

1. **Update Backend URL**
   ```dart
   // lib/services/ai_service.dart
   static const String baseUrl = 'https://your-production-api.com';
   ```

2. **Update App Name & Version**
   ```yaml
   # pubspec.yaml
   name: jarvis_x
   version: 1.0.0+1
   ```

3. **Configure Android Signing**
   ```bash
   keytool -genkey -v -keystore ~/jarvis_keystore.jks \
     -keyalg RSA -keysize 2048 -validity 10000 \
     -alias jarvis_key
   ```

4. **Configure iOS Signing**
   - Go to Xcode project settings
   - Team ID, Bundle Identifier, Signing Certificate

---

## 📊 Deployment Checklist

- [ ] Code is tested and reviewed
- [ ] Version number is updated
- [ ] Backend API endpoint is configured
- [ ] Signing certificates are generated (Android/iOS)
- [ ] App privacy policy is ready
- [ ] App screenshots and description are prepared
- [ ] Content rating is submitted
- [ ] Release notes are written
- [ ] All tests pass locally
- [ ] Code analysis shows no errors

---

## 🚀 Quick Deploy Commands

```bash
# Android
flutter build apk --release

# iOS
flutter build ios --release

# Web
flutter build web --release

# All platforms
flutter build apk --release && flutter build ios --release && flutter build web --release
```

---

## 📱 Testing Before Deployment

```bash
# Local testing
flutter run -v

# Build variants
flutter run --flavor dev   # if configured
flutter run --flavor prod  # if configured

# Code quality
flutter analyze
flutter test
```

---

## 🆘 Troubleshooting Deployment

| Issue | Solution |
|-------|----------|
| Signing errors | Regenerate certificates or update keystore path |
| API connection errors | Update `baseUrl` in `ai_service.dart` |
| Build failures | Run `flutter clean` and `flutter pub get` |
| Large APK size | Enable ProGuard minification in Android build |
| iOS build hangs | Update CocoaPods: `pod repo update` |

---

## 📞 Support

For deployment help:
- [Flutter Deployment Docs](https://docs.flutter.dev/deployment)
- [Google Play Store Setup](https://support.google.com/googleplay/android-developer)
- [App Store Connect](https://appstoreconnect.apple.com/)
- GitHub Issues: https://github.com/siva716/jarvis/issues
