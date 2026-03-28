<div align="center">

<!-- Animated Banner -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=4E0911&height=200&section=header&text=SIST%20Campus%20Navigator&fontSize=42&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Real-time%20Campus%20Navigation%20%7C%20Sathyabama%20Institute%20of%20Science%20%26%20Technology&descAlignY=58&descSize=16" />

<!-- Badges -->
<p>
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white" />
  <img src="https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white" />
  <img src="https://img.shields.io/badge/Socket.io-010101?style=for-the-badge&logo=socketdotio&logoColor=white" />
  <img src="https://img.shields.io/badge/Leaflet-199900?style=for-the-badge&logo=leaflet&logoColor=white" />
  <img src="https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white" />
</p>

<p>
  <img src="https://img.shields.io/badge/status-live-brightgreen?style=flat-square" />
  <img src="https://img.shields.io/badge/license-ISC-blue?style=flat-square" />
  <img src="https://img.shields.io/badge/version-1.0.0-orange?style=flat-square" />
</p>

</div>

---

## What is this?

**SIST Campus Navigator** is a full-stack real-time navigation and tracking system built for **Sathyabama Institute of Science and Technology**. Students can register, get turn-by-turn directions to any campus landmark using a graph-based pathfinding algorithm (Dijkstra), and share their live GPS location — while security personnel monitor all active users on a live map dashboard.

---

## Live Demo

| Service | URL |
|---|---|
| Frontend (Vercel) | [sistnavigation.vercel.app](https://sistnavigation.vercel.app) |
| Backend API (Render) | [sistnavigation.onrender.com](https://sistnavigation.onrender.com) |

---

## Features

```
✅ User registration with Aadhaar & phone validation
✅ Real-time GPS tracking via WebSockets (Socket.io)
✅ Dijkstra shortest-path algorithm for campus routing
✅ Satellite map view powered by Leaflet + ArcGIS
✅ Turn-by-turn directions panel
✅ Wrong-direction detection with vibration alert
✅ GPS accuracy meter with color-coded feedback
✅ Security dashboard — live tracking of all users
✅ Unique color-coded markers per tracked user
✅ Mobile-first responsive design
✅ Custom alert system (no browser popups)
```

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                        CLIENT                           │
│                                                         │
│  index.html ──► userInput ──► user (map) ──► security   │
│                    │               │              │      │
│               Registration    Navigation      Tracking   │
│               + Validation    + Dijkstra    Dashboard    │
└──────────────────────┬──────────────┬───────────────────┘
                       │  REST API    │  WebSocket
                       ▼              ▼
┌─────────────────────────────────────────────────────────┐
│                        SERVER                           │
│                                                         │
│   Express.js  ──►  userRouter  ──►  userController      │
│                                         │               │
│   Socket.io  ◄──── updateLocation       │               │
│       │             locationUpdate      ▼               │
│       └──────────────────────────►  MongoDB             │
│                                    (User Model)         │
└─────────────────────────────────────────────────────────┘
```

---

## Project Structure

```
📦 sist-campus-navigator
├── 📁 client/
│   ├── 📁 src/
│   │   ├── 📄 index.html              # Entry point (redirects to userInput)
│   │   ├── 📄 apiConfig.js            # Base API URL config
│   │   ├── 📄 axiosInstance.js        # Axios with base URL + timeout
│   │   ├── 📁 userInput/              # Registration page
│   │   │   ├── userInput.html
│   │   │   ├── userInput.js           # Form validation + submission
│   │   │   └── userInput.css
│   │   ├── 📁 userpage/               # Navigation map (student view)
│   │   │   ├── user.html
│   │   │   ├── user.js                # Dijkstra + GPS + Socket.io
│   │   │   └── user.css
│   │   ├── 📁 securitypage/           # Live tracking dashboard
│   │   │   ├── security.html
│   │   │   ├── security.js            # Multi-user tracking via Socket.io
│   │   │   └── security.css
│   │   └── 📁 services/
│   │       └── service.js             # API service layer
│   └── 📄 vercel.json                 # Vercel static deployment config
│
└── 📁 server/
    ├── 📄 index.js                    # Express + Socket.io + MongoDB setup
    └── 📁 src/
        ├── 📁 controllers/
        │   └── userController.js      # POST /users handler
        ├── 📁 model/
        │   └── userModel.js           # Mongoose User schema
        └── 📁 router/
            └── userRouter.js          # API routes
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript (ES Modules) |
| Maps | Leaflet.js + ArcGIS Satellite Tiles |
| Pathfinding | Dijkstra's Algorithm (custom implementation) |
| Real-time | Socket.io (WebSockets) |
| HTTP Client | Axios |
| Backend | Node.js + Express.js v5 |
| Database | MongoDB + Mongoose |
| Deployment | Vercel (client) + Render (server) |

---

## API Reference

### POST `/api/v1/users`

Register a new user.

**Request Body**
```json
{
  "name": "string",
  "phone": "string (10 digits)",
  "aadhaar": "string (12 digits)"
}
```

**Responses**

| Status | Description |
|---|---|
| `201` | User created successfully |
| `400` | Missing required fields |
| `409` | User already exists with this phone |
| `500` | Internal server error |

---

## WebSocket Events

| Event | Direction | Payload |
|---|---|---|
| `updateLocation` | Client → Server | `{ phoneNumber, name, location: { lat, lng, accuracy, timestamp } }` |
| `locationUpdate` | Server → All Clients | `{ phoneNumber, name, location }` |

---

## Getting Started

### Prerequisites

- Node.js >= 18
- MongoDB URI (Atlas or local)

### 1. Clone the repo

```bash
git clone https://github.com/your-username/sist-campus-navigator.git
cd sist-campus-navigator
```

### 2. Setup the server

```bash
cd server
npm install
```

Create a `.env` file:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
```

Start the server:

```bash
npm run dev
```

### 3. Serve the client

The client is pure static HTML/CSS/JS. You can open it directly in a browser or serve it with any static server:

```bash
cd client
npx serve src
```

Or deploy to Vercel using the included `vercel.json`.

---

## Campus Landmarks

The navigation system covers **60+ nodes** across the SIST campus including:

- Central Library
- Startup Cell
- International Research Centre
- Ocean Research Park
- Block 1
- Remibai Auditorium
- Indoor Stadium
- Boys & Girls Hostels
- Sathyabama General Hospital
- Administration Block
- Main Canteen & Mess
- New CSE Block
- And many more...

---

## How Navigation Works

1. User registers with name, phone, and Aadhaar number
2. On the map page, user selects a destination from the dropdown
3. The app finds the **closest graph node** to the user's GPS position
4. **Dijkstra's algorithm** computes the shortest path through the campus road network
5. The route is drawn on the satellite map with road name labels
6. Live GPS updates are broadcast via **Socket.io** to the security dashboard
7. If the user deviates from the route, a **wrong-direction alert** triggers with haptic feedback

---

## Environment Variables

| Variable | Description |
|---|---|
| `PORT` | Server port (default: `5000`) |
| `MONGO_URI` | MongoDB connection string |

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=4E0911&height=100&section=footer&animation=fadeIn" />

<p>Built with ❤️ for Sathyabama Institute of Science and Technology</p>

</div>
