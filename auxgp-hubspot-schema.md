# AUXGP HubSpot Schema (Agency Mode)
**Purpose:** Build a clean, scalable HubSpot contact import structure optimized for outbound prospecting, client acquisition, segmentation, and reporting.

---

# Philosophy

Every field must support one of these functions:

- **Sell** → improve outreach / conversion  
- **Segment** → sort leads intelligently  
- **Score** → identify best opportunities  
- **Route** → assign owners / workflows  
- **Report** → measure pipeline performance  

If a field does none of these, remove it.

---

# Recommended Import Schema (v1 Lean + Powerful)

| Column Name | Type | Source | Purpose | Priority |
|---|---|---|---|---|
| First Name | Contact | Seamless | Personalization | Critical |
| Last Name | Contact | Seamless | Identity | Critical |
| Email Address | Contact | Seamless | Primary communication | Critical |
| Phone Number | Contact | Seamless | Direct outreach | High |
| LinkedIn Profile URL | Contact | Seamless | Research / social selling | High |
| Job Title | Contact | Seamless | ICP targeting | High |
| Department | Contact | Seamless | Routing / relevance | Medium |
| Seniority | Contact | Seamless | Decision-maker filtering | High |
| Company Name | Company | Seamless | Account association | Critical |
| Website | Company | Seamless | Research / enrichment | High |
| Industry | Company | Seamless | Segmentation | High |
| Employee Count | Company | Seamless | ICP size fit | High |
| Revenue Range | Company | Seamless | Qualification | Medium |
| City | Geo | Seamless | Territory targeting | Medium |
| State | Geo | Seamless | Territory routing | Medium |
| Country | Geo | Seamless | Global segmentation | High |
| Lifecycle Stage | CRM | Static | lead / MQL / SQL | Critical |
| Lead Source | CRM | Static | Seamless AI | Critical |
| Contact Owner | CRM | Manual/Auto | Rep assignment | High |
| Import Date | CRM | Script | Audit trail | High |
| Notes | CRM | Optional | Human notes | Low |

---

# Recommended HubSpot Custom Properties

Create these custom fields in HubSpot if not existing:

## Contact Properties

- LinkedIn Profile URL
- Department
- Seniority
- Lead Source
- Import Date

## Company Properties

- Revenue Range
- Employee Count
- Industry

---

# Lifecycle Stage Logic

| Condition | Value |
|---|---|
| Fresh import | Lead |
| Opened / engaged | Marketing Qualified Lead |
| Replied / interested | Sales Qualified Lead |
| Discovery booked | Opportunity |
| Won | Customer |

---

# Contact Owner Logic

## Option A — Manual
Jayson assigns leads manually.

## Option B — Round Robin
Auto-distribute evenly across reps.

## Option C — Territory Based
Assign by country/state/industry.

---

# Lead Scoring Model (Simple)

| Signal | Score |
|---|---|
| Director+ Seniority | +25 |
| 50–500 Employees | +20 |
| Valid LinkedIn | +10 |
| Company Website Exists | +10 |
| U.S. Based | +15 |
| Revenue Range Mid+ | +20 |

**80+ = Priority Lead**

---

# Suggested Sheet Tabs

```text
SeamlessAI_Data_Raw
HubSpot_Contacts_Data
Mapping_Config
Logs
Dashboard
````

---

# Example Final Import Columns

```text
First Name
Last Name
Email Address
Phone Number
LinkedIn Profile URL
Job Title
Department
Seniority
Company Name
Website
Industry
Employee Count
Revenue Range
City
State
Country
Lifecycle Stage
Lead Source
Contact Owner
Import Date
```

---

# What to Avoid

## Do NOT import:

* Empty columns
* Duplicate phone fields
* Fake AI confidence columns unless useful
* Random placeholder fields
* Raw messy exports directly into HubSpot

---

# Recommended GAS Automation Flow

```text
Paste Seamless CSV
↓
Normalize Fields
↓
Remove Duplicates
↓
Validate Email
↓
Create HubSpot Import Sheet
↓
Upload to HubSpot
↓
Track Results
```

---

# AUXGP Strategic Advantage

Most agencies blast lists.

AUXGP can build a **clean prospect intelligence engine**.

That means:

* Better deliverability
* Better targeting
* Better close rates
* Better reporting
* Better scale

---

# Phase 2 Expansion

* Apollo / Clay enrichment
* AI personalization lines
* Sequence sync
* Closed-loop ROI dashboard
* Client acquisition pipeline analytics

---
