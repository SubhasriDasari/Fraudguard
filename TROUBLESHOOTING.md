# FraudGuard Troubleshooting Guide

## Common Errors and Solutions

### 1. MongoDB Connection Error

**Error:**
```
❌ MongoDB connection error: MongooseServerSelectionError
```

**Solutions:**
- Ensure MongoDB is installed and running
- Check if MongoDB service is active:
  ```bash
  # Mac
  brew services list
  
  # Linux
  sudo systemctl status mongod
  
  # Windows
  Check Services app for MongoDB service
  ```
- Start MongoDB:
  ```bash
  # Mac
  brew services start mongodb-community
  
  # Linux
  sudo systemctl start mongod
  
  # Windows
  Start MongoDB service from Services app
  ```
- Verify connection string in `backend/.env`:
  ```
  MONGODB_URI=mongodb://localhost:27017/fraudguard
  ```

### 2. Port Already in Use

**Error:**
```
Error: listen EADDRINUSE: address already in use :::5000
```

**Solution:**
```bash
# Find and kill process using port 5000
lsof -ti:5000 | xargs kill -9

# Or change port in backend/.env
PORT=5001
```

### 3. JWT_SECRET Not Set

**Error:**
```
Error: secretOrPrivateKey must have a value
```

**Solution:**
- Ensure `JWT_SECRET` is set in `backend/.env`
- Generate a strong secret:
  ```bash
  node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
  ```
- Add to `.env`:
  ```
  JWT_SECRET=generated_secret_here
  ```

### 4. CORS Error in Frontend

**Error:**
```
Access to XMLHttpRequest blocked by CORS policy
```

**Solutions:**
- Verify backend CORS is enabled (check `server.js`)
- Ensure API URL is correct in `frontend/.env`:
  ```
  VITE_API_URL=http://localhost:5000/api
  ```
- Make sure backend is running before starting frontend

### 5. Module Not Found

**Error:**
```
Cannot find module 'express'
```

**Solution:**
```bash
# Reinstall dependencies
cd backend
rm -rf node_modules package-lock.json
npm install

cd ../frontend
rm -rf node_modules package-lock.json
npm install
```

### 6. Vite Build Errors

**Error:**
```
Module not found or import errors
```

**Solutions:**
- Clear Vite cache:
  ```bash
  cd frontend
  rm -rf node_modules/.vite
  npm run dev
  ```
- Check all imports use correct paths
- Verify all dependencies are installed

### 7. Authentication Not Working

**Issues:**
- User can't login
- Token expired errors
- Redirected to login repeatedly

**Solutions:**
- Clear browser localStorage:
  ```javascript
  // In browser console
  localStorage.clear()
  ```
- Check token expiration (default: 7 days)
- Verify JWT_SECRET matches between requests
- Check Authorization header format: `Bearer <token>`

### 8. Tailwind CSS Not Working

**Error:**
Styles not applied or default styles only

**Solutions:**
- Ensure `tailwind.config.js` exists
- Check `postcss.config.js` is present
- Verify `index.css` imports Tailwind:
  ```css
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
  ```
- Restart dev server after config changes

### 9. API Requests Failing

**Error:**
```
Network Error or 404 Not Found
```

**Solutions:**
- Verify backend is running on correct port
- Check API URL in frontend `.env`
- Ensure routes are correctly defined in backend
- Test endpoints with curl:
  ```bash
  curl http://localhost:5000/
  ```

### 10. Password Too Short Error

**Error:**
```
Password must be at least 6 characters
```

**Solution:**
- Use password with minimum 6 characters
- Check validation in both frontend and backend

## Development Tips

### Start Fresh Database
```bash
# Connect to MongoDB
mongosh

# Switch to database
use fraudguard

# Drop database
db.dropDatabase()

# Exit
exit
```

### View Backend Logs
```bash
cd backend
npm run dev
# Watch terminal for errors
```

### Debug Frontend
- Open browser DevTools (F12)
- Check Console for errors
- Check Network tab for API calls
- Use React DevTools extension

### Test API with Postman/curl

**Signup:**
```bash
curl -X POST http://localhost:5000/api/auth/signup \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "email": "test@example.com",
    "password": "password123"
  }'
```

**Login:**
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123"
  }'
```

**Scan URL:**
```bash
curl -X POST http://localhost:5000/api/scan/scan \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{
    "url": "http://suspicious-site.com/login"
  }'
```

## Still Having Issues?

1. Check all environment variables are set correctly
2. Ensure all dependencies are installed
3. Verify Node.js version (v16+)
4. Check MongoDB version (v5+)
5. Look for typos in file names
6. Review error messages carefully
7. Check browser console for frontend errors
8. Review terminal output for backend errors

## Getting Help

- Review the README.md
- Check code comments
- Search for similar errors online
- Verify your setup matches the project structure
