# 🥉 Bronze Tier Setup Guide

## Overview
This guide will help you complete the Bronze tier deliverables for the Personal AI Employee Hackathon.

**Bronze Tier Requirements:**
- ✅ Obsidian vault with Dashboard.md and Company_Handbook.md (DONE!)
- ⏳ One working Watcher script
- ⏳ Claude Code reading/writing to vault
- ✅ Basic folder structure (DONE!)
- ⏳ AI functionality as Agent Skills

---

## Step 1: Install Python Dependencies

```bash
# Navigate to your project directory
cd /path/to/your/project

# Install required packages
pip install watchdog python-dotenv
```

Or use the requirements.txt:
```bash
pip install -r requirements.txt
```

---

## Step 2: Configure Environment

Create a `.env` file in your project directory:

```bash
# .env file
VAULT_PATH=D:/AI_Employee_Vault
DROP_FOLDER=D:/AI_Employee_Drop
```

**Important:** Adjust the paths to match your actual Obsidian vault location!

---

## Step 3: Run the File Watcher

```bash
# Run the watcher
python file_watcher.py
```

You should see:
```
============================================================
🤖 AI Employee File Watcher v0.1
============================================================
Drop files into: D:/AI_Employee_Drop
Vault location: D:/AI_Employee_Vault
Checking every 5 seconds
Press Ctrl+C to stop
============================================================
```

---

## Step 4: Test the Watcher

1. **Create the drop folder** (if it doesn't exist):
   - Windows: `D:\AI_Employee_Drop`
   - Mac/Linux: Adjust path in .env

2. **Drop a test file:**
   - Copy any PDF, image, or document into the drop folder
   - Watch the terminal output

3. **Check your vault:**
   - Open Obsidian
   - Navigate to `Needs_Action/` folder
   - You should see a new file like `FILE_20260108_143022.md`

4. **Check the Inbox:**
   - The original file is copied to `Inbox/` with timestamp

---

## Step 5: Configure Claude Code

### Option A: Direct Command

```bash
# Navigate to your vault
cd D:/AI_Employee_Vault

# Run Claude Code to process pending actions
claude code "Read all files in the Needs_Action folder and create a processing plan for each item in the Plans folder. Follow the rules in Company_Handbook.md"
```

### Option B: Create Helper Script

Create `process_actions.sh` (Mac/Linux) or `process_actions.bat` (Windows):

**Windows (process_actions.bat):**
```batch
@echo off
cd D:\AI_Employee_Vault
claude code "Read all files in Needs_Action folder. For each item: 1) Analyze the content, 2) Create a detailed plan in Plans folder with filename PLAN_[timestamp].md, 3) Follow Company_Handbook rules. Be specific about next steps."
```

**Mac/Linux (process_actions.sh):**
```bash
#!/bin/bash
cd /path/to/AI_Employee_Vault
claude code "Read all files in Needs_Action folder. For each item: 1) Analyze the content, 2) Create a detailed plan in Plans folder with filename PLAN_[timestamp].md, 3) Follow Company_Handbook rules. Be specific about next steps."
```

Make it executable:
```bash
chmod +x process_actions.sh
```

---

## Step 6: Create Your First Agent Skill

Agent Skills allow you to encapsulate AI functionality as reusable modules.

### Create Email Processor Skill

1. **Create skill directory structure:**
```bash
# In your vault or separate skills directory
mkdir -p Skills/email-processor
```

2. **Create SKILL.md:**

```markdown
# Email Processor Skill

## Purpose
Process incoming email requests and generate appropriate responses with human approval.

## Trigger Conditions
Activate when:
- File in /Needs_Action with `type: email` in frontmatter
- Email contains action keywords: "invoice", "payment", "quote", "proposal"

## Processing Steps

1. **Read the email file** from /Needs_Action
2. **Extract key information:**
   - Sender name and email
   - Subject line
   - Request type (invoice, information, support)
   - Urgency level

3. **Check Company_Handbook.md** for relevant rules:
   - Communication standards
   - Approval thresholds
   - Response templates

4. **Create a plan** in /Plans folder:
   ```markdown
   ---
   type: email_response
   original_email: [path to original]
   requires_approval: true/false
   priority: high/normal/low
   ---
   
   ## Request Summary
   [Concise summary of what sender wants]
   
   ## Proposed Response
   [Draft response using handbook templates]
   
   ## Next Steps
   - [ ] Human reviews response
   - [ ] Move to /Approved if acceptable
   - [ ] Email sent via MCP
   ```

5. **For sensitive actions**, create approval request in /Pending_Approval

## Example Usage

Input file: `/Needs_Action/EMAIL_client_request.md`
```markdown
---
type: email
from: john@clientco.com
subject: Invoice for December Work
priority: high
---

Hi, can you send me the invoice for the work completed in December?
```

Output: `/Plans/PLAN_invoice_request_20260108.md`
```markdown
---
type: email_response
requires_approval: true
estimated_amount: $2500
---

## Request Analysis
Client John from ClientCo requesting December invoice.
This is a known client with payment history.

## Proposed Action
1. Generate invoice from December timesheet
2. Send via email with standard terms

## Draft Email
[Pre-filled template from handbook]

## Approval Needed
Yes - involves financial transaction documentation
```

## Success Criteria
- ✅ All emails are acknowledged within 4 hours
- ✅ Responses use handbook-approved templates
- ✅ Sensitive communications require approval
- ✅ Actions are logged properly

## Error Handling
- If sender is unknown, flag for human review
- If request is unclear, ask clarifying questions
- If urgent and ambiguous, escalate immediately
```

3. **Test the skill:**
```bash
claude code --skill /path/to/Skills/email-processor/SKILL.md "Process the email in Needs_Action folder"
```

---

## Step 7: Complete Bronze Tier Checklist

### ✅ Vault Structure
- [x] Dashboard.md created
- [x] Company_Handbook.md created
- [x] Business_Goals.md created
- [x] /Needs_Action folder
- [x] /Plans folder
- [x] /Done folder
- [x] /Pending_Approval folder
- [x] /Approved folder
- [x] /Rejected folder
- [x] /Logs folder

### ⏳ Working Watcher
- [ ] file_watcher.py running successfully
- [ ] Test file dropped and detected
- [ ] Action file created in /Needs_Action
- [ ] Original file copied to /Inbox

### ⏳ Claude Code Integration
- [x] Claude Code installed and accessible
- [x] Can read files from vault
- [x] Can write plan files to /Plans
- [x] Follows Company_Handbook rules

### ⏳ Agent Skills
- [x] At least one skill created (email-processor)
- [ ] Skill has clear trigger conditions
- [ ] Skill follows structured format
- [ ] Successfully processes test case

---

## Testing Your Bronze System

### Test Scenario 1: File Drop
1. Drop a PDF invoice into drop folder
2. Verify action file created
3. Run Claude Code to analyze it
4. Check Plans folder for output

### Test Scenario 2: Email Processing
1. Manually create email file in /Needs_Action
2. Use email processor skill
3. Verify plan created with draft response
4. Check if approval requested (if needed)

### Test Scenario 3: Dashboard Update
1. Have Claude read all recent activity
2. Ask it to update Dashboard.md
3. Verify metrics updated correctly

---

## Troubleshooting

### Watcher Not Detecting Files
- Check folder paths in .env are correct
- Ensure drop folder exists
- Try dropping file while watching terminal output
- Check file permissions

### Claude Code Can't Find Vault
- Verify you're running from vault directory
- Use absolute paths in commands
- Check Obsidian vault is not locked/encrypted

### Skills Not Working
- Ensure SKILL.md is properly formatted
- Use `--skill` flag when invoking Claude
- Check skill path is correct

---

## Next Steps: Moving to Silver Tier

Once Bronze is complete, you can:
1. Add Gmail API watcher for real emails
2. Create MCP server for sending emails
3. Add scheduling with cron/Task Scheduler
4. Build approval workflow automation
5. Create more sophisticated skills

---

## Submission for Bronze Tier

To complete Bronze tier submission:

1. **Create demo video** (2-3 minutes):
   - Show watcher detecting file
   - Show Claude processing it
   - Show plan creation

2. **Document your vault structure** (screenshot)

3. **Include your skill SKILL.md** file

4. **Submit via form:** https://forms.gle/JR9T1SJq5rmQyGkGA

---

## Resources

- [Claude Code Documentation](https://agentfactory.panaversity.org/docs/AI-Tool-Landscape/claude-code-features-and-workflows)
- [Agent Skills Guide](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [Obsidian Help](https://help.obsidian.md/)
- [Wednesday Meeting](https://us06web.zoom.us/j/87188707642?pwd=a9XloCsinvn1JzICbPc2YGUvWTbOTr.1)

---

**Congratulations on starting your AI Employee journey! 🎉**
