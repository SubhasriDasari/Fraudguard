# FraudGuard - Advanced URL Security Scanner

A full-stack web application for detecting fraudulent and malicious URLs using **advanced AI-powered security analysis** including Google Safe Browsing, SSL validation, machine learning, and real-time threat intelligence.

## 🚀 Features

### Core Features:
- **User Authentication**: Secure signup/login with JWT tokens
- **Advanced URL Scanning**: Multi-layered security analysis with 7 detection systems
- **Risk Scoring**: 0-100 risk score with AI/ML categorization
- **Threat Detection**: Identifies 15+ types of security threats
- **Scan History**: Track all previous scans with detailed results
- **Statistics Dashboard**: View analytics by risk level
- **Responsive Design**: Works on desktop and mobile

### 🆕 Advanced Security Features:

1. **✅ Google Safe Browsing API** - Real-time malware & phishing detection
2. **🔒 SSL Certificate Validation** - Verify HTTPS certificates
3. **📅 Domain Age Checking** - Identify newly registered domains
4. **🎯 Reputation Database** - Check domain trustworthiness
5. **🚫 Real-time Blacklist Checking** - Multi-source threat intelligence
6. **🔍 Website Content Analysis** - Analyze actual webpage content
7. **🤖 Machine Learning Model** - AI-powered risk prediction

**See [ADVANCED_FEATURES.md](ADVANCED_FEATURES.md) for detailed documentation**

## 🛠️ Tech Stack

### Backend
- **Node.js** & **Express.js** - Server framework
- **MongoDB** - Database
- **Mongoose** - ODM for MongoDB
- **JWT** - Authentication
- **bcryptjs** - Password hashing

### Frontend
- **React 18** - UI library
- **Vite** - Build tool
- **React Router** - Navigation
- **Tailwind CSS** - Styling
- **Axios** - HTTP client
- **Lucide React** - Icons

## 📋 Prerequisites

- Node.js (v16 or higher)
- MongoDB (v5 or higher)
- npm or yarn package manager

## 🔧 Installation

### 1. Clone the repository
```bash
git clone <repository-url>
cd fraudguard_system
```

### 2. Backend Setup

```bash
cd backend

# Install dependencies
npm install

# Create .env file (use the existing one or create from example)
# Make sure to update JWT_SECRET with a strong random string

# Start MongoDB (if not running)
# On Mac: brew services start mongodb-community
# On Ubuntu: sudo systemctl start mongod
# On Windows: Start MongoDB service from Services

# Run the backend server
npm run dev
```

The backend server will start on `http://localhost:5000`

### 3. Frontend Setup

```bash
cd ../frontend

# Install dependencies
npm install

# Create .env file
cp .env.example .env

# Start the development server
npm run dev
```

The frontend will start on `http://localhost:3000`

## 🌐 Environment Variables

### Backend (.env)
```env
MONGODB_URI=mongodb://localhost:27017/fraudguard
JWT_SECRET=your_very_long_and_random_secret_key_here
PORT=5000
NODE_ENV=development
```

### Frontend (.env)
```env
VITE_API_URL=http://localhost:5000/api
```

## 📁 Project Structure

```
fraudguard_system/
├── backend/
│   ├── controllers/
│   │   └── frauddetection.js    # Scan logic
│   ├── middleware/
│   │   └── auth.js               # JWT authentication
│   ├── models/
│   │   ├── user.js               # User schema
│   │   └── scan.js               # Scan schema
│   ├── routes/
│   │   ├── auth.js               # Auth routes
│   │   └── scan.js               # Scan routes
│   ├── .env
│   ├── package.json
│   └── server.js                 # Entry point
│
└── frontend/
    ├── src/
    │   ├── components/
    │   │   ├── ScanResult.jsx
    │   │   └── ScanHistory.jsx
    │   ├── pages/
    │   │   ├── Login.jsx
    │   │   ├── Signup.jsx
    │   │   └── Dashboard.jsx
    │   ├── utils/
    │   │   └── api.js            # API utilities
    │   ├── App.jsx
    │   ├── main.jsx
    │   └── index.css
    ├── index.html
    ├── package.json
    ├── vite.config.js
    └── tailwind.config.js
```

## 🔍 How It Works

### URL Analysis Algorithm

The fraud detection algorithm analyzes URLs based on multiple factors:

1. **Protocol Security** - Checks for HTTPS encryption
2. **IP Address Detection** - Flags URLs using IP addresses instead of domains
3. **URL Length** - Identifies suspiciously long URLs
4. **Subdomain Analysis** - Detects excessive subdomains
5. **Phishing Keywords** - Scans for common phishing terms
6. **Brand Impersonation** - Identifies fake brand domains
7. **Suspicious TLDs** - Flags high-risk domain extensions
8. **Character Validation** - Detects invalid characters
9. **URL Shorteners** - Identifies shortened URLs
10. **Login Page Security** - Extra checks for authentication pages

Each factor contributes to a risk score (0-100), which determines the risk level:
- **0-29**: Low Risk ✅
- **30-49**: Medium Risk ⚠️
- **50-69**: High Risk ⚡
- **70-100**: Critical Risk 🚨

## 🔐 API Endpoints

### Authentication
- `POST /api/auth/signup` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user (protected)

### Scanning
- `POST /api/scan/scan` - Scan a URL (protected)
- `GET /api/scan/history` - Get scan history (protected)
- `GET /api/scan/stats` - Get scan statistics (protected)

## 🚀 Deployment

### Backend Deployment (Heroku/Railway/Render)

1. Push code to GitHub
2. Connect to your deployment platform
3. Set environment variables
4. Deploy!

### Frontend Deployment (Vercel/Netlify)

```bash
cd frontend
npm run build
# Deploy the dist/ folder
```

## 🧪 Testing

### Test the Backend API
```bash
# Health check
curl http://localhost:5000/

# Signup
curl -X POST http://localhost:5000/api/auth/signup \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@example.com","password":"password123"}'

# Scan URL (replace TOKEN with actual JWT)
curl -X POST http://localhost:5000/api/scan/scan \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer TOKEN" \
  -d '{"url":"http://phishing-example.com/login"}'
```

## 🐛 Troubleshooting

### MongoDB Connection Issues
- Ensure MongoDB is running: `mongod --version`
- Check connection string in `.env`
- Verify MongoDB service is started

### Port Already in Use
```bash
# Kill process on port 5000 (backend)
lsof -ti:5000 | xargs kill -9

# Kill process on port 3000 (frontend)
lsof -ti:3000 | xargs kill -9
```

### CORS Issues
- Ensure backend CORS is properly configured
- Check API URL in frontend `.env`

## 🔮 Future Enhancements

- [ ] Real-time URL database checking
- [ ] Machine learning model for better detection
- [ ] Browser extension
- [ ] API rate limiting
- [ ] Email notifications for high-risk URLs
- [ ] Multi-factor authentication
- [ ] Social sharing of scan results
- [ ] PDF export of scan reports

## 📄 License

MIT License - feel free to use this project for learning and development!

## 👨‍💻 Author

Created with ❤️ for cybersecurity education

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

---

**⚠️ Disclaimer**: This tool is for educational purposes. It provides basic URL analysis and should not be the sole method for determining URL safety. Always exercise caution when visiting unknown websites.
