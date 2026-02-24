# 📓 Daily Journal — AI-Powered Voice Journal

Turn your daily commute into a structured, searchable record of your life — completely free.

If you want to build your own journaling app, follow the instructions in [Setup Guide](./SETUP.md) *(~45 minutes setup, $0 Cost)*

---

## What It Does

Traditional journaling requires time, effort, and discipline. This system removes all of
that friction. Your entire daily interaction is: open the app, tap a button, talk, tap
stop. Gemini AI handles everything else — transcribing your words, extracting meaningful
insights, and storing everything in an organized Google Sheet that compounds in value
over time.

No subscriptions. No servers. No manual data entry. Built entirely with free tools.

---

## How It Works
```
  You Talk          Voice is          Gemini AI           Data Lands
  for 2-5 min  ──▶  Transcribed  ──▶  Structures It  ──▶  in Google Sheets
  (on your          (in the           (via API)             (automatically)
   commute)          browser)
                                                                  │
                                                                  ▼
                                                     ── OPTIONAL ──────────────
                                                    │                          │
                                                    │   Chat with your journal │
                                                    │   using a Gemini Gem to  │
                                                    │   surface patterns and   │
                                                    │   insights over time     │
                                                    │                          │
                                                     ──────────────────────────
```

---

## The Stack

| Layer | Tool | Cost |
|-------|------|------|
| 🌐 Hosting | GitHub Pages | Free |
| 🎙️ Voice Transcription | Web Speech API | Free |
| ⚙️ Automation | Google Apps Script | Free |
| 🤖 AI Synthesis | Gemini 2.5 Flash API | Free tier |
| 📊 Storage | Google Sheets | Free |
| 💬 Insights (optional) | Gemini Gem | Free |

**Total cost: $0**

---

## What Gets Captured

Every entry is automatically structured into 17 columns in your Google Sheet:

| | Field | Description |
|-|-------|-------------|
| 📅 | Date & Day | When the entry was logged |
| 😊 | Mood Score | 1-10 numeric rating |
| 😐 | Mood Descriptor | One word mood label |
| ⚡ | Energy Level | 1-10 numeric rating |
| 😤 | Stress Level | 1-10 numeric rating |
| 💡 | Key Themes | 2-3 main topics from your entry |
| 🏆 | Wins / Highlights | Positive moments from the day |
| 😓 | Challenges | Difficulties or frustrations mentioned |
| 👥 | People Mentioned | Names of people referenced |
| ✅ | Follow Up | Action items to carry into tomorrow |
| 🙏 | Gratitude | One positive or grateful moment |
| 📝 | One Line Summary | Single sentence summary of the day |
| ⚖️ | Work vs Personal | Estimated content split |
| 📆 | Week Number | For weekly trend analysis |
| 🔢 | Word Count | Length of your entry |
| 🎙️ | Raw Transcript | Your original unedited words |

---

## The App Experience

The interface is designed to feel native on your phone — no browser chrome, no clutter.
Five full-screen states guide you through the entire flow:

**Idle** — Home screen with a large microphone button and today's date

**Recording** — Live countdown timer with your words appearing on screen in real time
as you speak

**Review** — Your full transcript displayed before anything is saved, giving you the
chance to confirm or start over

**Processing** — A brief synthesis screen while Gemini structures your entry

**Success** — Confirmation that your entry has been logged and stored

---

## The Insights Layer (Optional)

Once you have a few weeks of entries, you can create a custom Gemini Gem — a personalized
AI companion connected directly to your journal data via Google Drive. This transforms
your Sheet from a passive log into an active, conversational record of your life.

Ask it anything about your own data, or use built-in slash commands:

| Command | What It Does |
|---------|-------------|
| `/summary [timeframe]` | Narrative summary of entries for a period |
| `/mood` | Mood and energy trends, best and worst days |
| `/patterns` | Recurring themes, people, and challenges |
| `/wins` | Wins and highlights from a given period |
| `/lowpoints` | Harder days surfaced compassionately |
| `/people` | Who appears most and in what context |
| `/followups` | Unresolved action items across entries |
| `/compare [date1] [date2]` | Two days or periods side by side |
| `/growth` | How mood, stress, and themes have shifted over time |
| `/gratitude` | All gratitude moments compiled |
| `/stress` | Stress trends connected to real events |

The Gem works best after a few weeks of consistent entries — the more data it has,
the sharper and more personal its insights become.

---

## Security

The app uses a secret token to protect your Apps Script endpoint. Credentials are stored
in your browser's localStorage — they are entered once on first launch and never appear
in the source code. Your Google Sheet is private to your Google account and is never
exposed through this app.

---

## Setup

Full step-by-step setup instructions are available in the
[Setup Guide](./SETUP.md)  or follow the in-app credential screen
on first launch.

**What you need:**
- A Google account
- A smartphone with Chrome (Android) or Safari (iPhone)
- A free Gemini API key from [aistudio.google.com](https://aistudio.google.com)
- About 45 minutes for initial setup

---

## Browser Compatibility

| Browser | Supported |
|---------|-----------|
| Chrome (Android) | ✅ |
| Safari (iPhone) | ✅ |
| Firefox | ❌ |
| Chrome (iOS) | ❌ |

> The Web Speech API requires HTTPS and is not supported in all browsers.
> Chrome on Android and Safari on iPhone are the only recommended clients.

---

*Built with Google Apps Script · Gemini AI · Google Sheets · GitHub Pages · 100% Free*
