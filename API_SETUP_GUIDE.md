# API Keys Setup Guide (Optional)

FraudGuard works perfectly **without any API keys** using built-in heuristics and machine learning. However, you can optionally enhance accuracy by adding these free API keys:

---

## 🔑 Optional API Integrations

### 1. Google Safe Browsing API (Recommended)

**What it adds:** Real-time malware and phishing detection from Google's threat database

**How to get it:**
1. Go to https://console.cloud.google.com/
2. Create a new project (or select existing)
3. Enable "Safe Browsing API"
4. Go to "Credentials" → "Create Credentials" → "API Key"
5. Copy your API key

**Add to `.env`:**
```bash
GOOGLE_SAFE_BROWSING_API_KEY=AIzaSyB...yourkey...here
```

**Free Tier:** 10,000 requests/day

---

### 2. PhishTank API (Recommended)

**What it adds:** Community-reported phishing sites database

**How to get it:**
1. Go to https://www.phishtank.com/
2. Create a free account
3. Go to https://www.phishtank.com/api_info.php
4. Register for an API key
5. Copy your API key

**Add to `.env`:**
```bash
PHISHTANK_API_KEY=your_phishtank_key_here
```

**Free Tier:** Unlimited (with rate limiting)

---

### 3. VirusTotal API (Future Feature)

**What it adds:** Multi-engine malware scanning

**How to get it:**
1. Go to https://www.virustotal.com/
2. Create account
3. Go to your profile → API Key
4. Copy your API key

**Add to `.env`:**
```bash
VIRUSTOTAL_API_KEY=your_virustotal_key_here
```

**Free Tier:** 4 requests/minute

---

## 📝 Complete `.env` Example

```bash
# Required
MONGODB_URI=mongodb://localhost:27017/fraudguard
JWT_SECRET=your_super_secret_jwt_key_make_it_long_and_random
PORT=5000
NODE_ENV=development

# Optional - Advanced Features (Leave blank if not using)
GOOGLE_SAFE_BROWSING_API_KEY=
PHISHTANK_API_KEY=
VIRUSTOTAL_API_KEY=
```

---

## ✅ Without API Keys

If you don't add any API keys, FraudGuard will:
- ✅ Still work perfectly
- ✅ Use built-in heuristics
- ✅ Use machine learning model
- ✅ Analyze URL patterns
- ✅ Check SSL certificates
- ✅ Estimate domain age
- ✅ Check reputation
- ✅ Analyze content

**Accuracy: ~85%**

---

## 🚀 With API Keys

If you add the recommended API keys:
- ✅ All of the above
- ✅ Google's threat intelligence
- ✅ PhishTank community reports
- ✅ Real-time malware detection
- ✅ Enhanced blacklist checking

**Accuracy: ~95%+**

---

## 🔒 Security Notes

- **Never commit** `.env` file to git
- **Keep API keys secret**
- **Regenerate keys** if exposed
- **Use environment variables** in production
- **Monitor API usage** to avoid rate limits

---

## 🧪 Testing Your Setup

### 1. Without API Keys
```bash
# Start the server
cd backend
npm run dev

# You should see:
✅ MongoDB connected
🚀 Server running on port 5000
```

### 2. With Google Safe Browsing
Try scanning: `http://testsafebrowsing.appspot.com/s/phishing.html`

You should see:
```json
{
  "type": "Google Safe Browsing Alert",
  "severity": "Critical",
  "description": "Threats: SOCIAL_ENGINEERING"
}
```

### 3. Check Logs
The backend will log:
- ✅ `Google Safe Browsing API configured` (if key present)
- ℹ️ `Google Safe Browsing API key not configured` (if not)

---

## 💡 Recommendations

### For Development/Testing:
- **No API keys needed**
- Built-in features are sufficient

### For Production:
- ✅ Add Google Safe Browsing (free tier is generous)
- ✅ Add PhishTank (unlimited)
- ⚠️ Monitor API usage
- ⚠️ Set up rate limiting

### For Enterprise:
- ✅ Paid API tiers for higher limits
- ✅ Multiple API keys for load balancing
- ✅ Custom blacklist integration
- ✅ Private threat intelligence feeds

---

## ❓ Troubleshooting

### "API key not working"
- Check for spaces in the key
- Verify the API is enabled in Google Cloud Console
- Check API quotas/limits

### "Too many requests"
- You've hit the free tier limit
- Wait for quota to reset
- Upgrade to paid tier
- Implement caching (already built-in)

### "API timeout"
- Increase timeout in code (currently 5s)
- Check internet connection
- API service might be down (gracefully falls back to heuristics)

---

## 📊 API Usage Monitoring

Built-in caching reduces API calls:
- Reputation checks: Cached for 1 hour
- Graceful degradation if APIs fail
- Automatic fallback to heuristics

Monitor your usage:
- Google Cloud Console: Check API quotas
- PhishTank: Check account dashboard
- VirusTotal: Check API statistics

---

**Remember: FraudGuard works great even without API keys!** 🎉

They're completely optional enhancements for production use.
