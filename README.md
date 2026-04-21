# 📄 Unify Scan

**Unify Scan** is a smart exam processing system that allows lecturers to scan student scripts, automatically extract scores using AI, and store results in real-time.

It replaces manual result entry with a fast, reliable, and scalable workflow — and gives students full transparency into their marked work.

---

## 🚀 The Problem

Every exam season, a broken system puts thousands of students at risk.

**Exam officers** are responsible for stacks of physical scripts — sometimes thousands of them. If even one goes missing, the blame falls on them. There's no audit trail, no backup, no accountability layer. Just paper and pressure.

**Students** hand over months of hard work with zero transparency:
- A high-performing student applying for a scholarship has no way to verify their score
- Results can change between marking and entry — with no way to challenge it
- A single clerical error can derail an academic record built over years
- There is **no receipt, no record, no recourse**

Lecturers, meanwhile, spend hours:
- Carrying and sorting stacks of scripts
- Manually recording scores one by one
- Making input errors that ripple downstream

This process is slow, stressful, and — for students — completely opaque.

---

## 💡 The Solution

Unify Scan digitises the moment a script is marked, creating a permanent, verifiable record the instant a score is captured.

1. 📷 Scan student script using phone camera
2. 🤖 AI reads the score (e.g. `70/100`)
3. ✅ Lecturer confirms or edits
4. ☁️ Data is saved instantly to Firebase
5. 📊 Results appear in Google Sheets automatically
6. 👨‍🎓 Student can view their marked script in real-time

No more lost scripts. No more disputed scores. No more guesswork.

---

## ⚙️ Features

| Feature | Description |
|---|---|
| 🔍 **AI OCR (Claude API)** | Reads handwritten or printed scores from scanned scripts |
| 📊 **Google Sheets Integration** | Auto-logs timestamp, student name, score, and image reference |
| 🔥 **Firebase Sync** | Real-time data storage and instant student access |
| 📁 **CSV Export** | Download all session results as a backup instantly |
| 👁️ **Student Script Viewer** | Students can see every page of their marked script |
| 🏛️ **Hall Entry Management** | Track and log student attendance at the exam hall |
| 📱 **Mobile-first PWA** | Works directly on a lecturer's phone, no app install needed |

---

## 🧠 How It Works

```
Camera → AI (Claude) → Lecturer Confirms → Firebase → Google Sheets → Student View
```

The lecturer scans a script page. Claude extracts the score. The lecturer confirms it. Firebase stores everything in real-time. The student can immediately view their script and score with full transparency.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, JavaScript (PWA) |
| AI OCR | Anthropic Claude API |
| Backend | Firebase (Firestore) |
| Spreadsheet | Google Apps Script (Web App) |
| Hosting | Netlify |

---

## 📁 Project Structure

```
/
├── index.html          # Landing page — role selector
├── Lecturer.html       # Scanning station — capture & confirm scores
├── Student.html        # Script viewer — student-facing result view
├── Hall_entry.html     # Hall entry management
└── README.md
```

---

## 🔑 Setup Instructions

### 1. Add Claude API Key

In `Lecturer.html`:

```javascript
const CLAUDE_API_KEY = "YOUR_API_KEY_HERE";
```

### 2. Set Up Google Sheets Webhook

1. Create a Google Sheet
2. Go to **Extensions → Apps Script**
3. Add your script (OCR + `appendRow` logic)
4. Deploy as **Web App**
   - Execute as: **Me**
   - Access: **Anyone**
5. Copy the `/exec` URL

### 3. Connect the Webhook

In `Lecturer.html`:

```javascript
const SHEETS_WEBHOOK_URL = "YOUR_WEBHOOK_URL_HERE";
```

### 4. Deploy

Upload the project to Netlify and open on a mobile device.

---

## 🧪 Demo Instructions

1. Open the **Lecturer** interface
2. Capture a script with a visible score (e.g. `70/100`)
3. Confirm the extracted score
4. Verify:
   - Firebase updated in real-time
   - New row added to Google Sheet
5. Switch to the **Student** interface to view the result

---

## 🔒 Security Note

API keys are **not included** in this repository. Insert your own keys before running the project locally or deploying.

---

## 🌍 Why This Matters

Unify Scan is not just a time-saving tool — it's an accountability layer for a system that currently has none.

- Students get a verifiable record of their marked work
- Exam officers are protected by a digital audit trail
- Lecturers save hours of manual data entry
- Institutions move toward transparent, scalable digital workflows

Every student who works hard deserves to know their score is safe, accurate, and challengeable. Unify Scan makes that possible.

---

## 🚧 Future Improvements

- Automatic student identification via ID scan
- AI grading assistance
- Analytics dashboard for lecturers
- Score dispute and appeal workflow
- Integration with the broader Unify academic platform

---

## 👤 Author

**Joshua Olotuche**
Electronic & Computer Engineering Student
Lagos State University

---

## 🏁 Status

**Hackathon Demo — Ready ✅**
Real-world deployment in progress 🚀
