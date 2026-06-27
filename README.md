# AI Job Application Tracker
> Built for Gappy AI Hackathon 2026 · Powered by Lemma SDK

## 🔗 Live App
**https://job.apps.lemma.work/**

## 🎬 Demo Video
https://drive.google.com/file/d/1MFMtKlA4t-KQc3BBvGud-Asc702EUFlK/view?usp=sharing

---

## 🎯 Problem Statement
Students and recent graduates apply to many jobs across portals, referrals, emails, and LinkedIn — but struggle to track application status and miss follow-ups, losing opportunities due to poor pipeline management.

---

## 💡 Solution
An AI-powered Job Application Tracker built entirely with Lemma SDK that:
- Tracks every application with company, role, status, and date
- AI Followup Strategist ranks which applications need attention TODAY
- Drafts personalized follow-up messages automatically
- Add jobs in plain text — AI auto-parses and stores
- Flags overdue follow-ups with urgency tiers
- Weekly digest of full pipeline health

---

## 🏗️ Architecture (Lemma SDK)

### 📊 Tables (4)
```
applications
├── id, company, role, status
├── priority, date_applied, source
├── location, salary, notes
└── created_at

contacts
├── id, name, email, role
├── application_id (FK)
└── created_at

follow_ups
├── id, application_id (FK)
├── due_date, type (email|linkedin|call)
├── status (pending|sent|skipped|overdue)
├── draft_message, rationale
└── created_at

events (audit trail)
├── id, application_id (FK)
├── type, note
└── created_at
```

### 🤖 AI Agents (3)

#### 1. followup-strategist
Ranks applications by urgency using 5 tiers:
- 🔴 overdue → follow up immediately
- 🟠 stage-stalled → no movement in days
- 🟡 long-silence → no reply for a while
- 🟢 polite nudge → gentle check-in
- ⚪ skip → too early or recent activity

Returns: ranked queue + drafted messages + skip reasons

#### 2. pipeline-summary
Weekly pulse report:
- Counts by stage (applied/screen/onsite/offer)
- Response rate by source
- Stale applications list
- 3 concrete recommendations

#### 3. log-app
Parses plain text input like:
`"Applied to Google as SWE intern on June 25 via LinkedIn"`
- Creates application row automatically
- Schedules first 7-day follow-up
- Emits audit event
- Flags any ambiguity for human review

### ⚙️ Workflows (2)

#### log-application
```
FORM (intake)
  → AGENT (log-app parses input)
  → FORM (human confirms parse)
  → DECISION (looks right?)
  → END
```
Human approval checkpoint — nothing saved without confirmation.

#### weekly-digest
```
TIME trigger (every Sunday 8pm)
  → AGENT (pipeline-summary runs)
  → END
```

### 📅 Schedules (1)
- `weekly-recap` → fires `weekly-digest` every Sunday at 8pm

---

## 🚀 How to Use

### Add a Job Application
Type in plain text to the AI agent:
```
Applied to Swiggy as Product Manager on June 27 via referral
```
AI automatically parses and stores it!

### Check Follow-ups
Ask the strategist:
```
What should I follow up on today?
```
Get ranked list with drafted messages ready to send.

### Approve Follow-ups
Review drafted messages → approve → AI marks as sent and logs event.

---

## 🛠️ Tech Stack
| Layer | Tool |
|---|---|
| Infrastructure | Lemma SDK |
| AI Agents | Lemma + OpenAI API |
| Database | Lemma Tables (RLS enabled) |
| Workflows | Lemma Workflows |
| Frontend | Lemma Apps |
| Hosting | lemma.work |

---

## 📁 Judging Criteria Coverage

| Criteria | Weight | How we address it |
|---|---|---|
| Problem clarity & real-world fit | 35% | Real student pain point — job tracking across portals |
| Product judgment | 25% | Human approval loop, no auto-send, sensible agent design |
| Execution quality | 25% | Live app works end-to-end at job.apps.lemma.work |
| Lemma SDK utilisation | 15% | 4 tables, 3 agents, 2 workflows, 1 schedule, deployed on lemma.work |

---

## 👤 Submission Details
- **Hackathon:** Gappy AI — Ship to Get Hired 2026
- **Track:** AI Job Application Command Centre
- **Built with:** Lemma SDK (open-source infrastructure for agentic apps)
- **Pod access:** ayush@gappy.ai (granted for judging)

---

## 📞 Contact
- **Live App:** https://job.apps.lemma.work/
- **Demo:** https://drive.google.com/file/d/1MFMtKlA4t-KQc3BBvGud-Asc702EUFlK/view?usp=sharing
