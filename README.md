A single-file, offline-first Office Management System built for a government office workflow (MP/MLA/Corporator correspondence, appointments, meeting minutes, and task tracking) — running entirely in the browser with localStorage, optional local Excel file sync, and live Google Sheets integration for letters data.



मराठी सरकारी कार्यालयीन कामकाजासाठी (पत्रव्यवहार, भेटीची वेळ, इतिवृत्त, कामांचा पाठपुरावा) तयार केलेली एकल-फाईल (single HTML file) ऑफिस मॅनेजमेंट सिस्टीम.




Features

Moduleवर्णन (Description)📊 Dashboardसर्व मॉड्यूल्सचा एकत्रित सारांश (counts) — एका क्लिकवर संबंधित विभागात जा📅 Appointmentभेटीची नोंद, तारीख/वेळ अपडेट, कीबोर्डने "Appointment दिली" mark करणे, A4 print report📦 Cubboardफाईल/दस्तऐवज नोंद — विषय, path, शेरा📝 Minutesसभेचे इतिवृत्त — विषय, Pet Name, सभेची तारीख, फाईल path (एका क्लिकवर उघडते)🏛️ MP Lettersखासदारांच्या पत्रांची लाईव्ह यादी — Google Sheet वरून थेट फिल्टर (exact (खासदार) designation match)🏛️ MLA Lettersआमदारांच्या पत्रांची लाईव्ह यादी — exact (आमदार) designation match🏛️ Corporator LettersKDMC च्या सर्व ७६+ नगरसेवकांच्या प्रभाग-निहाय (ward-wise) यादीतून पत्र फिल्टर🔍 Search Lettersसंपूर्ण शीटमध्ये मराठी/इंग्रजी कीवर्ड शोध, matched text highlight⭐ Imp Thingsमहत्त्वाचे विषय + प्रत्येक विषयासाठी अमर्यादित actionable sub-tasks✅ View Tasksसर्व tasks (Imp Things मधील + स्वतंत्र Global tasks) एकाच ठिकाणी — Completed/Pending सारांश, Edit/Delete📌 Fixed Taskनियमित कामे (Daily/Weekly/Monthly/Quarterly/Six-Monthly/Yearly/Assembly/Monsoon/Other) + फिल्टर करता येणारा रिपोर्ट

Other highlights


 Keyboard-first navigation — ↑/↓ row navigate, Enter to open/edit, Space to mark appointment given, Delete to remove, Esc to close popup
 A4 Print reports for Appointments, MP/MLA/Search Letters, and Fixed Task reports
 Excel import/export per module (.xlsx via SheetJS)
 Auto-sync folder (Chrome File System Access API) — every save also writes an updated .xlsx file straight to a chosen local folder
 Live Google Sheet sync for letters (fetch + JSONP/gviz fallback so it also works when opened via file://)
 Fully offline-capable — localStorage is the source of truth; Excel/Sheets are sync layers on top



** Tech Stack**


Vanilla JavaScript (no framework, no build step)
HTML5 + CSS3 (single file, government-office style navy/blue UI)
SheetJS (xlsx.full.min.js) — Excel read/write
Google Sheets (published CSV + gviz JSONP) — live data source for MP/MLA/Corporator letters
Google Fonts — Mukta (Devanagari-friendly)
Browser File System Access API (showDirectoryPicker) for local Excel auto-sync — Chrome/Edge only


No backend server, no database, no build tools required.


**Project Structure**

.
└── Pjp_Office_Final.html   # Entire application — UI, styles, and logic in one file

Everything (markup, CSS, and JavaScript) lives in this single HTML file by design, so it can be opened directly or hosted anywhere without a build step.


**Getting Started**

Option 1 — Just open it


Download/clone the repo.
Double-click Pjp_Office_Final.html (or open it in Google Chrome — recommended for full feature support including folder sync and live sheet via JSONP).


Option 2 — Serve locally (recommended for the live Google Sheet fetch() path)

bash# any static server works, e.g.
python -m http.server 8000
# then open http://localhost:8000/Pjp_Office_Final.html


When opened via file://, the app automatically falls back to a JSONP (gviz) method to fetch the Google Sheet, so MP/MLA/Search Letters still work without a server.




**Configuration**

All configuration lives near the top of the <script> block:

jsvar SHEET_ID   = '16aQGu0FYdTWxHGjxWJicmwN60eeqceIlw9RTzwPCcTY';
var SHEET_NAME = 'Sheet1';
var SHOW_COLS  = [0,1,3,4,5,6,7,8]; // columns shown in MP/MLA/Search tables


SHEET_ID — the Google Sheet that holds the master letters data (must be published to the web as CSV for the fast fetch() path: File → Share → Publish to web → CSV).
Designation filtering (hasDesignation()) — MP letters are matched by the literal tag mp and MLA letters by mla inside the "Sender Details" column (column index 4), so the source sheet must tag senders consistently, e.g. ... (खासदार) / ... (आमदार).
Corporator list — hardcoded as a 76-option dropdown (<select id="corpSelect">) mapped to ward numbers (1-अ, 1-ब, ... etc.) for KDMC. Update this list directly in the HTML if ward/corporator data changes.


Local Excel files

Each module writes to its own workbook (filenames defined in FILES):

ModuleFileAppointmentsAppointments.xlsxCubboardCubboard.xlsxMinutesMinutes.xlsxImp ThingsImpThings.xlsxFixed TaskFixedTasks.xlsx

Click 📁 Excel Auto-sync Folder in the sidebar (Chrome only) to pick a local folder — every "Save" will then also silently rewrite the corresponding .xlsx file there, in addition to localStorage.


**Keyboard Shortcuts**

KeyAction↑ / ↓Navigate table rowsEnterOpen edit popup (Appointments)SpaceMark appointment as given (Appointments view)DeleteDelete selected row (Imp Things view)EscClose popup


**Data Model & Storage**


Primary store: localStorage (versioned keys, prefix oms_v1_), mirrored to sessionStorage as a backup.
Letters data (MP/MLA/Corporator/Search): fetched live from the configured Google Sheet — not stored locally, always reflects the latest sheet contents.
Excel files: generated on every save via SheetJS; can be exported manually (⬇️ Export Excel) or auto-written to a connected folder.
Import: any module's view tab supports ⬆️ Import Excel to bulk-load existing .xlsx data back into localStorage.
Tasks: stored under a dedicated tasks_all key, supporting both tasks linked to an "Imp Things" entry and standalone "Global" tasks.



**Browser Support**

FeatureRequirementCore app (forms, tables, Excel export/import)Any modern browser📁 Auto-sync folder (File System Access API)Chrome / Edge (Chromium-based)Live Google Sheet via plain fetch()Works when served over http(s)://Live Google Sheet via JSONP fallbackWorks even on file://


🤝 Contributing

This is a single-file project tailored to a specific office workflow — pull requests for bug fixes, new modules, or UI improvements are welcome. Please keep changes scoped and avoid unrelated refactors so the diff stays easy to review.
