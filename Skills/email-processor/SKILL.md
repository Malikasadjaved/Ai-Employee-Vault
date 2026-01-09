---
name: email-processor
description: "Process incoming email requests and generate appropriate responses with human approval. Use this skill when files in /Needs_Action have 'type: email' in frontmatter, or when emails contain action keywords like 'invoice', 'payment', 'quote', 'proposal'. Automates email analysis, response drafting, and approval workflows according to company handbook rules."
---

# Email Processor

## Overview

Process emails requiring action by analyzing sender information, extracting key details, applying company policies, and drafting appropriate responses for human approval.

## Workflow

Email processing follows the Standard Email Workflow from the Company Handbook:

1. **Analyze email** (watcher creates file in /Needs_Action)
2. **Extract information** and determine response type
3. **Check handbook rules** for approval thresholds and templates
4. **Draft response** following company standards
5. **Create plan** in /Plans for human approval
6. **Move to /Pending_Approval** when draft is ready
7. **Human reviews** and approves or requests changes
8. **Send via MCP** after approval
9. **Log action** and move to /Done

## Step 1: Read Email File

Read the email file from /Needs_Action folder. These files have frontmatter containing:

```yaml
---
type: email
from: sender@example.com
subject: Email Subject
received: 2026-01-08T14:30:00Z
priority: high/normal/low
status: pending
---
```

Extract the email body content below the frontmatter.

## Step 2: Extract Key Information

Analyze and extract:

- **Sender name and email address**
- **Subject line**
- **Request type** (invoice, information, support, payment, quote, proposal)
- **Urgency level** (check for alert keywords: urgent, asap, emergency, critical)
- **Action required** (approve, review, sign, confirm, verify)
- **Known vs new contact** (check if sender has been contacted before)
- **Financial implications** (any monetary amounts mentioned)

## Step 3: Check Company Handbook Rules

Read `Company_Handbook.md` in the vault root to determine:

### Approval Thresholds

Check the "Approval Thresholds" table:
- **Known contacts, routine responses** → Auto-approve eligible
- **New contacts, sensitive topics** → Require approval
- **Financial content** → Always require approval

### Alert Keywords

Flag messages containing:
- **Urgent indicators:** urgent, asap, emergency, critical, immediately
- **Financial indicators:** invoice, payment, paid, overdue, refund, charge
- **Action required:** approve, review, sign, confirm, verify
- **Problem indicators:** error, issue, problem, broken, not working, failed

### Communication Standards

- Be polite and professional
- Use clear, concise language
- Respond to urgent messages within 4 hours
- Apply appropriate response template

## Step 4: Select Response Template

Based on request type, select the appropriate template from the handbook:

**Invoice Request:** Use "Email Reply - Invoice Request" template
- Include invoice details (amount, due date, reference number)
- Attach invoice file
- Professional closing

**General Response:** Create professional response following handbook tone guidelines

See `references/email_templates.md` for additional examples.

## Step 5: Draft Response

Create response email using selected template. Include:

- Professional greeting using sender's name
- Clear acknowledgment of their request
- Specific information or actions (invoice details, answers, next steps)
- Professional closing with your name
- Any necessary attachments noted

**Tone:** Professional but friendly (per handbook preference)

## Step 6: Create Processing Plan

Create a detailed plan file in `/Plans` folder with this structure:

```markdown
# Processing Plan: [Subject Line]

**Created:** [Date]
**Item:** [Tracking file name]
**Priority:** [HIGH/Normal]
**Approval Required:** [YES/NO]

---

## Item Details

**From:** [Sender email]
**Subject:** [Subject line]
**Client:** [Sender name and company]
**Received:** [Timestamp]
**Deadline:** [If mentioned]

---

## Handbook Compliance Check

**Safety First:** [✅/⚠️] [Reasoning]
**Alert Keywords Detected:** [List any found]
**Communication Standards:** [Known/new contact analysis]
**Approval Threshold:** [Determination and reasoning]
**Response Timeline:** [Urgency assessment]

---

## Email Content Summary

[1-2 sentence summary of what sender wants]

---

## Processing Steps (Standard Email Workflow)

### Phase 1: Analysis ✅ COMPLETED
- [x] Email detected and flagged
- [x] Action file created
- [x] Verified contact status
- [x] Checked relevant logs

### Phase 2: Draft Response ✅ COMPLETED
- [x] Response email drafted
- [x] Template applied

### Phase 3: Human Approval ⏳ PENDING
- [ ] **REQUIRES HUMAN REVIEW**
- [ ] Verify response accuracy
- [ ] Review tone and professionalism
- [ ] Approve or request modifications

### Phase 4: Send Response
- [ ] Send email
- [ ] Set follow-up reminders if needed

### Phase 5: Documentation
- [ ] Log sent email
- [ ] Move to /Done
- [ ] Update dashboard

---

## Draft Response Email

[Insert drafted email here using template]

---

## Required Approvals

[List what needs verification before sending]

---

## Risk Assessment

**Risk Level:** [Low/Medium/High]
[Explanation of risk factors]

---

## Next Action

**Status:** [PENDING HUMAN APPROVAL / Ready to execute]
**Assigned to:** [Human review required / AI Employee]
**Action Required:** [Specific next step]
**Timeline:** [Response deadline]

---

## Handbook Reference

[List relevant handbook sections referenced]
```

## Step 7: Determine Approval Status

Based on handbook rules:

**Requires Approval:**
- New contacts (any topic)
- Known contacts with financial content
- Sensitive topics
- Any uncertainty about appropriateness

**Auto-Approve Eligible:**
- Known contacts
- Routine responses
- No financial implications
- Clear guidelines apply

**Always create a plan file for human review** even if auto-approve eligible. Mark status clearly.

## Step 8: Ready for Next Phase

After creating the plan:

1. **If approval required:** Notify human and wait for review
2. **If auto-approve eligible:** Note this in plan, but still wait for confirmation
3. **Save tracking file location** for later cleanup
4. **Update Dashboard** if urgent or high priority

## Resources

### references/

- `email_templates.md` - Additional email response templates and examples
- Link to `Company_Handbook.md` in vault root for policies and standards

---

## Notes

- **Never send emails without approval** - Safety first principle
- **Always log actions** - Maintain audit trail
- **Flag unusual patterns** - Multiple emails from same sender, suspicious requests
- **Preserve professionalism** - Follow handbook communication standards
- **Track financial mentions** - Any amounts over $50 need extra review
