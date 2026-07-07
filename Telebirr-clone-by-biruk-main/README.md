# Telebirr App Clone

A Flutter UI clone of **Telebirr** — Ethio Telecom's mobile payment and digital wallet application. Built by [Biruk](https://github.com/Burati063) as a portfolio project to replicate the look, feel, and layout of the original Telebirr app.

> **Disclaimer:** This project is not affiliated with, endorsed by, or connected to Ethio Telecom or the official Telebirr service. It is a UI demonstration built for learning and portfolio purposes only.

---

## Table of Contents

- [Overview](#overview)
- [Screenshots](#screenshots)
- [Project Structure](#project-structure)
- [Architecture](#architecture)
- [Screens](#screens)
- [Widgets](#widgets)
- [Constants & Theme](#constants--theme)
- [Dependencies](#dependencies)
- [Assets](#assets)
- [Getting Started](#getting-started)

---

## Overview

| Property | Value |
|---|---|
| **Platform** | Flutter (Android & iOS) |
| **Language** | Dart |
| **Version** | 1.0.0+1 |
| **Dart SDK** | >=3.1.5 <4.0.0 |
| **State Management** | `setState` (no external library) |

The app replicates the core visual design of the Telebirr home experience, including the balance dashboard, service shortcuts grid, promotional banner carousel, and bottom navigation bar.

---

## Screenshots

<img src="https://github.com/Burati063/Telebirr-clone/assets/106627959/37502f69-0ac8-42eb-a036-28e2a66ce57d.png" width="220" alt="App Screenshot" />

---

## Project Structure

```
lib/
├── main.dart                      # App entry point
├── colors.dart                    # Global color constants
├── constants.dart                 # Grid labels and carousel image list
├── screens/
│   ├── main_screen.dart           # Root scaffold with bottom navigation
│   └── home_screen.dart           # Home tab (fully implemented)
└── widgets/
    ├── balance_info.dart          # Toggleable balance card
    ├── grid_content.dart          # 4-column icon/label grid + icon definitions
    ├── notification_area.dart     # Search, notification badge, language selector
    ├── user_introduction.dart     # User greeting row
    └── transaction detail.dart    # "Transaction Details" link row
```

---

## Architecture

The app uses a simple, flat architecture with no external state management or routing library.

```
main.dart
  └── MyApp (MaterialApp)
        └── MainScreen (StatefulWidget)
              ├── HomeScreen         ← Tab 0 (fully built)
              ├── Center("Payment")  ← Tab 1 (stub)
              ├── Center("Apps")     ← Tab 2 (stub)
              └── Center("Account")  ← Tab 3 (stub)
```

`MainScreen` holds a `_currentIndex` integer and swaps between tab bodies using `setState`. There are no named routes or navigation stack — tab switching replaces the body widget directly.

---

## Screens

### `MainScreen` (`screens/main_screen.dart`)

The root shell of the app. Renders a `Scaffold` with:

- **Body:** whichever tab widget corresponds to `_currentIndex`
- **BottomNavigationBar:** four items with a semi-transparent green background (`#8DC73F` at 85% opacity), white icons/labels for both selected and unselected states

| Index | Label | Status |
|---|---|---|
| 0 | Home | Fully implemented |
| 1 | Payment | Stub (`Text('Payment')`) |
| 2 | Apps | Stub (`Text('Apps')`) |
| 3 | Account | Stub (`Text('Account')`) |

---

### `HomeScreen` (`screens/home_screen.dart`)

The only fully-implemented screen. Composed of a fixed green header and a scrollable body.

**AppBar**
- White background
- Ethio Telecom logo (left) and Telebirr wordmark (right)
- Status bar tinted green (`#8CC73F`)

**Header (fixed green section, height 170)**
- `UserIntroduction` widget — greeting row with person icon
- `NotificationArea` widget — search, notification badge (count: 23), language dropdown
- Three `BalanceInfo` cards:
  - **Balance (ETB)** — large, centered
  - **Endekise (ETB)** — small, left-aligned
  - **Reward (ETB)** — small, right-aligned
- Amber marquee ticker at the bottom edge: `"ONE APP FOR ALL YOUR NEEDS!"`

**Scrollable Body**
1. **Top service grid** — 4×2 grid of quick-action shortcuts (see [Constants](#constants--theme))
2. **Promotional banner carousel** — auto-playing image slider with 3 banners (`banner-1.jpg`, `banner-2.jpg`, `banner-3.jpg`); aspect ratio 39:9
3. **Dots indicator** — shows current carousel position with green-bordered inactive dots
4. **Transaction Details link** — right-aligned link row (navigates nowhere, UI only)
5. **Bottom service grid** — second 4×2 grid with additional service shortcuts

**Floating Action Button**
- Blue (`#0287D0`) full-width-ish button with a QR scanner icon
- Label: `Scan QR`
- `onPressed` is currently empty (no QR scanner integration)

---

## Widgets

### `BalanceInfo` (`widgets/balance_info.dart`)

A stateful widget that shows a labelled balance amount with a visibility toggle.

| Prop | Type | Description |
|---|---|---|
| `label` | `String` | Label text (e.g. `"Balance (ETB) "`) |
| `labelFontSize` | `double` | Font size for the label |
| `balanceFontSize` | `double` | Font size for the balance number |
| `crossAxisAlignment` | `CrossAxisAlignment` | Column alignment (default: center) |

Tapping the eye icon toggles between `"******"` (hidden) and a randomly generated number via `Random().nextInt(1000)`. The balance uses the Roboto font via `google_fonts`.

---

### `GridContent` (`widgets/grid_content.dart`)

A stateless widget that renders a 4-column, 2-row grid of service shortcuts.

| Prop | Type | Description |
|---|---|---|
| `gridIcon` | `List<Widget>` | 8 icon/image widgets |
| `gridLabel` | `List<String>` | 8 label strings |

Each cell is a white rounded-corner card (`BorderRadius.circular(10)`). Scrolling is disabled (`NeverScrollableScrollPhysics`) since the grid always sits inside the home screen's `SingleChildScrollView`.

**`GridIcons`** is a helper widget that renders a `Material` icon in the app's green color. If the icon is `Icons.ad_units_outlined` (Airtime), it wraps it in an amber `Badge` displaying `"+10%"`.

**Top grid icons & labels:**

| Icon | Label |
|---|---|
| `Icons.wallet` | Send Money |
| `Icons.wallet_giftcard` | Cash In/Out |
| `Icons.ad_units_outlined` *(badged)* | Airtime/Packages |
| `Icons.clean_hands_sharp` | Request Money |
| `images/dashen.png` | Financial Service With Dashen |
| `images/cbe.png` | Financial Service With CBE |
| `Icons.storefront` | Pay for Merchants |
| `Icons.window_sharp` | App |

**Bottom grid icons & labels:**

| Icon | Label |
|---|---|
| `images/christmas.png` | 33rd Christmas Trade Fair |
| `images/ethio-logo.png` | Pay Ethio Telecom Bill |
| `Icons.calendar_month_outlined` | Schedule Payment |
| `FontAwesomeIcons.bank` | Transfer to Bank |
| `Icons.wallet_sharp` | Transfer to Wallet |
| `Icons.payments_outlined` | Fuel Payment |
| `Icons.location_on` | Nearby Fuel Station |
| `Icons.add_circle_outline_sharp` | More |

---

### `NotificationArea` (`widgets/notification_area.dart`)

A stateless row widget rendered in the top-right corner of the home header.

- Search icon (white)
- Notification bell icon with a `Badge` showing `"23"`
- `DropDownLang` — a language selector dropdown (currently no items, UI only)

---

### `UserIntroduction` (`widgets/user_introduction.dart`)

A simple stateless row with a person icon and hardcoded greeting: `"Selam, BirukBr7"`.

---

### `TransactionDetails` (`widgets/transaction detail.dart`)

A right-aligned row with the text `"Transaction Details"` and a right-arrow icon, both in blue (`#1384B9`). Acts as a link placeholder — no navigation is wired up.

---

## Constants & Theme

**`lib/colors.dart`**
```dart
const Color mainColor = Color.fromRGBO(140, 202, 59, 1); // Telebirr green
```

**`lib/constants.dart`**

- `topGridLabel` — 8 strings for the top service grid
- `bottomGridLabel` — 8 strings for the bottom service grid
- `carouselImages` — list of 3 `Image` widgets from asset paths

---

## Dependencies

| Package | Version | Purpose |
|---|---|---|
| `cupertino_icons` | ^1.0.2 | iOS-style icons |
| `text_scroll` | ^0.2.0 | Scrolling text widget |
| `marquee` | ^2.2.3 | Continuous horizontal text ticker |
| `google_fonts` | ^6.1.0 | Custom typography (Roboto used in balances) |
| `font_awesome_flutter` | ^10.6.0 | Extended icon set (bank icon in bottom grid) |
| `carousel_slider` | ^4.2.1 | Auto-playing image carousel |
| `carousel_indicator` | ^1.0.6 | Carousel position indicator |
| `dots_indicator` | ^3.0.0 | Dot-based pagination indicator |
| `flutter_sticky_widgets` | ^0.0.3 | Sticky widget positioning |
| `dot_navigation_bar` | ^1.0.2 | Dot-style navigation bar |
| `flutter_launcher_icons` | ^0.13.1 | Custom app launcher icon generation |

---

## Assets

All image assets live in the `images/` directory:

| File | Used In |
|---|---|
| `ethio.png` | AppBar — Ethio Telecom logo |
| `telebirr.png` | AppBar — Telebirr wordmark |
| `cbe.png` | Top grid — CBE bank shortcut |
| `dashen.png` | Top grid — Dashen bank shortcut |
| `banner-1.jpg` | Carousel slide 1 |
| `banner-2.jpg` | Carousel slide 2 |
| `banner-3.jpg` | Carousel slide 3 |
| `ethio-logo.png` | Bottom grid — Ethio Telecom Bill shortcut |
| `christmas.png` | Bottom grid — Christmas trade fair shortcut |


---

## Getting Started

### Prerequisites

- [Flutter SDK](https://docs.flutter.dev/get-started/install) installed (Dart SDK >=3.1.5)
- Android Studio or VS Code with the Flutter extension
- A connected device or emulator

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Burati063/TeleBirr-clone.git
cd TeleBirr-clone

# 2. Install dependencies
flutter pub get

# 3. Run the app
flutter run
```

### Generate launcher icons (optional)

```bash
dart run flutter_launcher_icons
```

---

## Known Limitations / Future Work

- Payment, Apps, and Account tabs are stub placeholders with no content
- Scan QR button exists but the QR scanner is not integrated
- Language dropdown has no items and does not change app locale
- Balance amounts are randomly generated numbers, not real data
- No backend, API, or authentication — purely a UI demo
- `TransactionDetails` link does not navigate anywhere

---

## License

This project is open source and available under the [MIT License](LICENSE). It is not affiliated with Ethio Telecom or the Telebirr service.
