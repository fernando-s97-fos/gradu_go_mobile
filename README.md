# Celebra Club

Celebra Club is the Android and iOS app [Celebra Formaturas](http://celebraformaturas.com) graduates (formandos) use to find partner discounts and open their digital membership card.

You are in the `gradu_go_mobile` repository. The Flutter package name is `celebra_club`, and the screens are in Portuguese. The home screen opens without an account.

## What it can do

- On **Home**, filter partner discounts by city or business segment, search by name, and browse events. Pull down to reload both lists.
- Open a partner or an event to see the discount and any contact links saved for it (phone, map, Facebook, Instagram, or tickets).
- On **Sobre**, read how the club works and message the team (DAF) on WhatsApp.
- On **Seja Parceiro**, email or call the club to ask to join as a business.
- On **Carteirinha Digital**, sign in and swipe between the front and back of the membership card.

## Requirements

| Requirement | What you need |
| --- | --- |
| Flutter | 1.6.3, beta channel, revision `bc7bc940836f1f834699625426795fd6f07c18ec` (the revision in `.metadata`) |
| Dart | `>=2.2.2 <3.0.0` (`pubspec.yaml`). Flutter 1.6.3 includes Dart 2.3.2. |
| Android | JDK 8, Android SDK platform 28, minSdk 16, targetSdk 28, Kotlin 1.3.40, Android Gradle Plugin 3.4.2, Gradle 5.1.1. Application id `com.celebraformaturas.celebra_club`. |
| iOS | A Mac with Xcode. Deployment target 8.0. Bundle id `com.celebraformaturas.celebraClub`. The app queries the `whatsapp` URL scheme. |
| Network | Internet access. Partner, event, and sign-in calls use `URL_SERVER` in `lib/helpers/util.dart`. |

## Setup

1. Install that Flutter revision outside this repo and put it on your `PATH`:

   ```bash
   git clone https://github.com/flutter/flutter.git ~/flutter
   git -C ~/flutter checkout bc7bc940836f1f834699625426795fd6f07c18ec
   export PATH="$HOME/flutter/bin:$PATH"
   flutter doctor
   ```

   `flutter doctor` should report Flutter 1.6.3. Fix the Android or iOS row for the platform you will run before continuing.

2. From the root of this repo, fetch packages:

   ```bash
   flutter pub get
   ```

3. Android builds read `android/local.properties`, which is gitignored. Create it with your SDK path:

   ```properties
   flutter.sdk=/absolute/path/to/flutter
   ```

4. Connect a device or start an emulator, then run:

   ```bash
   flutter run
   ```

An Android release build also reads `android/key.properties` (`storeFile`, `storePassword`, `keyAlias`, `keyPassword`). A debug `flutter run` does not. Do not commit keystore passwords.

## Sign in

**Carteirinha Digital** asks for two fields:

| Field | What to type |
| --- | --- |
| Usuário | CPF, 11 digits, no dots or dashes |
| Senha | Date of birth, 8 digits, day then month then year (`ddMMyyyy`). 5 March 1998 is `05031998`. |

The account has to exist on the server. A wrong CPF, a wrong date, and a failed connection all stay on this screen with the same message: check the numbers and the network, then try again.

## Code

| Path | What it holds |
| --- | --- |
| `lib/fragments/` | Drawer pages: Home, Sobre, Seja Parceiro, Carteirinha Digital |
| `lib/pages/` | Event, partner, and membership-card screens |
| `lib/dao/` | Server calls for events, partners, and sign-in |
| `android/`, `ios/` | Native projects `flutter run` builds |

## Tests

```bash
flutter test
```

`test/widget_test.dart` is still the Flutter counter sample. The command fails on this app until that test is replaced. A failure there does not mean the drawer or the card broke.

Version history is in [CHANGELOG.md](CHANGELOG.md).
