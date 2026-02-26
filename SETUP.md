# 🛠️ Setup Guide

Everything you need to build your own AI-powered voice journal from scratch.
No coding experience required. Every step is explained clearly.

**Total setup time: ~45 minutes**
**Total cost: $0**

---

## Before You Start

You will need accounts on three free platforms. If you already have any of these, skip
that step.

- **Google account** — for Sheets, Apps Script, and Gemini
- **GitHub account** — for hosting the app
- **Google AI Studio account** — for your Gemini API key (uses your Google account)

---

## What You Will Be Collecting

Throughout this setup you will generate several important values that you need to keep
track of. Before you start, open a notes app — Apple Notes, Google Keep, Notion, or
anything you have handy — and save each item as you go.

By the end of setup you will have collected:

| Item | Where You Get It | What It Looks Like |
|------|-----------------|-------------------|
| 📊 **Sheet ID** | Google Sheets URL | Long string of letters and numbers |
| 🔑 **Gemini API Key** | Google AI Studio | Long string of random characters |
| 🔒 **Secret Token** | You make this up | Any long random string you create |
| 🌐 **Apps Script URL** | Google Apps Script | Starts with https://script.google.com/... |
| 📱 **GitHub Pages URL** | GitHub Settings | Starts with https://yourusername.github.io/... |

> 💡 Do not skip saving these as you go. Some of them — like your API key — are only
> shown once and cannot be retrieved later. A notes app is fine. A password manager
> is even better.

## Overview

Here is the full picture of what you are building before you start:
```
  You talk        →      App transcribes       →      Gemini AI
  on your phone          your voice                   structures
                         in real time                 your words
                                                           │
                                                           ▼
                                                    Google Sheets
                                                    stores your
                                                    entry forever
                                                           │
                                                           ▼
                                              (Optional) Gemini Gem
                                              lets you chat with
                                              your journal data
```

---

&nbsp;

# Part 1 — Create Your Accounts

---

### GitHub

GitHub is where your app will live. It hosts your code and serves the app to your phone
for free.

1. Go to **[github.com](https://github.com)** and click **Sign up**
2. Enter an email address, create a password, and choose a username
3. Verify your email address when prompted
4. You are in — no paid plan needed

---

### Google Account

You likely already have one. If not:

1. Go to **[accounts.google.com](https://accounts.google.com)** and click **Create account**
2. Follow the steps to set up a Gmail address
3. This same account will be used for Google Sheets, Apps Script, and Gemini

---

&nbsp;

# Part 2 — Set Up Your Google Sheet

Your journal entries will be stored here automatically after each recording.

---

### Step 1 &nbsp;·&nbsp; Create the Sheet

1. Go to **[sheets.google.com](https://sheets.google.com)**
2. Click the large **+** button to create a blank spreadsheet
3. Click the title at the top where it says **Untitled spreadsheet**
4. Rename it: **Daily Journal Log**

---

### Step 2 &nbsp;·&nbsp; Add the Column Headers

1. Click on cell **A1** (the very first cell in the top left)
2. Copy the entire line below — it will look like one long line of text
3. Paste it directly into cell A1
4. Google Sheets will automatically spread each item across its own column

<details>
<summary>📋 &nbsp; Click to expand — copy and paste this into cell A1</summary>

&nbsp;
```
Date	Day of Week	Mood Score (1-10)	Mood Descriptor	Energy Level (1-10)	Stress Level (1-10)	Key Themes	Wins / Highlights	Challenges / Frustrations	People Mentioned	Follow Up	Gratitude	One Line Summary	Work vs Personal	Week Number	Word Count	Raw Transcript
```

</details>

&nbsp;

Your sheet should now have 17 column headers across the top row.

---

### Step 3 &nbsp;·&nbsp; Save Your Sheet ID

You will need this later. Here is how to find it:

1. Look at the URL in your browser while the Sheet is open
2. It will look something like this:
```
https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms/edit
```

3. The Sheet ID is the long string of letters and numbers between **/d/** and **/edit**
4. Copy it and save it somewhere safe — a notes app works perfectly

> 💡 In the example above the Sheet ID is:
> `1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms`

---

&nbsp;

# Part 3 — Get Your Free Gemini API Key

This is the key that lets your journal talk to Google's Gemini AI. It is free and takes
about two minutes to set up.

---

### Step 1 &nbsp;·&nbsp; Go to Google AI Studio

1. Go to **[aistudio.google.com](https://aistudio.google.com)**
2. Sign in with your Google account if prompted

---

### Step 2 &nbsp;·&nbsp; Create the Key

1. Look for the **Get API Key** button in the left sidebar
2. Click it, then click **Create API Key**
3. A pop-up will ask about a Google Cloud project — click **Create API key in new project**
   and let it handle everything automatically

> ✅ You do not need to enter any billing or credit card information.
> The free tier is more than enough for daily personal journaling.

---

### Step 3 &nbsp;·&nbsp; Save Your Key

1. Your API key will appear as a long string of random characters
2. Click the **copy** icon next to it
3. Save it immediately in your notes app — you will not be able to see it again
   after leaving the page

> ⚠️ Treat your API key like a password. Do not share it with anyone or post it publicly.

---

&nbsp;

# Part 4 — Create Your Secret Token

A secret token is a password that protects your journal from anyone else writing to it.
You make this up yourself — it can be any combination of letters and numbers.

**Create a secret token now.** Make it at least 20 characters long and random.

A good example: `mJ7xK2pL9qR4nT8vW3yA`

Save it in your notes app next to your API key and Sheet ID. You will need it in the
next two steps.

> 💡 You can use a password generator or just mash your keyboard randomly.
> There is no wrong answer as long as it is long and unique.

---

&nbsp;

# Part 5 — Build the Apps Script Backend

Google Apps Script is a free tool built into Google that runs your automation in the
cloud. This is what receives your journal entry, sends it to Gemini, and writes the
results to your Sheet. You will paste in pre-written code — no coding required.

---

### Step 1 &nbsp;·&nbsp; Open Apps Script

1. Go back to your **Daily Journal Log** Google Sheet
2. Click **Extensions** in the top menu bar
3. Click **Apps Script**
4. A new tab will open with a code editor

---

### Step 2 &nbsp;·&nbsp; Replace the Default Code

1. Click anywhere inside the code editor
2. Press **Ctrl+A** (Windows) or **Cmd+A** (Mac) to select all existing code
3. Press **Delete** or **Backspace** to clear it
4. The editor should now be completely empty

---

### Step 3 &nbsp;·&nbsp; Paste the App Code

1. Click the section below to expand it and reveal the code
2. Click inside the code block and press **Ctrl+A** / **Cmd+A** to select all of it
3. Copy it and paste it into the empty Apps Script editor

<details>
<summary>💻 &nbsp; Click to expand — complete Apps Script code</summary>

&nbsp;
```javascript
const GEMINI_API_KEY = 'YOUR_GEMINI_API_KEY_HERE';
const SHEET_ID       = 'YOUR_SHEET_ID_HERE';
const SHEET_NAME     = 'Sheet1';
const SECRET_TOKEN   = 'YOUR_SECRET_TOKEN_HERE';

function doGet() {
  return ContentService.createTextOutput(JSON.stringify({ status: 'ok' }))
    .setMimeType(ContentService.MimeType.JSON);
}

function doPost(e) {
  try {
    const data = JSON.parse(e.postData.contents);

    if (data.token !== SECRET_TOKEN) {
      return ContentService.createTextOutput(JSON.stringify({ status: 'error', message: 'Unauthorized' }))
        .setMimeType(ContentService.MimeType.JSON);
    }

    processJournalEntry(data.transcript);

    return ContentService.createTextOutput(JSON.stringify({ status: 'success' }))
      .setMimeType(ContentService.MimeType.JSON);

  } catch(err) {
    return ContentService.createTextOutput(JSON.stringify({ status: 'error', message: err.message }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function processJournalEntry(rawTranscript) {
  const prompt = `
You are a personal journal assistant. Analyze the following journal entry transcript and extract structured data.
Return ONLY a valid JSON object with no markdown, no backticks, no explanation — just raw JSON.

Transcript:
"${rawTranscript}"

Return this exact JSON structure:
{
  "moodScore": ,
  "moodDescriptor": "",
  "energyLevel": ,
  "stressLevel": ,
  "keyThemes": "<2-3 main themes separated by commas>",
  "wins": "",
  "challenges": "",
  "peopleMentioned": "",
  "followUp": "",
  "gratitude": "",
  "oneLineSummary": "",
  "workVsPersonal": "<e.g. 70% work / 30% personal>",
  "wordCount": 
}

If something is not mentioned, use "N/A" for strings and make a reasonable inference for numbers based on context.
  `;

  const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${GEMINI_API_KEY}`;

  const response = UrlFetchApp.fetch(url, {
    method: 'post',
    contentType: 'application/json',
    payload: JSON.stringify({ contents: [{ parts: [{ text: prompt }] }] })
  });

  const json    = JSON.parse(response.getContentText());
  const rawText = json.candidates[0].content.parts[0].text;
  const data    = JSON.parse(rawText.replace(/```json|```/g, '').trim());

  const sheet = SpreadsheetApp.openById(SHEET_ID).getSheetByName(SHEET_NAME);
  const now   = new Date();
  const days  = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];

  sheet.appendRow([
    now.toLocaleDateString(),
    days[now.getDay()],
    data.moodScore,
    data.moodDescriptor,
    data.energyLevel,
    data.stressLevel,
    data.keyThemes,
    data.wins,
    data.challenges,
    data.peopleMentioned,
    data.followUp,
    data.gratitude,
    data.oneLineSummary,
    data.workVsPersonal,
    getWeekNumber(now),
    data.wordCount,
    rawTranscript
  ]);
}

function getWeekNumber(d) {
  d = new Date(Date.UTC(d.getFullYear(), d.getMonth(), d.getDate()));
  d.setUTCDate(d.getUTCDate() + 4 - (d.getUTCDay() || 7));
  const yearStart = new Date(Date.UTC(d.getUTCFullYear(), 0, 1));
  return Math.ceil((((d - yearStart) / 86400000) + 1) / 7);
}
```

</details>

---

### Step 4 &nbsp;·&nbsp; Fill In Your Values

Near the very top of the code you just pasted, find these four lines:
```javascript
const GEMINI_API_KEY = 'YOUR_GEMINI_API_KEY_HERE';
const SHEET_ID       = 'YOUR_SHEET_ID_HERE';
const SHEET_NAME     = 'Sheet1';
const SECRET_TOKEN   = 'YOUR_SECRET_TOKEN_HERE';
```

Replace the placeholder text inside the quotes with your real values:

- `YOUR_GEMINI_API_KEY_HERE` → your API key from Part 3
- `YOUR_SHEET_ID_HERE` → your Sheet ID from Part 2
- `YOUR_SECRET_TOKEN_HERE` → your secret token from Part 4
- Leave `Sheet1` as is unless you renamed your Sheet tab

---

### Step 5 &nbsp;·&nbsp; Save the File

1. Click the **floppy disk icon** at the top of the editor, or press **Ctrl+S** / **Cmd+S**
2. When asked to name the project, type **Journal App**
3. Click **OK**

---

### Step 6 &nbsp;·&nbsp; Deploy the App

1. Click the **Deploy** button in the top right corner
2. Click **New Deployment**
3. Click the **gear icon** next to "Select type" and choose **Web App**
4. Fill in the settings exactly like this:
   - **Description:** Journal App
   - **Execute as:** Me
   - **Who has access:** Anyone
5. Click **Deploy**

---

### Step 7 &nbsp;·&nbsp; Authorize the App

A pop-up will appear asking you to authorize the app:

1. Click **Authorize access**
2. Choose your Google account
3. You may see a screen that says **"Google hasn't verified this app"**
   — this is completely normal for personal scripts you write yourself
4. Click **Advanced** at the bottom left
5. Click **Go to Journal App (unsafe)**
6. Click **Allow**

---

### Step 8 &nbsp;·&nbsp; Copy Your Deployment URL

After authorizing you will see a screen with a **Web App URL**. It will look like:
```
https://script.google.com/macros/s/AKfycb.../exec
```

Copy this URL and save it in your notes app. This is your Apps Script endpoint.

> 💡 This URL never changes even when you update your code later.

---

&nbsp;

# Part 6 — Set Up GitHub and Deploy the App

GitHub will host the front end of your journal — the screen you see on your phone.

---

### Step 1 &nbsp;·&nbsp; Create a New Repository

1. Sign in to **[github.com](https://github.com)**
2. Click the **+** icon in the top right and select **New repository**
3. Fill in the details:
   - **Repository name:** daily-journal
   - **Visibility:** Public
   - Check **Add a README file**
4. Click **Create repository**

---

### Step 2 &nbsp;·&nbsp; Create the App File

1. Inside your new repository click **Add file → Create new file**
2. Name the file exactly: **index.html**
3. Click inside the large text area below the filename
4. Expand the code block below, copy everything inside it, and paste it in

<details>
<summary>💻 &nbsp; Click to expand — complete index.html code</summary>

&nbsp;
```html



  
  
  Daily Journal
  
    * { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { height: 100%; width: 100%; overflow: hidden; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      background: #0f0f1a;
      color: #fff;
    }
    .screen {
      position: fixed;
      inset: 0;
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 24px 20px;
    }

    /* SETUP */
    #setupScreen {
      background: radial-gradient(ellipse at 50% 40%, #1e1e3a 0%, #0f0f1a 70%);
      justify-content: flex-start;
      padding-top: 60px;
      overflow-y: auto;
    }
    .setup-title { font-size: clamp(28px, 8vw, 44px); font-weight: 700; margin-bottom: 10px; text-align: center; }
    .setup-subtitle { font-size: clamp(15px, 4vw, 20px); color: rgba(255,255,255,0.4); text-align: center; line-height: 1.6; margin-bottom: 40px; max-width: 500px; }
    .setup-form { width: 100%; max-width: 560px; display: flex; flex-direction: column; gap: 20px; padding-bottom: 40px; }
    .field-label { font-size: clamp(14px, 3.5vw, 17px); color: rgba(255,255,255,0.6); margin-bottom: 8px; font-weight: 600; }
    .field-hint { font-size: clamp(12px, 3vw, 14px); color: rgba(255,255,255,0.3); margin-bottom: 10px; line-height: 1.5; }
    .field-input {
      width: 100%; padding: clamp(16px, 4vw, 20px); border-radius: 16px;
      border: 1px solid rgba(255,255,255,0.15); background: rgba(255,255,255,0.06);
      color: #fff; font-size: clamp(14px, 3.5vw, 17px); outline: none;
      transition: border-color 0.2s; font-family: inherit;
    }
    .field-input:focus { border-color: rgba(102,126,234,0.7); }
    .field-input::placeholder { color: rgba(255,255,255,0.2); }
    .setup-btn {
      width: 100%; padding: clamp(20px, 5.5vw, 26px); border-radius: 20px; border: none;
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: white; font-size: clamp(20px, 5.5vw, 26px); font-weight: 700;
      cursor: pointer; margin-top: 8px; transition: opacity 0.2s, transform 0.15s;
    }
    .setup-btn:active { transform: scale(0.98); opacity: 0.9; }
    .setup-note { font-size: clamp(12px, 3vw, 14px); color: rgba(255,255,255,0.25); text-align: center; line-height: 1.6; margin-top: 4px; }

    /* IDLE */
    #idleScreen { background: radial-gradient(ellipse at 50% 40%, #1e1e3a 0%, #0f0f1a 70%); }
    .journal-title { font-size: clamp(36px, 11vw, 64px); font-weight: 700; margin-bottom: 10px; }
    .journal-date { font-size: clamp(18px, 5vw, 26px); color: rgba(255,255,255,0.4); margin-bottom: 52px; }
    .mic-ring {
      position: relative;
      width: clamp(200px, 52vw, 280px);
      height: clamp(200px, 52vw, 280px);
      margin-bottom: 52px;
    }
    .mic-ring::before {
      content: ''; position: absolute; inset: -10px; border-radius: 50%;
      background: conic-gradient(#667eea, #764ba2, #f093fb, #667eea);
      animation: spin 4s linear infinite; opacity: 0.7;
    }
    .mic-ring::after {
      content: ''; position: absolute; inset: -5px;
      border-radius: 50%; background: #0f0f1a;
    }
    .mic-btn {
      position: absolute; inset: 0; z-index: 1; border-radius: 50%; border: none;
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: white; font-size: clamp(60px, 16vw, 100px);
      cursor: pointer; width: 100%; height: 100%; transition: transform 0.15s ease;
    }
    .mic-btn:active { transform: scale(0.94); }
    .idle-hint { font-size: clamp(20px, 5.5vw, 28px); color: rgba(255,255,255,0.4); text-align: center; line-height: 1.6; }
    .settings-link {
      position: fixed; bottom: 28px; right: 24px;
      font-size: clamp(12px, 3vw, 14px); color: rgba(255,255,255,0.2);
      cursor: pointer; transition: color 0.2s;
    }
    .settings-link:hover { color: rgba(255,255,255,0.5); }

    /* RECORDING */
    #recordingScreen { background: radial-gradient(ellipse at 50% 40%, #2a0a1a 0%, #0f0f1a 70%); }
    .rec-indicator { display: flex; align-items: center; gap: 12px; margin-bottom: 36px; }
    .rec-dot { width: 18px; height: 18px; border-radius: 50%; background: #f5576c; animation: blink 1s infinite; }
    .rec-label { font-size: clamp(18px, 5vw, 26px); color: #f5576c; font-weight: 600; letter-spacing: 3px; text-transform: uppercase; }
    .timer-display { font-size: clamp(80px, 22vw, 150px); font-weight: 700; letter-spacing: -3px; line-height: 1; margin-bottom: 28px; font-variant-numeric: tabular-nums; }
    .live-transcript {
      width: 100%; max-width: 640px; overflow-y: auto;
      font-size: clamp(20px, 5.5vw, 28px); line-height: 1.6;
      color: rgba(255,255,255,0.7); text-align: center;
      padding: 0 8px; margin-bottom: 36px; max-height: 28vh;
    }
    .live-transcript .interim { color: rgba(255,255,255,0.3); }
    .stop-btn {
      width: clamp(150px, 42vw, 210px); height: clamp(150px, 42vw, 210px);
      border-radius: 50%; border: 3px solid rgba(245,87,108,0.5);
      background: rgba(245,87,108,0.15); color: #f5576c;
      font-size: clamp(52px, 14vw, 80px); cursor: pointer;
      transition: all 0.2s ease; animation: pulse-red 2s infinite;
    }
    .stop-btn:active { transform: scale(0.94); background: rgba(245,87,108,0.3); }

    /* REVIEW */
    #reviewScreen {
      background: radial-gradient(ellipse at 50% 40%, #0a1a2a 0%, #0f0f1a 70%);
      justify-content: flex-start; padding-top: 60px; overflow-y: auto;
    }
    .review-title { font-size: clamp(28px, 8vw, 48px); font-weight: 700; margin-bottom: 8px; }
    .review-subtitle { font-size: clamp(16px, 4.5vw, 22px); color: rgba(255,255,255,0.4); margin-bottom: 32px; text-align: center; }
    .transcript-preview {
      width: 100%; max-width: 640px;
      background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1);
      border-radius: 24px; padding: 28px;
      font-size: clamp(18px, 5vw, 24px); line-height: 1.7;
      color: rgba(255,255,255,0.8); margin-bottom: 32px;
      max-height: 36vh; overflow-y: auto;
    }
    .review-actions { width: 100%; max-width: 640px; display: flex; flex-direction: column; gap: 16px; padding-bottom: 40px; }
    .save-btn {
      width: 100%; padding: clamp(22px, 6vw, 30px); border-radius: 20px; border: none;
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: white; font-size: clamp(22px, 6vw, 30px); font-weight: 700;
      cursor: pointer; transition: opacity 0.2s, transform 0.15s;
    }
    .save-btn:active { transform: scale(0.98); opacity: 0.9; }
    .save-btn:disabled { opacity: 0.4; cursor: not-allowed; transform: none; }
    .retry-btn {
      width: 100%; padding: clamp(18px, 5vw, 24px); border-radius: 20px;
      border: 1px solid rgba(255,255,255,0.15); background: transparent;
      color: rgba(255,255,255,0.5); font-size: clamp(18px, 5vw, 24px);
      cursor: pointer; transition: all 0.2s;
    }
    .retry-btn:active { background: rgba(255,255,255,0.05); }

    /* PROCESSING */
    #processingScreen { background: radial-gradient(ellipse at 50% 40%, #0d1a2e 0%, #0f0f1a 70%); gap: 36px; }
    .spinner {
      width: clamp(72px, 20vw, 110px); height: clamp(72px, 20vw, 110px);
      border-radius: 50%; border: 5px solid rgba(102,126,234,0.2);
      border-top-color: #667eea; animation: spin 0.8s linear infinite;
    }
    .processing-label { font-size: clamp(24px, 7vw, 36px); font-weight: 600; color: rgba(255,255,255,0.8); text-align: center; }
    .processing-sub { font-size: clamp(16px, 4.5vw, 22px); color: rgba(255,255,255,0.35); text-align: center; }

    /* SUCCESS */
    #successScreen { background: radial-gradient(ellipse at 50% 40%, #0a2a1a 0%, #0f0f1a 70%); gap: 28px; }
    .success-icon { font-size: clamp(80px, 22vw, 120px); line-height: 1; }
    .success-title { font-size: clamp(36px, 10vw, 60px); font-weight: 700; color: #6ee7b7; }
    .success-sub { font-size: clamp(18px, 5vw, 26px); color: rgba(255,255,255,0.45); text-align: center; line-height: 1.6; }
    .done-btn {
      margin-top: 8px; padding: clamp(20px, 5vw, 26px) clamp(40px, 10vw, 64px);
      border-radius: 20px; border: 1px solid rgba(110,231,183,0.3);
      background: rgba(110,231,183,0.1); color: #6ee7b7;
      font-size: clamp(20px, 5.5vw, 28px); font-weight: 600; cursor: pointer;
    }
    .done-btn:active { background: rgba(110,231,183,0.2); }

    /* ERROR */
    .error-bar {
      position: fixed; bottom: 32px; left: 20px; right: 20px;
      background: rgba(245,87,108,0.15); border: 1px solid rgba(245,87,108,0.4);
      border-radius: 16px; padding: 18px 22px;
      font-size: clamp(16px, 4.5vw, 20px); color: #fca5a5;
      text-align: center; display: none; z-index: 100;
    }

    /* ANIMATIONS */
    @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
    @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0.2; } }
    @keyframes pulse-red {
      0%, 100% { box-shadow: 0 0 0 0 rgba(245,87,108,0.4); }
      50%       { box-shadow: 0 0 0 24px rgba(245,87,108,0); }
    }
  



  
  
    📓 Welcome
    Enter your credentials once to get started.They are saved privately on this device only.
    
      
        Apps Script URL
        Your Google Apps Script deployment URL — starts with https://script.google.com/...
        
      
      
        Secret Token
        The secret token you chose during setup
        
      
      Save & Get Started
      These values are stored locally on your device and never appear in the source code.
    
  

  
  
    📓 Daily Journal
    
    
      🎙️
    
    Tap the mic and talkabout your day
    ⚙️ settings
  

  
  
    
      
      Recording
    
    00:00
    
      Listening...
    
    ⏹️
  

  
  
    Review Your Entry
    Looks good? Save it and let Gemini do the rest.
    
    
      ✨ Synthesize & Save
      Start Over
    
  

  
  
    
    Synthesizing your day...
    Gemini is reading between the lines
  

  
  
    ✅
    Entry Saved
    Your day has been loggedand structured automatically
    Done
  

  
  

  
    let recognition;
    let isRecording = false;
    let transcript  = '';
    let timerInterval;
    let seconds     = 0;
    let wakeLock    = null;

    let APPS_SCRIPT_URL = localStorage.getItem('journal_url')   || '';
    let SECRET_TOKEN    = localStorage.getItem('journal_token') || '';

    document.getElementById('todayDate').textContent =
      new Date().toLocaleDateString('en-US', { weekday: 'long', month: 'long', day: 'numeric' });

    function init() {
      if (!APPS_SCRIPT_URL || !SECRET_TOKEN) {
        showScreen('setupScreen');
      } else {
        showScreen('idleScreen');
      }
    }

    function saveSetup() {
      const url   = document.getElementById('urlInput').value.trim();
      const token = document.getElementById('tokenInput').value.trim();
      if (!url || !token) {
        showError('Please fill in both fields before continuing.');
        return;
      }
      localStorage.setItem('journal_url',   url);
      localStorage.setItem('journal_token', token);
      APPS_SCRIPT_URL = url;
      SECRET_TOKEN    = token;
      showScreen('idleScreen');
    }

    function openSetup() {
      document.getElementById('urlInput').value   = APPS_SCRIPT_URL;
      document.getElementById('tokenInput').value = SECRET_TOKEN;
      showScreen('setupScreen');
    }

    function showScreen(id) {
      document.querySelectorAll('.screen').forEach(s => s.style.display = 'none');
      document.getElementById(id).style.display = 'flex';
    }

    async function requestWakeLock() {
      try { if ('wakeLock' in navigator) wakeLock = await navigator.wakeLock.request('screen'); } catch(e) {}
    }
    async function releaseWakeLock() {
      if (wakeLock) { await wakeLock.release(); wakeLock = null; }
    }
    document.addEventListener('visibilitychange', async () => {
      if (document.visibilityState === 'visible' && isRecording) await requestWakeLock();
    });

    async function startRecording() {
      hideError();
      try {
        const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
        stream.getTracks().forEach(t => t.stop());
      } catch(err) {
        showError(err.name === 'NotAllowedError'
          ? 'Microphone access denied. Check your browser settings and try again.'
          : 'Could not access microphone: ' + err.message);
        return;
      }
      if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
        showError('Speech recognition not supported. Use Chrome on Android or Safari on iPhone.');
        return;
      }
      await requestWakeLock();
      transcript = '';
      showScreen('recordingScreen');
      document.getElementById('liveTranscript').innerHTML = 'Listening...';
      const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
      recognition = new SR();
      recognition.continuous     = true;
      recognition.interimResults = true;
      recognition.lang           = 'en-US';
      recognition.onresult = (e) => {
        let final = '', interim = '';
        for (let i = e.resultIndex; i < e.results.length; i++) {
          if (e.results[i].isFinal) final  += e.results[i][0].transcript + ' ';
          else                       interim += e.results[i][0].transcript;
        }
        transcript += final;
        document.getElementById('liveTranscript').innerHTML =
          transcript + (interim ? '' + interim + '' : '');
      };
      recognition.onerror = (e) => {
        if (e.error !== 'no-speech') { showError('Microphone error: ' + e.error); stopRecording(); }
      };
      recognition.onend = () => { if (isRecording) recognition.start(); };
      recognition.start();
      isRecording = true;
      seconds = 0;
      timerInterval = setInterval(() => {
        seconds++;
        document.getElementById('timerDisplay').textContent =
          String(Math.floor(seconds / 60)).padStart(2, '0') + ':' +
          String(seconds % 60).padStart(2, '0');
      }, 1000);
    }

    async function stopRecording() {
      if (recognition) recognition.stop();
      isRecording = false;
      clearInterval(timerInterval);
      await releaseWakeLock();
      if (transcript.trim().length > 0) {
        document.getElementById('transcriptPreview').textContent = transcript.trim();
        showScreen('reviewScreen');
      } else {
        showError('Nothing was recorded. Tap the mic and try again.');
        showScreen('idleScreen');
      }
    }

    async function submitJournal() {
      if (!transcript.trim()) return;
      const saveBtn = document.getElementById('saveBtn');
      saveBtn.disabled = true;
      showScreen('processingScreen');
      try {
        const response = await fetch(APPS_SCRIPT_URL, {
          method: 'POST',
          headers: { 'Content-Type': 'text/plain' },
          body: JSON.stringify({ transcript: transcript, token: SECRET_TOKEN })
        });
        const result = await response.json();
        if (result.status === 'success') {
          showScreen('successScreen');
        } else {
          throw new Error(result.message || 'Unknown error');
        }
      } catch(err) {
        showScreen('reviewScreen');
        showError('Something went wrong: ' + err.message);
      } finally {
        saveBtn.disabled = false;
      }
    }

    function resetToIdle() {
      transcript = '';
      seconds    = 0;
      document.getElementById('timerDisplay').textContent = '00:00';
      document.getElementById('liveTranscript').innerHTML = 'Listening...';
      hideError();
      showScreen('idleScreen');
    }

    function showError(msg) {
      const bar = document.getElementById('errorBar');
      bar.textContent   = msg;
      bar.style.display = 'block';
      setTimeout(hideError, 6000);
    }
    function hideError() {
      document.getElementById('errorBar').style.display = 'none';
    }

    init();
  


```

</details>

&nbsp;

5. Click **Commit changes** in the top right
6. Click **Commit changes** again on the confirmation pop-up

---

### Step 3 &nbsp;·&nbsp; Enable GitHub Pages

1. Click **Settings** in the top menu of your repository
2. Click **Pages** in the left sidebar
3. Under **Branch** click the dropdown that says **None** and select **main**
4. Click **Save**
5. Wait about 60 seconds and refresh the page
6. You will see a banner that says **Your site is live at:**
```
https://yourusername.github.io/daily-journal
```

Copy this URL — this is your journal app.

---

&nbsp;

# Part 7 — First Launch & Setup

---

### Step 1 &nbsp;·&nbsp; Open the App

Open your GitHub Pages URL in:
- **Chrome** on Android
- **Safari** on iPhone

> ⚠️ Other browsers do not support voice recording. You must use Chrome or Safari.

---

### Step 2 &nbsp;·&nbsp; Enter Your Credentials

The first time you open the app you will see a welcome screen asking for two things:

**Apps Script URL** — paste the deployment URL you saved at the end of Part 5

**Secret Token** — paste the secret token you created in Part 4

Tap **Save & Get Started**. These values are saved privately on your device.
You will never need to enter them again.

---

### Step 3 &nbsp;·&nbsp; Add to Your Home Screen

**iPhone (Safari)**
1. Tap the **Share button** at the bottom of the screen (box with an arrow)
2. Scroll down and tap **Add to Home Screen**
3. Name it **Daily Journal** and tap **Add**

**Android (Chrome)**
1. Tap the **three dots** in the top right corner
2. Tap **Add to Home Screen**
3. Name it **Daily Journal** and tap **Add**

The app now lives on your home screen and opens like a native app.

---

### Step 4 &nbsp;·&nbsp; Run Your First Test

1. Tap the **Daily Journal** icon on your home screen
2. Tap the large microphone button — allow microphone access if prompted
3. Speak a few sentences casually about your day
4. Tap the stop button when you are done
5. Review your transcript on screen
6. Tap **✨ Synthesize & Save**
7. Wait 10–15 seconds while Gemini processes your entry
8. Open your **Daily Journal Log** Google Sheet
9. You should see a new row with all 17 columns filled in automatically

If the row appears — your journal is fully working. 🎉

---

&nbsp;

# Part 8 — Optional: Build a Gemini Gem

Once you have a few weeks of entries, you can create a custom AI companion that reads
your journal and lets you have natural conversations about your own data. This is where
the system gets really powerful.

---

### Step 1 &nbsp;·&nbsp; Open Gemini

Go to **[gemini.google.com](https://gemini.google.com)** and sign in with your Google account.

---

### Step 2 &nbsp;·&nbsp; Create a New Gem

1. Find **Gem manager** in the left sidebar
2. Click **New Gem**
3. Give your Gem a name — make it something personal that you will enjoy opening

---

### Step 3 &nbsp;·&nbsp; Add a Description

Paste this into the Description field:
```
Your personal journal companion. Ask about mood patterns, recurring themes, memorable
moments, and personal growth over time. Grounded entirely in your actual journal entries.
```

---

### Step 4 &nbsp;·&nbsp; Add Instructions

<details>
<summary>📋 &nbsp; Click to expand — copy and paste into the Instructions field</summary>

&nbsp;
```
You are a personal journal companion — warm, perceptive, and direct. You have a distinct
personality: thoughtful but never heavy, insightful but never preachy. You speak like a
trusted confidant who happens to have a perfect memory.

Your primary source is always the user's Daily Journal Log. Every response must be
grounded in actual entries — specific dates, real events, things the user actually said.
If you cannot find relevant data in the log, say so honestly rather than filling the gap
with assumptions.

Any personal context document in your knowledge base is background only. Use it to add
texture — for example, recognizing who someone is so the user does not have to explain.
Never lead with it, never summarize it back to the user, and never use it as a substitute
for real journal data.

When referencing entries, always cite the specific date. Keep responses concise and
conversational unless the user asks for a deep dive. Lead with what the data shows.

Slash commands you support:

/summary [timeframe] — Summarize actual logged entries from that period with dates
/mood — Pull mood and energy scores, identify trends, best and worst days
/patterns — Surface recurring themes, people, and challenges across entries
/wins — Compile wins and highlights from a given period
/lowpoints — Surface harder days compassionately, noting growth since then
/people — Show who appears most in entries and in what context
/followups — Pull all logged follow-up items that may still need attention
/compare [date1] [date2] — Compare two specific logged days side by side
/growth — Reflect on how mood, stress, and themes have shifted over time
/gratitude — Compile all logged gratitude moments into one summary
/stress — Analyze stress scores and connect spikes to what was actually happening

Always end with one short question rooted in something specific from the entries.
```

</details>

---

### Step 5 &nbsp;·&nbsp; Connect Your Google Sheet

1. In the **Knowledge** section click **Add a file or link**
2. Select **Google Drive**
3. Find and select your **Daily Journal Log** spreadsheet
4. Click **Insert**

---

### Step 6 &nbsp;·&nbsp; Add a Personal Context File &nbsp;*(optional)*

Creating a short personal context document makes your Gem dramatically more useful.
It means you never have to explain who the people in your life are or what your current
goals are — the Gem already knows.

Create a new Google Doc and write a few paragraphs covering:
- Your job and industry
- The people in your life and who they are to you
- Your current goals personally and professionally

Then upload it to the Knowledge section the same way you added your Sheet.

---

### Step 7 &nbsp;·&nbsp; Save and Start Chatting

Click **Save** to publish your Gem. Try these to get started:

- *Tell me about my recent journal entries*
- `/mood` — see your mood trends over time
- `/patterns` — surface what keeps coming up
- `/wins` — remind yourself what has gone well

> 💡 The Gem becomes much more useful after a few weeks of consistent entries.
> Give it time to accumulate data before asking big picture questions.

---

&nbsp;

# Troubleshooting

| Problem | Fix |
|---------|-----|
| Mic button does nothing | Make sure you are using Chrome (Android) or Safari (iPhone) |
| Microphone access denied | Go to your browser settings and allow microphone access for the site |
| Something went wrong on save | Check that your Apps Script URL and secret token are entered correctly in the app settings |
| `429` error | Gemini free tier rate limit — wait a minute and try again |
| Sheet not updating | Make sure `SHEET_NAME` in your Apps Script code matches your tab name exactly — default is `Sheet1` |
| Screen times out during recording | Keep your screen on manually — the app tries to prevent this but some devices override it |
| Setup screen keeps appearing | Make sure you tapped Save & Get Started after entering your credentials |

---

&nbsp;

# Tips for Getting the Most Out of It

**🗣️ Talk naturally** — The messier and more casual the better. You are not writing an essay. Just talk the way you would leave a voice note for a friend.

**📅 Anchor it to a habit** — The best time to journal is right after work on your commute home, on a walk, or during the first few minutes of unwinding. Attaching it to something you already do makes it stick.

**⏱️ Keep it short** — Two to three minutes of talking produces a rich, detailed entry. You do not need to cover everything — just what stood out.

**📈 Review monthly** — Once a month ask your Gem for a `/summary last month`. Patterns that are invisible day to day become very clear over longer windows.

**🔄 Refresh your Gem** — Make sure to connect your Journal Log Google Sheet via Google Drive such that it stays up to date.

**🔒 Your data is private** — Your Google Sheet is private to your Google account. Your Gem is private to your Google account. Nothing you log is visible to anyone else.

---

*Built with Google Apps Script · Gemini AI · Google Sheets · GitHub Pages · 100% Free*
