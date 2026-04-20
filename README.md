# TaxiFast — Ride-Sharing Android App

A native Android ride-sharing application (similar to Uber) built for the city of **Tarija, Bolivia**. The app connects passengers with nearby taxi drivers in real time, managing the full lifecycle of a ride from request to arrival.

<!-- ---

## Screenshots

> _Add screenshots here_ -->

---

## Features

### For Passengers

- Request a taxi and track its real-time location on the map
- See nearby available drivers before booking
- Receive push notifications when a driver is assigned and when they arrive
- Address search with autocomplete
- Ride history
- Facebook and email/password authentication

### For Drivers

- Toggle between passenger and driver mode from the same app
- Receive incoming ride requests with passenger location
- Accept or reject requests
- Continuous GPS tracking shared with the passenger during the trip

### Core

- Google Maps v2 integration with live marker updates
- Service area boundary enforcement (polygon-based geofencing for Tarija)
- Real-time state machine tracking ride phases: Waiting → Taxi Assigned → En Route → Arrived
- Firebase Cloud Messaging (FCM) for push notifications with custom sounds
- Long-polling architecture for driver ↔ server ↔ passenger communication

---

## Tech Stack

| Layer              | Technology                                          |
| ------------------ | --------------------------------------------------- |
| Platform           | Android (Java)                                      |
| Maps & Location    | Google Maps Android API v2, Fused Location Provider |
| Push Notifications | Firebase Cloud Messaging (FCM)                      |
| Authentication     | Email/Password, Facebook SDK v4                     |
| Networking         | HttpURLConnection + AsyncTask                       |
| Backend            | Custom PHP REST API                                 |
| Local Storage      | SharedPreferences                                   |
| Min SDK            | API 15 (Android 4.0.3)                              |
| Target SDK         | API 23 (Android 6.0)                                |

---

## Architecture

The project follows an **Activity-based architecture** with a single-package structure. Each screen is an `Activity` responsible for its own UI, networking, and state. Background HTTP calls are handled through a custom `HttpPost` AsyncTask wrapper with a callback interface.

```
com.vitalsoftware.taxifast/
├── Activities          # All screens (Login, Register, Main, Pedir, Historial, ...)
├── Models              # Taxista.java, Pedido.java
├── Services            # FCM handlers (NotificationGenie, MyFirebaseInstanceIDService)
└── Utilities           # HttpPost, DescargarFoto, AddressAdapter, Runner (geofence)
```

### Ride State Machine (MainActivity)

```
ES  →  TA  →  AB  →  HA
│       │       │       │
Waiting Taxi   On    Arrived
       Outside Board
```

---

## Integrations

- **Google Maps API v2** — live driver markers, camera animation, address geocoding
- **Firebase Cloud Messaging** — push notifications for ride events
- **Facebook Login SDK** — social authentication
- **Custom PHP backend** — REST endpoints for auth, ride requests, location polling, and notifications

---

## Getting Started

### Prerequisites

- Android Studio (Arctic Fox or later recommended)
- Google Maps API key
- `google-services.json` from a Firebase project
- Facebook App ID

### Setup

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/taxifast-app.git
   ```

2. Open the project in Android Studio.

3. Add your `google-services.json` to the `app/` directory.

4. Set your Google Maps API key in `AndroidManifest.xml`:

   ```xml
   <meta-data
       android:name="com.google.android.maps.v2.API_KEY"
       android:value="YOUR_API_KEY_HERE" />
   ```

5. Update the Facebook App ID in `strings.xml`:

   ```xml
   <string name="facebook_app_id">YOUR_FACEBOOK_APP_ID</string>
   ```

6. Build and run on a physical device or emulator with Google Play Services.

---

## What I Learned

- Integrating the Google Maps SDK with real-time location updates and custom map markers
- Implementing a geofence using polygon-based boundary checking to restrict the service area
- Using Firebase Cloud Messaging to deliver push notifications across both sides of a two-sided marketplace
- Building a dual-mode app (passenger + driver) within a single APK using shared session state
- Managing a ride lifecycle with a state machine across asynchronous network calls

---

## License

This project is for portfolio purposes. All rights reserved.
