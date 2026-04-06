# ShadowStudio---Studio-Docker-app

**Shadow Studio** is an Android application that acts as a unified dashboard for managing and accessing self-hosted services and applications. It provides a single, secure entry point for monitoring service health, launching web interfaces, configuring network settings, and keeping the app itself up to date — all from your Android device.

---

## Features

- **Service Dashboard** — A home screen displaying all configured services and apps with live online/offline status indicators.
- **WebView Integration** — Tap any service or app card to open its web interface directly within the app.
- **Network Configuration** — Per-service IP address, port, and protocol (HTTP/HTTPS) settings stored in a local Room database.
- **Security & Authentication** — Biometric authentication (fingerprint/face) and encrypted preference storage using `androidx.security.crypto`.
- **Self-Update Engine** — Built-in OTA update system that downloads and silently installs new APKs via the `PackageInstaller` session API, with optional auto-update via WorkManager.
- **Manage Services** — Enable or disable individual services and apps from within the app.
- **Settings** — App-wide settings including theme preferences and update configuration.

---

## Supported Services & Apps

Shadow Studio ships with support for a curated set of popular self-hosted tools, including:

| Category | Services / Apps |
|---|---|
| Media | Jellyfin |
| Storage | Nextcloud, Immich |
| Security | Bitwarden / VaultWarden, Pi-Hole |
| Networking | Nginx, Cloudflared, Tailscale |
| Monitoring | Beszel, Beszel Agent |
| Automation | n8n, Home Assistant |
| Development | Crafty 4, Ollama |
| Other | Traccar, Homepage, Open WebUI, ROM Manager, Ripper |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Kotlin |
| UI | Jetpack Compose + Material 3 |
| Navigation | Compose Navigation |
| Dependency Injection | Hilt |
| Local Database | Room |
| Preferences | DataStore |
| Networking | Retrofit + OkHttp + Gson |
| Background Work | WorkManager |
| Security | AndroidX Security Crypto, AndroidX Biometric |
| WebView | AndroidX WebKit |
| Build System | Gradle (Kotlin DSL) |

---

## Requirements

- **Minimum SDK:** Android 14 (API 34)
- **Target SDK:** Android 16 (API 36)
- **Compile SDK:** 36
- **Java Compatibility:** Java 17

---

## Project Structure

```
app/src/main/java/com/Studio/Shadows/
├── ShadowStudioApp.kt              # Application entry point
├── data/
│   ├── local/                      # Room DAOs and database
│   ├── managers/                   # UpdateManager (OTA updates)
│   ├── model/                      # AppModel, NetworkConfig, SecurityConfig
│   ├── preferences/                # UpdatePreferences (DataStore)
│   └── repository/                 # NetworkConfigRepository
├── di/
│   └── AppModule.kt                # Hilt dependency graph
├── receivers/
│   ├── InstallResultReceiver.kt    # Handles PackageInstaller session results
│   └── PackageReplacedReceiver.kt  # Post-update cleanup and restart
├── security/
│   ├── EncryptedPreferenceManager.kt
│   └── SecurityManager.kt
├── ui/
│   ├── MainActivity.kt
│   ├── screens/                    # Compose screens
│   │   ├── HomeScreen.kt
│   │   ├── ServicesScreen.kt
│   │   ├── ManageServicesScreen.kt
│   │   ├── NetworkSettingsScreen.kt
│   │   ├── SecuritySettingsScreen.kt
│   │   ├── ServiceWebViewScreen.kt
│   │   ├── SettingsScreen.kt
│   │   ├── UpdateScreen.kt
│   │   └── AuthScreen.kt
│   ├── theme/                      # Color, Type, Theme
│   └── viewmodel/                  # ViewModels for each screen
└── workers/
    └── AutoUpdateWorker.kt         # Background auto-update task
```

---

## Building

1. Clone the repository.
2. Open the project in Android Studio.
3. Sync Gradle dependencies.
4. Build and run on a device or emulator running Android 14+.

```bash
./gradlew assembleDebug    # Debug build
./gradlew assembleRelease  # Release build (minified + ProGuard)
```

---

## Permissions

The app requests the following permissions:

| Permission | Purpose |
|---|---|
| `INTERNET`, `ACCESS_NETWORK_STATE`, `ACCESS_WIFI_STATE` | Service connectivity and status checks |
| `USE_BIOMETRIC`, `USE_FINGERPRINT` | Biometric authentication |
| `POST_NOTIFICATIONS` | Update and status notifications |
| `REQUEST_INSTALL_PACKAGES` | Silent APK installation via PackageInstaller |
| `RECEIVE_BOOT_COMPLETED` | Reschedule auto-update worker after reboot |
| `READ/WRITE_EXTERNAL_STORAGE` | Legacy storage access (scoped storage used on API 34+) |

---

## Version History

| Version | Notes |
|---|---|
| 1.3.2 | Current release |

---

## License

This project is proprietary software developed by ShadowStudio Prod. All rights reserved.
