# FraudGuard System - Complete Project Structure

```
fraudguard_system/
│
├── 📄 README.md                          # Main project documentation
├── 📄 TROUBLESHOOTING.md                 # Common issues and solutions
├── 📄 package.json                       # Root package manager
├── 🔧 setup.sh                          # Automated setup script
│
├── 📁 backend/                          # Node.js + Express Backend
│   ├── 📁 controllers/
│   │   └── frauddetection.js           # Main fraud detection logic
│   │
│   ├── 📁 middleware/
│   │   └── auth.js                     # JWT authentication middleware
│   │
│   ├── 📁 models/
│   │   ├── user.js                     # User database schema
│   │   └── scan.js                     # Scan results schema
│   │
│   ├── 📁 routes/
│   │   ├── auth.js                     # Authentication routes
│   │   └── scan.js                     # Scanning routes
│   │
│   ├── 📄 server.js                    # Express server entry point
│   ├── 📄 package.json                 # Backend dependencies
│   ├── 📄 .env                         # Environment variables (MongoDB, JWT)
│   └── 📄 .gitignore                   # Git ignore rules
│
└── 📁 frontend/                         # React + Vite Frontend
    ├── 📁 src/
    │   ├── 📁 components/
    │   │   ├── ScanResult.jsx          # Display scan results
    │   │   └── ScanHistory.jsx         # Show scan history table
    │   │
    │   ├── 📁 pages/
    │   │   ├── Login.jsx               # Login page
    │   │   ├── Signup.jsx              # Registration page
    │   │   └── Dashboard.jsx           # Main app dashboard
    │   │
    │   ├── 📁 utils/
    │   │   └── api.js                  # Axios API configuration
    │   │
    │   ├── App.jsx                     # Main app component
    │   ├── main.jsx                    # React entry point
    │   └── index.css                   # Global styles with Tailwind
    │
    ├── 📄 index.html                   # HTML template
    ├── 📄 package.json                 # Frontend dependencies
    ├── 📄 vite.config.js               # Vite configuration
    ├── 📄 tailwind.config.js           # Tailwind CSS config
    ├── 📄 postcss.config.js            # PostCSS config
    ├── 📄 .env.example                 # Environment variables example
    └── 📄 .gitignore                   # Git ignore rules
```

## 📊 File Count Summary

**Backend Files:** 10 files
- 4 Model/Controller/Middleware files
- 2 Route files  
- 1 Server file
- 3 Configuration files

**Frontend Files:** 15 files
- 5 React component/page files
- 1 Utility file
- 9 Configuration/setup files

**Documentation:** 2 files
**Scripts:** 1 file

**Total:** 28+ files

## 🔑 Key Files Explained

### Backend
- **server.js**: Main entry point, sets up Express, MongoDB, routes
- **frauddetection.js**: Core algorithm analyzing URLs for threats
- **auth.js** (middleware): Validates JWT tokens
- **user.js** & **scan.js**: MongoDB schemas using Mongoose

### Frontend
- **App.jsx**: Main component with routing logic
- **Dashboard.jsx**: Main interface with URL scanner
- **api.js**: Centralized API calls with Axios
- **index.css**: Tailwind CSS imports and custom styles

### Configuration
- **.env**: Contains secrets (MongoDB URI, JWT secret)
- **vite.config.js**: Dev server and build settings
- **tailwind.config.js**: Tailwind customization

## 🚀 Getting Started

1. Run setup script: `./setup.sh`
2. Start backend: `cd backend && npm run dev`
3. Start frontend: `cd frontend && npm run dev`
4. Access app at: `http://localhost:3000`

## 📦 Dependencies

### Backend (10 packages)
- express, mongoose, bcryptjs, jsonwebtoken, cors, dotenv
- nodemon (dev)

### Frontend (12+ packages)
- react, react-dom, react-router-dom
- axios, lucide-react
- vite, tailwindcss, postcss, autoprefixer
