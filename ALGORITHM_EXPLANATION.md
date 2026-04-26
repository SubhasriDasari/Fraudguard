# FraudGuard Algorithm Explanation

## 🎯 How Scan Results Are Determined

FraudGuard uses a **multi-factor heuristic algorithm** that analyzes URLs based on 10+ security indicators commonly found in phishing and fraudulent websites.

---

## 📊 Scoring System

### Risk Score: 0-100 Points
Each security check adds points to the risk score. Higher score = Higher risk.

### Risk Levels:
- **Low (0-29)**: ✅ URL appears safe
- **Medium (30-49)**: ⚠️ Some suspicious elements detected
- **High (50-69)**: ⚡ Multiple red flags present
- **Critical (70-100)**: 🚨 Very likely fraudulent/dangerous

---

## 🔍 10 Security Checks Performed

### 1. **HTTPS/SSL Validation** (+20 points)
**What it checks:** Whether the URL uses HTTPS encryption

**Why it matters:** 
- Legitimate websites, especially those handling sensitive data, should use HTTPS
- HTTP sites can intercept your data

**Example:**
```
✅ Safe:      https://amazon.com
❌ Risky:     http://amazom-login.com
Risk Score: +20 points
Severity: Medium
```

---

### 2. **Insecure Login Pages** (+30 points)
**What it checks:** Login/signin/account pages without HTTPS

**Why it matters:**
- Login pages MUST use HTTPS to protect credentials
- This is a major red flag for phishing sites

**Example:**
```
❌ CRITICAL: http://paypal-verify.com/login
Risk Score: +30 additional points (50 total)
Severity: High
```

---

### 3. **IP Address Detection** (+25 points)
**What it checks:** URLs using IP addresses instead of domain names

**Why it matters:**
- Legitimate sites use domain names (google.com)
- Phishers often use IP addresses to hide identity
- Example: `http://192.168.1.1/login`

**Example:**
```
✅ Safe:      https://facebook.com
❌ Risky:     http://123.456.78.90/facebook-login
Risk Score: +25 points
Severity: Medium
```

---

### 4. **URL Length Analysis** (+15 points)
**What it checks:** URLs longer than 75 characters

**Why it matters:**
- Phishers create long URLs to hide malicious domains
- Long URLs often include redirect chains

**Example:**
```
❌ Suspicious: https://secure-banking-update-verify-account-information-here-12345.com/login?redirect=...
URL Length: 150 characters
Risk Score: +15 points
Severity: Low
```

---

### 5. **Subdomain Analysis** (+20 points)
**What it checks:** More than 4 subdomains (dots in domain)

**Why it matters:**
- Normal: `mail.google.com` (2 parts)
- Suspicious: `secure.login.verify.paypal.fake-site.com` (6 parts)
- Attackers use subdomains to make URLs look legitimate

**Example:**
```
✅ Safe:      https://mail.google.com
❌ Risky:     https://apple.secure.login.verify.update.tk
Risk Score: +20 points
Severity: Medium
```

---

### 6. **Phishing Keywords** (+10 points per keyword, max 3+)
**What it checks:** Suspicious words commonly used in phishing

**Keywords monitored:**
- verify, account, update, secure, banking
- paypal, amazon, confirm, suspend, urgent

**Why it matters:**
- Phishers create urgency: "Verify your account NOW!"
- Legitimate sites rarely use these tactics

**Example:**
```
❌ Multiple keywords detected:
URL: https://secure-verify-account-update.com
Keywords found: secure, verify, account, update
Risk Score: +40 points (4 keywords × 10)
Severity: High (3+ keywords)
```

---

### 7. **Brand Impersonation** (+35 points)
**What it checks:** Domain contains brand name but isn't official

**Brands monitored:**
- paypal, amazon, google, microsoft, apple
- facebook, instagram, twitter

**Why it matters:**
- Attackers register similar domains to trick users
- `paypal-secure.com` is NOT `paypal.com`

**Example:**
```
✅ Safe:      https://paypal.com
❌ DANGER:    https://paypal-login.tk
✅ Safe:      https://developer.apple.com
❌ DANGER:    https://apple-verify-account.com

Risk Score: +35 points
Severity: CRITICAL
```

---

### 8. **Suspicious Top-Level Domains (TLDs)** (+15 points)
**What it checks:** High-risk domain extensions

**Suspicious TLDs:**
- `.tk` `.ml` `.ga` `.cf` `.gq` (free domains)
- `.xyz` `.top` `.club` (commonly abused)

**Why it matters:**
- These TLDs are often free or very cheap
- Heavily used by spammers and phishers
- Legitimate businesses rarely use them

**Example:**
```
✅ Safe:      https://company.com
❌ Risky:     https://secure-login.tk
Risk Score: +15 points
Severity: Low
```

---

### 9. **Invalid Characters** (+25 points)
**What it checks:** Unusual characters in domain name

**Why it matters:**
- Domains should only contain: letters, numbers, dots, hyphens
- Special characters indicate suspicious URLs
- Example: `pаypal.com` (using Cyrillic 'а' instead of 'a')

**Example:**
```
✅ Safe:      https://my-store.com
❌ Risky:     https://my_store!@#.com
Risk Score: +25 points
Severity: Medium
```

---

### 10. **URL Shorteners** (+10 points)
**What it checks:** Use of URL shortening services

**Shorteners detected:**
- bit.ly, tinyurl.com, goo.gl, t.co, ow.ly

**Why it matters:**
- Hide the actual destination
- Could redirect to malicious sites
- Can't verify safety before clicking

**Example:**
```
⚠️ Warning:   https://bit.ly/abc123
Actual destination: Unknown
Risk Score: +10 points
Severity: Low
```

---

## 🧮 Real Examples with Calculations

### Example 1: Safe Website
```
URL: https://www.google.com

Checks:
✅ HTTPS: Yes (0 points)
✅ Login page: No (0 points)
✅ IP address: No (0 points)
✅ Length: 22 chars (0 points)
✅ Subdomains: 1 (0 points)
✅ Keywords: None (0 points)
✅ Brand impersonation: Official (0 points)
✅ TLD: .com (0 points)
✅ Characters: Valid (0 points)
✅ Shortener: No (0 points)

TOTAL RISK SCORE: 0
RISK LEVEL: LOW ✅
```

---

### Example 2: Suspicious Phishing Site
```
URL: http://paypal-verify-account.tk/login

Checks:
❌ HTTPS: No (+20 points)
❌ Login page without HTTPS (+30 points)
✅ IP address: No (0 points)
✅ Length: 40 chars (0 points)
✅ Subdomains: 2 (0 points)
❌ Keywords: verify, account (+20 points)
❌ Brand impersonation: paypal (+35 points)
❌ TLD: .tk (+15 points)
✅ Characters: Valid (0 points)
✅ Shortener: No (0 points)

TOTAL RISK SCORE: 120 → Capped at 100
RISK LEVEL: CRITICAL 🚨

Threats Detected: 6
- Insecure Connection
- Insecure Login Page
- Phishing Keywords (verify, account)
- Brand Impersonation (PayPal)
- Suspicious Domain Extension (.tk)
```

---

### Example 3: Medium Risk Site
```
URL: http://my-online-store-great-deals.com

Checks:
❌ HTTPS: No (+20 points)
✅ Login page: No (0 points)
✅ IP address: No (0 points)
❌ Length: 42 chars but close (0 points)
✅ Subdomains: 0 (0 points)
✅ Keywords: None (0 points)
✅ Brand impersonation: No (0 points)
✅ TLD: .com (0 points)
✅ Characters: Valid (0 points)
✅ Shortener: No (0 points)

TOTAL RISK SCORE: 20
RISK LEVEL: LOW ✅
(But warning shown for no HTTPS)
```

---

### Example 4: Complex Phishing Attack
```
URL: https://secure.login.verify.apple-support.xyz/account/update?urgent=true

Checks:
✅ HTTPS: Yes (0 points)
✅ Login page: Has login in URL but has HTTPS (0 points)
✅ IP address: No (0 points)
❌ Length: 78 chars (+15 points)
❌ Subdomains: 5 parts (+20 points)
❌ Keywords: login, verify, account, update, urgent (+50 points)
❌ Brand impersonation: apple (+35 points)
❌ TLD: .xyz (+15 points)
✅ Characters: Valid (0 points)
✅ Shortener: No (0 points)

TOTAL RISK SCORE: 135 → Capped at 100
RISK LEVEL: CRITICAL 🚨

Threats Detected: 5
- Suspicious URL Length
- Excessive Subdomains
- Phishing Keywords (5 found)
- Brand Impersonation (Apple)
- Suspicious Domain Extension (.xyz)
```

---

## 🎓 How to Interpret Results

### Low Risk (0-29)
- Generally safe to visit
- Still verify the domain carefully
- Check for HTTPS on sensitive pages

### Medium Risk (30-49)
- Proceed with caution
- Don't enter personal information
- Verify the website is legitimate

### High Risk (50-69)
- Multiple red flags detected
- **Do not** enter credentials
- Likely a scam or phishing attempt

### Critical Risk (70-100)
- **DANGER! Do not visit**
- Clear phishing/fraud indicators
- Report to authorities if received via email
- Delete the message/link immediately

---

## 🔄 Algorithm Limitations

### What FraudGuard DOES:
✅ Pattern-based analysis
✅ Heuristic security checks
✅ Common phishing indicators
✅ Domain analysis
✅ Fast scanning (<1 second)

### What FraudGuard DOESN'T:
❌ Access actual website content
❌ Check reputation databases
❌ Verify SSL certificates in detail
❌ Analyze website code
❌ Check against blacklists
❌ Guarantee 100% accuracy

---

## 💡 Best Practices

Even with a **Low risk score**, always:
1. Verify the exact domain name
2. Check for HTTPS before entering data
3. Be suspicious of urgent requests
4. Don't click links in unexpected emails
5. Type important URLs manually
6. Use 2-factor authentication
7. Keep software updated

---

## 🔬 Future Enhancements

Planned improvements:
- Integration with Google Safe Browsing API
- SSL certificate validation
- Domain age checking
- Reputation database lookups
- Machine learning model
- Real-time blacklist checking
- Website content analysis

---

**Remember:** FraudGuard is a helpful tool, but your judgment is the ultimate security measure! 🛡️
