# FraudGuard Algorithm Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                    USER ENTERS URL                          │
│              (e.g., http://paypal-verify.tk)                │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                 INITIALIZE SCORING                          │
│              riskScore = 0                                  │
│              threats = []                                   │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 1: HTTPS VALIDATION                          │
│   Question: Does URL use HTTPS?                            │
│   ├─ NO  → +20 points, Add "Insecure Connection" threat   │
│   └─ YES → Continue                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 2: INSECURE LOGIN PAGE                       │
│   Question: Is this a login page without HTTPS?            │
│   ├─ YES → +30 points, Add "Insecure Login" threat        │
│   └─ NO  → Continue                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 3: IP ADDRESS DETECTION                      │
│   Question: Is domain an IP address?                       │
│   ├─ YES → +25 points, Add "IP Address URL" threat        │
│   └─ NO  → Continue                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 4: URL LENGTH                                │
│   Question: Is URL longer than 75 characters?              │
│   ├─ YES → +15 points, Add "Suspicious Length" threat     │
│   └─ NO  → Continue                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 5: SUBDOMAIN COUNT                           │
│   Question: More than 4 subdomains?                        │
│   ├─ YES → +20 points, Add "Excessive Subdomains" threat  │
│   └─ NO  → Continue                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 6: PHISHING KEYWORDS                         │
│   Question: Contains suspicious keywords?                  │
│   Keywords: verify, account, update, secure, etc.          │
│   ├─ Found 1-2 → +10-20 points, Medium severity           │
│   ├─ Found 3+  → +30+ points, High severity               │
│   └─ None      → Continue                                  │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 7: BRAND IMPERSONATION                       │
│   Question: Domain mimics known brand?                     │
│   Brands: paypal, amazon, google, apple, etc.             │
│   ├─ YES → +35 points, Add "Brand Impersonation" (CRITICAL)│
│   └─ NO  → Continue                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 8: SUSPICIOUS TLD                            │
│   Question: Uses high-risk extension?                      │
│   TLDs: .tk, .ml, .ga, .cf, .gq, .xyz, .top, .club       │
│   ├─ YES → +15 points, Add "Suspicious TLD" threat        │
│   └─ NO  → Continue                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 9: INVALID CHARACTERS                        │
│   Question: Domain has unusual characters?                 │
│   ├─ YES → +25 points, Add "Invalid Characters" threat    │
│   └─ NO  → Continue                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CHECK 10: URL SHORTENER                            │
│   Question: Is this a shortened URL?                       │
│   Services: bit.ly, tinyurl.com, etc.                     │
│   ├─ YES → +10 points, Add "URL Shortener" threat         │
│   └─ NO  → Continue                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          CALCULATE FINAL RISK LEVEL                         │
│   Cap score at 100 if exceeded                            │
│   ├─ Score 70-100  → Risk Level: CRITICAL 🚨              │
│   ├─ Score 50-69   → Risk Level: HIGH ⚡                  │
│   ├─ Score 30-49   → Risk Level: MEDIUM ⚠️                │
│   └─ Score 0-29    → Risk Level: LOW ✅                   │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          RETURN RESULTS TO USER                             │
│   {                                                         │
│     riskScore: 85,                                         │
│     riskLevel: "Critical",                                 │
│     threats: [                                             │
│       { type: "Insecure Connection", ... },               │
│       { type: "Brand Impersonation", ... },               │
│       { type: "Suspicious TLD", ... }                     │
│     ]                                                      │
│   }                                                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          SAVE TO DATABASE & DISPLAY                         │
│   - Store scan in user's history                          │
│   - Update statistics                                      │
│   - Show results in dashboard                             │
└─────────────────────────────────────────────────────────────┘
```

## Example Walkthrough

**URL:** `http://paypal-verify.tk/login`

```
Step 1: HTTPS Check
  ❌ Uses HTTP (not HTTPS)
  Score: 0 + 20 = 20
  Threat: "Insecure Connection"

Step 2: Login Page Check
  ❌ Has "login" in URL without HTTPS
  Score: 20 + 30 = 50
  Threat: "Insecure Login Page"

Step 3: IP Address Check
  ✅ Not an IP address
  Score: 50 + 0 = 50

Step 4: URL Length Check
  ✅ Only 31 characters
  Score: 50 + 0 = 50

Step 5: Subdomain Check
  ✅ Only 2 parts (paypal-verify.tk)
  Score: 50 + 0 = 50

Step 6: Phishing Keywords
  ❌ Found "verify"
  Score: 50 + 10 = 60
  Threat: "Phishing Keywords"

Step 7: Brand Impersonation
  ❌ Contains "paypal" but not paypal.com
  Score: 60 + 35 = 95
  Threat: "Brand Impersonation (PayPal)"

Step 8: Suspicious TLD
  ❌ Uses .tk extension
  Score: 95 + 15 = 110 → Capped at 100
  Threat: "Suspicious Domain Extension"

Step 9: Invalid Characters
  ✅ Valid characters (letters and hyphen)
  Score: 100 + 0 = 100

Step 10: URL Shortener
  ✅ Not a shortener
  Score: 100 + 0 = 100

FINAL RESULT:
  Risk Score: 100
  Risk Level: CRITICAL 🚨
  Threats: 5 detected
  Recommendation: DO NOT VISIT THIS SITE
```

## Risk Score Breakdown by Check

| Check                  | Max Points | Typical Range |
|------------------------|------------|---------------|
| HTTPS Validation       | 20         | 0 or 20       |
| Insecure Login         | 30         | 0 or 30       |
| IP Address             | 25         | 0 or 25       |
| URL Length             | 15         | 0 or 15       |
| Subdomains             | 20         | 0 or 20       |
| Phishing Keywords      | 10-50      | 0-50          |
| Brand Impersonation    | 35         | 0 or 35       |
| Suspicious TLD         | 15         | 0 or 15       |
| Invalid Characters     | 25         | 0 or 25       |
| URL Shortener          | 10         | 0 or 10       |
| **Maximum Possible**   | **225**    | **Capped@100**|

## Color Coding in UI

```
Risk Score 0-29   → Green  → Safe to proceed
Risk Score 30-49  → Yellow → Caution advised
Risk Score 50-69  → Orange → High risk, avoid
Risk Score 70-100 → Red    → Critical danger
```
