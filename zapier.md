## **PHASE 1 MANUAL STAGING EXECUTION PLAN**

### **Stage 1: Pre-Test Checkpoint**

**You will do (before triggering Zap):**

1. ✅ Identify 20-50 real contacts from `HubSpot_Contacts_Data` (post-GAS normalization)
2. ✅ Include realistic distribution:
   - 60% clean qualified (email + company + all fields)
   - 20% missing email (company present → should reject)
   - 10% missing company (email present → should reject)
   - 5% duplicate emails from SeamlessAI ingestion
   - 5% edge cases (special characters, very long names, etc.)
3. ✅ Take screenshot of before state:
   - HubSpot contact count (baseline)
   - Logs sheet row count (baseline)
   - No synced contacts yet

**Zap status:** DRAFT (ready but not published)

---

### **Stage 2: Execute Batch Test**

**You will do:**

1. **Add test rows to HubSpot_Contacts_Data** (pick real rows from your GAS output)
   - Don't create synthetic rows—use actual normalized contacts from your pipeline
   - Add them all at once or in small waves (10-15 at a time)

2. **Manual trigger:** Update any row in HubSpot_Contacts_Data to trigger the Zap
   - OR: If Zap isn't published, manually test from editor (Test Step 1 each time)

3. **Wait 2-5 minutes** for Zap to process all rows

**Zap will do automatically:**
- Path A: Sync qualified contacts to HubSpot + create tasks
- Path B: Log rejected contacts to Logs sheet

---

### **Stage 3: Observe & Document**

**After Zap runs, check these 4 data sources:**

#### **OBSERVATION 1: HubSpot Contacts**
```
Location: HubSpot → Contacts (search by name/email)
Check for:
├─ NEW Contacts created (should match Path A qualified count)
├─ Updated Contacts (if duplicate emails, verify upsert worked)
├─ Lifecycle Stage = "lead" (all synced contacts)
├─ All 7 fields populated: firstname, lastname, email, phone, jobtitle, company, website
└─ NO duplicate contacts (same email should not create 2 records)

OBSERVATION TEMPLATE:
- Total contacts created: ____ (expected: ~60% of test batch)
- Total contacts updated: ____ (expected: ~5% for duplicates)
- Total contacts with errors: ____ (expected: 0)
- Spot check (pick 3 contacts):
  ├─ Contact 1: Name _____, Email _____, Company _____, Lifecycle: _____
  ├─ Contact 2: ...
  └─ Contact 3: ...
```

#### **OBSERVATION 2: HubSpot Tasks**
```
Location: HubSpot → Activities → Tasks (filter by owner wesley.ecomva@gmail.com)
Check for:
├─ Task count (should = Path A qualified count)
├─ Subject format: "LinkedIn Outreach: [Name] @ [Company]"
├─ Body contains: contact details + "Review LinkedIn profile..."
├─ Status: NOT_STARTED
├─ Owner: wesley.ecomva@gmail.com
└─ IF duplicate email → observe task stacking (expected: new task created, not updated)

OBSERVATION TEMPLATE:
- Total tasks created: ____ (expected: same as synced contacts)
- Task subject examples:
  ├─ "LinkedIn Outreach: _____ @ _____"
  ├─ "LinkedIn Outreach: _____ @ _____"
  └─ "LinkedIn Outreach: _____ @ _____"
- Body preview: [Copy first 100 chars]
- Tasks assigned to duplicate emails (stack or replace?): ____
```

#### **OBSERVATION 3: Logs Sheet (Rejections)**
```
Location: Google Sheets → Logs tab
Check for:
├─ Row count added (should = ~30% of test batch)
├─ Each row has: Timestamp, Name, "Not Qualified", Reason
├─ Reason field = "Missing email or company data"
├─ Timestamps match Zap execution time
└─ Names match rejected contacts from HubSpot_Contacts_Data

OBSERVATION TEMPLATE:
- Total rejection logs: ____ (expected: ~30% of test batch)
- Log examples:
  ├─ Row 1: Timestamp _____, Name _____, Reason _____
  ├─ Row 2: ...
  └─ Row 3: ...
- All rejected contacts ARE missing email OR company? YES / NO
```

#### **OBSERVATION 4: Zap Execution History**
```
Location: Zap Editor → History tab (when published, else use Manual Test)
Check for:
├─ Total runs: ____ (should = 1 if batch added at once, or N if waves)
├─ Success rate: ____% (should be 100%)
├─ Failed runs: ____ (expected: 0)
├─ Error messages: [Any red flags?]
└─ Timing: Avg execution time per contact ____ms

OBSERVATION TEMPLATE:
- Run status: SUCCESS / PARTIAL_SUCCESS / FAILED
- Total tasks used: ____ (each sync = 1 task, each rejection log = 1 task)
- Any errors? YES / NO
  If YES: [Copy error message]
- Any skipped contacts? YES / NO
  If YES: [How many? Why?]
```

---

### **Stage 4: Validate Against Expectations**

**Fill this comparison table:**

| Metric | Expected | Observed | Match? | Notes |
|---|---|---|---|---|
| Qualified contacts synced (60% of batch) | ~__ | __ | ✅/❌ | |
| Rejected contacts logged (30% of batch) | ~__ | __ | ✅/❌ | |
| HubSpot tasks created | = synced count | __ | ✅/❌ | |
| Email deduplication (upsert on duplicate) | 1 contact per email | __ | ✅/❌ | |
| Task duplication (on re-run same email) | Stack (new task) | __ | ✅/❌ | |
| All fields populated | firstname, lastname, email, phone, title, company, website | ✅/❌ | ✅/❌ | Missing fields: ____ |
| Lifecycle stage | "lead" on all | ✅/❌ | ✅/❌ | |
| Rejection logs accurate | All missing email OR company | ✅/❌ | ✅/❌ | False positives: ____ |
| Zap execution clean | 0 errors | __ errors | ✅/❌ | Error type: ____ |

---

### **Stage 5: Idempotency Test (Re-run Duplicate Emails)**

**After observing Stage 4, run TEST 2 from the runbook:**

1. **Re-add the duplicate email contact** (same email, different name/company)
   - Example: If Row 1 was "Mahmoud Arram / mahmoud@octogen.io"
   - Add: "Mohammad Arram / mahmoud@octogen.io" (same email, different first name)

2. **Wait 2-5 min for Zap to process**

3. **Observe:**
   ```
   HubSpot Contacts:
   ├─ Should still be 1 contact (upsert merged on email)
   ├─ First name: [Original or Updated?]
   ├─ Email: mahmoud@octogen.io (same)
   
   HubSpot Tasks:
   ├─ Task count: [Increased by 1, or same?]
   ├─ Multiple tasks on same contact? YES / NO
   
   IDEMPOTENCY RESULT: ✅ PASS / ❌ FAIL
   ```

**Expected:** Contact updates, task stacks (new task created for manual outreach workflow)

---

### **Stage 6: Edge Case Observations**

**Test against real SeamlessAI patterns you've seen:**

```
Document any anomalies:
├─ Special characters in names: [handled? YES/NO]
├─ Very long company names: [truncated? YES/NO]
├─ Email formatting edge cases: [accepted? YES/NO]
├─ Phone number formats: [preserved? YES/NO]
├─ Website URLs: [valid? YES/NO]
└─ Any data loss or transformation? [describe]
```

---

## **PHASE 1 VALIDATION RUNBOOK TEMPLATE**

Save this and fill during your batch test:

```
═════════════════════════════════════════════════════════════════
AUXGP PHASE 1 BATCH TEST - MANUAL STAGING RESULTS
═════════════════════════════════════════════════════════════════

TEST DATE: ________________
TEST BATCH SIZE: ____ contacts
DISTRIBUTION: __%qualified, __%missing_email, __%missing_company, __%duplicates, __%edge_cases

─────────────────────────────────────────────────────────────────
OBSERVATION 1: HubSpot Contacts
─────────────────────────────────────────────────────────────────
Baseline contact count (before): ____
Final contact count (after): ____
Contacts created: ____
Contacts updated (duplicates): ____
Expected qualified: ~___ | Actual: ___
Result: ✅ PASS / ❌ FAIL
Notes: _________________________________________________

─────────────────────────────────────────────────────────────────
OBSERVATION 2: HubSpot Tasks
─────────────────────────────────────────────────────────────────
Tasks created: ____
Expected count: ~___ | Actual: ___
Sample task subject: "LinkedIn Outreach: _____ @ _____"
Task owner: wesley.ecomva@gmail.com
Status: NOT_STARTED
Duplicate email task stacking: YES / NO
Result: ✅ PASS / ❌ FAIL
Notes: _________________________________________________

─────────────────────────────────────────────────────────────────
OBSERVATION 3: Logs Sheet (Rejections)
─────────────────────────────────────────────────────────────────
Baseline row count (before): ____
Final row count (after): ____
Rows added: ____
Expected rejects: ~___ | Actual: ___
All reasons accurate: YES / NO
Sample rejection log:
  Name: _____, Reason: _____
Result: ✅ PASS / ❌ FAIL
Notes: _________________________________________________

─────────────────────────────────────────────────────────────────
OBSERVATION 4: Zap Execution
─────────────────────────────────────────────────────────────────
Total runs executed: ____
Total tasks used: ____
Success rate: ____%
Errors encountered: YES / NO
Error details: _________________________________________________
Result: ✅ PASS / ❌ FAIL
Notes: _________________________________________________

─────────────────────────────────────────────────────────────────
IDEMPOTENCY TEST (Re-run Duplicate Emails)
─────────────────────────────────────────────────────────────────
Contact records after re-run: ____ (should stay 1 per email)
Task records after re-run: ____ (should increase by 1)
Email dedup working: YES / NO
Task stacking working: YES / NO
Result: ✅ PASS / ❌ FAIL
Notes: _________________________________________________

─────────────────────────────────────────────────────────────────
EDGE CASE OBSERVATIONS
─────────────────────────────────────────────────────────────────
Special characters handled: YES / NO
Long company names: __________ (preserved/truncated?)
Email validation: __________ (any rejected?)
Phone formats: __________ (any reformatted?)
Data loss: YES / NO (describe if yes)

─────────────────────────────────────────────────────────────────
OVERALL PHASE 1 VALIDATION RESULT
─────────────────────────────────────────────────────────────────
✅ READY FOR STAGING PUBLICATION  /  ⚠️ NEEDS FIXES  /  ❌ BLOCKED

Blocking issues (if any): ____________________________________
Recommended next steps: ________________________________________

Signed: ________________ | Date: ________________
```

---

## **NEXT: WAITING FOR YOUR BATCH TEST**

Once you **complete the batch test** with real GAS data, provide:

1. ✅ **Filled runbook** (from template above)
2. ✅ **Screenshots** (HubSpot contacts, tasks, Logs sheet)
3. ✅ **Zap execution history** (any errors?)
4. ✅ **Observations** on divergence from expected behavior

Then I'll:
- 🔍 Analyze actual vs expected behavior
- 📋 Document any system adjustments needed
- ✅ Give final go/no-go for staging publication
- 🚀 Prepare for controlled activation with monitoring

---

## **KEY PRINCIPLE FOR THIS TEST**

**This is NOT about perfect test data—it's about REAL data validation.**

Your GAS output includes:
- Natural duplicates from SeamlessAI ingestion
- Real missing fields (not just email/company, but others too)
- Actual edge cases from your data sources
- True quality distribution

This is where you'll discover if the Zap behaves correctly under real conditions.
