# 📓 Daily Journal — AI-Powered Voice Journal

A fully automated, AI-powered personal journaling system that turns casual voice recordings
into structured, searchable data. Speak freely about your day, and the system handles
everything else — transcription, AI synthesis, and organized storage in Google Sheets.

---

## Overview

Traditional journaling requires time, effort, and discipline. This system removes all of
that friction. The entire daily interaction is: open the app, tap a button, talk, tap stop.
Within seconds, Gemini AI analyzes your words and automatically extracts structured insights
— mood, energy, stress, wins, challenges, themes, and more — storing everything in a clean
Google Sheet that compounds in value over time.

Built entirely with free tools. No subscriptions. No servers. No manual data entry.

---

## How It Works
```
┌─────────────────────────────────────────────────────────────────┐
│                         USER FLOW                               │
│                                                                 │
│  1. Open app on phone home screen                               │
│  2. Tap microphone button                                       │
│  3. Talk casually for 2-5 minutes                               │
│  4. Tap stop                                                    │
│  5. Review transcript on screen                                 │
│  6. Tap Synthesize & Save                                       │
│  7. Done — entry is logged automatically                        │
└─────────────────────────────────────────────────────────────────┘
```

### Architecture
```
┌──────────────┐     ┌──────────────┐     ┌──────────────────┐
│   GitHub     │     │   Browser    │     │   Google Apps    │
│   Pages      │────▶│  Web Speech  │────▶│    Script        │
│  (Frontend)  │     │     API      │     │   (Backend)      │
└──────────────┘     └──────────────┘     └────────┬─────────┘
                                                    │
                                          ┌─────────▼─────────┐
                                          │   Gemini 2.5      │
                                          │   Flash API       │
                                          │  (AI Synthesis)   │
                                          └─────────┬─────────┘
                                                    │
                                          ┌─────────▼─────────┐
                                          │   Google Sheets   │
                                          │   (Data Storage)  │
                                          └─────────┬─────────┘
                                                    │
                          ┌─────────────────────────┘
                          │
          ╔═══════════════▼══════════════════════════════════╗
          ║           OPTIONAL — INSIGHTS LAYER              ║
          ║                                                  ║
          ║   ┌─────────────────┐     ┌──────────────────┐  ║
          ║   │  Personal       │     │  Google Sheets   │  ║
          ║   │  Context Doc    │────▶│  Journal Log     │  ║
          ║   │  (Background    │     │  (All Entries)   │  ║
          ║   │   about you)    │     └────────┬─────────┘  ║
          ║   └─────────────────┘              │            ║
          ║                                    │            ║
          ║                          ┌─────────▼─────────┐  ║
          ║                          │   Gemini Gem      │  ║
          ║                          │  (Custom AI       │  ║
          ║                          │   Persona with    │  ║
          ║                          │   Slash Commands) │  ║
          ║                          └─────────┬─────────┘  ║
          ║                                    │            ║
          ║                          ┌─────────▼─────────┐  ║
          ║                          │  Conversational   │  ║
          ║                          │  Insights         │  ║
          ║                          │  /mood /patterns  │  ║
          ║                          │  /wins /growth    │  ║
          ║                          └───────────────────┘  ║
          ╚══════════════════════════════════════════════════╝
```

### Layer by Layer

**1. Frontend — GitHub Pages**
The user interface is a single static `index.html` file hosted on GitHub Pages. It runs
entirely in the browser with no framework or build process required. GitHub Pages serves
it over HTTPS, which is critical — the Web Speech API and microphone access require a
secure context and will not function over HTTP or inside restricted iframes.

**2. Voice Capture — Web Speech API**
When the user taps the microphone button, the app calls `navigator.mediaDevices.getUserMedia`
to explicitly request microphone permission from the browser. Once granted, it initializes
the browser's built-in `SpeechRecognition` API in continuous mode, meaning it keeps
transcribing until the user manually stops it. Interim results are displayed in real time
on screen as the user speaks, with finalized text accumulating in a transcript string.
A Wake Lock is requested simultaneously to prevent the phone screen from timing out during
recording.

**3. Review Screen**
When the user stops recording, the complete transcript is displayed on a review screen
before anything is sent anywhere. This gives the user a chance to verify the transcription
was accurate and decide whether to save or start over.

**4. Backend — Google Apps Script**
When the user taps Synthesize & Save, the app sends an HTTP POST request to a Google Apps
Script deployment URL. The request body contains the raw transcript and a secret token.
Apps Script is Google's cloud automation platform — it runs JavaScript in Google's
infrastructure at no cost, with no server setup required. The `doPost` function receives
the request, validates the secret token, and if authorized, passes the transcript to the
`processJournalEntry` function.

**5. AI Synthesis — Gemini 2.5 Flash API**
The `processJournalEntry` function constructs a carefully engineered prompt that instructs
Gemini to analyze the raw transcript and return a structured JSON object containing 13
distinct data fields. The prompt explicitly instructs the model to return raw JSON only —
no markdown, no backticks, no explanation — making the response directly parseable without
additional cleaning. The Gemini 2.5 Flash model is used for its combination of speed,
accuracy, and generous free tier quota.

The prompt extracts the following fields:
- `moodScore` — numeric 1-10 rating of overall mood
- `moodDescriptor` — single word describing the mood
- `energyLevel` — numeric 1-10 rating of energy
- `stressLevel` — numeric 1-10 rating of stress
- `keyThemes` — 2-3 main topics from the entry
- `wins` — highlights and positive moments
- `challenges` — frustrations or difficulties mentioned
- `peopleMentioned` — names of people referenced
- `followUp` — action items or things to carry into tomorrow
- `gratitude` — one positive or grateful moment
- `oneLineSummary` — single sentence summary of the day
- `workVsPersonal` — estimated split of work vs personal content
- `wordCount` — word count of the raw transcript

**6. Data Storage — Google Sheets**
The parsed JSON response is written as a new row in a Google Sheet using the
`SpreadsheetApp` service built into Apps Script. The date, day of week, and week number
are calculated and prepended automatically. The raw transcript is stored in the final
column as a permanent record of the original unedited entry. Each row represents one
complete journal entry.

**7. Insights Layer — Gemini Gem (Optional)**
Once sufficient entries have accumulated, a custom Gemini Gem can be connected to the
Google Sheet via Google Drive. The Gem acts as a conversational interface over the journal
data, allowing the user to ask natural language questions and use slash commands to surface
patterns, trends, and insights grounded in actual logged entries.

---

## Tech Stack

| Layer | Technology | Cost |
|-------|-----------|------|
| Hosting | GitHub Pages | Free |
| Voice Transcription | Web Speech API | Free (built into browser) |
| Backend / Automation | Google Apps Script | Free |
| AI Synthesis | Gemini 2.5 Flash API | Free tier |
| Data Storage | Google Sheets | Free |
| Insights | Gemini Gem | Free |

**Total infrastructure cost: $0**

---

## App Screens

The app is built as a single HTML file with five distinct full-screen states managed
entirely in JavaScript — no page reloads, no routing library required.

| Screen | Trigger | Purpose |
|--------|---------|---------|
| **Idle** | App load / reset | Home screen with mic button and today's date |
| **Recording** | Tap mic | Live timer, real-time transcript, pulsing stop button |
| **Review** | Tap stop | Full transcript preview with save or start over options |
| **Processing** | Tap save | Spinner while Gemini synthesizes the entry |
| **Success** | Save complete | Confirmation that entry was logged |

---

## Security

The Apps Script endpoint is protected by a secret token that must be present in every
POST request. Requests without the correct token are rejected before any processing occurs.
The token is stored in the `index.html` file and in the Apps Script `Code.gs` file — both
values must match for the system to function.

> ⚠️ If this repository is public, the secret token is visible in the source code.
> The token prevents unauthorized writes to your Google Sheet but is not truly secret
> in a public repo. A future improvement is to move the token out of the source code
> entirely using a browser localStorage setup screen, allowing the repo to remain
> public while keeping credentials private.

---

## Google Sheet Structure

Each journal entry is stored as a row with the following 17 columns:

| Column | Field | Description |
|--------|-------|-------------|
| A | Date | Entry date |
| B | Day of Week | Monday, Tuesday, etc. |
| C | Mood Score | 1-10 numeric rating |
| D | Mood Descriptor | One word mood label |
| E | Energy Level | 1-10 numeric rating |
| F | Stress Level | 1-10 numeric rating |
| G | Key Themes | 2-3 main topics |
| H | Wins / Highlights | Positive moments |
| I | Challenges / Frustrations | Difficulties mentioned |
| J | People Mentioned | Names referenced |
| K | Follow Up | Action items for tomorrow |
| L | Gratitude | One grateful moment |
| M | One Line Summary | Single sentence summary |
| N | Work vs Personal | Estimated content split |
| O | Week Number | ISO week number |
| P | Word Count | Transcript word count |
| Q | Raw Transcript | Original unedited transcript |

---

## Setup Guide

### Prerequisites
- Google account (Gmail)
- Smartphone with Chrome (Android) or Safari (iPhone)
- GitHub account

### Step 1 — Google Sheet
1. Create a new Google Sheet at sheets.google.com
2. Name it **Daily Journal Log**
3. Paste the column headers from the table above into row 1
4. Copy the Sheet ID from the URL — the string between `/d/` and `/edit`

### Step 2 — Gemini API Key
1. Go to aistudio.google.com
2. Click **Get API Key → Create API Key**
3. Copy and save the key securely

### Step 3 — Apps Script
1. In your Google Sheet go to **Extensions → Apps Script**
2. Replace all default code with the contents of `Code.gs`
3. Fill in `GEMINI_API_KEY`, `SHEET_ID`, and `SECRET_TOKEN` at the top
4. Save and deploy as a **Web App**:
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Copy the deployment URL

### Step 4 — GitHub Pages
1. Fork or clone this repository
2. In `index.html` replace `YOUR_APPS_SCRIPT_URL_HERE` with your deployment URL
3. Replace `YOUR_SECRET_TOKEN_HERE` with your chosen secret token
4. Go to **Settings → Pages → Branch: main → Save**
5. Wait ~60 seconds for your site to go live

### Step 5 — Add to Home Screen
**iPhone:** Open in Safari → Share → Add to Home Screen

**Android:** Open in Chrome → Three dots → Add to Home Screen

---

## Browser Compatibility

| Browser | Voice Recording | Recommended |
|---------|----------------|-------------|
| Chrome (Android) | ✅ Full support | ✅ Yes |
| Safari (iPhone) | ✅ Full support | ✅ Yes |
| Firefox | ❌ Not supported | ❌ No |
| Chrome (iOS) | ❌ Limited | ❌ No |

> The Web Speech API requires HTTPS and is not supported in all browsers.
> Chrome on Android and Safari on iPhone are the recommended clients.

---

## Troubleshooting

| Problem | Solution |
|---------|---------|
| Mic button does nothing | Use Chrome (Android) or Safari (iPhone) |
| Microphone access denied | Check browser mic permissions for the site |
| Something went wrong on save | Verify Apps Script URL and token match in both files |
| `429` error | Gemini free tier rate limit — wait a minute and retry |
| Sheet not updating | Confirm `SHEET_NAME` in Apps Script matches your tab name exactly |
| Screen times out while recording | Keep screen on manually — Wake Lock API may be overridden by some devices |

---

## Planned Improvements

- [ ] localStorage setup screen so credentials are never stored in source code
- [ ] Make repository private once credential management is improved
- [ ] Monthly digest — automated Gemini summary of the past month's entries
- [ ] Streak counter — track consecutive days of journaling
- [ ] Mood chart — visual trend line built from Sheet data

---

## License

Personal use project. Feel free to fork and adapt for your own use.
