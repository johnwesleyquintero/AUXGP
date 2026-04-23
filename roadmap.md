# AUXGP — Roadmap

## Overview

AUXGP is a lightweight RevOps automation system that transforms raw lead data from SeamlessAI into clean, structured, HubSpot-ready contacts using Google Sheets + Google Apps Script (GAS).

The goal is simple:

**Reduce manual CRM work and turn messy lead data into usable sales intelligence.**

This roadmap focuses on stability first, then control, then automation, and finally intelligence.

---

## Guiding Principle

> Stability first. Visibility second. Automation last.

We do not scale chaos. We stabilize before adding complexity.

---

# Phase 1 — STABILITY (CURRENT)

## Goal
Ensure reliable transformation from raw SeamlessAI data → clean HubSpot-ready contacts.

## Scope
- SeamlessAI_Data_Raw ingestion
- GAS normalization script (v3)
- Duplicate email filtering
- Basic field mapping (Name, Email, Phone, LinkedIn, Company)
- HubSpot_Contacts_Data output validation

## Success Criteria
- 100% consistent output format
- No duplicate emails in output
- No broken or missing required fields (email required)
- Manual run produces predictable results every time

---

# Phase 2 — OPS CONTROL (NEXT)

## Goal
Gain visibility and traceability of all imports.

## Scope
- Add Batch ID per import run
- Logs tab improvements (success/failure tracking)
- Count metrics per run (processed / skipped / duplicates)
- Basic dashboard in Google Sheets
- Run history tracking

## Success Criteria
- Every dataset import is traceable
- Errors can be debugged by batch
- Clear visibility of system health

---

# Phase 3 — AUTOMATION

## Goal
Reduce manual execution of imports.

## Scope
- Time-based GAS triggers (scheduled runs)
- Optional auto-processing on data update
- One-click import pipeline via sidebar UI
- Basic workflow automation for HubSpot sync preparation

## Success Criteria
- System can run without manual triggering (optional mode)
- Reduced operator dependency
- Repeatable scheduled ingestion

---

# Phase 4 — INTELLIGENCE LAYER

## Goal
Improve lead quality and prioritization.

## Scope
- Lead scoring (seniority, company size, LinkedIn presence)
- ICP tagging (ideal customer fit)
- Data enrichment flags (missing fields detection)
- Priority segmentation (hot / warm / cold leads)

## Success Criteria
- Users can prioritize outreach directly from output sheet
- High-value leads are clearly identified
- Reduced time spent on low-quality contacts

---

# Phase 5 — SCALING (FUTURE)

## Goal
Move from internal tool → scalable RevOps system.

## Scope
- Multi-client support
- API-based ingestion (beyond sheets)
- Database upgrade (optional: Supabase / BigQuery)
- Full HubSpot sync automation
- Reporting dashboard (Looker Studio / custom UI)

## Success Criteria
- AUXGP usable across multiple teams
- Minimal manual intervention
- Stable performance at higher data volume

---

# Current System Stack

- Google Sheets → staging database
- Google Apps Script (GAS) → transformation engine
- HubSpot → CRM destination layer
- SeamlessAI → lead source
- Sidebar UI → operator control panel

---

# Non-Goals (Important)

- No complex CRM replacement
- No over-engineered database migration (for now)
- No multi-system integrations until stability is proven
- No feature expansion without weekly review approval

---

# Weekly Improvement Cycle

Every week:

1. Review system stability
2. Identify friction points
3. Apply only 1–2 controlled improvements
4. Validate with real dataset
5. Deploy and observe

---

# Strategic Intent

AUXGP is not a tool project.

It is a **revenue data pipeline system**.

Its purpose is to ensure:

- clean lead flow
- minimal manual cleanup
- reliable CRM ingestion
- scalable outbound readiness
