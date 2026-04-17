# Unify-Scan
# Unify Scan — Frontend Reference

Three vanilla HTML/CSS/JS interfaces matching the procedure. Each is fully self-contained, drops into the existing Unify codebase with no framework dependencies.

## The corrected flow

1. **Hall entry** — Invigilator links matric number ↔ booklet serial as students walk in.
2. **Lecturer station** — Lecturer marks, feeds booklets into scanner. Device reads booklet number off every page and routes automatically. No ID scanning here.
3. **Student profile** — Student sees marked script on their Unify profile, including any extra sheets they requested mid-exam.

## 1. `hall-entry.html` — Invigilator station

Two-step flow per student:
- **Step 1:** Scan/type matric number → device looks up student in Firestore.
- **Step 2:** Scan booklet QR → writes `bookletID → studentUID` link to `/booklets/{bookletID}`.

Handles:
- **Duplicate check-in** — blocks if student already has a booklet.
- **Duplicate booklet** — blocks if serial already issued to someone else.
- **Extra sheets** — dedicated modal for mid-exam requests. Links the extra booklet to the student AND tags it as a child of their main booklet.
- **Live issue log** on the right sidebar, counters for students + extras.

Firebase writes:
```
/booklets/{bookletSerial} = {
  studentUID, matric, name,
  courseCode, session, hall,
  issuedBy, issuedAt,
  isExtra: false,
  parentBooklet: null  // set if isExtra: true
}
```

## 2. `lecturer.html` — Scanning station

Key correction from my first pass: **the lecturer does nothing identity-related**. They just feed pages. The device:

1. Captures the page image.
2. Reads the booklet serial printed in the corner (OCR/QR).
3. Reads the score grid on the cover (OCR with confidence).
4. Looks up the booklet in Firestore → resolves to `studentUID`.
5. Uploads page to Storage at `/scripts/{studentUID}/{courseCode}/p{N}.jpg`.
6. If OCR confidence < threshold → flag the booklet for lecturer review.

What the interface shows:
- **Live preview** of the current page being fed, with OCR reading overlay (serial + score + confidence).
- **Queue sidebar** showing every booklet scanned so far with student name, page count, and flag status.
- **"Currently scanning"** card tracks the active booklet's page count against expected (16/16).
- **Feed Next Page** button (demo stand-in for the physical feeder advancing).
- **Flag modal** pops up automatically when a cover page has low OCR confidence — lecturer confirms score via keypad.

Out-of-order handling is built in. The demo feed sequence includes a page from booklet 0417 arriving AFTER booklet 0418 pages — the queue still sorts it correctly into booklet 0417.

Extra sheets arrive with their parent booklet's serial tagged — the UI groups them under the main script in the queue.

Page count integrity check: when lecturer taps "Finish Booklet" with fewer pages than expected, auto-flag with reason "Incomplete booklet".

## 3. `student.html` — Script viewer in profile

Two views:
- **List** — all scripts as cards with color-coded scores. "+4 extra pages" tag shows when student used extras. NEW tag for unseen scripts.
- **Viewer** — thumbnail rail groups pages into "Main Booklet" and "Extra Sheets" sections so students understand what they're looking at. Paper render includes the small booklet serial tag at the bottom corner (matches the physical paper).

The CPE 401 script in the demo has extra sheets attached. Open it to see the "Extra Sheets" group in the thumbnail rail.

Keyboard nav: ←/→/Esc.

## What a backend dev needs to build

**Hall entry station:**
- Firestore read: `/students/{matric}` → name, level, dept
- Firestore write: `/booklets/{serial}` with the linkage record
- Duplicate guards on both matric and booklet

**Lecturer station (device-side, Python on the Pi):**
- Camera capture via GPIO button press
- Per-page OCR: (1) booklet serial in corner, (2) score on cover
- For each page: `booklets/{serial}` lookup → get `studentUID` → upload to Storage
- Local SQLite buffer for offline mode — queue uploads when connection drops, flush on reconnect
- Set `flagged: true` when OCR confidence < 0.80

**Firestore collections:**
```
/booklets/{serial}        — written at hall entry, read at scanning
/scanJobs/{jobId}         — one per booklet per exam; contains pages[], scoreConfirmed, flagged
/processSessions/{sid}    — one per scanning batch (lecturer's session)
```

**Student-side reads** (the existing Unify app):
- `scanJobs where studentUID == currentUser.uid` → list view
- Individual `scanJobs/{jobId}` → viewer
- Pages render from their Storage URLs (in the prototype they're fake-drawn; in production swap `renderPaperFor` for an `<img>` tag)

**Security rules** the dev will need:
- Device has `isDevice: true` custom claim → can write to `/booklets` and `/scanJobs`
- Students can read only their own `scanJobs`
- Lecturers can read/flag-update `scanJobs` for their courses

## Fonts
Google Fonts: Playfair Display + DM Sans only, per brand kit.

## Demo paths

- `hall-entry.html` — use matric `190591089` (Chidinma) for the smooth path. Try duplicates to see guards fire.
- `lecturer.html` — press "Feed Next Page" 10 times. Page 7 is intentionally out-of-order (belongs to booklet 0417, fed between booklet 0418 pages) to demonstrate sorting. Booklet 0418's cover is deliberately low-confidence to trigger the flag modal.
- `student.html` — click into "CPE 401" to see extra sheets grouped separately in the thumbnail rail.
