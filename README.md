# Mobile (Celebra Club)

Mobile is the Flutter source code of Celebra Club, an Android and iOS app for graduates (formandos) of Celebra Formaturas, a graduation and events company. Graduates use it to browse partner discounts and events and to open their digital membership card.

The app screens are in Portuguese (`pt_BR`). The code dates from 2019: the Git history has two commits, from 2019-09-20 and 2019-10-23.

## What the app does

The drawer menu has four screens. Everything except **Carteirinha Digital** opens without an account.

| Screen | What you can do |
| --- | --- |
| **Home** | Browse event flyers. Filter partners by city and segment, or search a partner by name with the search button. Pull down to reload. Open a partner to see its segment, discount, phone and Facebook, Instagram and map links. Open an event to see when it happens, its discount and its ticket, Facebook and map links. |
| **Sobre** | Read what Celebra and the club are, how to take part, and reach the company on WhatsApp. |
| **Seja Parceiro** | Ask to join as a partner company by email or phone. |
| **Carteirinha Digital** | Sign in and swipe between the front and back of your digital membership card. |

### Sign in

The sign-in form asks for two fields:

- **Usuário**: CPF, 11 digits, numbers only.
- **Senha**: date of birth, 8 digits, numbers only.

If sign-in fails, the app asks you to check your data and your network connection, and to try again. A **NÃO CONSEGUE ACESSAR?** link opens WhatsApp to contact the company.

### Where the data comes from

The app reads events, partners and graduates with HTTP POST requests to a server. The base URL is `Util.URL_SERVER` in `lib/helpers/util.dart`. The endpoints are `evento/read.php`, `parceiro/read.php`, `parceiro/readByCidadeSegmento.php`, `formando/authenticate.php` and `formando/read.php`. The app needs internet access (`INTERNET` permission in `android/app/src/main/AndroidManifest.xml`). Nothing in this repository starts or configures that server.

## Requirements

The project is pinned to the 2019 toolchain. Use these versions, not current ones.

| Requirement | Version | Source |
| --- | --- | --- |
| Flutter | beta channel, revision `bc7bc940836f1f834699625426795fd6f07c18ec` (the same commit as the Flutter tag `v1.6.3`) | `.metadata` |
| Dart | `>=2.2.2 <3.0.0` | `pubspec.yaml` |
| Android | compile and target SDK 28, minimum SDK 16, Kotlin 1.3.40, Android Gradle Plugin 3.4.2, Gradle 5.1.1 | `android/app/build.gradle`, `android/build.gradle`, `android/gradle/wrapper/gradle-wrapper.properties` |
| iOS | deployment target 8.0, Xcode on a Mac | `ios/Runner.xcodeproj/project.pbxproj`, `ios/Flutter/AppFrameworkInfo.plist` |

## Run the app

1. Install the Flutter revision above outside this repository and add it to your `PATH`:

   ```bash
   git clone https://github.com/flutter/flutter.git ~/flutter
   git -C ~/flutter checkout bc7bc940836f1f834699625426795fd6f07c18ec
   export PATH="$HOME/flutter/bin:$PATH"
   flutter doctor
   ```

2. From the root of this repository, fetch the packages:

   ```bash
   flutter pub get
   ```

3. Android builds need the Flutter SDK path in `android/local.properties`. Git ignores that file. Create it with:

   ```properties
   flutter.sdk=/absolute/path/to/flutter
   ```

4. Connect a device or start an emulator, then run:

   ```bash
   flutter run
   ```

An Android release build signs with the values in `android/key.properties` (`storeFile`, `storePassword`, `keyAlias`, `keyPassword`), as `android/app/build.gradle` defines.

## Project facts

| Item | Value | Source |
| --- | --- | --- |
| Package name | `celebra_club` | `pubspec.yaml` |
| Version | `1.2.9+1.2.9` | `pubspec.yaml` |
| App name | Celebra Club | `AndroidManifest.xml`, `ios/Runner/Info.plist` |
| Android application id | `com.celebraformaturas.celebra_club` | `android/app/build.gradle` |
| iOS bundle id | `com.celebraformaturas.celebraClub` | `ios/Runner.xcodeproj/project.pbxproj` |
| Launcher icon source | `assets/images/celebra_club_icon.png` | `pubspec.yaml` (`flutter_icons`) |

## Code map

| Path | What it holds |
| --- | --- |
| `lib/main.dart` | App entry point and theme. |
| `lib/pages/` | `HomePage` (drawer) and the event, partner and digital ID card screens. |
| `lib/fragments/` | The four drawer screens: main (Home), about, become partner, login. |
| `lib/dao/` | Server calls for events, graduates and partners. |
| `lib/domain/` | `Event`, `Graduate` and `Partner` models. |
| `lib/helpers/` | Shared lists, the city and segment filters (`repository.dart`) and utilities such as `URL_SERVER` (`util.dart`). |
| `assets/images/` | Images the app bundles, listed in `pubspec.yaml`. |
| `android/`, `ios/` | The native projects. |
| `test/` | `widget_test.dart`. |

## Tests

`test/widget_test.dart` is still the default Flutter counter sample. It looks for the text `0` and taps an add icon (`Icons.add`), and this app has no counter and no add button, so the test does not describe the app. A failure there does not mean a screen is broken.
