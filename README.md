# 2021471533.github.io
my-app
# UFS-AEBA – Campus Banking (Android)

Pure **Java** Android client for the UFS Accommodation Easy Banking App API.

Matches the web app login look (dark navy header, white sign-in card) and talks to the same .NET API.

## Requirements

- Android Studio Hedgehog+ (or compatible)
- JDK 11+
- Android Emulator (API 24+)
- The **UFSCampusBank** API running on the host machine

## 1. Start the API first

```bash
cd UFS-AEBA-WithApi/UFS-AEBA-SQLServer-VehicleFix/UFSCampusBank
dotnet restore
dotnet run --urls "http://0.0.0.0:5150"
```

Confirm in a browser: http://localhost:5150

## 2. Open & run the Android app

1. Open folder `UFS-AEBA-Android` in Android Studio
2. Let Gradle sync
3. Start an emulator
4. Run the `app` configuration

The app uses **`http://10.0.2.2:5150/`** which is the emulator’s alias for the host’s `localhost`.

## Demo accounts

| Role               | Email                    | Password     |
|--------------------|--------------------------|--------------|
| Admin              | admin@ufs.ac.za          | Admin@123    |
| Financial Officer  | finance@ufs.ac.za        | Finance@123  |
| Student (Client)   | student@ufs4life.ac.za   | Student@123  |
| Staff (Client)     | staff@ufs.ac.za          | Staff@123    |

## Features

- **Splash** → auto-login if session exists
- **Login** screen matching the web/mobile mock (UFS-AEBA / Campus Banking)
- Role-based navigation:
  - **Client** – balance, total paid, pending, recent payments
  - **Admin** – clients, revenue, pending payments, units
  - **Financial Officer** – pending payments with Accept / Reject
- JWT stored in SharedPreferences and sent on every API call
- Cleartext HTTP allowed for `10.0.2.2` (emulator)

## Project structure (Java only)

```
app/src/main/java/za/ac/ufs/aeba/
  UfsApp.java
  api/          ApiClient, ApiService
  model/        DTOs matching the .NET ApiDtos
  ui/           Splash, Login, Client/Admin/Finance dashboards
  util/         SessionManager
```

## Physical device

On a real phone, change `ApiClient.BASE_URL` to your PC’s LAN IP, e.g.:

```java
public static final String BASE_URL = "http://192.168.1.100:5150/";
```

and ensure the phone and PC are on the same Wi-Fi, with the API bound to `0.0.0.0:5150`.

## Tech stack

Same approach as UniTag Asset Tracker:

- Java 11
- Retrofit 2 + Gson + OkHttp logging
- Material Components 3
- RecyclerView, SwipeRefreshLayout
