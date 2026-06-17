# SalesForceCRM — Hands-On Salesforce Admin Learning Project

## What this is

This is a self-training project to learn Salesforce Administration the practical way — by building a working CRM for a fictional beverage company called **MegaCRM** (the company persona used inside the demo data, not the project name itself). The project mirrors how a real Salesforce Admin job actually works day to day: setting up users, building automation, managing data, creating reports, and keeping the system clean and secure.

This project is designed to do two things at once:
1. Build real, demonstrable Salesforce skills you can show in interviews or on a resume.
2. Cover the topics tested on the **Salesforce Certified Platform Administrator** exam (refreshed Dec 15, 2025 — now weighted heavily toward Data & Analytics, with a new Agentforce AI section).

## The fictional company

**MegaCRM** is a fictional beverage/FMCG company used purely as a realistic data scenario — similar in spirit to the GlobalTrader Inc. setup used in the D365CRM project, but adapted to fit Salesforce's data model (Leads, Accounts, Contacts, Opportunities, Cases) instead of Dataverse.

**Sales team (4 reps):**
- Jordan Reyes — East Region
- Maya Chen — West Region
- Devon Walsh — Central Region
- Lina Osei — National Accounts

## How to use this repo

1. Sign up for a free Salesforce Developer Edition org (any personal Gmail works, no company domain needed).
2. Work through `TASKS.md` in order, phase by phase. Each task has a full description so you can copy/paste it back into a chat later and get the exact same context.
3. Use the files in `/data` for the bulk CSV import tasks (Data Loader / Data Import Wizard practice).
4. Tasks marked **(Optional)** are stretch tasks — good for going deeper, not required to finish the core project or to be exam-ready.

## Folder structure

```
SalesForceCRM/
├── README.md              <- this file
├── TASKS.md                <- full task list, all phases
├── data/                   <- CSV files for bulk import practice
│   ├── leads_sample.csv
│   ├── accounts_sample.csv
│   ├── contacts_sample.csv
│   └── duplicate_accounts.csv
└── scripts/
    └── create_salesforce_issues.py   <- pushes all tasks to GitHub as Issues
```

## Certification note

Completing this task list will cover almost everything on the Salesforce Certified Platform Administrator exam blueprint (Org Setup, Data Model, Security, Automation, Reports/Analytics, Service Cloud basics), plus the new Agentforce AI topic. It is **not** a substitute for timed practice exams — Phase 12 includes dedicated concept-check and practice-exam tasks for that reason.

## Other related projects (kept separate, do not mix)

- **D365CRM** — Dynamics 365 Sales learning project (GlobalTrader Inc.)
- **D365FO** — Dynamics 365 Finance & Operations learning project (Cacacumo Inc.)
- **snowflakesLearning** — Snowflake data warehouse project (ingests D365CRM data)

This project (SalesForceCRM / MegaCRM) is a 4th, fully independent track.
