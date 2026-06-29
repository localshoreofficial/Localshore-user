# LocalShore 🛍️

> Hyperlocal commerce & instant delivery platform — order from shops near you, delivered in minutes.

LocalShore connects buyers with nearby local shops (groceries, electronics, fashion, meat & seafood, kids & toys, and more) and provides an end-to-end shopping experience: browse shops within a 15 km radius, add products to cart, check out, track your delivery live on a map, and manage orders, wishlist, and your profile — all from a single page app backed by Firebase.

---

## ✨ Features

- **Location-aware shop discovery** — auto-detects user location and shows only shops within a 15 km radius, sorted by distance.
- **Category browsing** — quick-filter shops by category (Fashion, Kids & Toys, Appliances, Sports, Meat & Seafood, Electronics, and admin-defined categories from Firestore).
- **Live search** — instant dropdown search across both products and shops.
- **Dynamic homepage content** — banner gallery and "Hot Offers" carousel driven by Firestore (`admin_banners`, `admin_offers`), with automatic fallback to default content when empty. Banners adapt their height to the uploaded image/video aspect ratio.
- **Newly added products** — a "Fresh Picks" section showing the latest product from each nearby vendor.
- **Cart & checkout** — persistent cart (per device), quantity controls, single-shop cart enforcement, and a delivery details flow with:
  - Interactive Leaflet/OpenStreetMap pin-drop or auto-detect for delivery address
  - Reverse geocoding via Nominatim
  - Cash on Delivery or Online Payment options
- **Order management**
  - Order history with filters (All / Active / Delivered / Cancelled)
  - Live order tracking with a simulated delivery-partner marker, ETA, and step timeline
  - Order cancellation (while still "Confirmed")
  - Delivered-order confirmation flow ("Did you receive it?") with issue reporting
  - Return/refund request shortcut for delivered items within the return window
- **Wishlist** — heart any product, view/manage saved items, jump straight to the shop.
- **Authentication** — Firebase Auth (email/password) with login/signup modal, guest browsing mode, and persisted session/profile data.
- **Profile management** — edit name, email, phone, and saved delivery address.
- **Real-time notifications** — Firestore-backed notification feed with categories (Offers, Orders, Alerts), unread badges, toast/sound alerts for new notifications, and read-state tracking.
- **Personalization**
  - Light / Dark theme toggle
  - Multi-language support: **English, Hindi (हिंदी), Tamil (தமிழ்)**
- **Fully responsive UI** — optimized layouts for desktop, tablet, and mobile.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3 (custom design system, no framework), Vanilla JavaScript |
| Backend / Data | [Firebase](https://firebase.google.com/) (Firestore + Authentication) |
| Maps & Geolocation | [Leaflet.js](https://leafletjs.com/) + [OpenStreetMap](https://www.openstreetmap.org/) tiles, [Nominatim](https://nominatim.org/) reverse geocoding |
| Fonts | Google Fonts — Syne & DM Sans |

No build tools, bundlers, or backend server required — it's a static site that talks directly to Firebase.

---

## 📂 Firestore Data Model

The app reads/writes the following Firestore collections:

| Collection | Purpose |
|---|---|
| `shops` | Shop listings (name, category, location, address, rating, hours, etc.) |
| `products` | Products linked to a shop via `shopId` (name, price, stock, category, image) |
| `orders` | Customer orders (cart snapshot, delivery info, payment, status, tracking) |
| `notifications` | Push-style in-app notifications (targeted by device or broadcast) |
| `admin_banners` | Homepage banner slides (image, video, or gradient type) — managed by an admin panel |
| `admin_offers` | "Hot Offers" carousel items — managed by an admin panel |
| `admin_categories` | Dynamic homepage category cards |

> Note: Order/notification identity is tracked per-browser via a generated `deviceId` stored in `localStorage`, in addition to Firebase Auth for account-gated actions (orders, wishlist, profile, notifications).

---

## 🚀 Getting Started

### Prerequisites
- A [Firebase](https://console.firebase.google.com/) project with **Firestore** and **Authentication (Email/Password)** enabled.
- The Firestore collections above, populated with at least some `shops` and `products` documents.

### Setup

1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/localshore.git
   cd localshore
   ```

2. Replace the Firebase config in `home.html` (inside the `<script>` block) with your own project credentials:
   ```js
   firebase.initializeApp({
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_PROJECT.firebaseapp.com",
     projectId: "YOUR_PROJECT_ID",
     storageBucket: "YOUR_PROJECT.firebasestorage.app",
     messagingSenderId: "YOUR_SENDER_ID",
     appId: "YOUR_APP_ID"
   });
   ```
   The app also initializes a second Firebase app (`localshoreAuth`) used for authentication — update or remove this as needed for your setup.

3. Set appropriate **Firestore Security Rules** for `shops`, `products`, `orders`, `notifications`, and the `admin_*` collections based on your access model (public read for shops/products, authenticated writes for orders, etc.).

4. Serve the file locally (a simple static server is enough — geolocation and Firebase require HTTPS or `localhost`):
   ```bash
   npx serve .
   # or
   python3 -m http.server 8080
   ```

5. Open `home.html` in your browser.

---

## 📱 Usage Notes

- On first load, allow location access so the app can show shops within 15 km and personalize the experience.
- Guests can browse shops, products, banners, and offers freely. Logging in is required for cart checkout, wishlist, orders, profile, and notifications.
- Use the **settings (⚙️) menu** in the navbar to switch theme or language at any time.

---

## 🗺️ Roadmap Ideas

- Vendor & delivery-partner dashboards (separate apps already in progress)
- Push notifications via FCM
- Real-time delivery-partner GPS instead of simulated tracking
- Online payment gateway integration
- Server-side order validation via Cloud Functions

---

## 📄 License

This project is currently unlicensed for public reuse. Add a `LICENSE` file (MIT, Apache-2.0, etc.) if you intend to open-source it.

---

## 🙌 Acknowledgements

- [Leaflet](https://leafletjs.com/) & [OpenStreetMap](https://www.openstreetmap.org/) contributors
- [Nominatim](https://nominatim.org/) geocoding service
- [Firebase](https://firebase.google.com/) by Google
