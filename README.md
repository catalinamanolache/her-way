# HerWay — Nighttime Safety Map & Pedestrian Routing for Women

![Angular](https://img.shields.io/badge/Angular-18+-DD0031?style=flat&logo=angular&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=flat&logo=typescript&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-BaaS-FFCA28?style=flat&logo=firebase&logoColor=black)
![ArcGIS](https://img.shields.io/badge/ArcGIS_Maps_SDK-JS_4.x-0079C1?style=flat&logo=arcgis&logoColor=white)

**HerWay** is a Web GIS application designed to help women feel safer when walking at night in Bucharest. Unlike standard navigation apps that optimize strictly for shortest distance or time, HerWay calculates **safe pedestrian routes** that dynamically avoid community-reported risk zones (poorly lit streets, isolated areas, harassment incidents, stray dogs) while highlighting nearby 24/7 **Safe Spots**.

Developed for the **Information Systems Integration (ISI)** course at the National University of Science and Technology POLITEHNICA Bucharest (Faculty of Automatic Control and Computer Science).

---

## Key Features

- **Comparative Safe Pedestrian Routing:** Computes and displays the *Safest Route* (avoiding active `RiskZones` via ArcGIS spatial barriers) side-by-side with the *Shortest Route*, showing distance, duration, and avoided hazards.
- **24/7 Safe Spots & 1-Click Refuge Routing:** Interactive map of non-stop pharmacies, police stations, hospitals, gas stations, and 24/7 shops with category filtering and an instant 1-click route to the nearest safe location.
- **Real-Time Collaborative Reporting:** Authenticated users can pin risk zones on the map with category, severity (1–3), description, and optional compressed photo upload, plus automatic expiration for temporary incidents.
- **Risk Heatmap & Analytics Dashboard:** Dynamic `HeatmapRenderer` layer for visualizing incident density alongside statistical charts by risk category.
- **Saved Routes & Shareable Links:** Users can save frequent routes (e.g., *University → Dorm*) and generate a read-only share link for a trusted contact.
- **Admin Moderation Panel:** Role-protected (`adminGuard`) interface for approving/removing community reports and managing official Safe Spots.
- **No-Login Emergency Access:** Viewing the map, calculating safe routes, and routing to the nearest Safe Spot require no authentication.

---

## Architecture & Tech Stack

HerWay follows a **Web + Firebase (Serverless / BaaS)** architecture integrated with **ArcGIS Online**:

| Layer | Technologies & Modules |
| :--- | :--- |
| **Frontend** | Angular (Standalone Components), TypeScript, RxJS, Angular Material, Tailwind CSS, `ng2-charts` |
| **GIS & Mapping** | ArcGIS Online (Hosted Feature Layers, Route Service, Locator), ArcGIS Maps SDK for JavaScript (`@arcgis/core`) |
| **Backend Services (BaaS)** | Firebase Authentication, Cloud Firestore (NoSQL + Security Rules), Firebase Storage, Firebase Hosting (`@angular/fire`) |

### Data Model Overview
- **ArcGIS Online Feature Layers:**
  - `SafeSpots` (Point, WGS84): `spot_id`, `name`, `category`, `address`, `phone`, `is_24_7`, `verified`
  - `RiskZones` (Point/Polygon, WGS84): `risk_id`, `risk_type`, `severity`, `status`, `reported_by`, `created_at`, `expires_at`, `confirmations`
- **Firebase Cloud Firestore Collections:**
  - `users`: profile data, `role` (`user` | `admin`), `emergencyContact`
  - `saved_routes`: frequent routes, coordinates, `safetyScore`, `shareToken`
  - `report_logs`: audit trail linking `users.uid`, `RiskZones.risk_id`, moderation status, and Firebase Storage `imageUrl`

---

## Getting Started

### Prerequisites
- [Node.js](https://nodejs.org/) (v18+ LTS)
- [Angular CLI](https://angular.dev/tools/cli) (`npm install -g @angular/cli`)
- [Firebase CLI](https://firebase.google.com/docs/cli) (`npm install -g firebase-tools`)
- An **ArcGIS Developer / ArcGIS Online** account (API Key + Hosted Feature Layer URLs)
- A **Firebase** project with Authentication, Firestore, and Storage enabled

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<organization>/<repo>.git
   cd <repo>
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Configure environment variables:**
   Create `src/environments/environment.ts` (and `environment.development.ts`) with your Firebase and ArcGIS credentials:
   ```typescript
   export const environment = {
     production: false,
     firebase: {
       apiKey: 'YOUR_FIREBASE_API_KEY',
       authDomain: 'YOUR_PROJECT.firebaseapp.com',
       projectId: 'YOUR_PROJECT_ID',
       storageBucket: 'YOUR_PROJECT.appspot.com',
       messagingSenderId: 'YOUR_SENDER_ID',
       appId: 'YOUR_APP_ID'
     },
     arcgis: {
       apiKey: 'YOUR_ARCGIS_API_KEY',
       safeSpotsLayerUrl: 'YOUR_SAFESPOTS_FEATURE_LAYER_URL',
       riskZonesLayerUrl: 'YOUR_RISKZONES_FEATURE_LAYER_URL',
       routeServiceUrl: 'https://route-api.arcgis.com/arcgis/rest/services/World/Route/NAServer/Route_World'
     }
   };

4. **Run the development server:**
   ```bash
   ng serve
   ```
   Open `http://localhost:4200/` in your browser.

### Deployment (Firebase Hosting)
```bash
ng build --configuration production
firebase deploy
```

---

## Project Links

- **Trello Board:** `[https://trello.com/b/qsOMDbeb]`
- **Live Demo:** `TODO` *(available after final deployment)*
- **Documentation:** See `/docs` for M0 documentation and PlantUML (`.puml`) diagram sources.

---

## Team (Group 345C4)

| Name | Role | Responsibilities |
| :--- | :--- | :--- |
| **Manolache Maria-Cătălina** | Team Lead | ArcGIS Feature Layers, pilot dataset (Bucharest), Angular setup, `MapView` & Safe Spots filters |
| **Postelnicu Cristina-Marina** | Member | Firebase Auth/Firestore/Storage, Security Rules, incident reporting, saved routes & Admin moderation |
| **Țigău Mara** | Member | Safe routing service (`route.solve` with barriers), 1-click Safe Spot routing, Heatmap & analytics dashboard |

---

## Disclaimer

HerWay is an informational and preventive tool powered by community-reported data. It is **not** a replacement for official emergency services. In case of immediate danger, please call **112**.
