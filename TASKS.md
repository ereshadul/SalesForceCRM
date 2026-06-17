# SalesForceCRM — Full Task List
Fictional company persona: **MegaCRM** (beverage/FMCG)
Reps: Jordan Reyes (East), Maya Chen (West), Devon Walsh (Central), Lina Osei (National Accounts)

Each task below has a full description. Copy/paste any single task back into a chat later and the context is self-contained.

---

## Phase 0 — Org Setup & Signup

**T001. Sign up for a free Salesforce Developer Edition org**
Go to developer.salesforce.com and sign up using a personal Gmail address (no company domain needed). This gives you a free, permanent sandbox-style org (200MB storage) to build everything in. Save your login URL, username, and a strong password somewhere safe — you'll need this URL specifically (it's different from login.salesforce.com).

**T002. Configure Company Profile**
In Setup → Company Information, set: Org name to "MegaCRM", fiscal year start month, business hours (e.g., Mon-Fri 9-5), default time zone, and currency to USD. This is the foundational org-level config every admin job touches first.

**T003. Set up your own admin user profile**
Confirm your own login is set to System Administrator profile (default for the org creator). This is your "god mode" account — equivalent to your D365Sales login in the D365CRM project.

**T004. Create 4 sales rep users**
In Setup → Users, create 4 new users: Jordan Reyes (East Region), Maya Chen (West Region), Devon Walsh (Central Region), Lina Osei (National Accounts). Use throwaway/alias emails since Developer orgs require unique emails per user (e.g., yourgmail+jordan@gmail.com works with Gmail's + trick).

**T005. Assign profiles and roles to each rep**
Assign each of the 4 reps the "Standard User" profile (not Admin). This teaches the difference between Profile (what they CAN do) vs Role (where they sit in the hierarchy, covered later in Phase 5).

**T006. Configure an Org-Wide Email Address**
In Setup → Organization-Wide Addresses, add an email like sales@megacrm-demo.com (doesn't need to be real/verified for learning purposes, just demonstrates the config). This is what system emails (like Flow notifications) appear to send from.

---

## Phase 1 — Data Model & Custom Fields

**T007. Review standard objects**
Spend time in Setup → Object Manager looking at Lead, Account, Contact, Opportunity, and Case. Understand what each one is for and how they relate (Lead converts into Account+Contact+Opportunity). This is the Salesforce equivalent of understanding Dataverse entities in D365.

**T008. Create custom fields on Account**
Add 3 custom fields to the Account object: "Beverage Category" (picklist: Soda, Juice, Water, Energy Drink), "Distribution Channel" (picklist: Retail, Wholesale, Direct), "Account Tier" (picklist: Bronze/Silver/Gold, or a formula field based on revenue — your choice).

**T009. Create custom fields on Opportunity**
Add 3 custom fields to Opportunity: "Product Line" (picklist), "Case Pack Size" (number), "Contract Term" (picklist: Monthly/Quarterly/Annual).

**T010. Build a custom object: Retail Promotion**
Create a new custom object called "Retail Promotion" with a lookup relationship to both Account and Opportunity. This teaches custom object creation and relationship building, which is core admin work beyond just using standard objects.

**T011. Set up Record Types for Opportunity**
Create 3 Record Types on Opportunity: "New Business", "Renewal", "Promotion". Record Types let you show different page layouts/fields depending on what kind of deal it is.

**T012. Configure Page Layouts per Record Type**
Build a distinct Page Layout for each of the 3 Opportunity Record Types, showing/hiding fields relevant to that type (e.g., "Promotion" layout shows the Retail Promotion lookup, "New Business" doesn't).

**T013. Build a Schema Builder diagram**
Open Setup → Schema Builder and visually map out all the objects and relationships you've built so far (standard + custom). Take a screenshot — this is a great portfolio artifact showing you understand data modeling.

---

## Phase 2 — Lead Capture & Sources

**T014. Build a Web-to-Lead form**
In Setup → Web-to-Lead, generate the HTML form code for a "Contact Us" style form. This is Salesforce's native, no-code way to capture leads from a website — every submission auto-creates a Lead record.

**T015. Set up Email-to-Case + Flow to simulate Email-to-Lead**
Important context: Salesforce does NOT have a native "Email-to-Lead" feature the way it has native Email-to-Case. The real-world admin workaround is: set up Email-to-Case (native) to capture inbound emails as Cases, then build a Flow that detects a "new business inquiry" type Case and auto-creates a Lead from it. This task teaches you the actual production pattern used when a company says "can leads come from email."

**T016. Create Lead Assignment Rules**
Build assignment rules that route new Leads to the correct rep based on region (e.g., Lead State = NY/NJ/CT → Jordan Reyes). This automates what would otherwise be manual triage.

**T017. Build a Campaign and import attendee leads**
Create a Campaign called "MegaCRM Summer Beverage Expo". Manually add 5-10 sample Leads as Campaign Members to simulate trade show attendees. This teaches Campaign-based lead attribution, which ties into reporting later.

**T018. CSV bulk import using Data Import Wizard**
Use the provided `data/leads_sample.csv` file (50 sample leads — distributors and retailers) and load it using Setup → Data Import Wizard. This is the simplest native bulk-load tool, good for one-time small imports, equivalent to using Excel + Data Import Wizard in D365.

**T019. CSV bulk import using Data Loader**
Download the desktop Data Loader tool and use it to load `data/accounts_sample.csv` and `data/contacts_sample.csv` (100 combined records). Data Loader is the heavier-duty tool for larger volumes and supports insert/update/upsert/delete — this is your ETL skillset transferring directly, closest to how Matillion/ADF would push data into Salesforce via the Bulk API in a real job.

**T020. Manually create 10 sample Leads through the UI**
Create 10 Leads by hand through the standard New Lead button. Compare the experience to the bulk-loaded ones — this helps you explain in an interview why bulk tools exist (speed, consistency) versus when manual entry still makes sense (one-off, high-touch records).

---

## Phase 3 — Sales Process / Opportunity Management

**T021. Build Opportunity Stages**
Customize the Opportunity Stage picklist to: Prospecting → Qualification → Proposal → Negotiation → Closed Won / Closed Lost. Set the probability % for each stage.

**T022. Configure Path for Opportunities**
Set up the Path component (the visual horizontal stage tracker at the top of a record) for Opportunity, including key fields to show and optional guidance text per stage.

**T023. Build a Flow: auto-create a Task on stage change**
Build a Record-Triggered Flow that automatically creates a follow-up Task assigned to the Opportunity owner whenever the Opportunity moves into "Proposal" stage. This is a very common real-world automation request.

**T024. Build Quote/Product/Price Book setup**
Create a Price Book with 5-6 sample MegaCRM beverage SKUs (e.g., "MegaCola 12-pack", "MegaJuice Orange 24-pack") with prices. Add Products to an Opportunity using these Price Book entries.

**T025. Configure Opportunity-to-Order conversion**
Explore (and if available in your edition, configure) the Quote → Order process, where a won Opportunity's Quote converts into an Order record. Document the steps even if your Developer org has limited Order object access.

**T026. Set up an Approval Process**
Build an Approval Process that requires manager sign-off if a discount field on the Opportunity exceeds 15%. This teaches Approval Processes, a commonly tested admin topic.

**T027. Manually enter 5 sample Opportunities**
Create 5 Opportunities spread across different reps and different stages, so your reports in Phase 6 have realistic data to show.

---

## Phase 4 — Automation: Flow Builder

**T028. Build a Record-Triggered Flow: auto-assign Account Tier**
Build a Flow that automatically sets the "Account Tier" field (Bronze/Silver/Gold) based on an Annual Revenue field threshold, whenever an Account is created or edited.

**T029. Build a Screen Flow: New Distributor Onboarding wizard**
Build a multi-screen guided Flow that walks a rep through entering a new distributor: collects company info, creates the Account, then creates a related Contact — all in one guided wizard. This is a strong portfolio piece since Screen Flows are heavily used in real orgs.

**T030. Build a Scheduled Flow: stale Opportunity reminder**
Build a Scheduled-Triggered Flow that runs weekly and sends an email to the Opportunity owner if an Opportunity hasn't been updated in 14+ days. Teaches the Scheduled Flow type specifically (different trigger mechanism than Record-Triggered).

**T031. Note on legacy automation tools (no build required)**
Process Builder and Workflow Rules are both deprecated/retired by Salesforce — Flow Builder fully replaces them. You do not need to build anything in these legacy tools; this task is just to document that you know they exist and why they're no longer used (this is a real exam topic).

**T032. Build a Flow with a subflow**
Build a small reusable subflow (e.g., "Send Notification Email") and call it from inside one of your earlier Flows. This teaches modular Flow design, which is how complex automation is built in real orgs to avoid duplicating logic.

**T033. Test and debug a Flow**
Use Flow Builder's built-in Debug feature to step through one of your Flows and view exactly which path it took and why. This is the real-world skill of troubleshooting automation that isn't behaving as expected.

---

## Phase 5 — Security & Access

**T034. Configure Profiles vs Permission Sets**
Create a Permission Set called "Promotion Manager" that grants extra access to the Retail Promotion object, and assign it to one rep on top of their base Standard User profile. This teaches the real-world pattern: Profile = baseline access, Permission Set = additive extra access.

**T035. Set up Role Hierarchy**
Build a simple Role Hierarchy: the 4 reps report up to a "Regional Sales Manager" role, which reports up to "VP of Sales". This controls record visibility (a manager can see their reports' records by default).

**T036. Configure Sharing Rules**
Set Organization-Wide Default (OWD) for Account to Private, then build a Sharing Rule that gives all 4 reps read-only visibility into each other's Accounts (cross-region visibility), even though OWD is private.

**T037. Set up Field-Level Security**
Hide the "Contract Term" field on Opportunity from a specific Profile (e.g., a hypothetical "Junior Rep" profile), so it's invisible regardless of page layout settings. Teaches the difference between page layout visibility and true field-level security.

**T038. Configure login/session security settings**
Explore Setup → Session Settings and Network Access (trusted IP ranges). Document what these control even if you don't lock down your own dev org access.

---

## Phase 6 — Reports, Dashboards & Data Quality

**T039. Build a Report: Open Opportunities by Rep and Stage**
Build a Summary Report grouped by Owner, then by Stage, showing count and total Amount. This is the most common "give me a pipeline view" report request in a real job.

**T040. Build a Report: Lead Source effectiveness**
Build a report comparing Lead counts grouped by Lead Source (Web, Email/Case-converted, Campaign, CSV-imported manual tag if you added one). Helps you practice grouping/summarizing by source, a common sales-ops question.

**T041. Build a Dashboard: MegaCRM Sales Overview**
Build a Dashboard with 4 components (e.g., a bar chart of Opportunities by stage, a gauge of total pipeline vs. goal, a table of top 5 Accounts by revenue, a pie chart of Leads by source) filtered by region.

**T042. Configure Duplicate Rules and Matching Rules**
Set up a Matching Rule + Duplicate Rule on Account (matching by Name + similar address) to block or warn on duplicate distributor records. This is now one of the most heavily tested topics on the current exam (Data & Analytics is the top-weighted section).

**T043. Build Validation Rules**
Create a Validation Rule that prevents an Opportunity from being marked Closed Won unless the "Distribution Channel" field on the related Account is populated. Validation Rules are a near-guaranteed exam topic.

**T044. Run a Data Quality audit and merge duplicates**
Use the provided `data/duplicate_accounts.csv` (5 intentionally duplicated Account records) — load them in, then use Salesforce's native "Merge Accounts" tool to identify and merge the duplicates. Document which fields you kept from which record and why.

**T045. Schedule a Report to email automatically**
Set up a Report Subscription so one of your Phase 6 reports automatically emails the regional reps every Monday morning. Teaches the "set it and forget it" reporting pattern real sales ops teams rely on.

**T046. Build a Matrix Report comparing reps to product lines**
Build a Matrix Report (groups by both rows AND columns) showing Closed Won Opportunity totals with reps as rows and Product Line as columns. Matrix reports are explicitly called out as ideal for this kind of two-dimensional comparison and are a distinct, testable report format from Summary reports.

**T047. (Optional, Advanced) Build a Joined Report with a cross-block formula and a Cross Filter**
This is the most complex native reporting feature in Salesforce and is optional — strong for a portfolio/interview story but not required to finish the core project. Build a Joined Report with two blocks: Block 1 = Opportunities (filtered to a specific stage), Block 2 = Cases (filtered to High priority), both blocks sharing the Account relationship. Add a cross-block summary formula that divides the Opportunity count by the High-Priority Case count (a rough "deal risk per account" metric). Then add a Cross Filter to a separate report showing "Accounts WITHOUT any Opportunity in the last 90 days" — a common exception/hygiene report pattern. Note real platform limits while you build this: joined reports allow up to 10 summary formulas per block and 50 total, and cross filters are unavailable on Professional Edition orgs (not a concern for your free Developer org, but worth knowing for interviews).

---

## Phase 7 — Service Cloud

**T048. Set up Case object basics**
Review and configure Case Origin (Email/Web/Phone) and Case Status picklists, and create a Case Queue called "MegaCRM Support Queue" that unassigned Cases route into.

**T049. Build Case Assignment Rules and Auto-Response Rules**
Build a Case Assignment Rule that routes Cases by Origin (e.g., Web cases go to the Support Queue, Email cases route to a specific rep). Build an Auto-Response Rule that sends an automatic "we got your message" email to the Case contact.

**T050. Set up a basic Knowledge Base article**
Enable Salesforce Knowledge (if available in your edition) and create one sample article: "How do I check my distributor order status?" Document the steps even if full Knowledge isn't available on Developer Edition.

**T051. (Optional) Explore Service Cloud Voice**
Service Cloud Voice is Salesforce's native built-in phone/CTI system — it lets calls ring directly inside Salesforce, auto-pop the matching Contact record, and use Einstein for call transcription/AI summaries. It is NOT available to configure hands-on in a free Developer org (it requires telephony provisioning and additional licensing), so this task is research/documentation only: read the setup requirements and write a short summary of how it would work for MegaCRM's support team. Good talking point for interviews about "AI covering missed calls," similar to the D365 Contact Center conversation.

---

## Phase 8 — Experience Cloud

**T052. Build a basic Experience Cloud partner portal**
Create an Experience Cloud site (use a template like "Customer Account Portal") so an external distributor contact could log in and view their Order/Opportunity status. This is a stretch but valuable admin skill — portals are a common ask once a company scales.

**T053. Configure portal user access**
Set up one Contact record as an External User with portal login access, and build one simple branded page showing their related records.

---

## Phase 9 — Apex & LWC Basics

**T054. Write a simple Apex trigger**
Write a basic Apex trigger on Account that automatically updates the "Account Tier" field when Annual Revenue changes (the declarative version of this was built in T028 via Flow — this task teaches the code-based equivalent, which is what the JD's "Apex knowledge" line is testing for).

**T055. Write a basic Apex test class**
Write a simple test class covering the trigger from T054. Salesforce requires at least 75% code coverage to deploy Apex to production, so test classes are mandatory, not optional, in real orgs.

**T056. Build a simple Lightning Web Component**
Build a basic LWC that displays a "Top 5 Open Opportunities" list using sample data or a simple Apex controller. This is the JavaScript-based modern UI framework referenced by the "LWC knowledge" line in the JD you found.

**T057. Deploy the LWC to a Lightning App Page**
Add your new LWC component to a Lightning App Page (e.g., the Home page) so it's visible to users, not just sitting unused in the metadata.

---

## Phase 10 — Smart/AI Layer

**T058. Explore Agentforce setup**
Enable Agentforce in your Developer org (free to explore) and review the default agent templates available. Agentforce AI is a brand-new topic added to the Salesforce Admin certification exam as of the December 2025 refresh, so hands-on familiarity here directly helps with exam prep, not just the build.

**T059. Build a simple Agentforce action**
Configure a simple Agentforce action that can summarize recent activity on an Account (e.g., "summarize this account's last 3 interactions") using your existing Account/Opportunity data.

**T060. Set up Einstein Activity Capture**
Connect Einstein Activity Capture to auto-log emails and meetings from Outlook or Gmail directly into the Account/Contact timeline. This is the closest Salesforce-native equivalent to what Copilot for Sales does in your D365CRM project — good comparison point for interviews.

---

## Phase 11 — DevOps Basics

**T061. Connect your org to Git**
Set up a connection between your Salesforce org and a Git repo (using either VS Code + Salesforce Extensions, or Salesforce CLI) and push your metadata (Flows, Objects, Layouts) into version control. This is the foundation all modern Salesforce DevOps tooling sits on top of.

**T062. (Optional) Document a Copado/Gearset-style deployment plan**
Copado and Gearset are paid third-party CI/CD tools for Salesforce that are not available to provision in a free Developer org. Rather than building anything hands-on, write a short deployment plan describing how changes would flow from a Sandbox to Production using a tool like this (the stages: develop in sandbox → validate → promote → deploy → rollback plan). This shows JD-relevant knowledge ("Copado, Git, DevOps knowledge") without requiring a paid subscription.

**T063. Build a Change Set**
Build a native Change Set (Salesforce's free, built-in alternative to Copado) and document the steps to deploy it from one org to another. This is the hands-on equivalent of T062, fully buildable for free.

---

## Phase 12 — Certification Alignment / Exam-Style Review

**T064. Concept check: Profiles vs Permission Sets vs Roles**
Write a short scenario-based explanation (no building required) of when you'd use a Profile change vs. a Permission Set vs. a Role change to solve an access problem. This mirrors the judgment-call style of real exam questions.

**T065. Concept check: Sharing Rules vs Role Hierarchy vs OWD**
Explain how these 3 security layers interact and override each other (OWD sets the baseline, Role Hierarchy and Sharing Rules open up additional access on top). This is one of the most commonly missed exam concepts.

**T066. Concept check: Flow types**
Write out the difference between Record-Triggered, Screen, Scheduled, and Autolaunched Flows, and give one MegaCRM example of when you'd use each (you've actually now built 3 of these 4 types in Phase 4).

**T067. Concept check: Lead conversion**
Explain exactly what happens to a Lead's fields when it's converted into an Account, Contact, and Opportunity — which fields map over, and what happens to custom fields you added in Phase 1 if they're not mapped.

**T068. Concept check: Data & Analytics judgment scenario**
Given a messy data scenario (e.g., "5,000 duplicate Leads just got imported from a trade show list"), write out your decision process: would you use a Duplicate Rule, a Validation Rule, a Flow, or a manual merge — and why, in what order.

**T069. Take a full practice exam**
Find a practice exam matching the real format (60 questions, 105 minutes, 65% to pass) and take it under timed conditions. Log your score and specific gap areas before scheduling the real $200 exam.

---

## Summary

**Total: 69 tasks across 13 phases (Phase 0–12)**
**3 tasks marked (Optional):** T047 (Joined Report/Cross Filter), T051 (Service Cloud Voice), T062 (Copado deployment plan doc)

All optional tasks are valuable for interview talking points but are not required to complete the core project or be ready for the certification exam.
