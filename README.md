# 📄 Unify Scan

**Unify Scan** is a smart assessment processing system that eliminates the inefficiency of paper-based exam management.  
It allows assessors to scan scripts, automatically extract scores using AI, and store verified results in real-time — creating a transparent, accountable record from the moment a script is marked.

> Built for the **SPE Nexus 2026 Hackathon** · LASU Faculty of Engineering

---

## 🚀 The Problem

Across Africa, assessments that determine people's futures — university exams, professional certifications, technical qualifications, energy sector safety clearances — are still processed on paper.

This is not a niche problem. It affects every institution that evaluates people at scale, and it fails everyone involved.

### 🗂️ For Exam Officers
Officers are personally accountable for thousands of physical scripts. If one goes missing, the blame falls on them. There is no audit trail, no digital backup, no way to prove what happened. Just paper and pressure — with careers on the line.

### 👤 For Candidates
Whether it's a university student, a trainee engineer, or a field technician sitting a safety certification — anyone who works hard and performs well hands over that effort with **zero transparency**:
- Scores can be altered between marking and data entry, with no way to detect or challenge it
- A single clerical error can cost someone a degree, a job, or a certification they genuinely earned
- Candidates applying for scholarships, specialist roles, or international opportunities have no verifiable record to present
- **There is no receipt. No record. No recourse.**

In high-stakes environments — like energy sector safety assessments where a wrongly passed candidate becomes a risk on a rig, or a wrongly failed one loses their livelihood — the consequences of this opacity go far beyond inconvenience.

### ⏳ For Assessors & Institutions
Assessors and administrators spend hours:
- Sorting and transporting stacks of scripts
- Manually transcribing scores into spreadsheets
- Introducing input errors that cascade into payroll, deployment, compliance, and academic systems

The inefficiency is structural. And it scales badly — the bigger the institution, the worse the problem.

---

## 💡 The Solution

Unify Scan digitises the moment a script is marked — creating a **permanent, verifiable, instantly accessible record** the second a score is captured.

1. 📷 Assessor scans script using phone camera
2. 🤖 AI reads the score (e.g. `70/100`)
3. ✅ Assessor confirms or edits
4. ☁️ Result saved instantly to Firebase
5. 📊 Record appears in Google Sheets automatically
6. 👤 Candidate views their marked script and verified score in real-time

No more lost scripts. No more disputed results. No more unverifiable records.

---

## ⚙️ Features

| Feature | Description |
|---|---|
| 🔍 **AI OCR (Claude API)** | Reads handwritten or printed scores from any scanned script |
| 📊 **Google Sheets Integration** | Auto-logs timestamp, candidate name, score, and image reference |
| 🔥 **Firebase Sync** | Real-time storage — results are live the moment they're confirmed |
| 👁️ **Candidate Script Viewer** | Candidates see every page of their marked script with full transparency |
| 🏛️ **Hall Entry Management** | Tracks and logs candidate attendance at exam or assessment centres |
| 📁 **CSV Export** | Download full session results instantly as a backup |
| 📱 **Mobile-first PWA** | Runs on any smartphone — no app install, no special hardware needed |

---

## 🧠 How It Works

```
Camera → AI (Claude) → Assessor Confirms → Firebase → Google Sheets → Candidate View
```

The assessor scans a script page. Claude extracts the score. The assessor confirms it. Firebase stores the result with a timestamp and image reference. The candidate can immediately view their script and score — a verifiable digital record they can reference for any purpose: dispute, certification, employment, or scholarship.

---

## 🌍 Why This Matters

The inefficiency of paper-based assessment isn't an edge case — it's the default across African institutions, from universities to professional licensing bodies to energy sector training centres.

Unify Scan is designed for this reality:

- **No expensive infrastructure** — runs entirely on a smartphone
- **No changes to existing process** — works with paper scripts as they are
- **Offline-tolerant** — captures and queues results even with poor connectivity
- **Instant accountability** — every result is timestamped, attributed, and traceable
- **Scales to any context** — universities, technical colleges, QHSE training, professional certifications, and beyond

The same broken workflow that loses a student's exam paper also loses an oil worker's safety certification. Unify Scan fixes both — because the root problem is the same: **no digital record at the point of marking.**

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
├── Lecturer.html       # Assessor station — capture & confirm scores
├── Student.html        # Candidate viewer — result & script access
├── Hall_entry.html     # Exam hall entry management
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

1. Open the **Assessor** interface
2. Capture a script with a visible score (e.g. `70/100`)
3. Confirm the extracted score
4. Verify:
   - Firebase updated in real-time
   - New row added to Google Sheet with timestamp
5. Switch to the **Candidate** interface to view the result and full script

---

## 🔒 Security Note

API keys are **not included** in this repository. Insert your own keys before running the project locally or deploying.

---

## 🚧 Future Improvements

- Automatic candidate identification via ID or QR scan
- Integration with professional certification databases (COREN, NCDMB, NYSC, etc.)
- AI-assisted grading and feedback generation
- Score dispute and formal appeal workflow
- Analytics dashboard for institutions and compliance bodies
- Integration with the broader Unify academic and workforce platform

---

## 👤 Author

**Joshua Olotuche**  
Electronic & Computer Engineering Student  
Lagos State University

---

## 🏁 Status

**SPE Nexus 2026 Hackathon Submission ✅**  
Real-world deployment in progress 🚀
