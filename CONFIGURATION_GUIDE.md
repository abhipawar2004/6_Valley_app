# 6Valley E-Commerce App - Configuration Guide

This guide will help you configure the app properly to fix the runtime errors you're experiencing.

## 🚨 Critical Configuration Required

The app currently has placeholder values that need to be replaced with your actual credentials. Follow these steps:

---

## 1. Backend API Configuration ⚙️

### File: `lib/utill/app_constants.dart`

**Line 11:** Replace the base URL with your actual backend API URL:

```dart
static const String baseUrl = 'https://your-api-url.com';
```

**Example:**
```dart
static const String baseUrl = 'https://api.my6valley.com';
// or for local development:
static const String baseUrl = 'http://192.168.1.100:8000';
```

> ⚠️ **Without a valid base URL, the app will crash immediately on startup!**

---

## 2. Firebase Configuration 🔥

### Android Setup

1. **Go to Firebase Console:** https://console.firebase.google.com/
2. **Create or select your project**
3. **Download `google-services.json`:**
   - Go to Project Settings → General
   - Under "Your apps", find your Android app
   - Click "Download google-services.json"
4. **Place the file:** Copy `google-services.json` to `android/app/`

### Option A: Use google-services.json (Recommended)

If you've placed the `google-services.json` file correctly, Firebase will auto-configure. The app will fall back to this method.

### Option B: Manual Configuration

Edit `lib/main.dart` around line 73 and replace these values:

```dart
await Firebase.initializeApp(
  name: 'my-ecommerce-app',  // Your project name
  options: const FirebaseOptions(
    apiKey: "AIzaSyD...",      // From google-services.json
    projectId: "my-project",
    messagingSenderId: "123456789",
    appId: "1:123456789:android:abc123"
  )
);
```

**Where to find these values in `google-services.json`:**
- `apiKey` → `client[0].api_key[0].current_key`
- `projectId` → `project_info.project_id`
- `messagingSenderId` → `project_info.project_number`
- `appId` → `client[0].client_info.mobilesdk_app_id`

### iOS Setup

1. **Download `GoogleService-Info.plist`** from Firebase Console
2. **Place it in:** `ios/Runner/`
3. Firebase will auto-configure on iOS

---

## 3. Google Sign-In Configuration 🔐

### File: `lib/utill/app_constants.dart`

**Line 14:** Replace with your Google Server Client ID:

```dart
static const String googleServerClientId = 'YOUR_GOOGLE_SERVER_CLIENT_ID';
```

**How to get this:**
1. Go to Firebase Console → Authentication → Sign-in Method
2. Enable Google Sign-In
3. Find the "Web SDK configuration" section
4. Copy the "Web client ID" (OAuth 2.0 Client ID)

**Example:**
```dart
static const String googleServerClientId = '123456789-abc123def456.apps.googleusercontent.com';
```

---

## 4. Facebook Login Configuration 📘

### Android Configuration

**File: `android/app/src/main/res/values/strings.xml`**

Replace these values:

```xml
<string name="facebook_app_id">YOUR_FACEBOOK_APP_ID</string>
<string name="fb_login_protocol_scheme">fb_YOUR_FACEBOOK_APP_ID</string>
<string name="facebook_client_token">YOUR_FACEBOOK_CLIENT_TOKEN</string>
```

**How to get these:**
1. Go to https://developers.facebook.com/apps
2. Create or select your app
3. **App ID:** Dashboard → Settings → Basic → App ID
4. **Client Token:** Settings → Advanced → Security → Client Token

**Example:**
```xml
<string name="facebook_app_id">123456789012345</string>
<string name="fb_login_protocol_scheme">fb_123456789012345</string>
<string name="facebook_client_token">a1b2c3d4e5f6g7h8i9j0</string>
```

### iOS Configuration

**File: `ios/Runner/Info.plist`**

Update these values (around lines 33-35):

```xml
<key>FacebookAppID</key>
<string>YOUR_FACEBOOK_APP_ID</string>
<key>FacebookClientToken</key>
<string>YOUR_FACEBOOK_CLIENT_TOKEN</string>
```

And in CFBundleURLSchemes (around line 30):
```xml
<string>fb_YOUR_FACEBOOK_APP_ID</string>
```

---

## 5. Testing Without Social Login (Quick Start)

If you want to test the app without configuring social logins:

### Disable Facebook/Google Login Temporarily

You can disable these features in your backend API configuration, or simply skip the social login buttons in the UI. The app should still work with email/phone authentication.

---

## ✅ Configuration Checklist

Before running the app, ensure:

- [ ] Backend API URL is set in `app_constants.dart`
- [ ] Firebase `google-services.json` is in `android/app/` (Android)
- [ ] Firebase `GoogleService-Info.plist` is in `ios/Runner/` (iOS)
- [ ] Google Server Client ID is set in `app_constants.dart`
- [ ] Facebook App ID is set in `strings.xml` (Android)
- [ ] Facebook App ID is set in `Info.plist` (iOS)
- [ ] Facebook Client Token is set in both platforms

---

## 🚀 After Configuration

1. **Clean the project:**
   ```bash
   flutter clean
   ```

2. **Get dependencies:**
   ```bash
   flutter pub get
   ```

3. **For Android, rebuild:**
   ```bash
   cd android
   ./gradlew clean
   cd ..
   ```

4. **Run the app:**
   ```bash
   flutter run
   ```

---

## 📝 Common Issues

### "Invalid argument (baseUrl): Must be a valid URL"
- **Cause:** Base URL is still set to placeholder
- **Fix:** Update `app_constants.dart` with your actual API URL

### "Error validating application. Invalid application ID" (Facebook)
- **Cause:** Facebook App ID not configured
- **Fix:** Update `strings.xml` (Android) and `Info.plist` (iOS) with real Facebook credentials

### Firebase initialization fails
- **Cause:** Missing or incorrect Firebase configuration
- **Fix:** Ensure `google-services.json` and `GoogleService-Info.plist` are correctly placed

---

## 🆘 Need Help?

1. Check that your backend API is running and accessible
2. Verify all configuration files are saved
3. Make sure you're using valid credentials (not placeholder text)
4. Try running with `flutter run -v` for verbose error logs

---

**Last Updated:** December 31, 2025
